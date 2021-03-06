---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: rogarana
ms.openlocfilehash: 55456a6be938411d3c08a0eaa8fdbfb0844e7129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72391717"
---
Azure Portal 또는 SMB를 사용하여 Azure 파일 공유에서 변경한 사항은 즉시 검색되지 않고 서버 엔드포인트의 변경 사항처럼 복제됩니다. Azure Files에는 아직 변경 알림/저널링이 없어서 파일이 변경될 때 동기화 세션을 자동으로 시작할 방법이 없습니다. Windows Server에서 Azure 파일 동기화는 [Windows USN 저널링](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx)을 사용하여 파일이 변경될 때 자동으로 동기화 세션을 시작합니다.

Azure 파일 공유의 변경 사항을 검색하기 위해 Azure 파일 동기화에는 *변경 검색 작업*이라는 예약된 작업이 있습니다. 변경 검색 작업은 파일 공유의 모든 파일을 열거한 다음 해당 파일의 동기화 버전과 비교합니다. 변경 검색 작업에서 파일이 변경된 것으로 판단되면 Azure 파일 동기화는 동기화 세션을 시작합니다. 변경 검색 작업은 24시간마다 시작됩니다. 변경 검색 작업은 Azure 파일 공유의 모든 파일을 열거하여 작동하므로 변경 검색은 큰 네임스페이스가 작은 네임스페이스보다 오래 걸립니다. 큰 네임스페이스의 경우 변경된 파일을 확인하는 것이 24시간마다 한 번 이상이 걸릴 수 있습니다.

Azure 파일 공유에서 변경된 파일을 즉시 동기화하려면 **Invoke-AzStorageSyncDetection** PowerShell cmdlet을 사용하여 Azure 파일 공유의 변경 내용 검색을 수동으로 시작할 수 있습니다. 이 cmdlet은 일부 자동화된 프로세스 유형이 Azure 파일 공유를 변경하거나 관리자가 변경(예: 파일 및 디렉터리를 공유로 이동)하는 시나리오를 위한 것입니다. 최종 사용자 변경의 경우 IaaS VM에 Azure 파일 동기화 에이전트를 설치하고 최종 사용자가 IaaS VM을 통해 파일 공유에 액세스하도록 하는 것이 좋습니다. 이렇게 하면 Invoke-AzStorageSyncIncmdlet을 사용할 필요 없이 모든 변경 내용이 다른 에이전트에 빠르게 동기화됩니다. 자세한 내용은 [Invoke-AzStorageSync검색](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) 문서를 참조하십시오.

>[!NOTE]
>REST를 사용하여 Azure 파일 공유에 대한 변경 내용은 SMB 마지막 수정 시간을 업데이트하지 않으며 동기화에 의한 변경으로 볼 수 없습니다.

Windows Server의 볼륨에 대해 USN과 유사한 Azure 파일 공유에 대한 변경 검색 기능을 추가하는 방법을 모색하고 있습니다. 이 기능의 향후 개발을 위해 [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)에서 투표하여 우선 순위 지정에 도움을 주세요.
