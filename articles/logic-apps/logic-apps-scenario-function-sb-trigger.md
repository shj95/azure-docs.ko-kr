---
title: Azure Functions를 사용하여 논리 앱 호출
description: Azure Service Bus를 수신 대기하여 논리 앱을 호출하거나 트리거하는 Azure 함수 만들기
services: logic-apps
ms.suite: integration
ms.reviewer: jehollan, klam, logicappspm
ms.topic: article
ms.date: 11/08/2019
ms.openlocfilehash: afd2735bae2a79ad942c347219019ef200b61070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75428716"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Azure Function 및 Azure 서비스 버스를 사용하여 논리 앱을 호출하거나 트리거합니다.

[Azure Functions를](../azure-functions/functions-overview.md) 사용하여 장기 실행 리스너 또는 작업을 배포해야 할 때 논리 앱을 트리거할 수 있습니다. 예를 들어 [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) 큐에서 수신 대기하고 논리 앱을 푸시 트리거로 즉시 트리거하는 Azure 함수를 만들 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 Azure 구독이 없는 경우 [체험 Azure 계정에 등록](https://azure.microsoft.com/free/)합니다.

* Azure 서비스 버스 네임스페이스입니다. 네임스페이스가 없는 경우 [먼저 네임스페이스를 만듭니다.](../service-bus-messaging/service-bus-create-namespace-portal.md)

* Azure 함수에 대 한 컨테이너는 Azure 함수 응용 프로그램입니다. 함수 앱이 없는 경우 [함수 앱을 먼저 만들고](../azure-functions/functions-create-first-azure-function.md).NET을 런타임 스택으로 선택해야 합니다.

* [논리 앱을 만드는 방법에](../logic-apps/quickstart-create-first-logic-app-workflow.md) 대한 기본 지식

## <a name="create-logic-app"></a>논리 앱 만들기

이 시나리오에서는 트리거하려는 각 논리 앱을 실행하는 함수가 있습니다. 먼저 HTTP 요청 트리거로 시작하는 논리 앱을 만듭니다. 이 함수는 큐 메시지를 수신할 때마다 엔드포인트를 호출합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인하고, 빈 논리 앱을 만듭니다.

   논리 앱을 처음 접하는 경우 [빠른 시작: 첫 번째 논리 앱 만들기](../logic-apps/quickstart-create-first-logic-app-workflow.md)를 검토하세요.

1. 검색 상자에 `http request`를 입력합니다. 트리거 목록에서 **HTTP 요청이 수신된 경우 트리거를** 선택합니다.

   ![트리거 선택](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   요청 트리거를 사용하면 선택적으로 JSON 스키마를 입력하여 큐 메시지와 함께 사용할 수 있습니다. JSON 스키마는 논리 앱 디자이너가 입력 데이터의 구조를 이해하고 워크플로에서 출력을 더 쉽게 사용할 수 있도록 도와줍니다.

1. 스키마를 지정하려면 다음과 같이 **요청 본문 JSON 스키마** 상자에 스키마를 입력합니다.

   ![JSON 스키마 지정](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   스키마는 없지만 JSON 형식의 샘플 페이로드가 있는 경우 해당 페이로드로 스키마를 생성할 수 있습니다.

   1. 요청 트리거에서 **샘플 페이로드를 사용하여 스키마 생성**을 선택합니다.

   1. **샘플 JSON 페이로드를 입력하거나 붙여넣기**에서 샘플 페이로드를 입력한 다음 **완료를 선택합니다.**

      ![샘플 페이로드 입력](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   이 샘플 페이로드는 트리거에 표시되는 이 스키마를 생성합니다.

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. 큐 메시지를 받은 후 실행하려는 다른 작업을 추가합니다.

   예를 들어 Office 365 Outlook 커넥터를 사용하여 이메일을 보낼 수 있습니다.

1. 논리 앱을 저장합니다. 그러면 이 논리 앱의 트리거에 대한 콜백 URL이 생성됩니다. 나중에 Azure 서비스 버스 큐 트리거에 대 한 코드에서이 콜백 URL을 사용 합니다.

   콜백 URL은 **HTTP POST URL** 속성에 나타납니다.

   ![트리거에 대해 생성된 콜백 URL](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure 함수 만들기

다음으로 트리거로 작동하고 큐에 수신 대기하는 함수를 만들겠습니다.

1. 아직 함수 앱을 열지 않았으면 Azure Portal에서 함수 앱을 열고 확장합니다. 

1. 함수 앱 이름 아래에서 **Functions**를 확장합니다. **함수** 창에서 **새 기능을**선택합니다.

   !["기능"을 확장하고 "새 기능"을 선택합니다.](./media/logic-apps-scenario-function-sb-trigger/add-new-function-to-function-app.png)

1. .NET을 런타임 스택으로 선택한 새 함수 앱을 만들었는지 또는 기존 함수 앱을 사용하고 있는지 여부에 따라 이 템플릿을 선택합니다.

   * 새 함수 앱의 경우 이 템플릿: **서비스 버스 큐 트리거를** 선택합니다.

     ![새 함수 앱용 템플릿 선택](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * 기존 함수 앱의 경우 이 템플릿을 선택합니다: **서비스 버스 큐 트리거 - C#**

     ![기존 함수 앱에 대한 템플릿 선택](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. Azure **Service Bus Queue 트리거** 창에서 트리거에 대한 이름을 제공하고 Azure Service Bus SDK `OnMessageReceive()` 수신기를 사용하는 큐에 대한 **서비스 버스 연결을** 설정하고 **에서 만들기를**선택합니다.

1. 큐 메시지를 트리거로 사용하여 이전에 만든 논리 앱 끝점을 호출하는 기본 함수를 작성합니다. 함수를 작성하기 전에 다음 사항을 검토합니다.

   * 이 예제에서는 `application/json` 메시지 콘텐츠 형식을 사용하지만, 필요에 따라 형식을 변경할 수 있습니다.
   
   * 가능한 동시에 실행 중인 함수, 대량 볼륨 또는 과부하로 인해 `using` [HTTPClient 클래스를](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) 명령문으로 인스턴스화하고 요청당 HTTPClient 인스턴스를 직접 만들지 마십시오. 자세한 내용은 [HttpClientFactory 사용을 참조하여 복원력 있는 HTTP 요청을 구현합니다.](https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests#issues-with-the-original-httpclient-class-available-in-net-core)
   
   * 가능하면 HTTP 클라이언트의 인스턴스를 다시 사용하십시오. 자세한 내용은 [Azure Functions의 연결 관리를](../azure-functions/manage-connections.md)참조하십시오.

   이 예제에서는 [ `Task.Run` ](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run) [비동기](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async) 모드에서 메서드를 사용합니다. 자세한 내용은 [비동기 프로그래밍을 참조하고 대기합니다.](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/)

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Can also fetch from App Settings or environment variable
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible: https://docs.microsoft.com/azure/azure-functions/manage-connections
   private static HttpClient httpClient = new HttpClient();

   public static async Task Run(string myQueueItem, TraceWriter log) 
   {
      log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
      var response = await httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")); 
   }
   ```

1. 함수를 테스트하기 위해 [Service Bus 탐색기](https://github.com/paolosalvatori/ServiceBusExplorer) 같은 도구를 사용하여 큐 메시지를 추가합니다.

   함수가 메시지를 받는 즉시 논리 앱이 트리거됩니다.

## <a name="next-steps"></a>다음 단계

* [HTTP 끝점을 사용하여 워크플로를 호출, 트리거 또는 중첩](../logic-apps/logic-apps-http-endpoint.md)
