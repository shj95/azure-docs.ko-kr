---
title: Azure 포털 - Azure IoT Edge에서 대규모 모듈 배포
description: Azure Portal을 사용하여 IoT Edge 디바이스 그룹에 대한 자동 배포 만들기
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a20ea4236683e26c51bc75309435c65e24271d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271435"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Azure Portal을 사용하여 대규모 IoT Edge 모듈 배포 및 모니터링

Azure 포털에서 **IoT Edge 자동 배포를** 만들어 여러 장치에 대해 한 번에 진행 중인 배포를 관리합니다. IoT Edge를 위한 자동 배포는 IoT Hub의 [자동 장치 관리](/azure/iot-hub/iot-hub-automatic-device-management) 기능의 일부입니다. 배포는 여러 장치에 여러 모듈을 배포하고, 모듈의 상태 및 상태를 추적하고, 필요한 경우 변경할 수 있는 동적 프로세스입니다.

자세한 내용은 [단일 장치 또는 규모에 대한 IoT Edge 자동 배포 이해를](module-deployment-monitoring.md)참조하십시오.

## <a name="identify-devices-using-tags"></a>태그를 사용하여 디바이스 식별

배포를 만들려면 먼저 적용할 디바이스를 지정할 수 있어야 합니다. Azure IoT Edge는 디바이스 쌍의 **태그**를 사용하여 디바이스를 식별합니다. 각 장치에는 솔루션에 적합한 방식으로 정의하는 여러 태그가 있을 수 있습니다.

예를 들어 스마트 빌딩의 캠퍼스를 관리하는 경우 장치에 위치, 룸 유형 및 환경 태그를 추가할 수 있습니다.

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

디바이스 쌍 및 태그에 대한 자세한 내용은 [IoT Hub의 디바이스 쌍 이해 및 사용](../iot-hub/iot-hub-devguide-device-twins.md)을 참조하세요.

## <a name="create-a-deployment"></a>배포 만들기

IoT Edge는 시나리오를 사용자 지정하는 데 사용할 수 있는 두 가지 유형의 자동 배포를 제공합니다. 해당 시스템 런타임 모듈과 추가 모듈 및 경로를 포함하는 표준 *배포를*만들 수 있습니다. 각 장치는 하나의 배포만 적용할 수 있습니다. 또는 시스템 런타임이 아닌 사용자 지정 모듈 및 경로만 포함하는 *계층화된 배포를*만들 수 있습니다. 여러 계층화된 배포는 표준 배포 외에도 장치에서 결합할 수 있습니다. 두 가지 유형의 자동 배포가 함께 작동하는 방식에 대한 자세한 내용은 [단일 장치 또는 규모에 대한 IoT Edge 자동 배포 이해를](module-deployment-monitoring.md)참조하십시오.

배포 및 계층화된 배포를 만드는 단계는 매우 유사합니다. 모든 차이점은 다음 단계에서 호출됩니다.

1. Azure [포털에서](https://portal.azure.com)IoT 허브로 이동합니다.
1. 왼쪽 창의 메뉴에서 **자동 장치 관리에서** **IoT 가장자리를** 선택합니다.
1. 위쪽 막대에서 **배포 만들기** 또는 **계층화된 배포 만들기를**선택합니다.

배포를 만드는 데에는 5개 단계가 있습니다. 다음 섹션에서는 각 단계로 안내합니다.

### <a name="step-1-name-and-label"></a>1단계: 이름 및 레이블

1. 배포에 최대 128자의 소문자로 된 고유한 이름을 지정합니다. 공백과 잘못된 문자(`& ^ [ ] { } \ | " < > /`)는 사용하지 않도록 합니다.
1. 배포 추적에 도움이 되도록 레이블을 키-값 쌍으로 추가할 수 있습니다. 예를 들어 **HostPlatform** 및 **Linux** 또는 **Version** 및 **3.0.1**입니다.
1. **다음: 모듈을** 선택하여 2단계로 이동합니다.

### <a name="step-2-modules"></a>2단계: 모듈

배포에 최대 20개의 모듈을 추가할 수 있습니다. 모듈이 없는 배포를 만들면 대상 장치에서 현재 모듈이 제거됩니다.

배포에서 IoT Edge 에이전트 및 IoT Edge 허브 모듈의 설정을 관리할 수 있습니다. **런타임 설정을** 선택하여 두 런타임 모듈을 구성합니다. 계층화된 배포에서는 런타임 모듈이 포함되지 않으므로 구성할 수 없습니다.

세 가지 유형의 모듈을 추가할 수 있습니다.

* IoT 에지 모듈
* 마켓플레이스 모듈
* Azure 스트림 분석 모듈

#### <a name="add-an-iot-edge-module"></a>IoT 에지 모듈 추가

사용자 지정 코드를 모듈로 추가하거나 Azure 서비스 모듈을 수동으로 추가하려면 다음 단계를 수행합니다.

1. 페이지의 **컨테이너 레지스트리 자격 증명** 섹션에서 이 배포에 대한 모듈 이미지가 포함된 개인 컨테이너 레지스트리의 이름과 자격 증명을 제공합니다. Docker 이미지에 대한 컨테이너 레지스트리 자격 증명을 찾을 수 없는 경우 IoT Edge 에이전트는 오류 500을 보고합니다.
1. 페이지의 **IoT Edge 모듈** 섹션에서 **추가**를 클릭합니다.
1. 드롭다운 메뉴에서 **IoT 에지 모듈을** 선택합니다.
1. 모듈에 **IoT 에지 모듈 이름을**지정합니다.
1. **이미지 URI** 필드에 대해 모듈의 컨테이너 이미지를 입력합니다.
1. 드롭다운 메뉴를 사용하여 **다시 시작 정책**을 선택합니다. 다음 옵션 중에서 선택합니다.
   * **항상** - 어떤 이유로든 모듈이 종료되면 항상 다시 시작됩니다.
   * **never** - 어떤 이유로든 모듈이 종료되면 모듈이 다시 시작되지 않습니다.
   * **오류 시** - 모듈이 충돌하면 다시 시작되지만 닫히면 다시 시작됩니다.
   * **비정상** 상태 - 비정상 상태가 충돌하거나 반환되는 경우 모듈이 다시 시작됩니다. 상태 기능을 구현하는 것은 각 모듈에 달려 있습니다.
1. 드롭다운 메뉴를 사용하여 모듈에 대한 **원하는 상태**를 선택합니다. 다음 옵션 중에서 선택합니다.
   * **실행** 중 - 실행이 기본 옵션입니다. 모듈이 배포된 직후에 실행됩니다.
   * **중지됨** - 배포된 후 모듈은 귀하 또는 다른 모듈에서 시작하도록 요청될 때까지 유휴 상태로 유지됩니다.
1. 컨테이너에 전달되어야 하는 **컨테이너 만들기 옵션**을 지정합니다. 자세한 내용은 [docker create](https://docs.docker.com/engine/reference/commandline/create/)를 참조하세요.
1. **모듈 트윈에** 태그 또는 기타 속성을 추가하려면 모듈 트윈 설정을 선택합니다.
1. 이 모듈에 대한 **환경 변수**를 입력합니다. 환경 변수는 모듈에 구성 정보를 제공합니다.
1. 모듈을 배포에 추가하려면 **추가를** 선택합니다.

#### <a name="add-a-module-from-the-marketplace"></a>마켓플레이스에서 모듈 추가

Azure 마켓플레이스에서 모듈을 추가하려면 다음 단계를 따르십시오.

1. 페이지의 **IoT Edge 모듈** 섹션에서 **추가**를 클릭합니다.
1. 드롭다운 메뉴에서 **마켓플레이스 모듈을** 선택합니다.
1. **IoT 에지 모듈 마켓플레이스** 페이지에서 모듈을 선택합니다. 선택한 모듈은 구독, 리소스 그룹 및 장치에 대해 자동으로 구성됩니다. 그런 다음 IoT Edge 모듈 목록에 나타납니다. 일부 모듈은 추가 구성이 필요할 수 있습니다. 자세한 내용은 [Azure 마켓플레이스에서 모듈 배포를](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace)참조하십시오.

#### <a name="add-a-stream-analytics-module"></a>스트림 분석 모듈 추가

Azure Stream Analytics에서 모듈을 추가하려면 다음 단계를 수행합니다.

1. 페이지의 **IoT Edge 모듈** 섹션에서 **추가**를 클릭합니다.
1. 드롭다운 메뉴에서 **Azure 스트림 분석 모듈을** 선택합니다.
1. 오른쪽 창에서 **구독을**선택합니다.
1. IoT **에지 작업을**선택합니다.
1. **저장**을 선택하여 모듈을 배포에 추가합니다.

#### <a name="configure-module-settings"></a>모듈 설정 구성

배포에 모듈을 추가한 후 해당 이름을 선택하여 **IoT Edge 모듈 업데이트** 페이지를 열 수 있습니다. 이 페이지에서 모듈 설정, 환경 변수, 만들기 옵션 및 모듈 쌍편집을 할 수 있습니다. 마켓플레이스에서 모듈을 추가한 경우 이러한 매개 변수 중 일부가 이미 채워져 있을 수 있습니다.

계층화된 배포를 만드는 경우 동일한 장치를 대상으로 하는 다른 배포에 있는 모듈을 구성할 수 있습니다. 다른 버전을 덮어 쓰지 않고 모듈 트윈을 업데이트하려면 모듈 트윈 **Module Twin Property** **설정** 탭을 엽니다. `properties.desired.settings` `properties.desired` 필드 내에서 속성을 정의하는 경우 우선 순위가 낮은 배포에 정의된 모듈에 대해 원하는 속성을 덮어씁니다.

![계층화된 배포를 위한 모듈 트윈 속성 설정](./media/how-to-deploy-monitor/module-twin-property.png)

계층화된 배포의 모듈 쌍 구성에 대한 자세한 내용은 [계층화된 배포](module-deployment-monitoring.md#layered-deployment)를 참조하십시오.

배포에 대한 모든 모듈이 구성된 후에는 **다음: 경로에서** 3단계로 이동합니다.

### <a name="step-3-routes"></a>3단계: 경로

경로는 배포 내 모듈 간에 서로 통신하는 방식을 정의합니다. 기본적으로 마법사는 **업스트림이라는** 경로를 제공하고 **from\* /messages/INTO $upstream**정의하므로 모든 모듈에서 출력하는 모든 메시지가 IoT hub로 전송됩니다.  

[경로 선언](module-composition.md#declare-routes)의 정보를 포함한 경로를 추가하거나 업데이트한 다음, **다음**을 선택하여 검토 섹션을 진행합니다.

**다음: 메트릭을 선택합니다.**

### <a name="step-4-metrics"></a>4단계: 메트릭

메트릭은 디바이스가 구성 콘텐츠를 적용한 결과로 다시 보고할 수 있는 다양한 상태의 요약 수를 제공합니다.

1. **메트릭 이름에**대한 이름을 입력합니다.

1. **메트릭 조건**에 대한 쿼리를 입력합니다. 쿼리는 IoT Edge 허브 모듈 쌍의 [보고된 속성](module-edgeagent-edgehub.md#edgehub-reported-properties)을 기반으로 합니다. 메트릭은 쿼리에 의해 반환되는 행 수를 나타냅니다.

   예를 들어:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

**다음 을 선택합니다: 대상 장치.**

### <a name="step-5-target-devices"></a>5단계: 대상 장치

디바이스의 tags 속성을 사용하여 이 배포를 받아야 하는 특정 디바이스를 대상으로 지정합니다.

여러 배포에서 동일한 디바이스를 대상으로 할 수 있으므로 각 배포에 우선 순위 번호를 부여해야 합니다. 충돌이 발생하면 우선 순위가 가장 높은 배포(값이 클수록 우선 순위가 높음을 나타)가 승리합니다. 두 배포의 우선 순위 번호가 동일하면 가장 최근에 만든 배포가 먼저 적용됩니다.

여러 배포가 동일한 장치를 대상으로 하는 경우 우선 순위가 높은 장치만 적용됩니다. 여러 계층화된 배포가 동일한 장치를 대상으로 하는 경우 모두 적용됩니다. 그러나 이름이 같은 두 개의 경로가 있는 경우와 같이 속성이 중복되면 우선 순위가 높은 계층화 배포의 경로가 나머지를 덮어씁니다.

장치를 대상으로 하는 계층화된 배포는 적용하기 위해 기본 배포보다 우선 순위가 높아야 합니다.

1. 배포 **우선 순위**에 대해 양의 정수를 입력합니다.
1. **대상 조건**을 입력하여 이 배포의 대상으로 지정할 디바이스를 결정합니다.조건은 디바이스 쌍 태그 또는 보고되는 디바이스 쌍 속성을 기반으로 하며, 표현 형식이 일치해야 합니다.예를 들어 `tags.environment='test'` 또는 `properties.reported.devicemodel='4000x'`입니다.

**다음: 검토 + 만들기를** 선택하여 최종 단계로 이동합니다.

### <a name="step-6-review-and-create"></a>6단계: 검토 및 만들기

배포 정보를 검토한 다음 **을 선택합니다.**

## <a name="monitor-a-deployment"></a>배포 모니터링

배포의 세부 정보를 확인하고 이를 실행하는 디바이스를 모니터링하려면 다음 단계를 사용합니다.

1. [Azure 포털에](https://portal.azure.com) 로그인하고 IoT Hub로 이동합니다.
1. **IoT Edge**를 선택합니다.
1. **IoT 에지 배포 탭을 선택합니다.**

   ![IoT Edge 배포 보기](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. 배포 목록을 검사합니다.각 배포에 대해 다음 세부 정보를 볼 수 있습니다.
   * **ID** - 배포의 이름
   * **유형** - 배포 **유형(배포** 또는 **계층화된 배포)**
   * **대상 조건** - 대상 장치를 정의하는 데 사용되는 태그입니다.
   * **우선 순위** - 배포에 할당된 우선 순위 번호
   * **대상 시스템 메트릭대상은** - **Targeted** 대상 조건과 일치하는 IoT Hub의 장치 쌍 수를 지정하고, **Applied는** 배포 콘텐츠가 IoT Hub의 모듈 쌍에 적용된 장치 수를 지정합니다.
   * **장치 메트릭** - IoT Edge 클라이언트 런타임의 성공 또는 오류를 보고하는 배포의 IoT Edge 장치 수입니다.
   * **사용자 지정 메트릭** - 배포에 대해 정의한 모든 메트릭에 대한 배포 보고 데이터의 IoT Edge 장치 수입니다.
   * **생성 시간** - 배포를 만들 때의 타임스탬프입니다. 이 타임스탬프는 두 배포의 우선 순위가 동일한 경우 연결을 중단하는 데 사용됩니다.
1. 모니터링하려는 배포를 선택합니다.  
1. 배포 세부 정보를 검사합니다. 탭을 사용하여 배포의 세부 내용을 검토할 수 있습니다.

## <a name="modify-a-deployment"></a>배포 수정

배포를 수정하면 변경 내용이 모든 대상 디바이스에 즉시 복제됩니다.

대상 조건을 업데이트하면 다음과 같은 업데이트가 발생합니다.

* 디바이스에서 이전 대상 조건을 충족하지 않았지만 새 대상 조건은 충족하고 이 배포가 해당 디바이스에 대해 가장 높은 순위이면 이 배포가 디바이스에 적용됩니다.
* 이 배포를 현재 실행 중인 디바이스에서 더 이상 대상 조건을 충족하지 않으면 이 배포가 제거되고 다음으로 우선 순위가 가장 높은 배포가 적용됩니다.
* 이 배포를 현재 실행 중인 디바이스에서 더 이상 대상 조건을 충족하지 않고 다른 배포의 대상 조건도 충족하지 않으면 디바이스에서 변경되지 않습니다. 디바이스는 현재 모듈을 현재 상태로 계속 실행하지만 더 이상 이 배포의 일부로 관리되지 않습니다. 다른 배포의 대상 조건을 충족하면 이 배포를 제거하고 새 배포가 적용됩니다.

배포를 수정하려면 다음 단계를 수행합니다.

1. [Azure 포털에](https://portal.azure.com) 로그인하고 IoT Hub로 이동합니다.
1. **IoT Edge**를 선택합니다.
1. **IoT 에지 배포 탭을 선택합니다.**

   ![IoT Edge 배포 보기](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. 수정하려는 배포를 선택합니다.
1. 다음 탭에서 업데이트합니다.
   * **대상 조건**
   * **측정항목** - 정의한 측정항목을 수정 또는 삭제하거나 새 측정항목을 추가할 수 있습니다.
   * **레이블**
   * **모듈**
   * **경로**
   * **배포**

1. **저장**을 선택합니다.
1. [배포 모니터링](#monitor-a-deployment)의 단계에 따라 변경 내용이 배포되는지 확인합니다.

## <a name="delete-a-deployment"></a>배포 삭제

배포를 삭제하면 배포된 모든 장치가 다음으로 우선 순위가 높은 배포를 수행합니다. 디바이스에서 다른 배포의 대상 조건을 충족하지 않으면 배포를 삭제해도 모듈이 제거되지 않습니다.

1. [Azure 포털에](https://portal.azure.com) 로그인하고 IoT Hub로 이동합니다.
1. **IoT Edge**를 선택합니다.
1. **IoT 에지 배포 탭을 선택합니다.**

   ![IoT Edge 배포 보기](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. 확인란을 사용하여 삭제하려는 배포를 선택합니다.
1. **삭제를 선택합니다.**
1. 이 작업으로 이 배포가 삭제되고 모든 디바이스에 대한 이전 상태로 되돌릴 것임을 알리는 메시지가 표시됩니다.즉 우선 순위가 낮은 배포가 적용됩니다.다른 배포가 대상으로 지정되지 않으면 모듈이 제거되지 않습니다. 디바이스에서 모든 모듈을 제거하려는 경우 모듈이 없는 배포를 만들어서 동일한 디바이스에 배포합니다.**예**를 선택하여 계속합니다.

## <a name="next-steps"></a>다음 단계

[IoT Edge 장치에 모듈 배포에](module-deployment-monitoring.md)대해 자세히 알아보십시오.
