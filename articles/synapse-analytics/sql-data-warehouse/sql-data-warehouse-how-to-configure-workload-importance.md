---
title: 워크로드 중요도 구성
description: Azure Synapse 분석에서 요청 수준 중요도를 설정하는 방법을 알아봅니다.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 0ab7b8be8780f7edb2734d99587bc7709ced9436
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633359"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Azure 시냅스 분석에서 워크로드 중요도 구성

Azure Synapse에 대한 Synapse SQL에서 중요도를 설정하면 쿼리 일정에 영향을 줍니다. 중요도가 높은 쿼리는 중요도가 낮은 쿼리 보다 앞에 실행되도록 예약됩니다. 쿼리에 중요도를 지정하려면 워크로드 분류기를 만들어야 합니다.

## <a name="create-a-workload-classifier-with-importance"></a>중요도가 있는 워크로드 분류기 만들기

데이터 웨어하우스 시나리오에서는 쿼리를 빠르게 실행해야 하는 사용자가 있는 경우가 많습니다.  사용자는 보고서를 실행해야 하는 회사의 임원이거나 adhoc 쿼리를 실행하는 분석가일 수 있습니다. 쿼리에 중요도를 할당하는 워크로드 분류기를 만듭니다.  아래 예제는 새 [만들기 워크로드 분류자](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 구문을 사용하여 두 개의 분류자를 만듭니다. `Membername`단일 사용자 또는 그룹일 수 있습니다. 개별 사용자 분류가 역할 분류보다 우선합니다. 기존 데이터 웨어하우스 사용자를 찾으려면 다음을 실행합니다.

```sql
Select name from sys.sysusers
```

워크로드 분류자를 만들려면 중요도가 높은 사용자를 위해 다음을 실행합니다.

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  above_normal);  

```

중요도가 낮은 adhoc 쿼리를 실행하는 사용자에 대한 워크로드 분류자를 만들려면 다음을 수행하십시오.  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  below_normal);  
```

## <a name="next-steps"></a>다음 단계

- 워크로드 관리에 대한 자세한 내용은 [워크로드 분류를](sql-data-warehouse-workload-classification.md) 참조하십시오.
- 중요도에 대한 자세한 내용은 [워크로드 중요도를](sql-data-warehouse-workload-importance.md) 참조하십시오.

> [!div class="nextstepaction"]
> [워크로드 중요도 관리 및 모니터링으로 이동](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
