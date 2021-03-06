---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/30/2019
ms.author: zivr
ms.custom: include file
ms.openlocfilehash: 3215f5952daef053c94432bc8fdef15e1775047a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73171115"
---
VM을 단일 영역에 배치하면 인스턴스 간의 물리적 거리가 줄어듭니다. 단일 가용성 영역 내에 배치하면 물리적으로 더 가까워집니다. 그러나 Azure 사용 공간이 증가함에 따라 단일 가용성 영역이 여러 물리적 데이터 센터에 걸쳐 있을 수 있으며, 이로 인해 네트워크 대기 시간이 응용 프로그램에 영향을 미칠 수 있습니다. 

VM을 가능한 한 가깝게 설정하여 가능한 가장 낮은 대기 시간을 달성하려면 근접 배치 그룹 내에 배포해야 합니다.

근접 배치 그룹은 Azure 계산 리소스가 물리적으로 서로 가까이 있는지 확인하는 데 사용되는 논리적 그룹입니다. 근접 배치 그룹은 대기 시간이 짧은 워크로드에 유용합니다.


- 독립 실행형 VM 간의 짧은 대기 시간.
- 단일 가용성 집합 또는 가상 시스템 규모 집합의 VM 간의 대기 시간이 낮습니다. 
- 독립 실행형 VM, 여러 가용성 집합의 VM 또는 여러 규모 집합 간의 짧은 대기 시간입니다. 단일 배치 그룹에 여러 계산 리소스를 사용하여 다중 계층 응용 프로그램을 통합할 수 있습니다. 
- 서로 다른 하드웨어 유형을 사용하는 여러 응용 프로그램 계층 간의 대기 시간이 낮습니다. 예를 들어 가용성 집합에서 M-시리즈를 사용하여 백 엔드를 실행하고 D 시리즈 인스턴스의 프런트 엔드를 단일 근접 배치 그룹에서 스케일 집합으로 실행합니다.


![근접 배치 그룹에 대한 그래픽](./media/virtual-machines-common-ppg/ppg.png)

## <a name="using-proximity-placement-groups"></a>근접 배치 그룹 사용 

근접 배치 그룹은 Azure의 새 리소스 유형입니다. 다른 리소스와 함께 사용하기 전에 하나를 만들어야 합니다. 생성된 후에는 가상 컴퓨터, 가용성 집합 또는 가상 시스템 크기 집합과 함께 사용할 수 있습니다. 근접 배치 그룹 ID를 제공하는 계산 리소스를 작성할 때 근접 배치 그룹을 지정합니다. 

기존 리소스를 근접 배치 그룹으로 이동할 수도 있습니다. 리소스를 근접 배치 그룹으로 이동할 때 는 해당 지역의 다른 데이터 센터에 잠재적으로 다시 배포되므로 코로케이션 제약 조건을 충족하므로 먼저 자산을 중지(deallocate)해야 합니다. 

가용성 집합 및 가상 시스템 크기 집합의 경우 개별 가상 컴퓨터가 아닌 리소스 수준에서 근접 배치 그룹을 설정해야 합니다. 

근접 배치 그룹은 고정 메커니즘이 아닌 코로케이션 제약 조건입니다. 사용할 첫 번째 리소스가 배포되어 특정 데이터 센터에 고정됩니다. 근접 배치 그룹을 사용하는 모든 리소스가 중지(할당 할당)되거나 삭제되면 더 이상 고정되지 않습니다. 따라서 여러 VM 계열의 근접 배치 그룹을 사용하는 경우 가능한 경우 템플릿에 필요한 모든 형식을 미리 지정하거나 배포 순서를 따라 성공적인 배포 가능성을 높이는 것이 중요합니다. 배포에 실패하면 배포할 첫 번째 크기로 실패한 VM 크기로 배포를 다시 시작합니다.


## <a name="best-practices"></a>모범 사례 
- 가장 낮은 대기 시간에 대 한 가속된 네트워킹 함께 근접 배치 그룹을 사용 합니다. 자세한 내용은 [가속 네트워킹을 사용하여 Linux 가상 컴퓨터 만들기](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 또는 가속 [네트워킹을 사용하여 Windows 가상 컴퓨터 만들기를](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)참조하십시오.
- 모든 VM 크기를 단일 템플릿에 배포합니다. 필요한 모든 VM SCO 및 크기를 지원하지 않는 하드웨어에 대한 방문을 방지하려면 모든 응용 프로그램 계층을 단일 템플릿에 포함시켜 모두 동시에 배포할 수 있도록 합니다.
- PowerShell, CLI 또는 SDK를 사용하여 배포를 스크립팅하는 경우 `OverconstrainedAllocationRequest`할당 오류가 발생할 수 있습니다. 이 경우 모든 기존 VM을 중지/할당 배치하고 배포 스크립트의 시퀀스를 변경하여 실패한 VM SKU/크기로 시작해야 합니다. 
- VM이 삭제된 기존 배치 그룹을 다시 사용할 때VM을 추가하기 전에 삭제가 완전히 완료될 때까지 기다립니다.
- 대기 시간이 최우선 인 경우 VM을 근접 배치 그룹에 넣고 전체 솔루션을 가용성 영역에 배치합니다. 그러나 복원력을 최우선으로 하는 경우 여러 가용성 영역에 인스턴스를 분산합니다(단일 근접 배치 그룹은 영역을 포괄할 수 없음).