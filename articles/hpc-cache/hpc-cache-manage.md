---
title: Azure HPC 캐시 관리 및 업데이트
description: Azure 포털을 사용하여 Azure HPC 캐시를 관리하고 업데이트하는 방법
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: rohogue
ms.openlocfilehash: da260074fc69fac9e98d3698bb2d40fdf80d7118
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77252045"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Azure 포털에서 캐시 관리

Azure 포털의 캐시 개요 페이지에는 캐시에 대한 프로젝트 세부 정보, 캐시 상태 및 기본 통계가 표시됩니다. 또한 캐시를 중지하거나 시작하고, 캐시를 삭제하고, 데이터를 장기 저장소로 플러시하고, 소프트웨어를 업데이트하는 컨트롤도 있습니다.

개요 페이지를 열려면 Azure 포털에서 캐시 리소스를 선택합니다. 예를 들어 **모든 리소스** 페이지를 로드하고 캐시 이름을 클릭합니다.

![Azure HPC 캐시 인스턴스의 개요 페이지의 스크린샷](media/hpc-cache-overview.png)

페이지 상단의 단추를 사용하면 캐시를 관리하는 데 도움이 될 수 있습니다.

* **시작** 및 [**중지**](#stop-the-cache) - 캐시 작업 일시 중단
* [**플러시**](#flush-cached-data) - 저장소 대상에 변경된 데이터를 기록합니다.
* [**업그레이드**](#upgrade-cache-software) - 캐시 소프트웨어 업데이트
* **새로 고침** - 개요 페이지를 다시 로드합니다.
* [**삭제**](#delete-the-cache) - 캐시를 영구적으로 삭제합니다.

아래에서 이러한 옵션에 대해 자세히 알아보기.

## <a name="stop-the-cache"></a>캐시 중지

비활성 기간 동안 비용을 줄이기 위해 캐시를 중지할 수 있습니다. 캐시가 중지되는 동안에는 가동 시간에 대한 요금이 청구되지 않지만 캐시의 할당된 디스크 저장소에 대한 요금이 부과됩니다. (자세한 내용은 [가격 책정](https://aka.ms/hpc-cache-pricing) 페이지를 참조하십시오.)

중지된 캐시는 클라이언트 요청에 응답하지 않습니다. 캐시를 중지하기 전에 클라이언트의 마운트를 해제해야 합니다.

**중지** 단추는 활성 캐시를 일시 중단합니다. **중지** 단추는 캐시의 상태가 **정상** 또는 **저하된**경우 사용할 수 있습니다.

![Stop 강조 표시된 상단 버튼의 스크린샷과 중지 동작을 설명하고 '계속하시겠습니까?' 예(기본값) 및 없음 버튼](media/stop-cache.png)

예를 클릭하여 캐시 중지를 확인하면 캐시가 자동으로 해당 내용을 저장소 대상으로 플러시합니다. 이 프로세스는 다소 시간이 걸릴 수 있지만 데이터 일관성을 보장합니다. 마지막으로 캐시 상태가 **중지됨으로**변경됩니다.

중지된 캐시를 다시 활성화하려면 **시작** 단추를 클릭합니다. 확인이 필요하지 않습니다.

![시작이 강조 표시된 상단 버튼의 스크린샷](media/start-cache.png)

## <a name="flush-cached-data"></a>캐시된 데이터 플러시

개요 페이지의 **플러시** 단추는 캐시에 저장된 모든 변경된 데이터를 백 엔드 저장소 대상에 즉시 작성하도록 캐시에 알려줍니다. 캐시는 정기적으로 데이터를 저장소 대상에 저장하므로 백 엔드 저장소 시스템이 최신 상태인지 확인하지 않는 한 수동으로 이 작업을 수행할 필요가 없습니다. 예를 들어 저장소 스냅숏을 찍거나 데이터 집합 크기를 확인하기 전에 **플러시를** 사용할 수 있습니다.

> [!NOTE]
> 플러시 프로세스 중에 캐시는 클라이언트 요청을 처리할 수 없습니다. 캐시 액세스가 일시 중단되고 작업이 완료된 후 다시 시작됩니다.

![플러시 강조 표시된 상단 버튼의 스크린샷과 플러시 동작을 설명하고 '계속하시겠습니까?' 예(기본값) 및 없음 버튼](media/hpc-cache-flush.png)

캐시 플러시 작업을 시작하면 캐시가 클라이언트 요청 수락을 중지하고 개요 페이지의 캐시 상태가 **플러싱으로**변경됩니다.

캐시의 데이터는 적절한 저장소 대상에 저장됩니다. 플러시해야 하는 데이터의 양에 따라 프로세스는 몇 분 또는 1시간 이상 걸릴 수 있습니다.

모든 데이터가 저장소 대상에 저장되면 캐시가 자동으로 클라이언트 요청을 다시 가져오기 시작합니다. 캐시 상태가 **정상**으로 돌아갑니다.

## <a name="upgrade-cache-software"></a>캐시 소프트웨어 업그레이드

새 소프트웨어 버전을 사용할 수 있는 경우 **업그레이드** 버튼이 활성화됩니다. 또한 소프트웨어 업데이트에 대한 메시지가 페이지 상단에 표시됩니다.

![업그레이드 버튼이 활성화된 단추의 맨 위 행 스크린샷](media/hpc-cache-upgrade-button.png)

소프트웨어 업그레이드 중에 클라이언트 액세스가 중단되지 않지만 캐시 성능이 느려집니다. 사용량이 적은 시간 이나 계획된 유지 관리 기간에 소프트웨어를 업그레이드할 계획입니다.

소프트웨어 업데이트에는 몇 시간이 걸릴 수 있습니다. 처리량이 높은 캐시는 피크 처리량 값이 작은 캐시보다 업그레이드하는 데 시간이 오래 걸릴 수 있습니다.

소프트웨어 업그레이드를 사용할 수 있는 경우 수동으로 적용하려면 1주일 정도 가집니다. 종료 날짜가 업그레이드 메시지에 나열됩니다. 이 시간 동안 업그레이드하지 않으면 Azure에서 자동으로 업데이트를 캐시에 적용합니다. 자동 업그레이드 의 타이밍은 구성할 수 없습니다. 캐시 성능에 미치는 영향이 우려되는 경우 기간이 만료되기 전에 소프트웨어를 직접 업그레이드해야 합니다.

종료 날짜가 지나면 캐시가 중지되면 캐시는 다음에 시작할 때 자동으로 소프트웨어를 업그레이드합니다. (업데이트가 즉시 시작되지 않을 수도 있지만 첫 번째 시간에 시작됩니다.)

**업그레이드** 단추를 클릭하여 소프트웨어 업데이트를 시작합니다. 캐시 상태가 작업이 완료될 때까지 **업그레이드로** 변경됩니다.

## <a name="delete-the-cache"></a>캐시 삭제

**삭제** 단추는 캐시를 파괴합니다. 캐시를 삭제하면 모든 리소스가 소멸되고 더 이상 계정 요금이 발생하지 않습니다.

저장소 대상으로 사용되는 백 엔드 저장소 볼륨은 캐시를 삭제할 때 영향을 받지 않습니다. 나중에 나중에 추가하거나 별도로 해제할 수 있습니다.

> [!NOTE]
> Azure HPC 캐시는 캐시를 삭제하기 전에 캐시에서 백 엔드 저장소 시스템으로 변경된 데이터를 자동으로 쓰지 않습니다.
>
> 캐시의 모든 데이터가 장기 저장소에 기록되었는지 확인하려면 [캐시를](#stop-the-cache) 삭제하기 전에 캐시를 중지합니다. 삭제 단추를 클릭하기 전에 **중지된** 상태가 표시되는지 확인합니다.
<!--... written to long-term storage, follow this procedure:
>
> 1. [Remove](hpc-cache-edit-storage.md#remove-a-storage-target) each storage target from the Azure HPC Cache by using the delete button on the Storage targets page. The system automatically writes any changed data from the cache to the back-end storage system before removing the target.
> 1. Wait for the storage target to be completely removed. The process can take an hour or longer if there is a lot of data to write from the cache. When it is done, a portal notification says that the delete operation was successful, and the storage target disappears from the list.
> 1. After all affected storage targets have been deleted, it is safe to delete the cache.
>
> Alternatively, you can use the [flush](#flush-cached-data) option to save cached data, but there is a small risk of losing work if a client writes a change to the cache after the flush completes but before the cache instance is destroyed.-->

## <a name="cache-metrics-and-monitoring"></a>캐시 메트릭 및 모니터링

개요 페이지에는 캐시 처리량, 초당 작업 및 대기 시간등 몇 가지 기본 캐시 통계에 대한 그래프가 표시됩니다.

![샘플 캐시에 대해 위에서 언급한 통계를 보여주는 세 줄 그래프의 스크린샷](media/hpc-cache-overview-stats.png)

이러한 차트는 Azure의 기본 제공 모니터링 및 분석 도구의 일부입니다. 포털 사이드바의 **모니터링** 제목 아래의 페이지에서 추가 도구 및 경고를 사용할 수 있습니다. [Azure 모니터링 설명서의](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal)포털 섹션에서 자세히 알아봅니다.

## <a name="next-steps"></a>다음 단계

<!-- * Learn more about metrics and statistics for hpc cache -->
* [Azure 측정항목 및 통계 도구에](../azure-monitor/index.yml) 대해 자세히 알아보기
* [Azure HPC 캐시에 대한 도움말](hpc-cache-support-ticket.md) 받기
