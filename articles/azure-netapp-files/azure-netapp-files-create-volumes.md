---
title: Azure NetApp 파일에 대한 NFS 볼륨 생성 | 마이크로 소프트 문서
description: Azure NetApp 파일에 대한 NFS 볼륨을 만드는 방법에 대해 설명합니다.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/01/2019
ms.author: b-juche
ms.openlocfilehash: 9e8817f802ca1d73ca0f6bfa2b32b1b14b37d7da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274087"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Azure NetApp Files에 대한 NFS 볼륨 만들기

Azure NetApp 파일은 NFS(NFSv3 및 NFSv4.1) 및 SMBv3 볼륨을 지원합니다. 볼륨의 용량 소비는 해당 풀의 프로비전된 용량에 대해 계산됩니다. 이 문서에서는 NFS 볼륨을 만드는 방법을 보여 주십습니다. SMB 볼륨을 만들려면 Azure [NetApp 파일에 대한 SMB 볼륨 만들기를](azure-netapp-files-create-volumes-smb.md)참조하십시오. 

## <a name="before-you-begin"></a>시작하기 전에 
용량 풀을 설정해야 합니다.   
[용량 풀 설정](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files에 서브넷을 위임해야 합니다.  
[Azure NetApp Files에 서브넷 위임](azure-netapp-files-delegate-subnet.md)

## <a name="considerations"></a>고려 사항 

* 사용할 NFS 버전 결정  
  NFSv3는 다양한 사용 사례를 처리할 수 있으며 일반적으로 대부분의 엔터프라이즈 응용 프로그램에 배포됩니다. 응용 프로그램에 필요한 버전(NFSv3 또는 NFSv4.1)의 유효성을 검사하고 적절한 버전을 사용하여 볼륨을 만들어야 합니다. 예를 들어 [아파치 ActiveMQ를](https://activemq.apache.org/shared-file-system-master-slave)사용하는 경우 NFSv4.1을 사용한 파일 잠금을 NFSv3보다 사용하는 것이 좋습니다. 

* 보안  
  NFSv3 및 NFSv4.1에서는 UNIX 모드 비트(읽기, 쓰기 및 실행)에 대한 지원을 사용할 수 있습니다. NFS 볼륨을 탑재하려면 NFS 클라이언트에서 루트 수준 액세스가 필요합니다.

* NFSv4.1에 대한 로컬 사용자/그룹 및 LDAP 지원  
  현재 NFSv4.1은 볼륨에 대한 루트 액세스를 지원합니다. [Azure NetApp 파일에 대한 NFSv4.1 기본 도메인 구성을](azure-netapp-files-configure-nfsv41-domain.md)참조하십시오. 

## <a name="best-practice"></a>모범 사례

* 볼륨에 적합한 마운트 지침을 사용하고 있는지 확인해야 합니다.  [Windows 또는 Linux 가상 컴퓨터용 볼륨 마운트 또는 마운트 해제를](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)참조하십시오.

* NFS 클라이언트는 Azure NetApp 파일 볼륨과 동일한 VNet 또는 피어된 VNet에 있어야 합니다. VNet 외부에서 연결하는 것이 지원됩니다. 그러나 추가 대기 시간이 발생하리며 전반적인 성능이 저하됩니다.

* NFS 클라이언트가 최신 상태이고 운영 체제에 대한 최신 업데이트를 실행해야 합니다.

## <a name="create-an-nfs-volume"></a>NFS 볼륨 만들기

1.  용량 풀 블레이드에서 **볼륨** 블레이드를 클릭합니다. 

    ![볼륨으로 이동](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  **+ 볼륨 추가**를 클릭하여 볼륨을 만듭니다.  
    볼륨 만들기 창이 나타납니다.

3.  볼륨 만들기 창에서 **만들기를** 클릭하고 다음 필드에 대한 정보를 제공합니다.   
    * **볼륨 이름**      
        만들고 있는 볼륨의 이름을 지정합니다.   

        볼륨 이름은 각 용량 풀 내에서 고유해야 합니다. 3자 이상이어야 합니다. 모든 사용 문자는 사용할 수 있습니다.   

        볼륨 이름으로 `default` 사용할 수 없습니다.

    * **용량 풀**  
        볼륨을 만들 용량 풀을 지정합니다.

    * **할당량**  
        볼륨에 할당되는 논리 스토리지의 크기를 지정합니다.  

        **사용 가능한 할당량** 필드는 새 볼륨을 만들 때 사용할 수 있는 선택한 용량 풀에서 사용되지 않은 공간의 양을 보여줍니다. 새 볼륨의 크기는 사용 가능한 할당량을 초과해서는 안 됩니다.  

    * **가상 네트워크**  
        볼륨에 액세스하려는 Azure Vnet(가상 네트워크)을 지정합니다.  

        지정하는 VNet에는 Azure NetApp Files에 위임된 서브넷이 있어야 합니다. Azure NetApp Files 서비스는 동일한 VNet 또는 VNet 피어링을 통해 볼륨과 동일한 영역에 있는 VNet에서만 액세스할 수 있습니다. 익스프레스 루트를 통해 온-프레미스 네트워크에서 볼륨에 액세스할 수도 있습니다.   

    * **서브넷**  
        볼륨에 사용할 서브넷을 지정합니다.  
        지정하는 서브넷은 Azure NetApp Files에 위임되어야 합니다. 
        
        서브넷을 위임하지 않은 경우에는 볼륨 만들기 페이지에서 **새로 만들기**를 클릭할 수 있습니다. 그런 다음, 서브넷 만들기 페이지에서 서브넷 정보를 지정하고 **Microsoft.NetApp/volumes**를 선택하여 Azure NetApp Files의 서브넷을 위임합니다. 각 Vnet에서 하나의 서브넷만 Azure NetApp 파일에 위임할 수 있습니다.   
 
        ![볼륨 만들기](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![서브넷 만들기](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **프로토콜**을 클릭한 후, 다음 작업을 완료합니다.  
    * **NFS**를 볼륨에 대한 프로토콜 유형으로 선택합니다.   
    * 새 볼륨에 대한 내보내기 경로를 만드는 데 사용할 **파일** 경로를 지정합니다. 내보내기 경로는 볼륨을 탑재하고 액세스하는 데 사용됩니다.

        파일 경로 이름에는 문자, 숫자 및 하이픈("-")만 포함할 수 있습니다. 이름은 16자~40자여야 합니다. 

        파일 경로는 각 구독 및 각 리전 내에서 고유해야 합니다. 

    * 볼륨의 NFS 버전(**NFSv3** 또는 **NFSv4.1**)을 선택합니다.  
    * 선택적으로 [NFS 볼륨에 대한 내보내기 정책을 구성합니다.](azure-netapp-files-configure-export-policy.md)

    ![NFS 프로토콜 지정](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. **검토 + 만들기를** 클릭하여 볼륨 세부 정보를 검토합니다.  그런 다음 **작성을** 클릭하여 NFS 볼륨을 만듭니다.

    만든 볼륨이 볼륨 페이지에 나타납니다. 
 
    볼륨은 해당 용량 풀에서 구독, 리소스 그룹, 위치 특성을 상속합니다. 볼륨 배포 상태를 모니터링하려면 알림 탭을 사용할 수 있습니다.


## <a name="next-steps"></a>다음 단계  

* [Azure NetApp Files에 대한 NFSv 4.1 기본 도메인 구성](azure-netapp-files-configure-nfsv41-domain.md)
* [Windows 또는 Linux 가상 머신에 대한 볼륨 탑재 또는 탑재 해제](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [NFS 볼륨에 대한 내보내기 정책 구성](azure-netapp-files-configure-export-policy.md)
* [Azure NetApp Files에 대한 리소스 제한](azure-netapp-files-resource-limits.md)
* [Azure 서비스에 대한 가상 네트워크 통합에 대해 자세히 알아보기](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
