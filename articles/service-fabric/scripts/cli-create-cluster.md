---
title: Azure CLI 스크립트 배포 샘플
description: Azure CLI(명령줄 인터페이스)를 사용하여 Azure에서 안전한 Service Fabric Linux 클러스터를 만드는 방법입니다.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 2ef8f322ff17eeb5d75d3cc8e4f8604f02d4ef0e
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366537"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Azure에서 안전한 Service Fabric Linux 클러스터 만들기

이 명령은 자체 서명된 인증서를 만들고, 키 자격 증명 모음에 추가하고, 해당 인증서를 로컬로 다운로드합니다.  새 인증서는 배포 시 클러스터를 보호하는 데 사용됩니다.  또한 새 인증서를 만드는 대신 기존 인증서를 사용할 수도 있습니다.  어쨌든 인증서의 주체 이름은 Service Fabric 클러스터에 액세스하는 데 사용하는 도메인과 일치해야 합니다. 이러한 일치는 클러스터의 HTTPS 관리 엔드포인트 및 Service Fabric Explorer에 TLS를 제공하는 데 필요합니다. `.cloudapp.azure.com` 도메인의 CA에서 TLS/SSL 인증서를 가져올 수 없습니다. 클러스터에 대한 사용자 지정 도메인 이름을 획득해야 합니다. CA에서 인증서를 요청하는 경우 인증서의 주체 이름이 클러스터에 사용되는 사용자 지정 도메인 이름과 일치해야 합니다.

필요한 경우 [Azure CLI](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)를 설치합니다.

## <a name="sample-script"></a>샘플 스크립트

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>배포 정리

스크립트 샘플을 실행한 후에는 다음 명령을 사용하여 리소스 그룹, 클러스터 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest) | 새 Service Fabric 클러스터를 만듭니다.  |

## <a name="next-steps"></a>다음 단계

Azure Service Fabric에 대한 추가 Service Fabric CLI 샘플은 [Service Fabric CLI 샘플](../samples-cli.md)에서 확인할 수 있습니다.
