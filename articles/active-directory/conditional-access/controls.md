---
title: Azure AD 조건부 액세스의 사용자 지정 컨트롤
description: Azure Active 디렉터리 조건부 액세스에서 사용자 지정 컨트롤이 작동하는 방식에 대해 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8c149279a755eb186a3fdc7891e9b511d18c7f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050544"
---
# <a name="custom-controls-preview"></a>사용자 지정 컨트롤(미리 보기)

사용자 지정 컨트롤은 Azure Active Directory의 미리 보기 기능입니다. 사용자 지정 컨트롤을 사용하는 경우 사용자는 Azure Active Directory 외부의 인증 요구 사항을 충족하기 위해 호환 되는 서비스로 리디렉션됩니다. 이 컨트롤을 충족하기 위해 사용자의 브라우저는 외부 서비스로 리디렉션되고 필요한 인증을 수행한 다음 Azure Active Directory로 다시 리디렉션됩니다. Azure Active Directory는 응답을 확인하고 사용자가 성공적으로 인증또는 유효성을 검사한 경우 사용자는 조건부 액세스 흐름에서 계속됩니다.

> [!NOTE]
> 사용자 지정 제어 기능에 대한 변경 사항에 대한 자세한 내용은 2020년 2월 [업데이트](../fundamentals/whats-new.md#upcoming-changes-to-custom-controls)내용을 참조하십시오.

## <a name="creating-custom-controls"></a>사용자 지정 컨트롤 만들기

사용자 지정 컨트롤은 승인된 인증 공급자 집합에서 작동합니다. 사용자 지정 컨트롤을 만들려면 먼저 사용할 공급자에게 문의해야 합니다. Microsoft가 아닌 각 공급자는 서비스의 일부가 되어 등록, 구독 또는 기타 방법으로 통합하고 조건부 액세스와 통합하려는 경우 를 나타내는 자체 프로세스 및 요구 사항을 가지고 있습니다. 이 시점에서 공급자는 JSON 형식의 데이터 블록을 제공합니다. 이 데이터를 사용하면 공급자와 조건부 Access가 테넌트에 대해 함께 작업하고, 새 컨트롤을 만들고, 사용자가 공급자와 의 확인을 성공적으로 수행했는지 확인할 수 있는 방법을 정의합니다.

JSON 데이터를 복사한 다음 관련 텍스트 상자에 붙여넣습니다. 변경 내용을 명시적으로 이해하지 않는 한 JSON을 변경하지 마십시오. 변경하면 공급자와 Microsoft 간의 연결이 끊어져 잠재적으로 사용자가 사용자 계정 밖에서 잠길 수 있습니다.

사용자 지정 컨트롤을 만드는 옵션은 **조건부 액세스** 관리 페이지의 **관리** 섹션에 있습니다.

![제어](./media/controls/82.png)

**새 사용자 지정 컨트롤**을 클릭하면 컨트롤의 JSON 데이터에 대한 텍스트 상자가 있는 블레이드가 열립니다.  

![제어](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>사용자 지정 컨트롤 삭제

사용자 지정 컨트롤을 삭제하려면 먼저 조건부 액세스 정책에서 사용되지 않는지 확인해야 합니다. 작업이 완료되면 다음을 수행합니다.

1. 사용자 지정 컨트롤 목록으로 이동합니다.
1. …를 클릭합니다.  
1. **삭제를 선택합니다.**

## <a name="editing-custom-controls"></a>사용자 지정 컨트롤 편집

사용자 지정 컨트롤을 편집하려면 현재 컨트롤을 삭제하고 업데이트된 정보로 새 컨트롤을 만들어야 합니다.

## <a name="known-limitations"></a>알려진 제한 사항

사용자 지정 컨트롤은 Azure 다단계 인증, Azure AD 셀프 서비스 암호 재설정(SSPR), 다단계 인증 클레임 요구 사항을 충족하거나 권한 있는 역할을 높이기 위해 필요한 ID Protection의 자동화와 함께 사용할 수 없습니다. ID 관리자(PIM).

## <a name="next-steps"></a>다음 단계

- [조건부 액세스 공통 정책](concept-conditional-access-policy-common.md)

- [보고서 전용 모드](concept-conditional-access-report-only.md)

- [조건부 액세스 What If 도구를 사용하여 로그인 동작 시뮬레이션](troubleshoot-conditional-access-what-if.md)
