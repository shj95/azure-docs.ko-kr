---
title: 텍스트 모듈 참조에서 N-Gram 기능 추출
titleSuffix: Azure Machine Learning
description: Azure 기계 학습에서 N-Gram 추출 모듈을 사용하여 텍스트 데이터를 위축시키는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: efe09c1d516b37c23b024e07ae387772fa7e5992
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477615"
---
# <a name="extract-n-gram-features-from-text-module-reference"></a>텍스트 모듈 참조에서 N-Gram 기능 추출

이 문서에서는 Azure 기계 학습 디자이너(미리 보기)의 모듈에 대해 설명합니다. 텍스트에서 N-Gram 피처 추출 모듈을 사용하여 구조화되지 않은 텍스트 데이터를 *위축시합니다.* 

## <a name="configuration-of-the-extract-n-gram-features-from-text-module"></a>텍스트 모듈에서 N-Gram 추출 기능 구성

이 모듈은 n-gram 사전을 사용하기 위한 다음 시나리오를 지원합니다.

* 사용 자유 텍스트 열에서 [새 n-gram 사전을 만듭니다.](#create-a-new-n-gram-dictionary)

* [기존 텍스트 기능 집합을 사용하여](#use-an-existing-n-gram-dictionary) 무료 텍스트 열을 위화합니다.

* n-gram을 사용하는 [모델을 점수 매기거나 게시합니다.](#score-or-publish-a-model-that-uses-n-grams)

### <a name="create-a-new-n-gram-dictionary"></a>새 n-gram 사전 만들기

1.  텍스트에서 N-Gram 피처 추출을 파이프라인에 추가하고 처리할 텍스트가 있는 데이터 집합을 연결합니다.

1.  **텍스트 열을** 사용하여 추출할 텍스트가 포함된 문자열 형식의 열을 선택합니다. 결과는 상세하므로 한 번에 하나의 열만 처리할 수 있습니다.

1. **어휘 모드를** **만들기로** 설정하여 n-gram 기능의 새 목록을 만들고 있음을 나타냅니다. 

1. **N-그램 크기를** 설정하여 추출 및 저장할 n그램의 *최대* 크기를 나타냅니다. 

    예를 들어 3을 입력하면 유니그램, 빅램 및 삼중그램이 만들어집니다.

1. **가중치 함수는** 문서 기능 벡터를 빌드하는 방법과 문서에서 어휘를 추출하는 방법을 지정합니다.

    * **이진 가중치**: 추출된 n-grams에 이진 존재 값을 할당합니다. 각 n-gram의 값은 문서에 있을 때 1이고 그렇지 않으면 0입니다.

    * **TF 가중치**: 추출된 n-grams에 용어 주파수(TF) 점수를 할당합니다. 각 n-gram의 값은 문서의 발생 빈도입니다.

    * **IDF 가중치**: 추출된 n-grams에 역문서 주파수(IDF) 점수를 할당합니다. 각 n-gram의 값은 전체 코퍼스에서 발생 빈도로 나눈 코퍼스 크기의 로그입니다.
    
      `IDF = log of corpus_size / document_frequency`
 
    *  **TF-IDF 가중치**: 추출된 n-그램에 용어 주파수/역 문서 빈도(TF/IDF) 점수를 할당합니다. 각 n-gram의 값은 TF 점수에 IDF 점수를 곱한 값입니다.

1. **최소 단어 길이를** n-gram의 *단일 단어에서* 사용할 수 있는 최소 문자 수로 설정합니다.

1. **최대 단어 길이를** 사용하여 n-gram의 *단일 단어에서* 사용할 수 있는 최대 문자 수를 설정합니다.

    기본적으로 단어 또는 토큰당 최대 25자까지 허용됩니다.

1. **최소 n-gram 문서 절대 주파수를** 사용하여 n-gram 사전에 포함되는 n-gram에 필요한 최소 발생을 설정합니다. 

    예를 들어 기본값 5를 사용하는 경우 n-gram 사전에 포함되려면 모든 n-gram이 코퍼스에 5번 이상 나타나야 합니다. 

1.  전체 코퍼스의 행 수에 대해 특정 n-gram을 포함하는 행 수의 최대 비율로 **최대 n-gram 문서 비율을** 설정합니다.

    예를 들어 1의 비율은 모든 행에 특정 n-gram이 있더라도 n-gram 사전에 n-gram을 추가할 수 있음을 나타냅니다. 일반적으로 모든 행에서 발생하는 단어는 노이즈 단어로 간주되어 제거됩니다. 도메인종속 노이즈 단어를 필터링하려면 이 비율을 줄이십시오.

    > [!IMPORTANT]
    > 특정 단어의 발생 속도는 균일하지 않습니다. 문서마다 다릅니다. 예를 들어 특정 제품에 대한 고객의 의견을 분석하는 경우 제품 이름은 매우 높은 빈도로 노이즈 단어에 가까우므로 다른 컨텍스트에서 중요한 용어가 될 수 있습니다.

1. **n-gram 피쳐 벡터 정규화** 옵션을 선택하여 피쳐 벡터를 정규화합니다. 이 옵션을 사용하도록 설정하면 각 n-gram 특징 벡터는 L2 표준으로 나뉩니다.

1. 파이프라인을 제출합니다.

### <a name="use-an-existing-n-gram-dictionary"></a>기존 n-gram 사전 사용

1.  텍스트에서 N-Gram 피처 추출을 파이프라인에 추가하고 처리할 텍스트가 있는 데이터 집합을 **데이터 집합** 포트에 연결합니다.

1.  **텍스트 열을** 사용하여 위화하려는 텍스트가 포함된 텍스트 열을 선택합니다. 기본적으로 모듈은 형식 **문자열의**모든 열을 선택합니다. 최상의 결과를 얻으려면 한 번에 하나의 열을 처리합니다.

1. 이전에 생성된 n-gram 사전이 포함된 저장된 데이터 집합을 추가하고 **입력 어휘** 포트에 연결합니다. 텍스트 모듈에서 N-Gram 피처 추출의 업스트림 인스턴스의 **결과 어휘** 출력을 연결할 수도 있습니다.

1. **어휘 모드의**경우 드롭다운 목록에서 **ReadOnly** 업데이트 옵션을 선택합니다.

   **ReadOnly** 옵션은 입력 어휘에 대한 입력 코퍼스를 나타냅니다. 새 텍스트 데이터 집합(왼쪽 입력)에서 용어 주파수를 계산하는 대신 입력 어휘의 n-gram 가중치가 있는 것처럼 적용됩니다.

   > [!TIP]
   > 텍스트 분류자 점수를 매기는 경우 이 옵션을 사용합니다.

1.  다른 모든 옵션에 대 한 [이전 섹션에서](#create-a-new-n-gram-dictionary)속성 설명을 참조 하십시오.

1.  파이프라인을 제출합니다.

### <a name="score-or-publish-a-model-that-uses-n-grams"></a>n-그램을 사용하는 모델점수 매기기 또는 게시

1.  학습 데이터 흐름에서 점수 매기기 데이터 흐름에 텍스트 모듈에서 **N-Gram 피처 추출을** 복사합니다.

1.  학습 데이터 흐름의 **결과 어휘** 출력을 점수 매기기 데이터 흐름의 **입력 어휘에** 연결합니다.

1.  점수 매기기 워크플로에서 텍스트 모듈에서 N-Gram 피처 추출을 수정하고 **어휘 모드** 매개 변수를 **ReadOnly로**설정합니다. 다른 모든 것을 동일하게 둡니다.

1.  파이프라인을 게시하려면 **결과 어휘를** 데이터 집합으로 저장합니다.

1.  저장된 데이터 집합을 점수 매기기 그래프의 텍스트 모듈에서 N-Gram 피처 추출에 연결합니다.

## <a name="results"></a>결과

텍스트 모듈의 N-Gram 피처 추출은 두 가지 유형의 출력을 만듭니다. 

* **결과 데이터 집합**: 이 출력은 추출된 n-gram과 결합된 분석된 텍스트의 요약입니다. **텍스트 열** 옵션에서 선택하지 않은 열은 출력으로 전달됩니다. 분석하는 텍스트의 각 열에 대해 모듈은 다음 열을 생성합니다.

  * **n-gram 발생 행렬**: 모듈은 총 코퍼스에서 발견되는 각 n그램에 대한 열을 생성하고 각 열에 점수를 추가하여 해당 행에 대한 n그램의 가중치를 나타냅니다. 

* **결과 어휘**: 어휘는 분석의 일환으로 생성되는 용어 주파수 점수와 함께 실제 n-gram 사전을 포함합니다. 다른 입력 집합으로 다시 사용하거나 이후 업데이트를 위해 데이터 집합을 저장할 수 있습니다. 모델링 및 채점을 위해 어휘를 재사용할 수도 있습니다.

### <a name="result-vocabulary"></a>결과 어휘

어휘에는 분석의 일부로 생성되는 주파수 점수라는 용어가 있는 n-gram 사전이 포함되어 있습니다. DF 및 IDF 점수는 다른 옵션에 관계없이 생성됩니다.

+ **ID**: 각 고유 n-gram에 대해 생성된 식별자입니다.
+ **NGram**: N-그램. 공백 또는 기타 단어 구분 기호는 밑줄 문자로 대체됩니다.
+ **DF**: 원래 코퍼스의 n-gram에 대한 용어 주파수 점수입니다.
+ **IDF**: 원래 코퍼스의 n-gram에 대한 역 문서 빈도 점수입니다.

이 데이터 집합을 수동으로 업데이트할 수 있지만 오류가 발생할 수 있습니다. 예를 들어:

* 모듈이 입력 어휘에서 동일한 키를 가진 중복 행을 발견하면 오류가 발생합니다. 어휘의 두 행에 같은 단어가 없는지 확인합니다.
* 어휘 데이터 집합의 입력 스키마는 열 이름 및 열 형식을 포함하여 정확히 일치해야 합니다. 
* **ID** 열과 **DF** 열은 정수 형식이어야 합니다. 
* **IDF** 열은 float 형식이어야 합니다.

> [!Note]
> 데이터 출력을 학습 모델 모듈에 직접 연결하지 마십시오. 자유 텍스트 열이 기차 모델에 공급되기 전에 제거해야 합니다. 그렇지 않으면 자유 텍스트 열은 범주형 기능으로 처리됩니다.

## <a name="next-steps"></a>다음 단계

Azure 기계 학습에 사용할 수 있는 [모듈 집합을](module-reference.md) 참조하십시오.
