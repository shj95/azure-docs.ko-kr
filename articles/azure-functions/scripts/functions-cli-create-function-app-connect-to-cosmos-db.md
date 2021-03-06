---
title: Azure Cosmos DB를 사용하여 함수 앱 만들기 - Azure CLI
description: Azure CLI 스크립트 샘플 - Azure Cosmos DB에 연결하는 Azure Function 만들기
ms.topic: sample
ms.date: 07/03/2018
ms.custom: mvc
ms.openlocfilehash: 5ee80283ed39789eabb702a48aa97f678a6409f9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75922708"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Azure Cosmos DB에 연결하는 Azure Function 만들기

이 Azure Functions 샘플 스크립트는 함수 앱을 만들고 해당 함수를 Azure Cosmos DB 데이터베이스에 연결합니다. 연결을 포함하는 생성된 앱 설정을 [Azure Cosmos DB 트리거 또는 바인딩](../functions-bindings-cosmosdb.md)에 사용할 수 있습니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

로컬로 CLI를 사용하면 Azure CLI 버전 2.0 이상을 실행하는지 확인합니다. 버전을 확인하려면 `az --version`을 실행합니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요. 

## <a name="sample-script"></a>샘플 스크립트

이 샘플은 Azure 함수 앱을 만들고 앱 설정에 Cosmos DB 엔드포인트 및 액세스 키를 추가합니다.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 표의 각 명령은 명령 관련 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 위치와 함께 리소스 그룹 만들기 |
| [az storage accounts create](/cli/azure/storage/account#az-storage-account-create) | 스토리지 계정 만들기 |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | 서버리스 [소비 계획](../functions-scale.md#consumption-plan)에서 함수 앱을 만듭니다. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB 데이터베이스를 만듭니다. |
| [az cosmosdb show](/cli/azure/cosmosdb#az-cosmosdb-show)| 데이터베이스 계정 연결을 가져옵니다. |
| [az cosmosdb list-keys](/cli/azure/cosmosdb#az-cosmosdb-list-keys)| 데이터베이스의 키를 가져옵니다. |
| [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | 함수 앱에서 연결 문자열을 앱 설정으로 설정합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.

추가 Azure Functions CLI 스크립트 샘플은 [Azure Functions 설명서](../functions-cli-samples.md)에서 확인할 수 있습니다.




