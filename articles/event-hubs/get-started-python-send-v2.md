---
title: Python(최신)을 사용하여 Azure Event Hubs에서 이벤트 보내기 또는 받기
description: 이 문서에서는 최신 azure/eventhubs 버전 5 패키지를 사용하여 Azure Event Hubs와 이벤트를 주고 받는 Python 애플리케이션을 만드는 과정을 연습할 수 있습니다.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 7c971dcac702318d15a27736828092e987468ca3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77162976"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-python-azure-eventhub-version-5"></a>Python(azure-eventhub 버전 5)을 사용하여 이벤트 허브에서 이벤트 보내기 또는 받기
이 빠른 시작에서는 **azure-eventhubs 버전 5** Python 패키지를 사용하여 이벤트 허브와 이벤트를 주고 받는 방법을 보여줍니다.

> [!IMPORTANT]
> 이 빠른 시작에서는 최신 azure-eventhub 버전 5 패키지를 사용합니다. 이전 azure-eventhub 버전 1 패키지를 사용하는 빠른 시작은 [azure-eventhub 버전 1을 사용하여 이벤트 보내기 및 받기](event-hubs-python-get-started-send.md)를 참조하세요. 

## <a name="prerequisites"></a>사전 요구 사항
Azure Event Hubs를 처음 사용하는 경우 이 빠른 시작을 수행하기 전에 [Event Hubs 개요](event-hubs-about.md)를 참조하세요. 

이 빠른 시작을 완료하려면 다음 필수 구성 요소가 필요합니다.

- **Microsoft Azure 구독**. Azure Event Hubs를 비롯한 Azure 서비스를 사용하려면 구독이 필요합니다.  기존 Azure 계정이 없는 경우 [평가판](https://azure.microsoft.com/free/)에 가입하거나 [계정을 만들 때](https://azure.microsoft.com) MSDN 구독자 혜택을 사용할 수 있습니다.
- PIP가 설치 및 업데이트된 Python 2.7 또는 3.5 이상.
- Event Hubs용 Python 패키지. 

    이 패키지를 설치하려면 경로에 Python이 있는 명령 프롬프트에서 다음 명령을 실행합니다.

    ```cmd
    pip install azure-eventhub
    ```

    Azure Blob 스토리지를 검사점 저장소로 사용하여 이벤트를 수신하기 위한 다음 패키지를 설치합니다.

    ```cmd
    pip install azure-eventhub-checkpointstoreblob-aio
    ```
- **Event Hubs 네임스페이스 및 이벤트 허브 만들기** 첫 번째 단계에서는 [Azure Portal](https://portal.azure.com)을 사용하여 Event Hubs 형식의 네임스페이스를 만들고 애플리케이션에서 Event Hub와 통신하는 데 필요한 관리 자격 증명을 얻습니다. 네임스페이스 및 이벤트 허브를 만들려면 [이 문서](event-hubs-create.md)의 절차를 따릅니다. 그리고 다음 문서의 지침에 따라 **Event Hubs 네임스페이스에 대한 연결 문자열**을 가져옵니다. [연결 문자열 가져오기](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) 이 빠른 시작의 뒷부분에서 연결 문자열을 사용합니다.

## <a name="send-events"></a>이벤트 보내기
이 섹션에서는 이전에 만든 이벤트 허브에 이벤트를 전송하는 Python 스크립트를 만듭니다.

1. 선호하는 Python 편집기(예: [Visual Studio Code](https://code.visualstudio.com/))를 엽니다.
2. *send.py*라는 스크립트를 만듭니다. 이 스크립트는 이전에 만든 이벤트 허브에 이벤트 일괄 처리를 보냅니다.
3. 다음 코드를 *send.py*에 붙여넣습니다.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubProducerClient
    from azure.eventhub import EventData

    async def run():
        # Create a producer client to send messages to the event hub.
        # Specify a connection string to your event hubs namespace and
            # the event hub name.
        producer = EventHubProducerClient.from_connection_string(conn_str="EVENT HUBS NAMESPACE - CONNECTION STRING", eventhub_name="EVENT HUB NAME")
        async with producer:
            # Create a batch.
            event_data_batch = await producer.create_batch()

            # Add events to the batch.
            event_data_batch.add(EventData('First event '))
            event_data_batch.add(EventData('Second event'))
            event_data_batch.add(EventData('Third event'))

            # Send the batch of events to the event hub.
            await producer.send_batch(event_data_batch)

    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())

    ```

    > [!NOTE]
    > 정보 주석을 비롯한 전체 소스 코드는 [GitHub send_async.py 페이지](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/send_async.py)로 이동하세요.

## <a name="receive-events"></a>이벤트 수신
이 빠른 시작에서는 검사점 저장소로 Azure Blob 스토리지를 사용합니다. 검사점 저장소는 검사점(마지막 읽기 위치)을 유지하는 데 사용됩니다.  

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure 스토리지 계정 및 blob 컨테이너 만들기
다음 단계를 수행하여 Azure 스토리지 계정 및 blob 컨테이너를 만듭니다.

1. [Azure Storage 계정 만들기](../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Blob 컨테이너 만들기](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [스토리지 계정에 대한 연결 문자열 가져오기](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

수신 코드에서 나중에 사용할 수 있도록 연결 문자열과 컨테이너 이름을 기록해 두어야 합니다.


### <a name="create-a-python-script-to-receive-events"></a>이벤트를 수신하는 Python 스크립트 만들기

이 섹션에서는 이벤트 허브에서 이벤트를 수신하는 Python 스크립트를 만듭니다.

1. 선호하는 Python 편집기(예: [Visual Studio Code](https://code.visualstudio.com/))를 엽니다.
2. *recv.py*라는 스크립트를 만듭니다.
3. 다음 코드를 *recv.py*에 붙여넣습니다.

    ```python
    import asyncio
    from azure.eventhub.aio import EventHubConsumerClient
    from azure.eventhub.extensions.checkpointstoreblobaio import BlobCheckpointStore


    async def on_event(partition_context, event):
        # Print the event data.
        print("Received the event: \"{}\" from the partition with ID: \"{}\"".format(event.body_as_str(encoding='UTF-8'), partition_context.partition_id))

        # Update the checkpoint so that the program doesn't read the events
        # that it has already read when you run it next time.
        await partition_context.update_checkpoint(event)

    async def main():
        # Create an Azure blob checkpoint store to store the checkpoints.
        checkpoint_store = BlobCheckpointStore.from_connection_string("AZURE STORAGE CONNECTION STRING", "BLOB CONTAINER NAME")

        # Create a consumer client for the event hub.
        client = EventHubConsumerClient.from_connection_string("EVENT HUBS NAMESPACE CONNECTION STRING", consumer_group="$Default", eventhub_name="EVENT HUB NAME", checkpoint_store=checkpoint_store)
        async with client:
            # Call the receive method.
            await client.receive(on_event=on_event)

    if __name__ == '__main__':
        loop = asyncio.get_event_loop()
        # Run the main method.
        loop.run_until_complete(main())    
    ```

    > [!NOTE]
    > 추가 정보 주석을 비롯한 전체 소스 코드는 [GitHub recv_with_checkpoint_store_async.py 페이지](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/samples/async_samples/recv_with_checkpoint_store_async.py)로 이동하세요.


### <a name="run-the-receiver-app"></a>받는 사람 앱 실행

이 스크립트를 실행하려면 해당 경로에 Python이 있는 명령 프롬프트를 열고 다음 명령을 실행합니다.

```bash
python recv.py
```

### <a name="run-the-sender-app"></a>보낸 사람 앱 실행

이 스크립트를 실행하려면 해당 경로에 Python이 있는 명령 프롬프트를 열고 다음 명령을 실행합니다.

```bash
python send.py
```

받는 사람 창에 이벤트 허브로 전송된 메시지가 표시됩니다.


## <a name="next-steps"></a>다음 단계
이 빠른 시작에서는 이벤트를 비동기적으로 보내고 받았습니다. 이벤트를 동기적으로 보내고 받는 방법을 알아보려면 [GitHub sync_samples 페이지](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples/sync_samples)로 이동하세요.

GitHub의 모든 샘플(동기 및 비동기)은 [Python 샘플에 대한 Azure Event Hubs 클라이언트 라이브러리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhub/samples)로 이동합니다.
