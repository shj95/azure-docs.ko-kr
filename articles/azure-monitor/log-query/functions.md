---
title: Azure Monitor 로그 쿼리의 함수 | Microsoft Docs
description: 이 문서에서는 함수를 사용하여 Azure Monitor의 한 쿼리에서 다른 로그 쿼리를 호출하는 방법을 설명합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2018
ms.openlocfilehash: 7d94e53abbe8f4d2953729aa2363c3906ce94f74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670222"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Azure Monitor 로그 쿼리의 함수 사용

로그 쿼리를 다른 쿼리와 함께 사용하여 함수로 저장할 수 있습니다. 이렇게 하면 복잡한 쿼리를 여러 부분으로 나누어 간소화하고, 공통 코드를 여러 쿼리에서 다시 사용할 수 있습니다.

## <a name="create-a-function"></a>함수 만들기

Azure 포털에서 로그 분석을 사용하여 **저장을** 클릭한 다음 다음 표의 정보를 제공하여 함수를 만듭니다.

| 설정 | 설명 |
|:---|:---|
| 이름           | **쿼리 탐색기**에 나타나는 쿼리의 표시 이름입니다. |
| 다른 이름으로 저장        | 함수 |
| 함수 별칭 | 다른 쿼리에서 함수를 사용하기 위한 약식 이름입니다. 공백을 포함할 수 없으며 고유해야 합니다. |
| Category       | **쿼리 탐색기**에서 저장된 쿼리 및 함수를 구성하는 범주입니다. |

> [!NOTE]
> Azure Monitor의 함수는 다른 함수를 포함할 수 없습니다.




## <a name="use-a-function"></a>함수 사용
다른 쿼리에 해당 별칭을 포함하여 함수를 사용합니다. 다른 테이블처럼 사용할 수 있습니다.

## <a name="example"></a>예제
다음 샘플 쿼리는 마지막 날에 보고된 모든 누락된 보안 업데이트를 반환합니다. 별칭 _security_updates_last_day_를 사용하여 이 쿼리를 함수로 저장합니다. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

다른 쿼리를 만들고 _security_updates_last_day_ 함수를 참조하여 SQL 관련 필수 보안 업데이트를 검색합니다.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>다음 단계
Azure Monitor 로그 쿼리 작성에 대한 다른 단원을 참조하세요.

- [문자열 작업](string-operations.md)
- [날짜 및 시간 작업](datetime-operations.md)
- [집계 함수](aggregations.md)
- [고급 집계](advanced-aggregations.md)
- [JSON 및 데이터 구조](json-data-structures.md)
- [조인](joins.md)
- [차트](charts.md)
