---
title: 나이 미리 빌드된 엔터티 - LUIS
titleSuffix: Azure Cognitive Services
description: 이 문서에는 LUIS(Language Understanding)의 age 미리 빌드된 엔터티가 포함됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 1607b80276a9c35bf0ac2f0a00bd2226e2c07f97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270785"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>LUIS 앱용 Age 미리 빌드된 엔터티
미리 빌드된 age 엔터티는 age 값을 숫자로, 일, 주, 월 및 년에 따라 캡처합니다. 이 엔터티를 이미 학습했기 때문에 age를 포함하는 예제 발언을 애플리케이션 의도에 추가할 필요가 없습니다. Age 엔터티는 [여러 문화권](luis-reference-prebuilt-entities.md)에서 지원됩니다.

## <a name="types-of-age"></a>Age 유형
[나이인식기 텍스트](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) GitHub 리포지토리에서 관리됩니다.

## <a name="resolution-for-prebuilt-age-entity"></a>미리 빌드된 age 엔터티의 해결



#### <a name="v3-response"></a>[V3 응답](#tab/V3)

다음 JSON은 매개 `verbose` 변수를 `false`다음과 같은 것으로 설정합니다.

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 자세한 응답](#tab/V3-verbose)
다음 JSON은 매개 `verbose` 변수를 `true`다음과 같은 것으로 설정합니다.

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ],
    "$instance": {
        "age": [
            {
                "type": "builtin.age",
                "text": "90 day old",
                "startIndex": 2,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor"
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 응답](#tab/V2)

다음 예제에서는 **builtin.age** 엔터티의 해결을 보여 줍니다.

```json
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
```
* * *

## <a name="next-steps"></a>다음 단계

[V3 예측 엔드포인트](luis-migration-api-v3.md)에 대해 자세히 알아봅니다.

[currency](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) 및 [dimension](luis-reference-prebuilt-dimension.md) 엔터티에 대해 알아봅니다.
