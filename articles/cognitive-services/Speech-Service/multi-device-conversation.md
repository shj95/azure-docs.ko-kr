---
title: 다중 장치 대화(미리 보기) - 음성 서비스
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: dapine
ms.openlocfilehash: b3802e66b0ba5a68c898e69ec64b01edce1541c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371361"
---
# <a name="what-is-multi-device-conversation-preview"></a>다중 기기 대화(미리 보기)란 무엇입니까?

**다중 기기 대화를** 사용하면 여러 클라이언트 간에 음성 또는 텍스트 대화를 쉽게 만들고 서로 간에 전송된 메시지를 조정할 수 있습니다.

**다중 장치 대화를**사용하면 다음을 수행할 수 있습니다.

- 여러 클라이언트를 동일한 대화에 연결하고 클라이언트 간의 메시지 송수신을 관리합니다.
- 각 클라이언트의 오디오를 쉽게 전사하고 선택적 번역을 통해 전사를 다른 클라이언트로 전송할 수 있습니다.
- 선택적 번역을 통해 클라이언트 간에 문자 메시지를 쉽게 보낼 수 있습니다.

다양한 장치에서 작동하는 기능 또는 솔루션을 빌드할 수 있습니다. 각 장치는 독립적으로 다른 모든 장치에 메시지(오디오 또는 인스턴트 메시지의 전사)를 보낼 수 있습니다.

[**대화 기록은**](conversation-transcription.md) 멀티채널 마이크 어레이가 있는 단일 장치에서 작동하는 반면, **다중 장치 대화는** 각각 하나의 마이크가 있는 여러 장치가 있는 시나리오에 적합합니다.

>[!IMPORTANT]
> 다중 장치 대화는 클라이언트 간에 오디오 파일 전송을 지원하지 **않습니다.**

## <a name="key-features"></a>주요 기능

- **실시간 전사** – 모든 사람이 대화의 녹취록을 받게 되므로 실시간으로 텍스트를 팔로우하거나 나중에 저장할 수 있습니다.
- **실시간 번역** – 텍스트 번역을 위해 60개 이상의 [지원 언어를 지원하므로](language-support.md#text-languages) 사용자는 대화를 원하는 언어로 번역할 수 있습니다.
- **읽기 가능한 성적 증명서** - 문장 부호와 문장 휴식과 함께 전사 및 번역은 따라하기 쉽습니다.
- **음성 또는 텍스트 입력** - 각 사용자는 참가자가 선택한 언어에 대해 활성화된 언어 지원 기능에 따라 자신의 장치에서 말하거나 입력할 수 있습니다. [언어 지원을](language-support.md#speech-to-text)참조하십시오.
- **메시지 릴레이** - 다중 장치 대화 서비스는 한 클라이언트가 선택한 언어로 한 클라이언트가 보낸 메시지를 다른 모든 클라이언트에 배포합니다.
- **메시지 식별** - 대화에서 사용자가 받는 모든 메시지에는 메시지를 보낸 사용자의 별명이 태그됩니다.

## <a name="use-cases"></a>사용 사례

### <a name="lightweight-conversations"></a>가벼운 대화

대화를 만들고 참여하는 것은 쉽습니다. 한 사용자는 '호스트' 역할을 하고 대화를 생성하여 임의의 5개의 문자 대화 코드와 QR 코드를 생성합니다. 다른 모든 사용자는 대화 코드를 입력하거나 QR 코드를 스캔하여 대화에 참여할 수 있습니다. 

사용자가 대화 코드를 통해 가입하고 연락처 정보를 공유할 필요가 없으므로 즉석에서 빠르고 대화를 쉽게 만들 수 있습니다.

### <a name="inclusive-meetings"></a>포괄적인 회의

실시간 전사 및 번역은 다른 언어를 구사하거나 청각 장애가 있거나 청각 장애가 있는 사람들이 대화에 액세스할 수 있도록 하는 데 도움이 될 수 있습니다. 각 사람은 또한 자신이 선호하는 언어를 말하거나 인스턴트 메시지를 전송하여 대화에 적극적으로 참여할 수 있습니다.

### <a name="presentations"></a>프레젠테이션

또한 화면과 청중의 장치에서 프레젠테이션 및 강의에 대한 캡션을 제공할 수도 있습니다. 청중이 대화 코드에 참여하면 자신의 장치에서 원하는 언어로 자막을 볼 수 있습니다.

> [!NOTE]
> 예를 보려면 다중 장치 대화 서비스를 사용하는 PowerPoint 추가 기능인 [프레젠테이션 번역기를](https://www.microsoft.com/translator/apps/presentation-translator/)확인하십시오. [여기](https://www.microsoft.com/download/details.aspx?id=55024)서 다운로드할 수 있습니다.

## <a name="how-it-works"></a>작동 방법

모든 클라이언트는 Speech SDK를 사용하여 대화를 만들거나 참여합니다. Speech SDK는 참가자 목록, 각 클라이언트가 선택한 언어 및 보낸 메시지를 포함하여 대화의 수명을 관리하는 다중 장치 대화 서비스와 상호 작용합니다.  

각 클라이언트는 오디오 또는 인스턴트 메시지를 보낼 수 있습니다. 이 서비스는 음성 인식을 사용하여 오디오를 텍스트로 변환하고 있는 것처럼 인스턴트 메시지를 보냅니다. 클라이언트가 다른 언어를 선택하면 서비스는 모든 메시지를 각 클라이언트의 지정된 언어로 변환합니다.

![다중 기기 대화 개요 다이어그램](media/scenarios/multi-device-conversation.png)

## <a name="overview-of-conversation-host-and-participant"></a>대화, 호스트 및 참가자 개요

**대화는** 한 사용자가 다른 참여 사용자가 참여할 수 있도록 시작하는 세션입니다. 모든 클라이언트는 다섯 글자 대화 **코드를**사용하여 대화에 연결합니다.

각 대화에는 다음이 포함된 메타데이터가 생성됩니다.
-    대화가 시작되고 종료된 시점의 타임스탬프
-    각 사용자가 선택한 별명과 음성 또는 텍스트 입력을 위한 기본 언어를 포함하는 대화의 모든 참가자 목록입니다.


대화에는 **호스트와** **참가자의**두 가지 유형이 있습니다.

**호스트는** 대화를 시작하고 해당 대화의 관리자 역할을 하는 사용자입니다.
- 각 대화에는 호스트가 하나만 있을 수 있습니다.
- 대화 기간 동안 주최자는 대화에 연결되어 있어야 합니다. 주최자가 대화에서 나오면 다른 모든 참가자의 대화가 종료됩니다.
- 호스트에는 대화를 관리하는 몇 가지 추가 컨트롤이 있습니다. 
    - 대화 잠금 - 추가 참가자참여 방지
    - 모든 참가자 음소거 - 다른 참가자가 음성 또는 인스턴트 메시지에서 전사여부에 관계없이 대화에 메시지를 보내지 못하게 합니다.
    - 개별 참가자 음소거
    - 모든 참가자 의 음소거 를 해제
    - 개별 참가자 음소거 해제

**참가자는** 대화에 참여하는 사용자입니다.
- 참가자는 다른 참가자에 대한 대화를 종료하지 않고 언제든지 동일한 대화를 종료하고 다시 참여할 수 있습니다.
- 참가자는 대화를 잠글 수 없거나 다른 사람을 음소거/음소거 해제할 수 없습니다.

> [!NOTE]
> 각 대화에는 최대 100명의 참가자가 참여할 수 있으며, 그 중 10명은 언제든지 동시에 말할 수 있습니다.

## <a name="language-support"></a>언어 지원

대화를 만들거나 참여할 때 각 사용자는 말하고 인스턴트 메시지를 보낼 언어와 다른 사용자의 메시지를 볼 수 있는 언어를 **기본**언어로 선택해야 합니다.

언어에는 **음성-텍스트** 및 텍스트 전용의 두 가지 **종류가 있습니다.**
- 사용자가 **음성-텍스트** 언어를 기본 언어로 선택하면 대화에서 음성 및 텍스트 입력을 모두 사용할 수 있습니다.

- 사용자가 **텍스트 전용** 언어를 선택하면 텍스트 입력만 사용하고 대화에서 인스턴트 메시지를 보낼 수 있습니다. 텍스트 전용 언어는 텍스트 번역에 지원되지만 음성-텍스트 언어는 지원되지 않는 언어입니다. [언어 지원](supported-languages.md) 페이지에서 사용 가능한 언어를 볼 수 있습니다.

각 참가자는 기본 언어 외에도 대화를 번역하기 위한 추가 언어를 지정할 수도 있습니다.

다음은 사용자가 선택한 기본 언어에 따라 다중 장치 대화에서 수행할 수 있는 작업의 요약입니다.


| 대화에서 사용자가 수행할 수 있는 일 | 음성 텍스트 변환 | 텍스트 전용 |
|-----------------------------------|----------------|------|
| 음성 입력 사용 | ✔️ | ❌ |
| 인스턴트 메시지 보내기 | ✔️ | ✔️ |
| 대화 번역 | ✔️ | ✔️ |

> [!NOTE]
> 사용 가능한 음성-텍스트 및 텍스트 번역 언어 목록은 [지원되는 언어를](supported-languages.md)참조하십시오.



## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [실시간으로 대화 번역](quickstarts/multi-device-conversation.md)
