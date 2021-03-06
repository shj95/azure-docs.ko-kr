---
title: Azure VM에서 업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션 등록
description: Azure Automation에 포함된 업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션을 사용하여 Azure 가상 머신을 등록하는 방법을 알아봅니다.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 621b429f5dc3a6b6620e4d41ad46763e1d4fa226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299532"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Azure 가상 머신에서 업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션 등록

Azure Automation은 운영 체제 보안 업데이트를 관리하고, 변경 내용을 추적하며, 컴퓨터에 설치된 항목을 재고 자산으로 처리(인벤토리)하도록 돕는 솔루션을 제공합니다. 컴퓨터를 등록하는 방법은 여러 가지가 있습니다. 가상 머신, [Automation 계정](automation-onboard-solutions-from-automation-account.md), [여러 컴퓨터 검색](automation-onboard-solutions-from-browse.md)에서 또는 [Runbook](automation-onboard-solutions.md)을 사용하여 솔루션을 등록할 수 있습니다. 이 문서에서는 Azure 가상 머신에서 이러한 솔루션을 등록하는 방법에 대해 설명합니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[https://portal.azure.com](https://portal.azure.com) 에서 Azure Portal에 로그인합니다.

## <a name="enable-the-solutions"></a>솔루션을 사용하도록 설정

먼저 VM에서 세 가지 솔루션을 모두 활성화합니다.

1. Azure [포털에서](https://portal.azure.com)왼쪽 창에서 **가상 컴퓨터를** 선택하거나 **홈** 페이지에서 **가상 컴퓨터를** 검색하고 선택합니다.
2. 솔루션을 사용하도록 설정할 VM을 선택합니다.
3. VM 페이지에서 **작업**에서 **업데이트 관리,** **인벤토리**또는 **변경 추적을**선택합니다. 가상 컴퓨터는 자동화 계정의 위치에 관계없이 모든 지역에 존재할 수 있습니다. VM에서 솔루션을 온보딩할 때 VM이 `Microsoft.OperationalInsights/workspaces/read` 작업 영역에 온보딩되어 있는지 확인할 수 있는 권한이 있어야 합니다. 필요한 추가 권한에 대해 알아보려면 [컴퓨터를 온보온하는 데 필요한 권한을](automation-role-based-access-control.md#onboarding)참조하세요.

한 번에 여러 컴퓨터를 등록하는 방법에 대해 알아보려면 [업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션 등록](automation-onboard-solutions-from-automation-account.md)을 참조하세요.

Azure Log Analytics 작업 영역 및 Automation 계정을 선택한 다음, **사용**을 선택하여 솔루션을 사용하도록 설정합니다. 솔루션을 사용하도록 설정하는 데 최대 15분이 걸립니다.

![업데이트 관리 솔루션 등록](media/automation-tutorial-update-management/manageupdates-update-enable.png)

다른 솔루션으로 이동한 다음, **사용**을 선택합니다. 이러한 솔루션은 이전에 활성화된 솔루션과 동일한 작업 영역 및 자동화 계정을 사용하기 때문에 Log Analytics 작업 영역 및 자동화 계정 드롭다운 목록이 비활성화됩니다.

> [!NOTE]
> **변경 내용 추적** 및 **인벤토리**는 동일한 솔루션을 사용합니다. 이러한 솔루션 중 하나가 활성화되면 다른 것도 활성화됩니다.

## <a name="scope-configuration"></a>범위 구성

각 솔루션은 작업 영역에서 범위 구성을 사용하여 솔루션을 가져오는 컴퓨터를 대상으로 합니다. 범위 구성은 솔루션 범위를 특정 컴퓨터로 제한하는 데 사용되는 하나 이상의 저장된 검색 그룹입니다. **관련 리소스에서**자동화 계정에서 범위 구성에 액세스하려면 **작업 영역을**선택합니다. 작업 영역에서 작업 **영역 데이터 원본에서** **범위 구성을 선택합니다.**

선택한 작업 영역에 업데이트 관리 또는 변경 내용 추적 솔루션이 없는 경우 다음과 같은 범위 구성이 생성됩니다.

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

선택한 작업 영역에 솔루션이 이미 있는 경우 솔루션은 다시 배포되지 않고 범위 구성이 추가되지 않습니다.

이 구성 중 하나에서 줄임표(**...**)를 선택한 다음, **편집**을 선택합니다. **편집 범위 구성** 창에서 **컴퓨터 그룹 선택**을 선택합니다. **컴퓨터 그룹** 창은 범위 구성을 만드는 데 사용되는 저장된 검색을 표시합니다.

## <a name="saved-searches"></a>저장된 검색

컴퓨터가 업데이트 관리, 변경 내용 추적 또는 인벤토리 솔루션에 추가되면 컴퓨터는 작업 영역에서 두 가지 저장된 검색 중 하나에 추가됩니다. 저장된 검색은 이러한 솔루션을 대상으로 하는 컴퓨터가 포함된 쿼리입니다.

작업 영역으로 이동합니다. **일반** 아래에서 **저장된 검색**을 선택합니다. 다음 표에는 이러한 솔루션에서 사용하는 두 가지 저장된 검색이 나와 있습니다.

|이름     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | 업데이트        | Updates__MicrosoftDefaultComputerGroup         |

저장된 검색 중 하나를 선택하여 그룹을 채우는 데 사용된 쿼리를 살펴봅니다. 다음 이미지에서는 쿼리 및 해당 결과를 보여 줍니다.

![저장된 검색](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>작업 영역 연결 해제

다음 솔루션은 Log Analytics 작업 영역에 따라 다릅니다.

* [업데이트 관리](automation-update-management.md)
* [변경 추적](automation-change-tracking.md)
* [작업이 없는 동안 VM 시작/중지](automation-solution-vm-management.md)

자동화 계정을 Log Analytics 작업 영역과 더 이상 통합하지 않으려면 Azure 포털에서 직접 계정을 연결 해제할 수 있습니다.  계속하기 전에 앞에서 언급한 솔루션을 제거해야 합니다. 그러지 않으면 이 프로세스가 계속 진행되지 않습니다. 가져온 특정 솔루션에 대한 문서를 검토하여 제거에 필요한 단계를 이해하세요.

이러한 솔루션을 제거한 후에 다음 단계에 따라 Automation 계정 연결을 해제할 수 있습니다.

> [!NOTE]
> 이전 버전의 Azure SQL 모니터링 솔루션을 포함한 일부 솔루션에서 자동화 자산을 만들었을 수 있으며, 작업 영역을 연결 해제하기 전에 제거해야 할 수도 있습니다.

1. Azure Portal에서 Automation 계정을 열고 Automation 계정 페이지에서 왼쪽의 **관련된 리소스** 섹션 아래의 **연결된 작업 영역**을 선택합니다.

2. 작업 영역 연결 해제 페이지에서 **작업 영역 연결 해제**를 클릭합니다.

   ![작업 영역 연결 해제 페이지](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   계속할지 묻는 메시지가 나타납니다.

3. Azure Automation이 Log Analytics 작업 영역의 연결을 끊으려고 하는 동안 메뉴의 **알림**에서 진행 상황을 추적할 수 있습니다.

업데이트 관리 솔루션을 사용한 경우 솔루션을 제거한 후 더 이상 필요하지 않은 다음 항목을 제거할 수도 있습니다.

* 업데이트 일정 - 각 업데이트 배포와 일치하는 이름이 있습니다.

* 솔루션을 위해 만든 하이브리드 작업자 그룹 - 각각 machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8)와 유사하게 이름이 지정됩니다.

작업이 없는 동안 VM 시작/중지를 사용한 경우 솔루션을 제거한 후 더 이상 필요하지 않은 다음 항목을 제거할 수도 있습니다.

* VM runbook 시작 및 중지 일정
* VM runbook 시작 및 중지
* 변수

또는 Log Analytics 작업 영역에서 자동화 계정에서 작업 영역의 연결을 해제할 수도 있습니다. 작업 영역에서 **관련 리소스**에서 **자동화 계정을** 선택합니다. 자동화 계정 페이지에서 **계정 연결 해제를 선택합니다.**

## <a name="clean-up-resources"></a>리소스 정리

업데이트 관리에서 VM을 제거하려면:

* Log Analytics 작업 영역에서, 범위 구성 `MicrosoftDefaultScopeConfig-Updates`에 대한 저장된 검색에서 VM을 제거합니다. 저장된 검색은 작업 영역의 **일반**에서 찾을 수 있습니다.
* [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) 또는 [Linux용 Log Analytics 에이전트](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)를 제거합니다.

## <a name="next-steps"></a>다음 단계

솔루션을 사용하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

* [튜토리얼 - VM에 대한 업데이트 관리](automation-tutorial-update-management.md)

* [자습서 - VM에 설치된 소프트웨어 식별](automation-tutorial-installed-software.md)

* [자습서 - VM에 대한 변경 내용 문제 해결](automation-tutorial-troubleshoot-changes.md)
