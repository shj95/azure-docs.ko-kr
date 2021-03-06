---
title: Azure Data Lake Storage Gen2에 데이터 로드
description: Azure Data Factory를 사용하여 Azure Data Lake Storage Gen2에 데이터 복사
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/13/2019
ms.openlocfilehash: 90573f77c77d614923f882053145d2f84598953d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75440229"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-with-azure-data-factory"></a>Azure Data Factory를 사용하여 Azure Data Lake Storage Gen2에 데이터 로드

Azure Data Lake Storage Gen2는 [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)를 기반으로 하는 빅 데이터 분석 전용의 기능 세트입니다. 이를 사용하면 파일 시스템 및 개체 스토리지 패러다임을 모두 사용하여 데이터를 조작할 수 있습니다.

Azure 데이터 팩터리(ADF)는 완전히 관리되는 클라우드 기반 데이터 통합 서비스입니다. 분석 솔루션을 빌드할 때 서비스를 사용하여 풍부한 온-프레미스 및 크라우드 기반 데이터 저장소의 데이터로 레이크를 채우고 시간을 절약할 수 있습니다. 지원되는 커넥터의 자세한 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats) 표를 참조하세요.

Azure Data Factory는 스케일 아웃, 관리되는 데이터 이동 솔루션을 제공합니다. ADF의 스케일 아웃 아키텍처로 인해 높은 처리량으로 데이터를 수집할 수 있습니다. 자세한 내용은 [복사 작업 성능](copy-activity-performance.md)을 참조하세요.

이 아티클에서는 Data Factory 복사 데이터 도구를 사용하여 _Amazon Web Services S3 서비스_의 데이터를 _Azure Data Lake Storage Gen2_로 로드하는 방법을 설명합니다. 다른 데이터 저장소 유형에서 데이터를 복사할 때도 이와 유사한 단계를 따를 수 있습니다.

>[!TIP]
>Azure Data Lake Storage Gen1에서 Gen2로 데이터를 복사하는 방법은 [이 연습](load-azure-data-lake-storage-gen2-from-gen1.md)을 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독: Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/)을 만듭니다.
* 데이터 레이크 저장소 Gen2가 활성화된 Azure 저장소 계정: 저장소 계정이 없는 경우 [계정을 만듭니다.](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)
* 데이터를 포함하는 S3 버킷을 포함한 AWS 계정: 이 아티클에서는 Amazon S3에서 데이터를 복사하는 방법을 보여줍니다. 다음과 같은 유사한 단계를 수행하여 다른 데이터 저장소를 사용할 수 있습니다.

## <a name="create-a-data-factory"></a>데이터 팩터리 만들기

1. 왼쪽 메뉴에서 리소스 > **데이터 만들기 + 분석** > **데이터 팩터리** **만들기를**선택합니다.
   
   !["새로 만들기" 창에서 데이터 팩터리 선택](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. **새 데이터 팩터리** 페이지에서 다음 그림에 표시된 필드의 값을 제공합니다. 
      
   ![새 데이터 팩터리 페이지](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **이름**: Azure 데이터 팩터리의 전역 고유 이름을 입력합니다. "데이터 팩터리 이름 \"LoadADLSDemo\"를 사용할 수 없습니다" 오류가 발생하면 데이터 팩터리의 다른 이름을 입력합니다. 예를 들어 _**yourname**_**ADFTutorialDataFactory**라는 이름을 사용할 수 있습니다. 데이터 팩터리를 다시 만들어 봅니다. 데이터 팩터리 아티팩트에 대한 명명 규칙은 [데이터 팩터리 명명 규칙](naming-rules.md)을 참조하세요.
    * **구독**: 데이터 팩터리를 만들 Azure 구독을 선택합니다. 
    * **리소스 그룹**: 드롭다운 목록에서 기존 리소스 그룹을 선택하거나 **새로 만들기** 옵션을 선택하고 리소스 그룹의 이름을 입력합니다. 리소스 그룹에 대한 자세한 내용은 [리소스 그룹을 사용하여 Azure 리소스 관리](../azure-resource-manager/management/overview.md)를 참조하세요.  
    * **버전**: **V2**를 선택합니다.
    * **위치**: 데이터 팩터리의 위치를 선택합니다. 지원되는 위치만 드롭다운 목록에 표시됩니다. 데이터 팩터리에서 사용되는 데이터 저장소가 다른 위치 및 지역에 있어도 됩니다. 

3. **만들기**를 선택합니다.
4. 만들기가 완료되면 데이터 팩터리로 이동합니다. 다음 그림과 같이 **데이터 팩터리** 홈페이지가 표시됩니다. 
   
   ![데이터 팩터리 홈페이지](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   **작성 및 모니터링** 타일을 선택하여 별도의 탭에서 데이터 통합 애플리케이션을 시작합니다.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2에 데이터 로드

1. **시작** 페이지에서 **데이터 복사** 타일을 선택하여 데이터 복사 도구를 시작합니다. 

   ![데이터 복사 도구 타일](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. **속성** 페이지에서 **작업 이름** 필드를 **CopyFromAmazonS3ToADLS**로 지정하고 **다음**을 선택합니다.

    ![속성 페이지](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. **원본 데이터 저장소** 페이지에서 **+ 새 연결 만들기**를 클릭합니다.

    ![원본 데이터 저장소 페이지](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    커넥터 갤러리에서 **Amazon S3**을 선택하고 **계속**을 선택합니다.
    
    ![원본 데이터 저장소 s3 페이지](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. **Amazon S3 연결 지정** 페이지에서 다음 단계를 수행합니다.

   1. **액세스 키 ID** 값을 지정합니다.
   2. **비밀 액세스 키** 값을 지정합니다.
   3. **연결 테스트**를 클릭하여 설정의 유효성을 검사한 다음, **마침**을 선택합니다.
   4. 새 연결이 생성되었다고 표시됩니다. **다음**을 선택합니다.
   
      ![Amazon S3 계정 지정](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
      
5. **입력 파일 또는 폴더 선택** 페이지에서, 복사하려는 폴더 및 파일로 이동합니다. 폴더/파일을 선택하고 **선택**을 선택합니다.

    ![입력 파일 또는 폴더 선택](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. **재귀적으로 파일 복사** 및 **이진 복사** 옵션을 선택하여 복사 동작을 지정합니다. **다음을**선택합니다.

    ![출력 폴더 지정](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. **대상 데이터 스토리지** 페이지에서 **+ 새 연결 만들기**를 클릭한 다음, **Azure Data Lake Storage Gen2**를 선택하고 **계속**을 선택합니다.

    ![대상 데이터 저장소 페이지](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. **Azure Data Lake Storage 연결 지정** 페이지에서 다음 단계를 수행합니다.

   1. "스토리지 계정 이름" 드롭다운 목록에서 Data Lake Storage Gen2 계정을 선택합니다.
   2. **마침**을 선택하여 연결을 만듭니다. 그런 다음 을 **선택합니다.**
   
   ![Azure Data Lake Storage Gen2 계정 지정](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. 출력 **파일 또는 폴더 선택** 페이지에서 **copyfroms3을** 출력 폴더 이름으로 입력하고 **다음**을 선택합니다. ADF는 복사본이 존재하지 않는 경우 복사 중에 해당 ADLS Gen2 파일 시스템과 하위 폴더를 만듭니다.

    ![출력 폴더 지정](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. **설정** 페이지에서 **다음**을 선택하여 기본 설정을 사용합니다.

    ![설정 페이지](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. **요약** 페이지에서 설정을 검토하고, **다음**을 선택합니다.

    ![요약 페이지](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. **배포 페이지**에서 **모니터**를 선택하여 파이프라인을 모니터링합니다.

    ![배포 페이지](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. 왼쪽의 **모니터** 탭이 자동으로 선택됩니다. **작업** 열에는 활동 실행 세부 정보를 보고 파이프라인을 다시 실행하는 링크가 포함되어 있습니다.

    ![파이프라인 실행 모니터링](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. 파이프라인 실행과 연결된 활동 실행을 보려면 **작업** 열에서 **활동 실행 보기** 링크를 선택합니다. 파이프라인에는 하나의 작업(복사 작업)만 있으므로 하나의 항목만 표시됩니다. 파이프라인 실행 보기로 전환하려면 위쪽의 **파이프라인** 링크를 선택합니다. **새로 고침**을 선택하여 목록을 새로 고칩니다. 

    ![작업 실행 모니터링](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. 각 복사 작업의 실행 세부 정보를 모니터링하려면 작업 모니터링 보기의 **작업** 아래에서 **세부 정보** 링크(안경 이미지)를 선택합니다. 원본에서 싱크로 복사되는 데이터 볼륨, 데이터 처리량, 해당 기간의 실행 단계, 사용되는 구성 등의 세부 정보를 모니터링할 수 있습니다.

    ![작업 실행 세부 정보 모니터링](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. 데이터가 Data Lake Storage Gen2 계정에 복사되었는지 확인합니다.

## <a name="next-steps"></a>다음 단계

* [활동 개요 복사](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2 커넥터](connector-azure-data-lake-storage.md)
