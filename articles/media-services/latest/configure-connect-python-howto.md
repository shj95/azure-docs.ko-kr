---
title: Azure 미디어 서비스 v3 API에 연결 - 파이썬
description: 이 문서에서는 파이썬을 사용하여 미디어 서비스 v3 API에 연결하는 방법을 보여 줍니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 98a8cdf4120cf56184eb5735249640e3423acdf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74888464"
---
# <a name="connect-to-media-services-v3-api---python"></a>미디어 서비스 v3 API에 연결 - 파이썬

이 문서에서는 서비스 주체 로그인 방법을 사용하여 Azure 미디어 서비스 v3 Python SDK에 연결하는 방법을 보여 주며 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

- [python.org](https://www.python.org/downloads/) 파이썬 다운로드
- 환경 변수를 `PATH` 설정해야 합니다.
- [Media Services 계정 만들기](create-account-cli-how-to.md) 리소스 그룹 이름과 Media Services 계정 이름을 기억해야 합니다.
- [액세스 API](access-api-cli-how-to.md) 항목의 단계를 따릅니다. 이후 단계에서 필요한 구독 ID, 응용 프로그램 ID(클라이언트 ID), 인증 키(비밀) 및 테넌트 ID를 기록합니다.

> [!IMPORTANT]
> [명명 규칙을 검토합니다.](media-services-apis-overview.md#naming-conventions)

## <a name="install-the-modules"></a>모듈 설치

파이썬을 사용하여 Azure 미디어 서비스와 함께 작동하려면 이러한 모듈을 설치해야 합니다.

* Active `azure-mgmt-resource` Directory에 대한 Azure 모듈을 포함하는 모듈입니다.
* 미디어 `azure-mgmt-media` 서비스 엔터티를 포함하는 모듈입니다.

명령줄 도구를 열고 다음 명령을 사용하여 모듈을 설치합니다.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==1.1.1
```

## <a name="connect-to-the-python-client"></a>파이썬 클라이언트에 연결

1. `.py` 확장이 있는 파일 만들기
1. 즐겨찾는 편집기에서 파일 열기
1. 파일에 다음 코드를 추가합니다. 코드는 필요한 모듈을 가져오고 미디어 서비스에 연결하는 데 필요한 Active Directory 자격 증명 개체를 만듭니다.

      변수의 값을 [액세스 API에서](access-api-cli-how-to.md) 얻은 값으로 설정합니다.

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. 파일 실행

## <a name="next-steps"></a>다음 단계

- [Python SDK](https://aka.ms/ams-v3-python-sdk)를 사용합니다.
- Media Services [Python 참조](https://aka.ms/ams-v3-python-ref) 설명서를 검토하세요.
