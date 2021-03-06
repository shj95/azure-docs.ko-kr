---
title: HB 시리즈 - Azure 가상 시스템
description: HB 시리즈 VM용 사양.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 600f10e81742e9bb66c800b747fd7b2dc062754d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164834"
---
# <a name="hb-series"></a>HB 시리즈

HB 계열 VM은 유체 역학, 명시적 유한 요소 분석 및 날씨 모델링과 같은 메모리 대역폭에 의해 구동되는 애플리케이션에 최적화되어 있습니다. HB VM은 60AMD EPYC 7551 프로세서 코어, CPU 코어당 4GB의 RAM, 동시 멀티스레딩이 없습니다. HB VM은 최대 260GB/sec의 메모리 대역폭을 제공합니다.

ACU: 199-216

Premium Storage: 지원됨

Premium Storage 캐싱: 지원됨

라이브 마이그레이션: 지원되지 않음

업데이트 메모리 보존: 지원되지 않음

| 크기 | vCPU | 프로세서 | 메모리(GB) | 메모리 대역폭 GB/s | 기본 CPU 주파수(GHz) | 모든 코어 주파수(GHz, 피크) | 단일 코어 주파수(GHz, 피크) | RDMA 성능(Gb/s) | MPI 지원 | 임시 저장 장치(GB) | 최대 데이터 디스크 수 | 맥스 이더넷 NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | 모두 | 700 | 4 | 1 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>기타 크기

- [범용](sizes-general.md)
- [메모리 최적화](sizes-memory.md)
- [Storage에 최적화](sizes-storage.md)
- [GPU에 최적화](sizes-gpu.md)
- [고성능 컴퓨팅](sizes-hpc.md)
- [이전 세대](sizes-previous-gen.md)

## <a name="next-steps"></a>다음 단계

[ACU(Azure 컴퓨팅 단위)](acu.md)가 Azure SKU 간의 Compute 성능을 비교하는 데 어떻게 도움을 줄 수 있는지 알아봅니다.