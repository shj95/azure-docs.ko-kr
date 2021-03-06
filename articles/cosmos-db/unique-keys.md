---
title: Azure Cosmos DB의 고유 키 사용
description: Azure Cosmos 데이터베이스에 대해 고유한 키를 정의하고 사용하는 방법을 알아봅니다. 이 문서에서는 고유한 키가 데이터 무결성 계층을 추가하는 방법에 대해서도 설명합니다.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.reviewer: sngun
ms.openlocfilehash: f234579c6fb2b6f1bc0cd518b87ea69fae30093a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74869836"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Azure Cosmos DB의 고유 키 제약 조건

고유 키는 Azure Cosmos 컨테이너에 데이터 무결성 계층을 추가합니다. Azure Cosmos 컨테이너를 만들 때 고유 키 정책을 만듭니다. 고유 키를 사용하여 논리 파티션 내의 하나 이상 값이 고유한지 확인합니다. [파티션 키](partition-data.md)별로 고유성을 보장할 수도 있습니다.

고유한 키 정책을 사용하여 컨테이너를 만든 후 고유한 키 제약 조건에 지정된 대로 논리 파티션 내에서 중복되는 기존 항목의 새 또는 업데이트를 만들 수 없습니다. 파티션 키를 고유 키와 결합하면 컨테이너 범위 내에서 항목의 고유성이 보장됩니다.

예를 들어 메일 주소를 고유 키 제약 조건으로, `CompanyID`를 파티션 키로 사용하는 Azure Cosmos 컨테이너가 있다고 가정합니다. 사용자의 메일 주소를 고유 키를 구성하면 지정된 `CompanyID` 내에서 각 항목이 고유한 메일 주소를 갖습니다. 이메일 주소가 중복되고 파티션 키가 동일한 두 항목을 만들 수 없습니다. 

메일 주소는 같지만 이름, 성, 메일 주소는 다른 항목을 만들려면 고유 키 정책의 경로를 더 추가합니다. 이메일 주소만을 기반으로 고유한 키를 만드는 대신 이름, 성 및 이메일 주소를 조합하여 고유한 키를 만들 수도 있습니다. 이 키를 복합 고유 키라고 합니다. 이 경우 지정된 `CompanyID` 내에서 세 값의 고유한 조합이 허용됩니다. 

예를 들어 항목이 다음 값으로 컨테이너에 포함될 수 있으며, 이 경우 각 항목이 고유 키 제약 조건을 준수합니다.

|CompanyID|이름|성|메일 주소|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

앞의 표에 나열된 조합으로 다른 항목을 삽입하려고 하면 오류가 표시됩니다. 오류는 고유 키 제약 조건이 충족되지 않았음을 나타냅니다. `Resource with specified ID or name already exists` 또는 `Resource with specified ID, name, or unique index already exists` 반환 메시지로 수신됩니다. 

## <a name="define-a-unique-key"></a>고유 키를 정의

Azure Cosmos 컨테이너를 만들 때만 고유 키를 정의할 수 있습니다. 고유 키의 범위는 논리 파티션으로 지정됩니다. 이전 예제에서 우편 번호를 기준으로 컨테이너를 분할하면 결국 각 논리 파티션에서 중복 항목이 발생하게 됩니다. 고유 키를 만들 때는 다음 속성을 고려해야 합니다.

* 다른 고유 키를 사용하도록 기존 컨테이너를 업데이트할 수 없습니다. 다시 말해서, 고유 키 정책을 사용하여 컨테이너를 만든 후에는 정책을 변경할 수 없습니다.

* 기존 컨테이너에 대해 고유 키를 설정하려면 고유 키 제약 조건을 사용하여 새 컨테이너를 만듭니다. 적절한 데이터 마이그레이션 도구를 사용하여 기존 컨테이너에서 새 컨테이너로 데이터를 이동합니다. SQL 컨테이너의 경우 [데이터 마이그레이션 도구를](import-data.md) 사용하여 데이터를 이동합니다. MongoDB 컨테이너의 경우 [mongoimport.exe 또는 mongorestore.exe](mongodb-migrate.md)를 사용하여 데이터를 이동합니다.

* 고유 키 정책에 최대 16개의 경로 값을 사용할 수 있습니다. 예를 들어 값은 `/firstName`" `/lastName`및 `/address/zipCode` 각 고유 키 정책에는 최대 10개의 고유 키 제약 조건 또는 조합을 포함할 수 있습니다. 각 고유 인덱스 제약 조건에 대해 결합된 경로는 60바이트를 초과하지 않아야 합니다. 이전 예제에서는 이름, 성, 메일 주소가 결합되어 하나의 제약 조건이 되었습니다. 이 제약 조건은 사용 가능한 16개 경로 중 3개를 사용합니다.

* 컨테이너에 고유한 키 정책이 있는 경우 [RU(요청 단위)는](request-units.md) 항목을 생성, 업데이트 및 삭제하는 데 청구됩니다.

* 스파스 고유 키는 지원되지 않습니다. 일부 고유 경로 값이 누락되면 null 값으로 간주되어 고유성 제약 조건에 포함됩니다. 이러한 이유로, null 값을 갖는 단일 항목만 이 제약 조건을 충족할 수 있습니다.

* 고유 키 이름은 대/소문자를 구분합니다. 예를 들어 고유한 키 제약 조건이 로 `/address/zipcode`설정된 컨테이너를 예로 들어 보겠습니다. 데이터에 라는 `ZipCode`필드가 있는 경우 Azure Cosmos DB는 "null"을 고유 키로 `ZipCode` `zipcode` 삽입합니다. 이처럼 대/소문자를 구분하므로 ZipCode가 포함된 다른 모든 레코드는 삽입할 수 없습니다. 중복되는 “null”이 고유 키 제약 조건을 위반하기 때문입니다.

## <a name="next-steps"></a>다음 단계

* [논리 파티션](partition-data.md)에 대한 자세한 정보
* 컨테이너를 만들 때 [고유한 키를 정의하는 방법](how-to-define-unique-keys.md) 알아보기
