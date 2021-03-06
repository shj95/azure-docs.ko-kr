---
title: Azure AD 도메인 서비스에 대해 자주 묻는 질문 | 마이크로 소프트 문서
description: Azure Active Directory 도메인 서비스의 구성, 관리 및 가용성과 관련된 자주 묻는 질문 중 일부를 읽고 이해합니다.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/09/2020
ms.author: iainfou
ms.openlocfilehash: 86b68b794928900717bea25623e7eb833c23e86c
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655342"
---
# <a name="frequently-asked-questions-faqs"></a>FAQ(질문과 대답)

이 페이지에서는 Azure Active Directory 도메인 서비스에 대한 자주 묻는 질문에 대한 답변을 제공합니다.

## <a name="configuration"></a>구성

* [단일 Azure AD 디렉터리에 여러 관리되는 도메인을 만들 수 있나요?](#can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory)
* [클래식 가상 네트워크에서 Azure AD 도메인 서비스를 활성화할 수 있습니까?](#can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network)
* [Azure Resource Manager 가상 네트워크에서 Azure AD Domain Services를 사용할 수 있습니까?](#can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network)
* [기존 관리되는 도메인을 기존 가상 네트워크에서 리소스 관리자 가상 네트워크로 마이그레이션할 수 있습니까?](#can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network)
* [Azure CSP(클라우드 솔루션 공급자) 구독에서 Azure AD Domain Services를 사용할 수 있나요?](#can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription)
* [페더레이션된 Azure AD 디렉터리에서 Azure AD 도메인 서비스를 활성화할 수 있습니까? 암호 해시를 Azure AD에 동기화하지 않습니다. 이 디렉터리에서 Azure AD 도메인 서비스를 활성화할 수 있습니까?](#can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory)
* [Azure AD Domain Services를 내 구독 내의 여러 가상 네트워크에서 사용할 수 있나요?](#can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription)
* [PowerShell을 사용하여 Azure AD 도메인 서비스를 사용할 수 있나요?](#can-i-enable-azure-ad-domain-services-using-powershell)
* [Resource Manager 템플릿을 사용하여 Azure AD Domain Services를 사용할 수 있나요?](#can-i-enable-azure-ad-domain-services-using-a-resource-manager-template)
* [Azure AD 도메인 서비스 관리되는 도메인에 도메인 컨트롤러를 추가할 수 있나요?](#can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain)
* [내 디렉터리에 초대된 게스트 사용자가 Azure AD Domain Services를 사용할 수 있나요?](#can-guest-users-invited-to-my-directory-use-azure-ad-domain-services)
* [기존 Azure AD 도메인 서비스 관리 도메인을 다른 구독, 리소스 그룹, 지역 또는 가상 네트워크로 이동할 수 있습니까?](#can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network)
* [Azure AD 도메인 서비스에 고가용성 옵션이 포함되어 있습니까?](#does-azure-ad-domain-services-include-high-availability-options)

### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>단일 Azure AD 디렉터리에 여러 관리되는 도메인을 만들 수 있나요?
아니요. 단일 Azure AD 디렉터리에 Azure AD Domain Services에서 제공하는 단일 관리되는 도메인만 만들 수 있습니다.

### <a name="can-i-enable-azure-ad-domain-services-in-a-classic-virtual-network"></a>클래식 가상 네트워크에서 Azure AD 도메인 서비스를 활성화할 수 있습니까?
클래식 가상 네트워크는 새 배포에 대해 지원되지 않습니다. 클래식 가상 네트워크에 배포된 기존 관리 형 도메인은 2023년 3월 1일에 만료될 때까지 계속 지원됩니다. 기존 배포의 경우 [Azure AD 도메인 서비스를 클래식 가상 네트워크 모델에서 리소스 관리자로 마이그레이션할](migrate-from-classic-vnet.md)수 있습니다.

자세한 내용은 공식 [사용 중단 공지를](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/)참조하십시오.

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Azure Resource Manager 가상 네트워크에서 Azure AD Domain Services를 사용할 수 있습니까?
예. Azure Resource Manager 가상 네트워크에서 Azure AD Domain Services를 사용할 수 있습니다. 관리되는 도메인을 만들 때 클래식 Azure 가상 네트워크를 더 이상 사용할 수 없습니다.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>기존 관리되는 도메인을 클래식 가상 네트워크에서 리소스 관리자 가상 네트워크로 마이그레이션할 수 있습니까?
예. 자세한 내용은 [Azure AD 도메인 서비스 마이그레이션을 참조하여 클래식 가상 네트워크 모델에서 리소스 관리자로 마이그레이션합니다.](migrate-from-classic-vnet.md)

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Azure CSP(클라우드 솔루션 공급자) 구독에서 Azure AD Domain Services를 사용할 수 있나요?
예. 자세한 내용은 [Azure CSP 구독에서 Azure AD 도메인 서비스를 사용하도록 설정하는 방법을 참조하세요.](csp.md)

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>페더레이션된 Azure AD 디렉터리에서 Azure AD Domain Services를 사용할 수 있습니까? 암호 해시를 Azure AD와 동기화하지 않았습니다. 이 디렉터리에 Azure AD Domain Services를 사용할 수 있습니까?
아니요. NTLM 또는 Kerberos를 통해 사용자를 인증하려면 Azure AD 도메인 서비스는 사용자 계정의 암호 해시에 액세스해야 합니다. 페더레이션된 디렉터리에서 암호 해시는 Azure AD 디렉터리에 저장되지 않습니다. 따라서 Azure AD 도메인 서비스는 이러한 Azure AD 디렉터리에서 작동하지 않습니다.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Azure AD Domain Services를 내 구독 내의 여러 가상 네트워크에서 사용할 수 있나요?
서비스 자체는 이 시나리오를 직접 지원하지 않습니다. 관리되는 도메인은 한 번에 하나의 가상 네트워크에서만 사용할 수 있습니다. 그러나 Azure AD 도메인 서비스를 다른 가상 네트워크에 노출하도록 여러 가상 네트워크 간의 연결을 구성할 수 있습니다. 자세한 내용은 VPN 게이트웨이 또는 [가상 네트워크 피어링을](../virtual-network/virtual-network-peering-overview.md) [사용하여 Azure에서 가상 네트워크를 연결하는 방법을](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) 참조하세요.

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>PowerShell을 사용하여 Azure AD 도메인 서비스를 사용할 수 있나요?
예. 자세한 내용은 [PowerShell 을 사용하여 Azure AD 도메인 서비스를 사용하도록 설정하는 방법을](powershell-create-instance.md)참조하세요.

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Resource Manager 템플릿을 사용하여 Azure AD Domain Services를 사용할 수 있나요?
예. 리소스 관리자 템플릿을 사용하여 Azure AD 도메인 서비스 관리 도메인을 만들 수 있습니다. 관리를 위한 서비스 주체 및 Azure AD 그룹은 템플릿을 배포하기 전에 Azure Portal 또는 Azure PowerShell을 사용하여 만들어야 합니다. 자세한 내용은 [Azure 리소스 관리자 템플릿을 사용하여 Azure AD DS 관리 도메인 만들기를](template-create-instance.md)참조하십시오. Azure 포털에서 Azure AD 도메인 서비스 관리 도메인을 만들 때 추가 배포와 함께 사용할 템플릿을 내보낼 수도 있습니다.

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Azure AD 도메인 서비스 관리되는 도메인에 도메인 컨트롤러를 추가할 수 있나요?
아니요. Azure AD 도메인 서비스에서 제공하는 도메인은 관리되는 도메인입니다. 이 도메인에 대한 도메인 컨트롤러를 프로비전, 구성 또는 관리할 필요가 없습니다. 이러한 관리 활동은 Microsoft에서 서비스로 제공합니다. 따라서 관리되는 도메인에 대해 추가 도메인 컨트롤러(읽기-쓰기 또는 읽기 전용)를 추가할 수 없습니다.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>내 디렉터리에 초대된 게스트 사용자가 Azure AD Domain Services를 사용할 수 있나요?
아니요. [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) 초대 프로세스를 사용하여 Azure AD 디렉터리에 초대된 게스트 사용자는 Azure AD Domain Services 관리되는 도메인과 동기화됩니다. 그러나 이러한 사용자의 암호는 Azure AD 디렉터리에 저장되지 않습니다. 따라서 Azure AD 도메인 서비스는 이러한 사용자에 대한 NTLM 및 Kerberos 해시를 관리되는 도메인으로 동기화할 수 없습니다. 이러한 사용자는 관리되는 도메인에 컴퓨터를 로그인하거나 가입할 수 없습니다.

### <a name="can-i-move-an-existing-azure-ad-domain-services-managed-domain-to-a-different-subscription-resource-group-region-or-virtual-network"></a>기존 Azure AD 도메인 서비스 관리 도메인을 다른 구독, 리소스 그룹, 지역 또는 가상 네트워크로 이동할 수 있습니까?
아니요. Azure AD 도메인 서비스 관리 도메인을 만든 후에는 인스턴스를 다른 리소스 그룹, 가상 네트워크, 구독 등으로 이동할 수 없습니다. Azure AD DS 인스턴스를 배포할 때 가장 적합한 구독, 리소스 그룹, 지역 및 가상 네트워크를 선택해야 합니다.

### <a name="does-azure-ad-domain-services-include-high-availability-options"></a>Azure AD 도메인 서비스에 고가용성 옵션이 포함되어 있습니까?

예. 각 Azure AD 도메인 서비스 관리 도메인에는 두 개의 도메인 컨트롤러가 포함됩니다. 이러한 도메인 컨트롤러를 관리하거나 연결하지 않고 관리되는 서비스의 일부입니다. Azure AD 도메인 서비스를 가용성 영역을 지원하는 리전에 배포하는 경우 도메인 컨트롤러는 영역 간에 분산됩니다. 가용성 영역을 지원하지 않는 지역에서는 도메인 컨트롤러가 가용성 집합에 분산됩니다. 이 배포에 대한 구성 옵션이나 관리 제어가 없습니다. 자세한 내용은 Azure 의 [가상 컴퓨터에 대한 가용성 옵션을](../virtual-machines/windows/availability.md)참조하세요.

## <a name="administration-and-operations"></a>관리 및 운영

* [원격 데스크톱을 사용하여 관리되는 도메인의 도메인 컨트롤러에 연결할 수 있습니까?](#can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop)
* [Azure AD 도메인 서비스를 사용하도록 설정했습니다. 이 도메인에 컴퓨터를 도메인에 사용하는 사용자 계정은 무엇입니까?](#ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain)
* [Azure AD Domain Services에서 제공하는 관리되는 도메인에 대한 도메인 관리자 권한이 부여됩니까?](#do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services)
* [관리되는 도메인에서 LDAP 또는 다른 AD 관리 도구를 사용하여 그룹 멤버 자격을 수정할 수 있습니까?](#can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains)
* [Azure AD 디렉터리에 적용한 변경 사항이 내 관리되는 도메인에 표시되는 데 얼마나 걸리나요?](#how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain)
* [Azure AD Domain Services에서 제공하는 관리되는 도메인의 스키마를 확장할 수 있습니까?](#can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services)
* [관리되는 도메인에서 DNS 레코드를 수정하거나 추가할 수 있습니까?](#can-i-modify-or-add-dns-records-in-my-managed-domain)
* [관리되는 도메인에 대한 암호 수명 정책은 무엇인가요?](#what-is-the-password-lifetime-policy-on-a-managed-domain)
* [Azure AD Domain Services에서 AD 계정 잠금 보호를 제공하나요?](#does-azure-ad-domain-services-provide-ad-account-lockout-protection)

### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>원격 데스크톱을 사용하여 관리되는 도메인의 도메인 컨트롤러에 연결할 수 있습니까?
아니요. 원격 데스크톱을 사용하여 관리되는 도메인의 도메인 컨트롤러에 연결할 수 있는 권한이 없습니다. *AAD DC 관리자* 그룹의 구성원은 ADAC(활성 디렉터리 관리 센터) 또는 AD PowerShell과 같은 AD 관리 도구를 사용하여 관리되는 도메인을 관리할 수 있습니다. 이러한 도구는 관리되는 도메인에 가입한 Windows 서버의 *원격 서버 관리 도구* 기능을 사용하여 설치됩니다. 자세한 내용은 [Azure AD 도메인 서비스 관리 도메인을 구성하고 관리하는 관리 VM 만들기를](tutorial-create-management-vm.md)참조하십시오.

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Azure AD 도메인 서비스를 사용하도록 설정했습니다. 이 도메인에 도메인 가입 컴퓨터를 사용하려면 사용자 계정은 무엇입니까?
Azure AD DS 관리 도메인의 일부인 모든 사용자 계정은 VM에 가입할 수 있습니다. *AAD DC Administrators* 그룹의 구성원은 관리되는 도메인에 조인된 컴퓨터에 대한 원격 데스크톱 액세스 권한을 부여받습니다.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD Domain Services에서 제공하는 관리되는 도메인에 대한 도메인 관리자 권한이 부여됩니까?
아니요. 관리되는 도메인에 대한 관리 권한이 부여되지 않습니다. *도메인 관리자* 및 *엔터프라이즈 관리자* 권한은 도메인 내에서 사용할 수 없습니다. 온-프레미스 Active Directory의 도메인 관리자 또는 엔터프라이즈 관리자 그룹의 구성원은 관리되는 도메인에 대한 도메인/엔터프라이즈 관리자 권한도 부여되지 않습니다.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>관리되는 도메인에서 LDAP 또는 다른 AD 관리 도구를 사용하여 그룹 멤버 자격을 수정할 수 있습니까?
Azure Active Directory에서 Azure AD 도메인 서비스로 동기화된 사용자 및 그룹은 원본의 원본이 Azure Active Directory이므로 수정할 수 없습니다. 관리되는 도메인에서 시작된 모든 사용자 또는 그룹을 수정할 수 있습니다.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Azure AD 디렉터리에 적용한 변경 사항이 내 관리되는 도메인에 표시되는 데 얼마나 걸리나요?
Azure AD UI 또는 PowerShell을 사용하여 Azure AD 디렉터리에서 변경한 내용은 관리되는 도메인에 자동으로 동기화됩니다. 이 동기화 프로세스는 백그라운드에서 실행됩니다. 이 동기화가 모든 개체 변경 내용을 완료하는 데 정의된 기간은 없습니다.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD Domain Services에서 제공하는 관리되는 도메인의 스키마를 확장할 수 있습니까?
아니요. 스키마는 관리되는 도메인에 대해 Microsoft에서 관리합니다. 스키마 확장Azure AD 도메인 서비스에서 지원되지 않습니다.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>관리되는 도메인에서 DNS 레코드를 수정하거나 추가할 수 있습니까?
예. *AAD DC Administrators* 그룹의 구성원에는 관리되는 도메인의 DNS 레코드를 수정할 수 있는 *DNS 관리자* 권한이 부여됩니다. 이러한 사용자는 관리되는 도메인에 가입한 Windows Server를 실행하는 컴퓨터에서 DNS 관리자 콘솔을 사용하여 DNS를 관리할 수 있습니다. DNS 관리자 콘솔을 사용하려면 서버에 원격 서버 관리 *도구* 옵션 기능의 일부인 *DNS 서버 도구를*설치합니다. 자세한 내용은 [Azure AD 도메인 서비스 관리 도메인의 DNS 관리를](manage-dns.md)참조하십시오.

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>관리되는 도메인에 대한 암호 수명 정책은 무엇인가요?
Azure AD Domain Services 관리되는 도메인의 기본 암호 수명은 90일입니다. 이 암호 수명은 Azure AD에 구성된 암호 수명과 동기화되지 않습니다. 따라서 사용자의 암호가 관리되는 도메인에는 만료되지만 Azure AD에서는 여전히 유효한 상황이 발생할 수 있습니다. 이러한 시나리오에서 사용자는 Azure AD에서 자신의 암호를 변경해야 하며, 새 암호는 관리되는 도메인과 동기화됩니다. 또한 *암호-만료 되지 않습니다* 및 *사용자-변경 해야 암호-다음-로그온* 속성 사용자 계정에 대 한 관리되는 도메인에 동기화 되지 않습니다.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>Azure AD Domain Services에서 AD 계정 잠금 보호를 제공하나요?
예. 관리되는 도메인에서 2분 안에 5차례 암호를 잘못 입력하면 사용자 계정이 30분 동안 잠깁니다. 30분 후 사용자 계정이 자동으로 잠금 해제됩니다. 관리되는 도메인에서 잘못된 암호 시도는 Azure AD의 사용자 계정을 잠그지 않습니다. 사용자 계정은 Azure AD Domain Services 관리되는 도메인 안에서만 잠깁니다. 자세한 내용은 [관리되는 도메인의 암호 및 계정 잠금 정책을](password-policy.md)참조하십시오.

## <a name="billing-and-availability"></a>요금 청구 및 가용성

* [Azure AD Domain Services는 유료 서비스인가요?](#is-azure-ad-domain-services-a-paid-service)
* [서비스에 대한 무료 평가판이 있습니까?](#is-there-a-free-trial-for-the-service)
* [Azure AD Domain Services 관리되는 도메인을 일시 중지할 수 있나요?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [Azure AD Domain Services를 DR 이벤트의 다른 지역으로 장애 조치(failover)할 수 있나요?](#can-i-pause-an-azure-ad-domain-services-managed-domain)
* [EMS(엔터프라이즈 모빌리티 제품군)의 일부로 Azure AD 도메인 서비스를 받을 수 있습니까? Azure AD 도메인 서비스를 사용하려면 Azure AD 프리미엄이 필요합니까?](#can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event)
* [어떤 Azure 지역에서 서비스를 사용할 수 있습니까?](#can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services)

### <a name="is-azure-ad-domain-services-a-paid-service"></a>Azure AD Domain Services는 유료 서비스인가요?
예. 자세한 내용은 가격 [책정 페이지를](https://azure.microsoft.com/pricing/details/active-directory-ds/)참조하십시오.

### <a name="is-there-a-free-trial-for-the-service"></a>서비스에 대한 무료 평가판이 있습니까?
Azure AD 도메인 서비스는 Azure의 무료 평가판에 포함되어 있습니다. [Azure의 1개월 무료 평가판](https://azure.microsoft.com/pricing/free-trial/)에 등록할 수 있습니다.

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services 관리되는 도메인을 일시 중지할 수 있나요?
아니요. Azure AD 도메인 서비스 관리 도메인을 사용하도록 설정하면 관리되는 도메인을 삭제할 때까지 선택한 가상 네트워크 내에서 서비스를 사용할 수 있습니다. 서비스를 일시 중지할 수 있는 방법은 없습니다. 관리되는 도메인을 삭제할 때까지 시간 기준으로 계속 청구됩니다.

### <a name="can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event"></a>Azure AD Domain Services를 DR 이벤트의 다른 지역으로 장애 조치(failover)할 수 있나요?
아니요. Azure AD 도메인 서비스는 현재 지리적 중복 배포 모델을 제공하지 않습니다. Azure 지역의 단일 가상 네트워크로 제한됩니다. 여러 Azure 지역을 사용하려면 Azure IaaS VM에서 Active Directory 도메인 컨트롤러를 실행해야 합니다. 아키텍처 지침의 경우 [온-프레미스 Active Directory 도메인을 Azure로 확장을](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain)참조하십시오.

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Enterprise Mobility Suite(EMS)의 일부로 Azure AD 도메인 서비스를 가져올 수 있습니까? Azure AD Domain Services를 사용하려면 Azure AD Premium이 필요합니까?
아니요. Azure AD 도메인 서비스는 종량제 Azure 서비스이며 EMS의 일부가 아닙니다. Azure AD 도메인 서비스는 모든 버전의 Azure AD(무료 및 프리미엄)와 함께 사용할 수 있습니다. 사용량에 따라 시간단위로 요금이 청구됩니다.

### <a name="what-azure-regions-is-the-service-available-in"></a>어떤 Azure 지역에서 서비스를 사용할 수 있습니까?
Azure AD Domain Services를 사용할 수 있는 Azure 지역의 목록을 알아보려면 [지역별 Azure 서비스](https://azure.microsoft.com/regions/#services/) 페이지를 참조하세요.

## <a name="troubleshooting"></a>문제 해결

Azure AD Domain Services 구성 또는 관리에서 발생하는 일반적인 문제에 대한 솔루션은 [문제 해결 가이드](troubleshoot.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

Azure AD 도메인 서비스에 대한 자세한 내용은 [Azure Active Directory 도메인 서비스란 무엇입니까?](overview.md)

시작하려면 Azure [Active Directory 도메인 서비스 인스턴스 만들기 및 구성을](tutorial-create-instance.md)참조하십시오.
