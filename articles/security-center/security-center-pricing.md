---
title: Azure 보안 센터 계층의 가격 책정
description: Azure 보안 센터는 무료 및 표준의 두 계층으로 제공됩니다. 이 페이지에서는 무료에서 표준으로 업그레이드하는 방법을 보여 주며,
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2019
ms.author: memildin
ms.openlocfilehash: fd84058c8421d144678c91fac3e5671511d0fd4a
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435493"
---
# <a name="upgrade-to-standard-tier-for-enhanced-security"></a>보안 강화를 위해 표준 계층으로 업그레이드
Azure Security Center는 Azure, 온-프레미스 및 기타 클라우드용으로 통합 보안 관리 및 고급 위협 보호 기능을 제공합니다. 또한 하이브리드 클라우드 작업을 확인하고 제어하는 기능, 위협에 대한 노출을 줄이는 적극적인 방어 기능, 그리고 빠르게 발전하는 사이버 공격에 대응할 수 있는 지능형 검색 기능을 제공합니다.

## <a name="pricing-tiers"></a>가격 책정 계층
Security Center는 두 계층으로 제공됩니다.

- **프리** 티어는 Azure Portal의 Azure 보안 센터 대시보드를 처음 방문하거나 API를 통해 프로그래밍 방식으로 활성화된 경우 모든 Azure 구독에서 활성화됩니다. 프리 티어는 Azure 리소스를 보호하는 데 도움이 되는 보안 정책, 지속적인 보안 평가 및 실행 가능한 보안 권장 사항을 제공합니다.
- **표준** 계층의 경우 무료 계층의 기능이 프라이빗 클라우드 및 기타 퍼블릭 클라우드에서 실행되는 작업으로 확장 적용되며, 하이브리드 클라우드 작업 전반에 걸쳐 통합 보안 관리 및 위협 방지 기능이 제공됩니다. 또한 표준 계층에는 기본 제공 동작 분석 및 기계 학습을 사용하여 공격 및 제로 데이 익스플로잇, 액세스 및 응용 프로그램 제어를 식별하여 네트워크 공격 및 맬웨어에 대한 노출을 줄이는 위협 보호 기능도 추가됩니다. 또한 표준 계층은 가상 시스템에 대한 취약성 검색을 추가합니다. 당신은 무료로 표준 계층을 시도 할 수 있습니다. 보안 센터 표준은 VM, 가상 시스템 크기 집합, 앱 서비스, SQL 서버 및 저장소 계정을 포함한 Azure 리소스를 지원합니다. Azure 보안 센터 표준이 있는 경우 리소스 유형에 따라 지원을 옵트아웃할 수 있습니다. 

VM에 대한 대부분의 프리 티어 보안 평가와 많은 표준 계층 보안 경고에는 Log Analytics 에이전트 기능을 설치해야 합니다. 보안 센터에서 자동 프로비전을 활성화하여 Azure VM에 대한 에이전트를 자동으로 배포할 수 있습니다.

## <a name="try-standard-tier-free-for-30-days"></a>30일 동안 표준 티어 를 무료로 사용해 보십시오.
표준 등급은 처음 30일 동안 무료입니다. 30일 종료 시 서비스를 계속 사용하기로 선택하는 경우 사용량에 대한 요금이 자동으로 부과되기 시작합니다.

전체 Azure 구독을 표준 계층으로 업그레이드할 수 있으며, 이 계층은 구독 내의 모든 리소스에 의해 상속됩니다.

표준 계층을 얻으려면 다음을 수행하십시오.

1. **보안 센터** 주 메뉴에서 & **가격 설정을** 선택합니다.
2. 표준으로 업그레이드할 구독을 선택합니다.
3. **가격 책정 계층**을 선택합니다.
4. **표준**을 선택하여 업그레이드합니다.
5. **저장**을 클릭합니다.

[![Security Center 가격 책정](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> 모든 보안 센터 기능을 사용하려면 해당 가상 시스템이 포함된 구독에 표준 가격 책정 계층을 적용해야 합니다. 작업 영역에 대한 가격 책정을 구성해도 Azure 리소스에 대한 적시 VM 액세스, 적응형 응용 프로그램 제어 및 네트워크 검색을 사용할 수 없습니다.
>

## <a name="why-upgrade-to-standard"></a>왜 표준으로 업그레이드해야 합니까?
Security Center에서는 다음을 비롯하여 하이브리드 클라우드 작업을 위한 강화된 보안 및 위협 방지 기능을 제공합니다.

- **하이브리드 보안** – 모든 온-프레미스 및 클라우드 작업에 걸쳐 보안을 통합 확인할 수 있습니다. 또한 보안 정책을 적용하고 하이브리드 클라우드 작업의 보안을 지속적으로 평가하여 보안 표준을 준수할 수 있습니다. 방화벽 및 기타 파트너 솔루션을 비롯한 여러 소스에서 보안 데이터를 수집, 검색 및 분석합니다.
- **보안 경고** - 고급 분석 및 Microsoft 지능형 보안 그래프를 사용하여 진화하는 사이버 공격에 대한 우위를 확보합니다. 기본 제공 행동 분석 및 Machine Learning을 활용하여 공격 및 제로 데이 익스플로잇을 식별할 수 있습니다. 또한 네트워크, 컴퓨터 및 클라우스 서비스에서 들어오는 공격 및 위반 후 활동을 모니터링할 수 있습니다. 대화형 도구 및 상황에 맞는 위협 인텔리전스를 사용하면 조사를 손쉽게 수행할 수 있습니다.
- **가상 시스템에 대한 취약성 검사** - 취약성 관리를 위한 업계에서 가장 진보된 솔루션을 제공하는 모든 가상 시스템에 스캐너를 쉽게 배포할 수 있습니다. 보안 센터에서 직접 결과를 보고 조사하고 수정합니다. 
- **액세스 및 응용 프로그램 제어** - 특정 워크로드에 맞게 조정된 기계 학습 기반 화이트리스팅 권장 사항을 적용하여 맬웨어 및 기타 원치 않는 응용 프로그램을 차단합니다. Azure VM의 관리 포트에 대한 적시에 제어된 액세스를 통해 네트워크 공격 표면을 줄입니다. 이렇게 하면 무차별 암호 대입 및 기타 네트워크 공격에 대한 노출이 크게 줄어듭니다.
- **컨테이너 보안 기능** - 컨테이너화된 환경에서 취약성 관리 및 실시간 위협 보호의 이점을 누릴 수 있습니다. 컨테이너 레지스트리 리소스를 사용하도록 설정하면 모든 기능이 활성화될 때까지 최대 12시간이 걸릴 수 있습니다.


## <a name="next-steps"></a>다음 단계
이 문서에서는 Security Center의 가격 책정 방식에 대해 알아보았습니다. 표준 계층의 향상된 보안 및 고급 위협 보호에 대한 자세한 내용은 다음을 참조하십시오.

- [Azure 보안 센터의 위협 보호](threat-protection.md)
- [적시 VM 액세스 제어](security-center-just-in-time.md)
- [컨테이너 보안 개요](container-security.md)
- [선택한 통화로 된 가격 세부 정보 및 지역에 따라](https://azure.microsoft.com/pricing/details/security-center/)