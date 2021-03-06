---
title: Azure 응용 프로그램 오퍼에 대한 SUS 구성 | Azure 마켓플레이스
description: Azure 관리 애플리케이션 및 Azure 솔루션 템플릿용 SKU를 구성하는 방법입니다.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 043394a1303456ce5b209bb84b5afaf09f6beba4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289076"
---
# <a name="azure-application-skus-tab"></a>Azure 애플리케이션 SKU 탭

이 문서에는 SKU 탭을 사용하여 Azure 애플리케이션용 SKU를 만드는 방법을 설명합니다. 

> [!IMPORTANT]
> SKU를 구성하는 단계는 관리되는 애플리케이션 제품과 솔루션 템플릿 제품에서 다릅니다. 이러한 차이는 이 문서에 설명되어 있습니다. 

## <a name="configure-azure-application-skus"></a>Azure 애플리케이션 SKU 구성

### <a name="create-a-new-sku"></a>새 SKU 만들기

새 SKU를 만들려면 다음 단계를 수행합니다.

1. **SKU** 탭을 선택합니다.
2. SKU에서 **+ 새 SKU**를 선택합니다.

    ![새 SKU 프롬프트](./media/azureapp-plus-sku.png)

3. 새 SKU 팝업 창에 **SKU ID**를 입력합니다. 이 ID는 50자로 제한되며 소문자 영숫자 문자, 대시 또는 밑줄만 사용해야 합니다. SKU ID는 대시로 끝날 수 없습니다.
4. SKU ID는 제품 URL, Resource Manager 템플릿(해당되는 경우) 및 청구 보고서에서 고객이 볼 수 있습니다. 오퍼가 게시된 후에는 이 ID를 수정할 수 없습니다.

### <a name="sku-details-for-a-solution-template"></a>솔루션 템플릿에 대한 SKU 세부 정보

다음 화면 캡처에는 솔루션 템플릿에 대한 SKU 세부 정보 양식이 표시됩니다.

![솔루션 템플릿에 대한 SKU 세부 정보 양식](./media/azureapp-sku-details-solutiontemplate.png)

다음 SKU 값을 제공합니다.  별표가 추가된 필드는 필수입니다.

|    필드         |       설명                                                            |
|  ---------       |     ---------------                                                          |
|  **제목\***     | SKU의 제목입니다. 이 제목은 이 항목의 갤러리에 나타납니다.   |
| **요약\***    | SKU에 대한 간략한 요약 설명입니다. 최대 길이는 100자입니다.  |
| **설명\*** | SKU에 대한 자세한 설명입니다. 기본 HTML이 지원됩니다.                 | 
| **SKU 유형\***   | Azure 응용 프로그램 솔루션 유형은 이 시나리오에 대해 ***솔루션 템플릿을** 선택합니다. |
| **클라우드 가용성\*** | SKU의 위치입니다. 기본값은 **공용 Azure**입니다.  <b/>   **공용 Azure** - 앱은 마켓플레이스 통합이 있는 모든 공용 Azure 지역의 고객에게 배포할 수 있습니다.  <b/>   **Azure 정부 클라우드** - 앱이 Azure 정부 클라우드에 배포됩니다. [Azure 정부에](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)게시하기 전에 게시자가 이 환경에서 예상대로 솔루션이 작동하는지 테스트하고 유효성을 검사할 것을 권장합니다. 준비하고 테스트하려면 [평가판 계정](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/)을 요청하세요.  |
| **개인 SKU인가요?\*** | 선택한 고객 그룹에서만 이 SKU를 사용할 수 있는 경우 **예를** 선택합니다. |
|   |   |

  > [!NOTE] 
  > Microsoft Azure Government는 미국 연방, 주, 지방 또는 부족의 고객 및 이러한 실체에 서비스를 제공할 자격이 있는 파트너에 대한 액세스를 제어할 수 있는 정부 커뮤니티 클라우드입니다.


### <a name="sku-details-for-managed-application"></a>관리되는 애플리케이션에 대한 SKU 세부 정보

다음 화면 캡처에는 관리되는 애플리케이션에 대한 SKU 세부 정보 양식이 표시됩니다.

   ![관리되는 애플리케이션에 대한 SKU 세부 정보 양식](./media/azureapp-sku-details-managedapplication.png)

다음 SKU 설정을 구성합니다. 별표가 추가된 필드는 필수입니다.

|    필드         |       설명                                                            |
|  ---------       |     ---------------                                                          |
|  **제목\***     | SKU의 제목입니다. 이 제목은 이 항목의 갤러리에 나타납니다.   |
| **요약\***    | SKU에 대한 간략한 요약 설명입니다. 최대 길이는 100자입니다.  |
| **설명\*** | SKU에 대한 자세한 설명입니다. 기본 HTML이 지원됩니다.                 | 
| **SKU 유형\***   | Azure 응용 프로그램 솔루션의 유형은 이 시나리오에 대해 ***관리되는 응용 프로그램을** 선택합니다. 
| **클라우드 가용성\*** | SKU의 위치입니다. 기본값은 **공용 Azure**입니다.  <b/>   **공용 Azure** - 앱은 마켓플레이스 통합이 있는 모든 공용 Azure 지역의 고객에게 배포할 수 있습니다.  <b/>   **Azure 정부 클라우드** - 앱이 Azure 정부 클라우드에 배포됩니다. [Azure 정부에](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)게시하기 전에 게시자가 이 환경에서 예상대로 솔루션이 작동하는지 테스트하고 유효성을 검사할 것을 권장합니다. 준비하고 테스트하려면 [평가판 계정](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/)을 요청하세요.   Microsoft Azure Government는 미국 연방, 주, 지방 또는 부족의 고객 및 이러한 실체에 서비스를 제공할 자격이 있는 파트너에 대한 액세스를 제어할 수 있는 정부 커뮤니티 클라우드입니다. |
| **개인 SKU인가요?\*** | 선택한 고객 그룹에서만 이 SKU를 사용할 수 있는 경우 **예를** 선택합니다. |
| **국가/지역 이용 가능 여부\*** | **지역 선택을** 사용하여 사용 가능한 국가/지역 목록을 볼 수 있습니다. 각 국가/지역을 확인하고 **확인**을 선택하여 선택한 내용을 저장합니다.  <b/>   ![국가 및 지역 가용성 목록](./media/azure-app-select-country-region.png)  |
| **이전 가격\*** | SKU의 가격은 매월 USD입니다. 가격은 구성 당시의 환율을 사용하여 현지 통화로 설정합니다. 이러한 설정은 최종적으로 저장되므로 유효한지 검사합니다. 각 국가/지역의 가격을 개별적으로 설정하거나 보려면 가격 스프레드시트를 내보내고 사용자 지정 가격으로 가져오십시오.  가격 데이터를 내/가져오려면 가격 변경 내용을 저장해야 합니다.  |
| **간소화된 통화 가격\*** | SKU의 가격은 매월 USD입니다. 이전 가격 책정 설정과 동일해야 합니다. 자세한 내용은 [간소화된 통화 가격 책정](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-update-existing-offer)을 참조하세요. |
|  |  |


### <a name="package-details-for-solution-template"></a>솔루션 템플릿에 대한 패키지 세부 정보

![솔루션 템플릿에 대한 패키지 세부 정보](./media/azureapp-sku-pkgdetails-solutiontemplate.png)

다음 **패키지 세부 정보** 값을 제공합니다.  별표가 추가된 필드는 필수입니다.

- **버전\* ** - 업로드할 패키지의 버전입니다. 버전 태그는 X.Y.Z 형식이며, X, Y 및 Z는 정수여야 합니다.
- **패키지 파일(.zip)\* ** - 이 패키지에는 .zip 파일에 저장된 다음 파일이 포함되어 있습니다.
  - **mainTemplate.json\* ** - 솔루션/응용 프로그램을 배포하고 솔루션에 정의된 리소스를 만드는 데 사용되는 배포 템플릿 파일입니다. 자세한 내용은 [배포 템플릿 파일을 작성하는 방법을](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)참조하십시오.
  - **createUIDefinition.json\* ** - 이 파일은 Azure 포털에서 이 솔루션/응용 프로그램을 프로비전하기 위한 사용자 인터페이스를 생성하는 데 사용됩니다. 자세한 내용은 [관리되는 응용 프로그램에 대한 Azure 포털 사용자 인터페이스 만들기](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)를 참조하십시오.
  - 스크립트(필요한 경우) - 템플릿을 실행할 때 필요할 수 있는 추가 `Microsoft.Compute/virtualMachines/extensions`스크립트(예: )
  - 중첩된 템플릿(필요한 경우) - 추가 중첩된 템플릿입니다.

  > [!IMPORTANT] 
  > 이 패키지는 이 애플리케이션을 프로비전하는 데 필요한 중첩된 템플릿 또는 스크립트를 포함해야 합니다. mainTemplate.json 파일 및 createUIDefinition.json 파일은 루트 폴더에 있어야합니다. 배포 아티팩트에 대한 자세한 내용은 [Azure 리소스 관리자 템플릿 - 모범 사례 가이드를](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#deployment-artifacts-nested-templates-scripts)참조하십시오.


### <a name="package-details-for-managed-application"></a>관리되는 애플리케이션에 대한 패키지 세부 정보

   ![관리되는 애플리케이션에 대한 패키지 세부 정보](./media/azureapp-sku-pkgdetails-managedapplication.png)

다음 패키지 세부 정보를 제공합니다.  별표가 추가된 필드는 필수입니다.

- **버전\* ** - 업로드할 패키지의 버전입니다. 버전 태그는 X.Y.Z 형식이며, X, Y 및 Z는 정수여야 합니다.
- **패키지 파일(.zip)\* ** - 이 패키지에는 .zip 파일에 저장된 다음 파일이 포함되어 있습니다.
  - applianceMainTemplate.json - 솔루션/애플리케이션을 배포하고 정의된 리소스를 만드는 데 사용되는 배포 템플릿 파일입니다. 자세한 내용은 [빠른 시작: Azure 포털을 사용하여 Azure 리소스 관리자 템플릿 만들기 및 배포를](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)참조하세요. 
  - applianceCreateUIDefinition.json - 이 파일은 Azure Portal에서 이 솔루션/애플리케이션을 프로비전하기 위한 사용자 인터페이스를 생성하는 데 사용됩니다. 자세한 내용은 [관리되는 응용 프로그램에 대한 Azure 포털 사용자 인터페이스 만들기](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)를 참조하십시오.
  - mainTemplate.json - Microsoft.Solution/appliances 리소스만 포함하는 템플릿 파일입니다. 자세한 내용은 [Azure 리소스 관리자 템플릿의 구조 및 구문 이해를](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)참조하십시오. <br>
이 리소스의 다음 주요 속성을 기록해 둡니다.
    - "종류" - 마켓플레이스 관리 응용 프로그램의 경우 값은 "마켓플레이스"여야 합니다.
    - "ManagedResourceGroupId" - 어플라이언스MainTemplate.json에 정의된 모든 리소스가 배포되는 고객 구독의 리소스 그룹입니다.
    - "PublisherPackageId" - 패키지를 고유하게 식별하는 문자열입니다. 이 값은 다음과 같이 구성해야 합니다. [오퍼Id]-미리 보기[SKUID]. [패키지버전].

  >[!IMPORTANT] 
  >이 패키지는 이 애플리케이션을 프로비전하는 데 필요한 중첩된 템플릿 또는 스크립트를 포함해야 합니다. 이러한 파일은 루트 폴더에 있어야합니다 : MainTemplate.json, 어플라이언스MainTemplate.json, 및 어플라이언스CreateUIDefinition.json.

- **테넌트 ID\* ** - 조직의 Azure Active Directory 테넌트 ID입니다.
- **JIT 액세스를 사용합니까? \* ** - 이 제안을 사용하는 고객 배포에 대해 Just-In-Time 관리 액세스를 사용하려면 **예를** 선택합니다.

  >[!NOTE] 
  >JIT를 사용하는 경우 JIT 액세스를 지원하도록 CreateUiDefinition.json 파일을 업데이트해야 합니다.

관리되는 애플리케이션에 대해 권한 부여 및 정책 설정을 구성해야 합니다.


#### <a name="authorization"></a>권한 부여

관리되는 리소스 그룹에 대해 권한을 부여하려는 사용자, 그룹 또는 애플리케이션의 Azure Active Directory 식별자를 추가합니다. 부여된 권한은 역할 정의 ID로 표시됩니다. 소유자, 기여자 또는 사용자 지정 역할일 수 있습니다.


#### <a name="policy-settings"></a>정책 설정

관리되는 앱이 준수하는 정책을 추가합니다. Azure 리소스 정책에 대한 자세한 내용은 [Azure Policy란?](../../../governance/policy/overview.md)을 참조하세요.

   ![관리되는 애플리케이션에 대한 권한 부여 및 정책 설정](./media/azureapp-sku-details-managedapp-auth-policy.png)

**새 권한 부여를 만들려면**

1. **권한 부여** 아래에서 **+ 새 권한 부여**를 선택합니다.
2. **보안 주체 ID**에 대해 관리되는 리소스 그룹에 대해 권한을 부여하려는 사용자, 그룹 또는 애플리케이션의 Azure Active Directory 식별자를 추가합니다. 부여된 권한은 역할 정의에 의해 표시됩니다.
3. **역할 정의의**경우 드롭다운 목록에서 소유자 또는 기여자 중 하나를 선택합니다. 자세한 내용은 [Azure 리소스에 대한 기본 제공 역할](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)을 참조하세요.

>[!NOTE] 
>여러 권한 부여를 추가할 수 있습니다. 그러나 Active Directory 사용자 그룹을 만들고 "PrincipalId"에 ID를 지정하는 것이 좋습니다. 이렇게 하면 사용자 그룹에 더 많은 사용자를 추가할 수 있으며 SKU를 업데이트하지 않아도 됩니다.

**새 정책을 만들려면**

1. **정책 설정** 아래에서 **+ 새 정책**을 선택합니다.
2. **정책 이름**에 정책의 새 이름을 입력합니다. 이름의 최대 길이는 50자입니다.
3. **정책**에 대해 드롭다운 목록의 옵션 중 하나를 선택합니다. 데이터 공급자가 애플리케이션이 데이터를 사용할 때 사용하도록 설정하려는 정책을 선택합니다. 자세한 내용은 [Azure Policy 샘플](https://docs.microsoft.com/azure/governance/policy/samples/index)을 참조하세요.

    ![관리되는 애플리케이션에 대한 정책 설정](./media/azureapp-sku-policy-settings.png)

4. **정책 SKU**에서 정책 SKU 유형으로 무료 또는 표준을 선택합니다. 표준 SKU는 감사 정책에 필요합니다.


## <a name="next-steps"></a>다음 단계

[마켓플레이스 탭에서](./cpp-marketplace-tab.md)쿠폰 및 공급 마케팅 자산에 대해 자세히 설명합니다. 
