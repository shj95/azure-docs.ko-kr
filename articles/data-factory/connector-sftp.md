---
title: SFTP 서버에서 데이터 복사
description: Azure 데이터 팩터리(Azure Data Factory)를 사용하여 SFTP 서버에서 데이터를 복사하는 방법에 대해 알아봅니다.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/02/2020
ms.openlocfilehash: 06428d4a9c4a4178212d16d42b8b3adffb5c9718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250278"
---
# <a name="copy-data-from-and-to-sftp-server-using-azure-data-factory"></a>Azure 데이터 팩터리를 사용하여 SFTP 서버에서 데이터를 복사합니다.

> [!div class="op_single_selector" title1="사용 중인 Data Factory 서비스 버전을 선택합니다."]
> * [버전 1](v1/data-factory-sftp-connector.md)
> * [현재 버전](connector-sftp.md)

이 문서에서는 SFTP 서버에서 데이터를 복사하는 방법을 설명합니다. Azure Data Factory에 대해 자세히 알아보려면 [소개 문서](introduction.md)를 참조하세요.

## <a name="supported-capabilities"></a>지원되는 기능

이 SFTP 커넥터는 다음 활동에 대해 지원됩니다.

- [지원되는 소스/싱크 매트릭스로](copy-activity-overview.md) [활동 복사](copy-activity-overview.md)
- [조회 활동](control-flow-lookup-activity.md)
- [GetMetadata 작업](control-flow-get-metadata-activity.md)
- [활동 삭제](delete-activity.md)

특히 이 SFTP 커넥터는 다음을 지원합니다.

- **기본** 또는 **SshPublicKey** 인증을 사용하여 SFTP에서/SFTP로 파일을 복사합니다.
- 지원되는 파일 형식 및 압축 코덱으로 파일을 있는 것으로 복사하거나 파일을 구문 [분석/생성합니다.](supported-file-formats-and-compression-codecs.md)

## <a name="prerequisites"></a>사전 요구 사항

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>시작

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 SFTP에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

SFTP 연결된 서비스에 다음 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 형식 속성은 **Sftp**로 설정해야 합니다. |yes |
| host | SFTP 서버의 이름 또는 IP 주소입니다. |yes |
| 포트 | SFTP 서버가 수신하는 포트입니다.<br/>허용되는 값은 정수이며 기본값은 **22**입니다. |예 |
| skipHostKeyValidation | 호스트 키 유효성 검사를 건너뛸지 여부를 지정합니다.<br/>허용된 값은 **true**, false(기본값)입니다. **false**  | 예 |
| hostKeyFingerprint | 호스트 키의 지문을 지정합니다. | “skipHostKeyValidation”이 false로 설정된 경우 예  |
| authenticationType | 인증 유형을 지정합니다.<br/>허용되는 값은 **Basic** 및 **SshPublicKey**입니다. 기본 [인증 사용](#using-basic-authentication) 및 [SSH 공개 키 인증](#using-ssh-public-key-authentication) 섹션을 참조하여 더 많은 속성 및 JSON 샘플을 각각 참조합니다. |yes |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [통합 런타임입니다.](concepts-integration-runtime.md) [필수 구성 조건](#prerequisites) 섹션에서 자세히 알아보십시오. 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. |예 |

### <a name="using-basic-authentication"></a>기본 인증 사용

기본 인증을 사용하려면 “authenticationType” 속성을 **Basic**으로 설정하고, 마지막 섹션에서 소개한 SFTP 커넥터 일반 속성 외에 다음 속성을 지정합니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| userName | SFTP 서버에 액세스하는 사용자. |yes |
| password | 사용자(userName) 암호입니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나, [Azure Key Vault에 저장된 비밀을 참조](store-credentials-in-key-vault.md)합니다. | yes |

**예제:**

```json
{
    "name": "SftpLinkedService",
    "type": "linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH 공개 키 인증 사용

SSH 공개 키 인증을 사용하려면 “authenticationType” 속성을 **SshPublicKey**로 설정하고, 마지막 섹션에서 소개한 SFTP 커넥터 일반 속성 외에 다음 속성을 지정합니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| userName | SFTP 서버에 액세스하는 사용자 |yes |
| privateKeyPath | Integration Runtime에서 액세스할 수 있는 프라이빗 키 파일의 절대 경로를 지정합니다. 자체 호스팅된 유형의 Integration Runtime이 “connectVia”에 지정된 경우에만 적용됩니다. | `privateKeyPath` 또는 `privateKeyContent`를 지정합니다.  |
| privateKeyContent | Base64 인코딩된 SSH 프라이빗 키 콘텐츠입니다. SSH 프라이빗 키가 OpenSSH 형식이어야 합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나, [Azure Key Vault에 저장된 비밀을 참조](store-credentials-in-key-vault.md)합니다. | `privateKeyPath` 또는 `privateKeyContent`를 지정합니다. |
| passPhrase | 키 파일이 암호문으로 보호되는 경우 프라이빗 키를 해독하는 암호문/암호를 지정합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나, [Azure Key Vault에 저장된 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 프라이빗 키 파일이 암호문으로 보호되는 경우에는 필수입니다. |

> [!NOTE]
> SFTP 커넥터는 RSA/DSA OpenSSH 키를 지원합니다. 키 파일 콘텐츠는 "-----BEGIN [RSA/DSA] PRIVATE KEY-----"로 시작되어야 합니다. 프라이빗 키 파일이 ppk 형식 파일인 경우 Putty 도구를 사용하여 .ppk를 OpenSSH 형식으로 변환합니다. 

**예 1: 프라이빗 키 filePath를 사용하여 SshPublicKey 인증**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**예 2: 프라이빗 키 콘텐츠를 사용하여 SshPublicKey 인증**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 세트 속성

데이터 세트 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 세트](concepts-datasets-linked-services.md) 문서를 참조하세요. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

형식 기반 데이터 집합의 설정에서 `location` SFTP에 대해 다음 속성이 지원됩니다.

| 속성   | 설명                                                  | 필수 |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | 데이터 집합에서 `location` 아래의 형식 속성은 **SftpLocation**로 설정해야 합니다. | yes      |
| folderPath | 폴더에 대한 경로입니다. 와일드카드를 사용하여 폴더를 필터링하려면 이 설정을 건너뛰고 활동 소스 설정에서 지정합니다. | 예       |
| fileName   | 지정된 폴더 아래의 파일 이름Path입니다. 와일드카드를 사용하여 파일을 필터링하려면 이 설정을 건너뛰고 활동 소스 설정에서 지정합니다. | 예       |

**예제:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "SftpLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 SFTP 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="sftp-as-source"></a>SFTP를 원본으로

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

형식 기반 복사 소스의 설정에서 `storeSettings` SFTP에 대해 다음 속성이 지원됩니다.

| 속성                 | 설명                                                  | 필수                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | 아래의 `storeSettings` 형식 속성은 **SftpReadSettings로**설정되어야 합니다. | yes                                           |
| recursive                | 하위 폴더 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive를 true로 설정하고 싱크가 파일 기반 저장소인 경우 빈 폴더 또는 하위 폴더가 싱크에 복사되거나 만들어지지 않습니다. 허용된 **true** 값은 true(기본값) 및 **false입니다.** | 예                                            |
| 와일드 카드폴더패스       | 원본 폴더를 필터링하는 와일드카드 문자가 있는 폴더 경로입니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br>더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 예                                            |
| 와일드카드파일이름         | 지정된 폴더패스/와일드카드FolderPath 아래에 와일드카드 문자가 있는 파일 이름으로 소스 파일을 필터링합니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다.  더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 데이터 `fileName` 집합에 지정되지 않은 경우 예 |
| modifiedDatetimeStart    | 파일은 특성: 마지막으로 수정된 특성을 기반으로 필터링합니다. 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br> 속성은 NULL일 수 있습니다. 이 경우 파일 특성 필터가 데이터 세트에 적용되지 않습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다. | 예                                            |
| modifiedDatetimeEnd      | 위와 동일합니다.                                               | 예                                            |
| maxConcurrentConnections | 저장소 저장소에 동시에 연결할 연결 수입니다. 데이터 저장소에 대한 동시 연결을 제한하려는 경우에만 지정합니다. | 예                                            |

**예제:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "SftpReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sftp-as-sink"></a>싱크대로서의 SFTP

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

형식 기반 복사 싱크의 설정에서 `storeSettings` SFTP에 대해 다음 속성이 지원됩니다.

| 속성                 | 설명                                                  | 필수 |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | 아래의 `storeSettings` 형식 속성은 **SftpWriteSettings로**설정되어야 합니다. | yes      |
| copyBehavior             | 원본이 파일 기반 데이터 저장소의 파일인 경우 복사 동작을 정의합니다.<br/><br/>허용된 값은<br/><b>- PreserveHierarchy(기본값)</b>: 대상 폴더에서 파일 계층 구조를 유지합니다. 원본 폴더의 원본 파일 상대 경로는 대상 폴더의 대상 파일 상대 경로와 동일합니다.<br/><b>- 병합 계층 :</b>소스 폴더의 모든 파일은 대상 폴더의 첫 번째 수준에 있습니다. 대상 파일은 자동 생성된 이름을 갖습니다. <br/><b>- MergeFiles</b>: 원본 폴더의 모든 파일을 하나의 파일로 병합합니다. 파일 이름이 지정된 경우 병합되는 파일 이름은 지정된 이름입니다. 그렇지 않으면 자동 생성되는 파일 이름이 적용됩니다. | 예       |
| maxConcurrentConnections | 동시에 데이터 저장소에 연결할 연결 수입니다. 데이터 저장소에 대한 동시 연결을 제한하려는 경우에만 지정합니다. | 예       |
| useTempFile 다시 이름 | 임시 파일에 업로드하고 이름을 바꿀지 또는 대상 폴더/파일 위치에 직접 쓸지 여부를 나타냅니다. 기본적으로 ADF는 먼저 임시 파일에 쓰기 를 한 다음 업로드 완료 시 파일 이름을 바꾸기 위해 1) 동일한 파일에 다른 프로세스가 있는 경우 손상된 파일이 발생하는 충돌 쓰기를 피하고 2) 파일의 원래 버전이 있는 동안 파일의 원래 버전이 있는지 확인합니다. 전체 전송. SFTP 서버에서 이름 바꾸기 작업을 지원하지 않는 경우 이 옵션을 사용하지 않도록 설정하고 대상 파일에 동시 쓰기가 없는지 확인합니다. 이 표 아래의 문제 해결 팁을 참조하십시오. | 아니요. 기본값은 true입니다. |
| 작업 시간 시간 | SFTP 서버에 대한 각 쓰기 요청 의 대기 시간이 시간 단축됩니다. 기본값은 60분(01:00:00)입니다.|예 |

>[!TIP]
>SFTP에 데이터를 쓸 때 "UserErrorSftpPathNotFound", "UserErrorSftpPermissionPermission거부" 또는 "SftpOperationFail"의 오류가 발생하면 사용하는 SFTP 사용자에게 적절한 권한이 있는지 확인하여 SFTP 서버 지원 파일 이름 바꾸기`useTempFileRename`작업을 수행하려면 "임시 파일로 업로드"() 옵션을 사용하지 않도록 설정하고 다시 시도하십시오. 위의 표에서 이 속성에 대해 자세히 알아보십시오. 복사본에 자체 호스팅 통합 런타임을 사용하는 경우 버전 4.6 이상을 사용해야 합니다.

**예제:**

```json
"activities":[
    {
        "name": "CopyToSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BinarySink",
                "storeSettings":{
                    "type": "SftpWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>폴더 및 파일 필터 예제

이 섹션에서는 와일드카드 필터가 있는 폴더 경로 및 파일 이름의 결과 동작에 대해 설명합니다.

| folderPath | fileName | recursive | 소스 폴더 구조 및 필터 **결과(굵게 표시된** 파일이 검색됩니다)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (비어 있음, 기본값 사용) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (비어 있음, 기본값 사용) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**파일3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**파일3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="lookup-activity-properties"></a>조회 활동 속성

속성에 대한 자세한 내용을 보려면 [조회 활동을](control-flow-lookup-activity.md)선택합니다.

## <a name="getmetadata-activity-properties"></a>GetMetadata 활동 속성

속성에 대한 자세한 내용을 보려면 [GetMetadata 활동을](control-flow-get-metadata-activity.md) 선택합니다. 

## <a name="delete-activity-properties"></a>활동 속성 삭제

속성에 대한 자세한 내용을 보려면 [활동 삭제를](delete-activity.md) 선택합니다.

## <a name="legacy-models"></a>레거시 모델

>[!NOTE]
>다음 모델은 이전 버전과의 호환성을 위해 있는 것처럼 계속 지원됩니다. 위의 섹션에서 언급한 새 모델을 사용하는 것이 좋습니다.

### <a name="legacy-dataset-model"></a>레거시 데이터 집합 모델

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 세트의 형식 속성을 **FileShare**로 설정해야 합니다. |yes |
| folderPath | 파일의 경로입니다. 와일드카드 필터가 지원되며, 허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 파일 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br/><br/>예: rootfolder/subfolder/(더 많은 예제는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples) 참조) |yes |
| fileName |  지정된 "folderPath" 아래의 파일에 대한 **이름 또는 와일드 카드 필터**입니다. 이 속성의 값을 지정하지 않으면 데이터 세트는 폴더에 있는 모든 파일을 가리킵니다. <br/><br/>필터에 허용되는 와일드카드는 `*`(문자 0자 이상 일치) 및 `?`(문자 0자 또는 1자 일치)입니다.<br/>- 예 1: `"fileName": "*.csv"`<br/>- 예 2: `"fileName": "???20180427.txt"`<br/>실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. |예 |
| modifiedDatetimeStart | 파일은 특성: 마지막으로 수정된 특성을 기반으로 필터링합니다. 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 방대한 양의 파일에서 파일 필터를 수행하려는 경우 이 설정을 사용하도록 설정하면 데이터 이동의 전반적인 성능이 영향을 받을 수 있습니다. <br/><br/> 속성은 NULL일 수 있으며 이는 데이터 집합에 파일 특성 필터가 적용되지 않는다는 의미입니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 예 |
| modifiedDatetimeEnd | 파일은 특성: 마지막으로 수정된 특성을 기반으로 필터링합니다. 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 방대한 양의 파일에서 파일 필터를 수행하려는 경우 이 설정을 사용하도록 설정하면 데이터 이동의 전반적인 성능이 영향을 받을 수 있습니다. <br/><br/> 속성은 NULL일 수 있으며 이는 데이터 집합에 파일 특성 필터가 적용되지 않는다는 의미입니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 예 |
| format | 파일 기반 저장소(이진 복사본) 간에 있는 **파일로 파일을 복사하려면** 입력 및 출력 데이터 집합 정의모두에서 형식 섹션을 건너뜁니다.<br/><br/>특정 형식으로 파일을 구문 분석하려는 경우 **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**과 같은 파일 형식 유형이 지원됩니다. format의 **type** 속성을 이 값 중 하나로 설정합니다. 자세한 내용은 [텍스트 형식](supported-file-formats-and-compression-codecs-legacy.md#text-format), [Json 형식](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro 형식](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [Orc 형식](supported-file-formats-and-compression-codecs-legacy.md#orc-format) 및 [Parquet 형식](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) 섹션을 참조하세요. |아니요(이진 복사 시나리오에만 해당) |
| 압축 | 데이터에 대한 압축 유형 및 수준을 지정합니다. 자세한 내용은 [지원되는 파일 형식 및 압축 코덱](supported-file-formats-and-compression-codecs-legacy.md#compression-support)을 참조하세요.<br/>지원되는 유형은 **GZip,** **deflate,** **BZip2**및 **ZipDeflate입니다.**<br/>지원되는 수준은 **다음과** 같습니다. **Fastest** |예 |

>[!TIP]
>폴더 아래에서 모든 파일을 복사하려면 **folderPath**만을 지정합니다.<br>지정된 이름의 단일 파일을 복사하려면 폴더 부분으로 **folderPath** 및 파일 이름으로 **fileName**을 지정합니다.<br>폴더 아래에서 파일의 하위 집합을 복사하려면 폴더 부분으로 **folderPath** 및 와일드카드 필터로 **fileName**을 지정합니다.

>[!NOTE]
>파일 필터에 "fileFilter" 속성을 사용한 경우 그대로 계속 지원되지만, 이후 "fileName"에 추가된 새 필터 기능을 사용하도록 제안합니다.

**예제:**

```json
{
    "name": "SFTPDataset",
    "type": "Datasets",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>레거시 복사 활동 소스 모델

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 형식 속성을 **FileSystemSource**로 설정해야 합니다. |yes |
| recursive | 하위 폴더에서 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive가 true로 설정되고 싱크가 파일 기반 저장소인 경우 싱크에서 빈 폴더/하위 폴더가 복사/생성되지 않습니다.<br/>허용된 값은 다음과 같습니다: true(기본값), **false** **true** | 예 |
| maxConcurrentConnections | 저장소 저장소에 동시에 연결할 연결 수입니다. 데이터 저장소에 대한 동시 연결을 제한하려는 경우에만 지정합니다. | 예 |

**예제:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats)를 참조하세요.
