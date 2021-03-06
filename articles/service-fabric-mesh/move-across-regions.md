---
title: 서비스 패브릭 메시 응용 프로그램을 다른 지역으로 이동
description: 현재 템플릿의 복사본을 새 Azure 지역에 배포하여 서비스 패브릭 메시 리소스를 이동할 수 있습니다.
author: erikadoyle
ms.author: edoyle
ms.topic: how-to
ms.date: 01/14/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 376808a6d8f61d4dc03d17061323a473d48053a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76908164"
---
# <a name="move-a-service-fabric-mesh-application-to-another-azure-region"></a>서비스 패브릭 메시 응용 프로그램을 다른 Azure 지역으로 이동

이 문서에서는 서비스 패브릭 메시 응용 프로그램과 해당 리소스를 다른 Azure 지역으로 이동하는 방법에 대해 설명합니다. 여러 가지 이유로 리소스를 다른 지역으로 이동할 수 있습니다. 예를 들어, 가동 중단에 대한 응답으로 특정 지역에서만 사용할 수 있는 기능이나 서비스를 얻거나, 내부 정책 및 거버넌스 요구 사항을 충족하거나, 용량 계획 요구 사항에 응답합니다.

 [서비스 패브릭 메시는](../azure-resource-manager/management/region-move-support.md#microsoftservicefabricmesh) Azure 리전에서 리소스를 직접 이동하는 기능을 지원하지 않습니다. 그러나 현재 Azure Resource Manager 템플릿의 복사본을 새 대상 지역에 배포한 다음 새로 만든 Service Fabric Mesh 응용 프로그램에 대한 트래픽 및 종속성을 리디렉션하여 리소스를 간접적으로 이동할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* 클라이언트와 서비스 패브릭 메시 응용 프로그램 간의 트래픽을 라우팅하기 위한 중개자 역할을 하는 응용 프로그램 게이트웨이(예: [응용 프로그램 게이트웨이)를](https://docs.microsoft.com/azure/application-gateway/)입력합니다.
* 대상 Azure 영역에서 서비스 패브릭 메시(미리`westus`보기) `westeurope`가용성 (또는) `eastus`

## <a name="prepare"></a>준비

1. Azure 리소스 관리자 템플릿 및 가장 최근 배포에서 매개 변수를 내보내 서비스 패브릭 메시 응용 프로그램의 현재 상태에 대한 "스냅샷"을 생성합니다. 이렇게 하려면 Azure 포털을 사용하여 [배포한 후 내보내기 템플릿의](../azure-resource-manager/templates/export-template-portal.md#export-template-after-deployment) 단계를 따릅니다. Azure [CLI,](../azure-resource-manager/management/manage-resource-groups-cli.md#export-resource-groups-to-templates)Azure [PowerShell](../azure-resource-manager/management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)또는 [REST API를](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate)사용할 수도 있습니다.

2. 해당하는 경우 대상 지역에서 재배포할 수 있는 [동일한 리소스 그룹의 다른 리소스를 내보냅니다.](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal#export-template-from-a-resource-group)

3. 내보낸 템플릿을 검토하고 편집하여 기존 속성 값이 대상 지역에서 사용할 템플릿인지 확인합니다. 새(Azure `location` 지역)는 재배포 하는 동안 제공할 매개 변수입니다.

## <a name="move"></a>이동

1. 대상 지역에서 새 리소스 그룹을 만들거나 기존 리소스 그룹을 사용합니다.

2. 내보낸 템플릿을 사용하여 Azure 포털을 사용하여 [사용자 지정 템플릿에서 리소스 배포](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template) 단계를 따릅니다. Azure [CLI,](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-cli)Azure [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell)또는 [REST API를](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-rest)사용할 수도 있습니다.

3. [Azure Storage 계정과](../storage/common/storage-account-move.md)같은 관련 리소스 이동에 대한 지침은 여러 [리전간에 Azure 리소스 이동](../azure-resource-manager/management/move-region.md)항목 아래에 나열된 개별 서비스에 대한 지침을 참조하십시오.

## <a name="verify"></a>확인

1. 배포가 완료되면 응용 프로그램 끝점을 테스트하여 응용 프로그램의 기능을 확인합니다.

2. 또한 [Azure Service Fabric Mesh CLI를](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-quickstart-deploy-container#set-up-service-fabric-mesh-cli)사용하여 응용 프로그램 상태(az mesh 앱[표시)를](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/app?view=azure-cli-latest#ext-mesh-az-mesh-app-show)확인하고 응용 프로그램 로그 및[(az 메쉬 코드 패키지-로그)](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/code-package-log?view=azure-cli-latest)명령을 검토하여 응용 프로그램의 상태를 확인할 수도 있습니다.

## <a name="commit"></a>Commit

대상 영역에서 Service Fabric Mesh 응용 프로그램의 동등한 기능을 확인한 후에는 트래픽이 새 응용 프로그램으로 리디렉션하도록 받는 컨트롤러(예: [응용 프로그램 게이트웨이)를](../application-gateway/redirect-overview.md)구성합니다.

## <a name="clean-up-source-resources"></a>소스 리소스 정리

서비스 패브릭 메시 응용 프로그램의 이동을 완료하려면 [소스 응용 프로그램 및/또는 상위 리소스 그룹을 삭제합니다.](../azure-resource-manager/management/delete-resource-group.md)

## <a name="next-steps"></a>다음 단계

* [여러 리전 간에 Azure 리소스 이동](../azure-resource-manager/management/move-region.md)
* [지역 간 Azure 리소스 이동 지원](../azure-resource-manager/management/region-move-support.md)
* [새 리소스 그룹 또는 구독으로 리소스 이동](../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [리소스에 대한 이동 작업 지원](../azure-resource-manager/management/move-support-resources.md
)
