---
title: '빠른 시작: Geo AI Data Science Virtual Machine 만들기'
titleSuffix: Azure Data Science Virtual Machine
description: 지리 공간적 분석 및 기계 학습을 수행하기 위해 Azure에서 Geo AI Data Science Virtual Machine을 구성하고 만듭니다.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: f3ff9bd64f54d8f83fd1889078e8a4c01827d135
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77525892"
---
# <a name="quickstart-set-up-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>빠른 시작: Azure에서 지리적 AI Virtual Machine 설정 

Geo-DSVM(지리적 AI Data Science Virtual Machine)은 AI와 지리 공간적 분석을 결합하도록 특별히 구성된 인기 있는 [Azure Data Science Virtual Machine](https://aka.ms/dsvm)의 확장입니다. VM의 지리 공간적 분석은 [ArcGIS Pro](https://www.arcgis.com/features/index.html)에 기반합니다. DSVM(Data Science Virtual Machine)을 사용하면 기계 학습 및 딥 러닝 모델을 빠르게 학습시킬 수 있습니다. 이러한 모델을 개발하기 위해 지리적 정보로 보강된 데이터를 사용합니다. Geo-DSVM은 Windows 2016 DSVM에서만 지원됩니다. 

Geo-DSVM에 포함된 AI 도구는 다음과 같습니다.

- 인기 있는 딥 러닝 프레임워크의 GPU 버전 - Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2 및 Chainer
- 이미지 및 텍스트 데이터를 획득하고 전처리하는 도구
- 개발 활동용 도구 - Microsoft Machine Learning Server Developer Edition, Anaconda Python, Python 및 R용 Jupyter Notebook, Python 및 R용 IDE, SQL 데이터베이스
- ESRI의 ArcGIS Pro 데스크톱 소프트웨어 및 AI 애플리케이션의 지리 공간적 데이터를 사용할 수 있는 Python 및 R 인터페이스
 

## <a name="create-your-geo-ai-data-science-vm"></a>지역 AI 데이터 과학 VM 만들기

지리적 AI Data Science VM의 인스턴스를 만들려면 다음 단계를 수행합니다.

1. [Azure Portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows)에서 가상 머신 목록으로 이동합니다.
1. 아래쪽에서 **만들기**를 선택하여 마법사를 생성합니다.

   ![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)

1. 마법사의 4개 단계 각각에는 입력이 필요합니다. 이 입력에 대한 자세한 내용은 다음 섹션을 참조하세요.

### <a name="wizard-details"></a>마법사 세부 정보 ###

**기본 사항**:

- **Name**: 만들려는 데이터 과학 서버 이름입니다.
    
- **사용자 이름**: 관리자 계정 로그인 ID입니다.
    
- **암호**: 관리자 계정 암호
    
- **구독**: 둘 이상의 구독이 있는 경우 머신을 만들고 요금을 청구할 구독을 선택합니다.
    
- **리소스 그룹**: 새 그룹을 만들거나 구독에서 **비어 있는** 기존 Azure 리소스 그룹을 사용할 수 있습니다.
    
- **위치**: 가장 적합한 데이터 센터를 선택합니다. 일반적으로 대부분의 데이터가 있거나 네트워크에 가장 빠르게 액세스하기 위해 실제 위치에 가장 가까운 데이터 센터입니다. GPU에서 딥 러닝을 실행하려면 NC 시리즈 GPU VM 인스턴스가 있는 Azure 위치 중 하나를 선택해야 합니다. 현재 이러한 위치는 **미국 동부, 미국 중북부, 미국 중남부, 미국 서부 2, 북유럽, 서유럽**. 최신 목록은 [지역별 Azure 제품 페이지](https://azure.microsoft.com/regions/services/)를 확인하고 **Compute** 아래에서 **NC 시리즈**를 찾으세요. 
    
    
**설정**: Geo-DSVM의 GPU에서 딥 러닝을 실행하려면 NC 시리즈 GPU 가상 머신 크기 중 하나를 선택합니다. 그렇지 않으면 CPU 기반 인스턴스 중 하나를 선택할 수 있습니다. VM에 대한 스토리지 계정을 만듭니다. 
       
**요약**: 입력한 모든 정보가 올바른지 확인합니다.
    
**구입**: 프로비저닝 프로세스를 시작하려면 **구입**을 클릭합니다. 서비스 사용 약관에 대한 링크가 제공됩니다. VM에는 **크기** 단계에서 선택한 서버 크기에 대한 컴퓨팅 요금 이외의 추가 요금이 없습니다. 
 
 >[!NOTE]
 > 프로비저닝하는 데 20~30분 정도 걸립니다. 프로비전의 상태는 Azure 포털에 표시됩니다.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>지역 AI 데이터 과학 Virtual Machine에 액세스하는 방법

 VM이 만들어지면 여기에 설치되고 미리 구성된 도구를 사용하여 시작할 수 있습니다. 다양한 도구에 대한 시작 메뉴 타일 및 바탕 화면 아이콘이 있습니다. 이전의 **기본 사항** 섹션에서 구성한 관리자 계정 자격 증명을 사용하여 원격 데스크톱에서 VM에 액세스할 수 있습니다.

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>VM에 설치된 ArcGIS Pro 사용

Geo-DSVM에는 ArcGIS Pro 데스크톱이 미리 설치되어 있으며, DSVM의 모든 도구에서 작동하도록 해당 환경이 미리 구성되어 있습니다. ArcGIS를 시작하면 ArcGIS 계정에 대한 자격 증명을 요구하는 메시지가 표시됩니다. ArcGIS 계정이 이미 있고 소프트웨어에 대한 라이선스가 있는 경우 기존 자격 증명을 사용할 수 있습니다.  

![Arc-GIS-Logon](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

그렇지 않으면 새 ArcGIS 계정 및 라이선스에 가입하거나 [평가판](https://www.arcgis.com/features/free-trial.html)을 얻을 수 있습니다. 

![ArcGIS-Free-Trial](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

표준 ArcGIS 계정 또는 평가판 중 하나에 가입하면 [ArcGIS Pro 시작](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf)의 지침에 따라 계정에 대한 ArcGIS Pro 권한을 부여할 수 있습니다.

ArcGIS 계정을 통해 ArcGIS Pro 데스크톱에 로그인하면 지리 공간적 분석 및 기계 학습 프로젝트를 위해 VM에 설치되고 구성된 데이터 과학 도구를 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

다음 리소스의 지침에 따라 지리적 AI Data Science VM 사용을 시작합니다.

* [지역 AI 데이터 과학 VM 사용](use-geo-ai-dsvm.md)
