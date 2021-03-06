---
title: Office 365 그룹 만료 설정 - Azure Active Directory | Microsoft Docs
description: Azure Active Directory에서 Office 365 그룹에 대한 만료를 설정하는 방법
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/13/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 562b551bc8a46a45135bf6a9a8e328b4b0e74f98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80048257"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Office 365 그룹에 대한 만료 정책 구성

이 문서에서는 만료 정책을 설정하여 Office 365 그룹의 수명 주기를 관리하는 방법을 설명합니다. Azure Active Directory(Azure AD)의 Office 365 그룹에 대해서만 만료 정책을 설정할 수 있습니다.

만료할 그룹을 설정한 경우 다음이 적용됩니다.

- 만료가 가까워지면 사용자 활동이 있는 그룹이 자동으로 갱신됩니다.
- 그룹이 자동으로 갱신되지 않은 경우 그룹 소유자에게 그룹을 갱신하라는 알림이 표시됩니다.
- 갱신되지 않은 모든 그룹이 삭제됩니다.
- 삭제된 Office 365 그룹은 그룹 소유자 또는 관리자가 30일 이내에 복원할 수 있습니다.

현재 Azure AD 조직의 모든 Office 365 그룹에 대해 하나의 만료 정책만 구성할 수 있습니다.

> [!NOTE]
> Office 365 그룹에 대한 만료 정책을 구성하고 사용하려면 만료 정책이 적용되는 모든 그룹의 구성원에 Azure AD Premium 라이선스를 할당해야 하지만 반드시 할당할 필요는 없습니다.

Azure AD PowerShell cmdlet을 다운로드하여 설치하는 방법에 대한 내용은 [Azure Active Directory PowerShell for Graph 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137)을 참조하세요.

## <a name="activity-based-automatic-renewal"></a>활동 기준 자동 갱신

Azure AD 인텔리전스를 사용하면 그룹이 최근에 사용되었는지 여부에 따라 그룹이 자동으로 갱신됩니다. 이 기능은 Outlook, SharePoint 또는 Teams와 같은 Office 365 서비스 전체의 그룹 사용자 활동을 기반으로 하므로 그룹 소유자가 직접 작업할 필요가 없습니다. 예를 들어 소유자 또는 그룹 구성원이 SharePoint에서 문서를 업로드하거나 팀 채널을 방문하거나 Outlook에서 그룹에 전자 메일을 보내는 등의 작업을 수행하는 경우 그룹이 자동으로 갱신되고 소유자가 갱신 알림을 받지 못합니다.

### <a name="activities-that-automatically-renew-group-expiration"></a>그룹 만료를 자동으로 갱신하는 활동

다음 사용자 작업으로 인해 자동 그룹 갱신이 발생합니다.

- 공유점: 파일 보기, 편집, 다운로드, 이동, 공유 또는 업로드
- Outlook: 그룹 조인, 그룹 공간에서 읽기/쓰기 그룹 메시지(Outlook 웹 액세스에서 메시지처럼)
- 팀: 팀 채널 방문

### <a name="auditing-and-reporting"></a>감사 및 보고

관리자는 Azure AD의 활동 감사 로그에서 자동으로 갱신된 그룹 목록을 얻을 수 있습니다.

![활동에 따라 그룹의 자동 갱신](./media/groups-lifecycle/audit-logs-autorenew-group.png)

## <a name="roles-and-permissions"></a>역할 및 권한

다음은 Azure AD에서 Office 365 그룹의 만료를 구성하고 사용할 수 있는 역할입니다.

역할 | 사용 권한
-------- | --------
글로벌 관리자, 그룹 관리자 또는 사용자 관리자 | Office 365 그룹 만료 정책 설정을 만들거나, 읽거나, 업데이트하거나, 삭제할 수 있음<br>Office 365 그룹을 갱신할 수 있음
사용자 | 소유하는 Office 365 그룹을 갱신할 수 있음<br>소유하는 Office 365 그룹을 복원할 수 있음<br>만료 정책 설정을 읽을 수 있음

삭제된 그룹을 복원하는 권한에 대한 자세한 내용은 [Azure Active Directory에서 삭제된 Office 365 그룹 복원](groups-restore-deleted.md)을 참조하세요.

## <a name="set-group-expiration"></a>그룹 만료 설정

1. Azure [AD](https://aad.portal.azure.com) 조직의 전역 관리자인 계정으로 Azure AD 관리자 센터를 엽니다.

2. **그룹을**선택한 다음 **만료를** 선택하여 만료 설정을 엽니다.
  
   ![그룹에 대한 만료 설정](./media/groups-lifecycle/expiration-settings.png)

3. **만료** 페이지에서 다음을 수행할 수 있습니다.

    - 일 단위로 그룹 수명을 설정합니다. 미리 설정된 값 중 하나 또는 사용자 지정 값을 선택할 수 있습니다(31일 이상이어야 함).
    - 그룹에 소유자가 없는 경우 갱신 및 만료 알림이 전송되어야 하는 전자 메일 주소를 지정합니다.
    - 만료되는 Office 365 그룹을 선택합니다. 만료를 설정할 수 있습니다.
      - **모두** 사무실 365 그룹
      - **선택한** Office 365 그룹 목록
      - **없음은** 모든 그룹에 대한 만료를 제한하지 않습니다.
    - 완료되면 **저장**을 선택하여 설정을 저장합니다.

> [!NOTE]
> - 만료를 처음 설정하면 그룹이 자동으로 갱신되거나 소유자가 갱신하지 않는 한 만료 간격보다 오래된 그룹은 만료일까지 35일로 설정됩니다.
> - 동적 그룹이 삭제되고 복원되면 새 그룹으로 간주되어 규칙에 따라 다시 채워집니다. 이 프로세스는 최대 24시간이 소요될 수 있습니다.
> - 팀에서 사용되는 그룹에 대한 만료 알림은 팀 소유자 피드에 표시됩니다.

## <a name="email-notifications"></a>이메일 알림

그룹이 자동으로 갱신되지 않으면 그룹 만료 30일, 15일 및 1일 전에 Office 365 그룹 소유자에게 이 알림과 같은 전자 메일 알림이 전송됩니다. 전자 메일의 언어는 그룹 소유자의 기본 언어 또는 Azure AD 언어 설정에 따라 결정됩니다. 그룹 소유자가 기본 설정 언어를 정의했거나 여러 소유자가 동일한 기본 설정 언어를 사용하는 경우 해당 언어가 사용됩니다. 다른 모든 경우에 Azure AD 언어 설정이 사용됩니다.

![만료 이메일 알림](./media/groups-lifecycle/expiration-notification.png)

그룹 **알림 전자** 메일 갱신에서 그룹 소유자는 [액세스 패널의](https://account.activedirectory.windowsazure.com/r#/applications)그룹 세부 정보 페이지에 직접 액세스할 수 있습니다. 여기에 사용자는 그룹에 대해 마지막 갱신 시간, 예상 만료 시간, 그룹 갱신 능력 등에 대한 자세한 정보를 얻을 수 있습니다. 이제 그룹 세부 정보 페이지에는 Office 365 그룹 리소스에 대한 링크가 포함되어 있으므로 그룹 소유자는 해당 그룹의 콘텐츠 및 활동을 편리하게 볼 수 있습니다.

그룹이 만료되면 그룹은 만료 날짜 1일 후에 삭제됩니다. Office 365 그룹의 만료 및 후속 삭제에 대해 알려주는 이와 같은 전자 메일 알림은 Office 365 그룹 소유자에게 전송됩니다.

![그룹 삭제 이메일 알림](./media/groups-lifecycle/deletion-notification.png)

[Azure Active Directory에서 삭제된 Office 365 그룹 복원](groups-restore-deleted.md)에서 설명된 대로 **그룹 복원**을 선택하거나 PowerShell cmdlet을 사용하여 30일 내에 그룹을 복원할 수 있습니다. 30일 그룹 복원 기간은 사용자 지정할 수 없습니다.

복원하는 그룹에 문서, SharePoint 사이트 또는 기타 영구 개체가 포함된 경우 그룹 및 해당 내용을 완전히 복원하는 데 최대 24시간이 걸릴 수 있습니다.

## <a name="how-to-retrieve-office-365-group-expiration-date"></a>Office 365 그룹 만료 날짜를 검색 하는 방법

사용자가 만료 날짜와 마지막 갱신 날짜를 포함하여 그룹 세부 정보를 볼 수 있는 액세스 패널 외에도 Office 365 그룹의 만료 날짜를 Microsoft 그래프 REST API 베타에서 검색할 수 있습니다. 만료DateTime 그룹 속성으로 마이크로소프트 그래프 베타에서 사용 되었습니다. GET 요청으로 검색할 수 있습니다. 자세한 내용은 [이 예제를](https://docs.microsoft.com/graph/api/group-get?view=graph-rest-beta#example)참조하십시오.

> [!NOTE]
> 액세스 패널에서 그룹 구성원을 관리하려면 Azure Active Directory 그룹 일반 설정에서 "액세스 패널의 그룹에 대한 액세스 제한"을 "아니요"로 설정해야 합니다.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>법적 보존 상태의 사서함에 Office 365 그룹 만료가 작동하는 방식

그룹에 만료되고 삭제되면 삭제되고 30일 후에 Planner, Sites 또는 Teams 같은 앱의 그룹 데이터가 영구히 삭제되지만, 법적 보존 상태인 그룹 사서함은 영구적으로 삭제되지 않습니다. 관리자는 Exchange cmdlet을 사용하여 사서함을 복원한 후 데이터를 인출합니다.

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>보존 정책에 Office 365 그룹 만료가 작동하는 방식

보존 정책은 보안 및 준수 센터를 통해 구성됩니다. Office 365 그룹에 대해 보존 정책을 설정한 경우, 그룹이 만료되고 삭제되면 그룹 사서함의 그룹 대화와 그룹 사이트의 파일은 보존 기간에 정의된 특정 기간(일) 동안 보존 컨테이너에 유지됩니다. 사용자는 만료된 후 그룹 또는 해당 콘텐츠를 볼 수 없지만, eDiscovery를 통해 사이트 및 사서함 데이터를 복구할 수 있습니다.

## <a name="powershell-examples"></a>PowerShell 예제

다음은 PowerShell cmdlet을 사용하여 Azure AD 조직의 Office 365 그룹에 대한 만료 설정을 구성하는 방법의 예입니다.

1. PowerShell v2.0 모듈을 설치하고 PowerShell 프롬프트에서 로그인하십시오.

   ``` PowerShell
   Install-Module -Name AzureAD
   Connect-AzureAD
   ```

1. 만료 설정 구성 새 AzureADMSGroupLifecyclePolicy cmdlet을 사용하여 Azure AD 조직의 모든 Office 365 그룹에 대한 수명을 365일로 설정합니다. 소유자가 없는 Office 365 그룹에 대한 갱신emailaddress@contoso.com알림이 ''로 전송됩니다.
  
   ``` PowerShell
   New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
   ```

1. 기존 정책 검색: Get-AzureADMSGroupLifecyclePolicy: 이 cmdlet은 구성된 현재 Office 365 그룹 만료 설정을 검색합니다. 이 예제에서는 다음을 확인할 수 있습니다.

   - 정책 ID
   - Azure AD 조직의 모든 Office 365 그룹의 수명은 365일로 설정됩니다.
   - 소유자가 없는 Office 365 그룹에 대한 갱신emailaddress@contoso.com알림이 '.'로 전송됩니다.
  
   ```powershell
   Get-AzureADMSGroupLifecyclePolicy
  
   ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
   --                                    ------------------- ----------------- ---------------------------
   26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
   ```

1. 기존 정책 업데이트 Set-AzureADMSGroupLifecyclePolicy: 이 cmdlet은 기존 정책을 업데이트하는 데 사용됩니다. 아래 예제에서 기존 정책의 그룹 수명은 365일에서 180일로 변경됩니다.
  
   ```powershell
   Set-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
   ```
  
1. 정책에 특정 그룹 추가 Add-AzureADMSLifecyclePolicyGroup: 이 cmdlet은 수명 주기 정책에 그룹을 추가합니다. 예를 들어
  
   ```powershell
   Add-AzureADMSLifecyclePolicyGroup -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
   ```
  
1. 기존 정책 제거 Remove-AzureADMSGroupLifecyclePolicy: 이 cmdlet은 Office 365 그룹 만료 설정을 삭제하지만 정책 ID가 필요합니다 이 cmdletOffice 365 그룹에 대 한 만료를 사용 하지 않도록 설정 합니다.
  
   ```powershell
   Remove-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077"
   ```
  
다음 cmdlet은 정책을 보다 자세히 구성하는 데 사용할 수 있습니다. 자세한 내용은 [PowerShell 문서를](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups)참조하십시오.

- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Get-AzureADMSGroupLifecyclePolicy
- Set-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup
- Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>다음 단계

이러한 문서는 Azure AD 그룹에 대한 추가 정보를 제공합니다.

- [기존 그룹 보기](../fundamentals/active-directory-groups-view-azure-portal.md)
- [그룹의 설정 관리](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [그룹의 멤버 관리](../fundamentals/active-directory-groups-members-azure-portal.md)
- [그룹의 멤버 자격 관리](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [그룹의 사용자에 대한 동적 규칙 관리](groups-dynamic-membership.md)
