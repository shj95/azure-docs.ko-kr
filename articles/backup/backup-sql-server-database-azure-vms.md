---
title: Azure VM의 SQL Server 데이터베이스 백업
description: 이 문서에서는 Azure 백업을 사용하여 Azure 가상 컴퓨터에서 SQL Server 데이터베이스를 백업하는 방법을 알아봅니다.
ms.reviewer: vijayts
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 5b10907738feeecbec06669175e82578f2915f92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273333"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Azure VM의 SQL Server 데이터베이스 백업

SQL Server 데이터베이스는 낮은 RPO(복구 지점 목표) 및 장기 보존이 필요한 중요한 워크로드입니다. [Azure 백업](backup-overview.md)을 사용하여 Azure 가상 시스템(VM)에서 실행되는 SQL Server 데이터베이스를 백업할 수 있습니다.

이 문서에서는 Azure VM에서 실행 중인 SQL Server 데이터베이스를 Azure 백업 복구 서비스 자격 증명 모음에 백업하는 방법을 보여 주며 있습니다.

이 문서에서는 다음을 수행하는 방법을 알아봅니다.

> [!div class="checklist"]
>
> * 자격 증명 모음을 만들고 구성합니다.
> * 데이터베이스를 검색하고 백업을 설정합니다.
> * 데이터베이스에 대한 자동 보호를 설정합니다.

>[!NOTE]
>**Azure VM의 SQL 서버에 대한 소프트 삭제 및 Azure VM 워크로드의 SAP HANA에 대한 소프트 삭제를** 미리 보기에서 사용할 수 있습니다.<br>
>미리 보기에 등록하려면AskAzureBackupTeam@microsoft.com

## <a name="prerequisites"></a>사전 요구 사항

SQL Server 데이터베이스를 백업하기 전에 다음 조건을 확인하십시오.

1. SQL Server 인스턴스를 호스팅하는 VM과 동일한 리전 및 구독에서 [복구 서비스 자격 증명 모음을](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) 식별하거나 만듭니다.
2. VM에 네트워크 연결이 있는지 [확인합니다.](backup-sql-server-database-azure-vms.md#establish-network-connectivity)
3. SQL Server 데이터베이스가 Azure Backup 에 [대한 데이터베이스 이름 지정 지침을](#database-naming-guidelines-for-azure-backup)따르는지 확인합니다.
4. 데이터베이스에 대해 사용할 수 있는 다른 백업 솔루션이 없는지 확인합니다. 데이터베이스를 백업하기 전에 다른 모든 SQL Server 백업을 사용하지 않도록 설정합니다.

> [!NOTE]
> Azure VM 및 충돌 없이 VM에서 실행 중인 SQL Server 데이터베이스에 대해 Azure 백업을 사용하도록 설정할 수 있습니다.

### <a name="establish-network-connectivity"></a>네트워크 연결 설정

모든 작업에 대해 SQL Server VM은 Azure 공용 IP 주소에 연결해야 합니다. Azure 공용 IP 주소에 연결되지 않으면 VM 작업(데이터베이스 검색, 백업 구성, 백업 예약, 복구 지점 복원 등)이 실패합니다.

다음 옵션 중 하나를 사용하여 연결을 설정합니다.

#### <a name="allow-the-azure-datacenter-ip-ranges"></a>Azure 데이터 센터 IP 범위 허용

이 옵션은 다운로드한 파일의 [IP 범위](https://www.microsoft.com/download/details.aspx?id=41653)를 허용합니다. NSG(네트워크 보안 그룹)에 액세스하려면 Set-AzureNetworkSecurityRule cmdlet을 사용합니다. 안전한 수신자 목록에 지역별 IP만 포함되어 있는 경우 인증을 사용하도록 설정하려면 안전한 수신자 목록에 Azure AD(Azure Active Directory) 서비스 태그를 업데이트해야 합니다.

#### <a name="allow-access-using-nsg-tags"></a>NSG 태그를 사용하여 액세스 허용

NSG를 사용하여 연결을 제한하는 경우 AzureBackup 서비스 태그를 사용하여 Azure Backup에 대한 아웃바운드 액세스를 허용해야 합니다. 또한 Azure AD 및 Azure Storage에 대한 [규칙](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags)을 사용하여 인증 및 데이터 전송에 대한 연결을 허용해야 합니다. Azure Portal 또는 PowerShell에서 수행할 수 있습니다.

포털을 사용하여 규칙을 만들려면 다음을 수행합니다.

  1. **모든 서비스**에서 **네트워크 보안 그룹**으로 이동하여 네트워크 보안 그룹을 선택합니다.
  2. **설정** 아래에서 **아웃바운드 보안 규칙**을 선택합니다.
  3. **추가**를 선택합니다. [보안 규칙 설정](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings)에 설명된 대로 새 규칙을 만드는 데 필요한 세부 정보를 모두 입력합니다. **대상** 옵션이 **서비스 태그**로 설정되고 **대상 서비스 태그**가 **AzureBackup**으로 설정되어 있는지 확인합니다.
  4. **추가**를 클릭하여 새로 만든 아웃바운드 보안 규칙을 저장합니다.

PowerShell을 사용하여 규칙을 만들려면 다음을 수행합니다.

 1. Azure 계정 자격 증명을 추가하고 국가를 업데이트합니다.<br/>
      `Add-AzureRmAccount`<br/>

 2. NSG 구독을 선택합니다.<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. NSG를 선택합니다.<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Azure Backup 서비스 태그에 대한 아웃바운드 허용 규칙을 추가합니다.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Storage 서비스 태그에 대한 아웃바운드 허용 규칙을 추가합니다.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. AzureActiveDirectory 서비스 태그에 대한 아웃바운드 허용 규칙을 추가합니다.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. NSG를 저장합니다.<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Azure Firewall 태그를 사용하여 액세스를 허용합니다.** Azure Firewall을 사용하는 경우 AzureBackup [FQDN 태그](https://docs.microsoft.com/azure/firewall/fqdn-tags)를 사용하여 애플리케이션 규칙을 만듭니다. 이는 Azure Backup에 대한 아웃바운드 액세스를 허용합니다.

**트래픽을 라우팅하는 HTTP 프록시 서버를 배포합니다**. Azure VM에서 SQL Server 데이터베이스를 백업할 때 VM의 백업 확장은 HTTPS API를 사용하여 관리 명령을 Azure 백업으로 보내고 데이터를 Azure 저장소로 보냅니다. 백업 확장도 인증에 Azure AD를 사용합니다. HTTP 프록시를 통해 이 세 가지 서비스에 대한 백업 확장 트래픽을 라우팅합니다. Azure Backup과 함께 사용 중인 와일드카드 도메인이 없습니다. Azure에서 제공하는 이러한 서비스에 대한 공용 IP 범위를 사용해야 합니다. 확장의 공용 인터넷에 액세스하도록 구성된 유일한 구성 요소입니다.

연결 옵션에는 다음과 같은 장점과 단점이 있습니다.

**옵션** | **장점** | **단점**
--- | --- | ---
IP 범위 허용 | 추가 비용 없음 | 시간이 지남에 따라 IP 주소 범위가 변경되므로 관리가 복잡함 <br/><br/> Azure Storage뿐만 아니라 Azure 전체에 대한 액세스 제공
NSG 서비스 태그 사용 | 범위 변경이 자동으로 병합되어 관리가 더 쉬움 <br/><br/> 추가 비용 없음 <br/><br/> | NSG에만 사용할 수 있음 <br/><br/> 전체 서비스에 대한 액세스 제공
Azure Firewall FQDN 태그 사용 | 필요한 FQDN이 자동으로 관리되어 관리가 더 쉬움 | Azure Firewall하고만 함께 사용할 수 있음
HTTP 프록시 사용 | VM에 대한 인터넷 액세스의 단일 지점 <br/> | 프록시 소프트웨어로 VM을 실행하기 위해 추가 비용이 있음 <br/> 게시된 FQDN 주소가 없으며 허용 규칙은 Azure IP 주소 변경 사항이 적용됩니다.

#### <a name="private-endpoints"></a>프라이빗 엔드포인트

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

### <a name="database-naming-guidelines-for-azure-backup"></a>Azure 백업에 대한 데이터베이스 이름 지정 지침

데이터베이스 이름에 다음 요소를 사용하지 마십시오.

* 후행 및 선행 공간
* 후행 느낌표 (!)
* 닫기 대괄호(])
* 세미콜론 ';'
* 정방향 슬래시 '/'

지원되지 않는 문자에는 앨리어싱을 사용할 수 있지만 이를 피하는 것이 좋습니다. 자세한 내용은 [테이블 서비스 데이터 모델 이해](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN)를 참조하세요.

>[!NOTE]
>이름에 "+" 또는 "&"과 같은 특수 문자가 있는 데이터베이스에 대한 **보호 구성** 작업은 지원되지 않습니다. 데이터베이스 이름을 변경하거나 이러한 데이터베이스를 성공적으로 보호할 수 있는 **자동 보호를**사용하도록 설정할 수 있습니다.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server 데이터베이스 검색

VM에서 실행 중인 데이터베이스를 검색하는 방법:

1. [Azure Portal](https://portal.azure.com)에서 데이터베이스를 백업하는 데 사용하는 Recovery Services 자격 증명 모음을 엽니다.

2. 복구 **서비스 자격 증명 모음** 대시보드에서 **백업**을 선택합니다.

   ![Backup을 선택하여 백업 목표 메뉴 열기](./media/backup-azure-sql-database/open-backup-menu.png)

3. **백업 목표에서** **워크로드가 실행 중인 위치를** **Azure로**설정합니다.

4. **백업할 항목**에서 **Azure VM의 SQL Server**를 선택합니다.

    ![백업을 위해 Azure VM에서 SQL Server 선택](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. **백업 목표** > **에서 VM에서 DB를 검색하려면**검색 **시작을** 선택하여 구독에서 보호되지 않은 VM을 검색합니다. 이 검색은 구독의 보호되지 않은 VM 수에 따라 시간이 걸릴 수 있습니다.

   * 검색 후에는 보호되지 않은 VM이 이름 및 리소스 그룹별로 나열된 목록에 표시됩니다.
   * VM이 예상대로 나열되지 않은 경우 VM이 볼트에 이미 백업되어 있는지 확인합니다.
   * 여러 VM의 이름은 같을 수 있지만 다른 리소스 그룹에 속합니다.

     ![VM의 DB 검색 중에는 백업이 보류됩니다.](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM 목록에서 SQL Server 데이터베이스가 실행되는 VM, **DB 검색**을 차례로 선택합니다.

7. **알림**에서 데이터베이스 검색을 추적합니다. 이 작업에 필요한 시간은 VM 데이터베이스 수에 따라 다릅니다. 선택한 데이터베이스가 검색되면 성공 메시지가 표시됩니다.

    ![배포 성공 메시지](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup은 VM의 모든 SQL Server 데이터베이스를 검색합니다. 검색 하는 동안 다음 요소가 백그라운드에서 발생 합니다.

    * Azure Backup은 워크로드 백업을 위한 볼트에 VM을 등록합니다. 등록된 VM의 모든 데이터베이스는 이 자격 증명 모음에만 백업할 수 있습니다.
    * Azure Backup에서 AzureBackupWindowsWorkload 확장을 VM에 설치합니다. SQL 데이터베이스에 에이전트가 설치되어 있지 않습니다.
    * Azure Backup에서 NT Service\AzureWLBackupPluginSvc 서비스 계정을 VM에 만듭니다.
      * 모든 백업 및 복원 작업에는 서비스 계정이 사용됩니다.
      * NT 서비스\AzureWLBackupPluginSvcSQL 시스템 관리자 권한이 필요합니다. 마켓플레이스에서 생성된 모든 SQL Server VM에는 SqlIaSExtension이 설치되어 있습니다. AzureBackupWindowsWorkload 확장은 SQLIaaSExtension을 사용하여 필요한 권한을 자동으로 확보합니다.
    * 마켓플레이스에서 VM을 만들지 않았거나 SQL 2008 및 2008 R2에 있는 경우 VM에 SqlIaSExtension이 설치되어 있지 않을 수 있으며 오류 메시지UserErrorSQLNoSysAdminMembership를 사용하여 검색 작업이 실패합니다. 이 문제를 해결하려면 VM 사용 권한 설정아래의 지침을 [따릅니다.](backup-azure-sql-database.md#set-vm-permissions)

        ![VM 및 데이터베이스 선택](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>백업 구성  

1. **백업 목표** > **단계 2: 백업 구성,** **백업 구성**을 선택합니다.

   ![백업 구성 선택](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. **백업할 항목 선택에서**등록된 모든 가용성 그룹 및 독립 실행형 SQL Server 인스턴스가 표시됩니다. 행 왼쪽의 화살표를 선택하여 해당 인스턴스또는 Always On 가용성 그룹의 모든 보호되지 않은 데이터베이스 목록을 확장합니다.  

    ![독립 실행형 데이터베이스를 사용하는 모든 SQL Server 인스턴스 표시](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. 보호할 모든 데이터베이스를 선택한 다음 **확인을**선택합니다.

   ![데이터베이스 보호](./media/backup-azure-sql-database/select-database-to-protect.png)

   백업 로드를 최적화하기 위해 Azure Backup은 한 백업 작업의 최대 데이터베이스 수를 50개로 설정합니다.

     * 50개가 넘는 데이터베이스를 보호하려면 여러 백업을 구성합니다.
     * 전체 인스턴스 또는 Always 켜기 가용성 그룹을 [사용하려면](#enable-auto-protection) **AUTOPROTECT** 드롭다운 목록에서 **ON**을 선택한 다음 **확인을**선택합니다.

    > [!NOTE]
    > [자동 보호](#enable-auto-protection) 기능은 모든 기존 데이터베이스를 한 번에 보호할 수 없을 뿐만 아니라 해당 인스턴스 또는 가용성 그룹에 추가된 새 데이터베이스를 자동으로 보호합니다.  

4. **백업 정책을**열려면 **확인을** 선택합니다.

    ![Always On 가용성 그룹에 대한 자동 보호 사용](./media/backup-azure-sql-database/enable-auto-protection.png)

5. **백업 정책에서**정책을 선택한 다음 **확인을**선택합니다.

   * 기본 정책을 시간별 로그백업으로 선택합니다.
   * 이전에 SQL용으로 만든 기존 백업 정책을 선택합니다.
   * RPO(복구 지점 목표) 및 보존 범위를 기반으로 새 정책을 정의합니다.

     ![백업 정책 선택](./media/backup-azure-sql-database/select-backup-policy.png)

6. **백업**에서 **백업 사용**을 선택합니다.

    ![선택한 백업 정책 사용](./media/backup-azure-sql-database/enable-backup-button.png)

7. 포털의 **알림** 영역에서 구성 진행률을 추적합니다.

    ![알림 영역](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>백업 정책 만들기

백업 정책은 백업이 수행되는 시기와 유지되는 기간을 정의합니다.

* 정책은 자격 증명 모음 수준에서 만들어집니다.
* 다수의 자격 증명 모음은 자격 증명 모음은 동일한 백업 정책을 사용할 수 있지만 자격 증명 모음마다 백업 정책을 적용해야 합니다.
* 백업 정책을 만드는 경우 매일 전체 백업이 기본값입니다.
* 차등 백업을 추가할 수 있지만 전체 백업이 매주 발생하도록 구성하는 경우에만 가능합니다.
* 다양한 [유형의 백업 정책에](backup-architecture.md#sql-server-backup-types)대해 알아봅니다.

백업 정책을 만들려면:

1. 볼트에서 백업 **정책** > **추가를**선택합니다.
2. **추가에서** **Azure VM에서 SQL Server를** 선택하여 정책 유형을 정의합니다.

   ![새 백업 정책에 대한 정책 유형 선택](./media/backup-azure-sql-database/policy-type-details.png)

3. **정책 이름**에 새 정책의 이름을 입력합니다.
4. **전체 백업 정책**에서 **백업 빈도**를 선택합니다. **일일** 또는 **주간**중 하나를 선택합니다.

   * **매일**의 경우 백업 작업이 시작될 때 시간과 표준 시간대를 선택합니다.
   * **매주**의 경우 백업 작업이 시작되는 요일, 시간 및 표준 시간대를 선택합니다.
   * 전체 백업 옵션을 끌 수 없으므로 **전체 백업을** 실행합니다.
   * 정책을 보려면 **전체 백업을** 선택합니다.
   * 매일 전체 백업에 대해서는 차등 백업을 만들 수 없습니다.

     ![새 백업 정책 필드](./media/backup-azure-sql-database/full-backup-policy.png)  

5. **보존 범위에서**모든 옵션이 기본적으로 선택됩니다. 원하지 않는 보존 범위 제한을 지울 수 있는 다음 사용할 간격을 설정합니다.

    * 모든 유형의 백업(전체, 차등 및 로그)에 대한 최소 보존 기간은 7일입니다.
    * 복구 지점은 보존 범위를 기반으로 보존 태그가 지정됩니다. 예를 들어, 매일, 전체 백업을 선택하면 매일 하나의 전체 백업만 트리거됩니다.
    * 특정 일의 백업은 주간 보존 범위 및 주간 보존 설정에 따라 태그가 지정되고 유지됩니다.
    * 월별 및 연간 보존 범위는 비슷한 방식으로 행동합니다.

       ![보존 범위 간격 설정](./media/backup-azure-sql-database/retention-range-interval.png)

6. **전체 백업 정책** 메뉴에서 **확인**을 클릭하여 설정을 적용합니다.
7. 차등 백업 정책을 추가하려면 **차등 백업**을 선택합니다.

   ![보존 범위 간격 설정](./media/backup-azure-sql-database/retention-range-interval.png)
   ![차등 백업 정책 메뉴 열기](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. **차등 백업 정책**에서 **사용**을 선택하여 빈도 및 보존 컨트롤을 엽니다.

    * 하루에 하나의 차등 백업만 트리거할 수 있습니다.
    * 차등 백업은 최대 180일 동안 보존될 수 있습니다. 보존 기간을 연장할 수 있는 경우 전체 백업을 사용합니다.

9. **확인**을 선택하여 정책을 저장하고 주 **백업 정책** 메뉴로 돌아갑니다.

10. 트랜잭션 로그 백업 정책을 추가하려면 **로그 백업**을 선택합니다.
11. **로그 백업**에서 **사용**을 선택한 다음, 빈도 및 보존 컨트롤을 설정합니다. 로그 백업은 15분마다 자주 발생할 수 있으며 최대 35일 동안 보관할 수 있습니다.
12. **확인**을 선택하여 정책을 저장하고 주 **백업 정책** 메뉴로 돌아갑니다.

    ![로그 백업 정책 편집](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. 백업 **정책** 메뉴에서 **SQL 백업 압축을** 사용하도록 설정할지 여부를 선택합니다. 이 옵션은 기본적으로 비활성화됩니다. 활성화된 경우 SQL Server는 압축된 백업 스트림을 VDI로 보냅니다.  Azure Backup은 이 컨트롤의 값에 따라 압축/NO_COMPRESSION 절을 사용 하 여 인스턴스 수준 기본값을 재정의 합니다.

14. 백업 정책 편집을 완료 한 후, **확인**을 선택합니다.

> [!NOTE]
> 각 로그 백업은 이전 전체 백업에 연결되어 복구 체인을 형성합니다. 이 전체 백업은 마지막 로그 백업의 보존이 만료될 때까지 유지됩니다. 이는 모든 로그를 복구할 수 있도록 전체 백업이 추가 기간 동안 유지됨을 의미할 수 있습니다. 사용자가 주간 전체 백업, 일일 차동 및 2 시간 로그를 가지고 있다고 가정 해 봅시다. 그들 모두는 30 일 동안 유지됩니다. 그러나, 주간 전체 정말 정리 / 다음 전체 백업을 사용할 수있는 경우에만 삭제 할 수 있습니다, 즉, 후 30 + 7 일. 11월 16일에 는 매주 전체 백업이 수행됩니다. 보존 정책에 따라 12월 16일까지 보존해야 합니다. 이 전체의 마지막 로그 백업은 11월 22일에 예정된 다음 전체 로그 전에 수행됩니다. 이 로그는 12월 22일까지 사용할 수 있을 때까지 11월 16일 전체를 삭제할 수 없습니다. 그래서, 11 월 16 전체 12 월 22 일까지 유지 됩니다.

## <a name="enable-auto-protection"></a>자동 보호 사용  

자동 보호를 사용하여 모든 기존 및 향후 데이터베이스를 독립 실행형 SQL Server 인스턴스 또는 Always On 가용성 그룹에 자동으로 백업할 수 있습니다.

* 한 번에 자동 보호를 위해 선택할 수 있는 데이터베이스 수에는 제한이 없습니다.
* 자동 보호를 사용하도록 설정하면 인스턴스에서 데이터베이스를 보호하거나 제외할 수 없습니다.
* 인스턴스에 이미 일부 보호된 데이터베이스가 포함되어 있는 경우 자동 보호를 켜도 해당 정책에 따라 보호됩니다. 나중에 추가된 모든 보호되지 않은 데이터베이스에는 **백업 구성**에 나열된 자동 보호 를 사용하도록 설정할 때 정의한 단일 정책만 있습니다. 그러나 나중에 자동 보호 된 데이터베이스와 관련 된 정책을 변경할 수 있습니다.  

자동 보호를 사용하려면 다음을 수행하십시오.

  1. **백업할 항목**에서 자동 보호를 사용하도록 설정하려는 인스턴스를 선택합니다.
  2. **자동 보호**에서 드롭다운 목록을 선택하고 **ON을**선택한 다음 **확인을**선택합니다.

      ![가용성 그룹에서 자동 보호 사용](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. 백업은 모든 데이터베이스에 대해 함께 구성되며 **백업 작업**에서 추적할 수 있습니다.

자동 보호를 사용하지 않도록 설정해야 하는 경우 **백업 구성**에서 인스턴스 이름을 선택한 다음 인스턴스에 대한 자동 보호 **해제를** 선택합니다. 모든 데이터베이스는 계속 백업되지만 향후 데이터베이스는 자동으로 보호되지 않습니다.

![해당 인스턴스에서 자동 보호 를 사용하지 않도록 설정](./media/backup-azure-sql-database/disable-auto-protection.png)

## <a name="next-steps"></a>다음 단계

방법 배우기:

* [백업된 SQL Server 데이터베이스 복원](restore-sql-database-azure-vm.md)
* [백업된 SQL Server 데이터베이스 관리](manage-monitor-sql-database-backup.md)
