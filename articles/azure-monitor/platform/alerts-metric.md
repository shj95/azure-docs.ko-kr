---
title: Azure 모니터를 사용하여 메트릭 경고 생성, 보기 및 관리
description: Azure 포털 또는 CLI를 사용하여 메트릭 경고 규칙을 생성, 보기 및 관리하는 방법을 알아봅니다.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 03/13/2020
ms.subservice: alerts
ms.openlocfilehash: cefccd08ea66638f08f00e280fe2704444a7f916
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369389"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Azure Monitor를 사용하여 메트릭 경고 만들기, 보기 및 관리

Azure Monitor의 메트릭 경고는 메트릭 중 하나가 임계값을 초과할 때 알림을 받을 수 있는 방법을 제공합니다. 메트릭 경고는 다양한 다차원 플랫폼 메트릭, 사용자 지정 메트릭, Application Insights 표준 및 사용자 지정 메트릭에서 작동합니다. 이 문서에서는 Azure 포털 및 Azure CLI를 통해 메트릭 경고 규칙을 만들고 보고 관리하는 방법을 설명합니다. [별도의 문서에](alerts-metric-create-templates.md)설명된 Azure 리소스 관리자 템플릿을 사용하여 메트릭 경고 규칙을 만들 수도 있습니다.

메트릭 경고의 작동 원리는 [메트릭 경고 개요](alerts-metric-overview.md)에서 자세히 알아볼 수 있습니다.

## <a name="create-with-azure-portal"></a>Azure Portal을 사용하여 만들기

다음은 Azure Portal에서 메트릭 경고 규칙을 만드는 절차입니다.

1. [Azure 포털에서](https://portal.azure.com) **모니터를**클릭합니다. [모니터] 블레이드는 모든 모니터링 설정 및 데이터를 하나의 뷰에 통합합니다.

2. **경고**를 클릭한 다음, **+ 새로운 경고 규칙**을 클릭합니다.

    > [!TIP]
    > 대부분의 리소스 블레이드도 **모니터링** 아래의 리소스 메뉴에 **경고**가 있으며, 여기서 경고를 만들 수 있습니다.

3. **대상 선택**을 클릭하고, 로드되는 컨텍스트 창에서 경고를 사용할 대상 리소스를 선택합니다. **구독** 및 **리소스 종류** 드롭다운을 사용하여 모니터링하려는 리소스를 찾습니다. 검색 창을 사용하여 리소스를 검색할 수도 있습니다.

4. 선택한 리소스에 경고를 만들 수 있는 메트릭이 있으면 오른쪽 아래의 **사용 가능한 신호**에 메트릭이 포함됩니다. 이 [문서](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)의 메트릭 경고에 지원되는 리소스 종류의 전체 목록을 볼 수 있습니다.

5. 대상 리소스를 선택한 후에는 **조건 추가**를 클릭합니다.

6. 리소스에 지원되는 신호 목록이 표시되면 경고를 만들 메트릭을 선택합니다.

7. 지난 6시간 동안의 메트릭에 대한 차트가 표시됩니다. 차트 **기간** 드롭다운을 사용하여 메트릭의 더 긴 기록을 보려면 선택합니다.

8. 측정항목에 차원이 있는 경우 차원 테이블이 표시됩니다. 차원당 하나 이상의 값을 선택하세요.
    - 표시된 측정기준값은 지난 3일간의 측정항목 데이터를 기반으로 합니다.
    - 찾고 있는 차원 값이 표시되지 않으면 "+"를 클릭하여 사용자 지정 값을 추가합니다.
    - 차원에 대해 **\*를 선택**할 수도 있습니다. **\*를 선택**하면 선택 항목이 동적으로 차원의 모든 현재 및 미래 값으로 조정됩니다.

    메트릭 경고 규칙은 선택한 모든 값 조합에 대한 조건을 평가합니다. [다차원 메트릭에 대한 경고의 작동 방식에 대해 자세히 알아보세요](alerts-metric-overview.md).

9. **임계값** 유형, **연산자**및 **집계 유형을**선택합니다. 이렇게 하면 메트릭 경고 규칙이 평가할 논리가 결정됩니다.
    - **정적** 임계값을 사용하는 경우 **임계값을**계속 정의합니다. 메트릭 차트는 적절한 임계값을 결정하는 데 도움이 될 수 있습니다.
    - **동적** 임계값을 사용하는 경우 **임계값 민감도를**계속 정의합니다. 최근 데이터에 기반하여 계산된 임계값이 메트릭 차트에 표시됩니다. [동적 임계값 조건 형식 및 민감도 옵션에 대해 자세히 알아보세요](alerts-dynamic-thresholds.md).

10. 선택적으로 **집계 세분성** 및 **평가 빈도를**조정하여 조건을 구체화합니다. 

11. **완료**를 클릭합니다.

12. 필요에 따라 복잡한 경고 규칙을 모니터링하려면 다른 조건을 추가합니다. 현재 사용자가 동적 임계값을 단일 조건으로 적용하는 경고 규칙을 사용할 수 있습니다.

13. **경고 규칙 이름,** **설명**및 **심각도와**같은 **경고 세부 정보를** 입력합니다.

14. 기존 작업 그룹을 선택하거나 새 작업 그룹을 만들어서 경고에 작업 그룹을 추가합니다.

15. **완료**를 클릭하여 메트릭 경고 규칙을 저장합니다.

> [!NOTE]
> 포털을 통해 생성되는 메트릭 경고는 대상 리소스와 동일한 리소스 그룹에 생성됩니다.

## <a name="view-and-manage-with-azure-portal"></a>Azure Portal을 사용하여 보기 및 관리

[경고] 아래에서 [규칙 관리] 블레이드를 사용하여 메트릭 경고 규칙을 살펴보고 관리할 수 있습니다. 아래 절차는 메트릭 경고 규칙을 살펴보고 편집하는 방법을 보여줍니다.

1. Azure Portal에서 **모니터**로 이동합니다.

2. **경고**를 클릭하고 **규칙 관리**를 클릭합니다.

3. **규칙 관리** 블레이드에서 구독의 모든 경고 규칙을 볼 수 있습니다. **리소스 그룹**, **리소스 유형**및 리소스 를 사용하여 규칙을 추가로 필터링할 수 **있습니다.** 메트릭 경고만 보려면 **신호 유형**을 메트릭으로 선택합니다.

    > [!TIP]
    > **규칙 관리** 블레이드에서 여러 경고 규칙을 선택하고 활성화/비활성화할 수 있습니다. 이 기능은 특정 대상 리소스를 유지 관리해야 하는 경우에 유용하게 사용할 수 있습니다.

4. 편집하려는 메트릭 경고 규칙의 이름을 클릭합니다.

5. [규칙 편집]에서 편집하려는 **경고 조건**을 클릭합니다. 메트릭, 임계값 조건 및 기타 필드를 필요한 대로 변경할 수 있습니다.

    > [!NOTE]
    > 메트릭 경고가 만들어진 후에는 **대상 리소스** 및 **경고 규칙 이름**을 편집할 수 없습니다.

6. **완료**를 클릭하여 편집 내용을 저장합니다.

## <a name="with-azure-cli"></a>Azure CLI 사용

이전 섹션에서는 Azure 포털을 사용하여 메트릭 경고 규칙을 만들고 보고 관리하는 방법을 설명했습니다. 이번 섹션에서는 플랫폼 간 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)를 사용하여 동일한 작업을 수행하는 방법을 설명합니다. Azure CLI를 가장 빠르게 사용하는 방법은 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)입니다. 이 문서에서는 클라우드 셸을 사용합니다.

1. Azure 포털로 이동하여 **클라우드 셸을 클릭합니다.**

2. 프롬프트에서 ``--help`` 옵션이 있는 명령을 사용하여 명령 및 사용 방법에 대해 자세히 알아볼 수 있습니다. 예를 들어 다음 명령은 메트릭 경고를 생성, 보기 및 관리하는 데 사용할 수 있는 명령 목록을 보여 주며

    ```azurecli
    az monitor metrics alert --help
    ```

3. VM의 평균 백분율 CPU가 90을 초과하는지 모니터링하는 간단한 메트릭 경고 규칙을 만들 수 있습니다.

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. 다음 명령을 사용하여 리소스 그룹의 모든 메트릭 경고를 볼 수 있습니다.

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. 규칙의 이름 또는 리소스 ID를 사용하여 특정 메트릭 경고 규칙의 세부 정보를 볼 수 있습니다.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. 다음 명령을 사용하여 메트릭 경고 규칙을 비활성화할 수 있습니다.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. 다음 명령을 사용하여 메트릭 경고 규칙을 삭제할 수 있습니다.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>다음 단계

- [Azure Resource Manager 템플릿을 사용하여 메트릭 경고를 만듭니다](../../azure-monitor/platform/alerts-metric-create-templates.md).
- [메트릭 경고의 작동 원리를 이해합니다](alerts-metric-overview.md).
- [동적 임계값 조건을 사용하는 메트릭 경고의 작동 원리를 이해합니다](alerts-dynamic-thresholds.md).
- [메트릭 경고에 대한 웹후크 스키마를 이해합니다](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema).

