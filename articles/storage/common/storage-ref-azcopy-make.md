---
title: 아즈카피 메이크 | 마이크로 소프트 문서
description: 이 문서에서는 azcopy make 명령에 대한 참조 정보를 제공합니다.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 67d685684c5227377a0f8a7e822a06e785a69d89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034037"
---
# <a name="azcopy-make"></a>azcopy make

컨테이너 또는 파일 공유를 만듭니다.

## <a name="synopsis"></a>개요

지정된 리소스 URL로 표시되는 컨테이너 또는 파일 공유를 만듭니다.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>관련 개념 기사

- [AzCopy 시작](storage-use-azcopy-v10.md)
- [AzCopy 및 Blob 저장소로 데이터 전송](storage-use-azcopy-blobs.md)
- [AzCopy 및 파일 스토리지를 사용하여 데이터 전송](storage-use-azcopy-files.md)
- [AzCopy 구성, 최적화 및 문제 해결](storage-use-azcopy-configure.md)

## <a name="examples"></a>예

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>옵션

|옵션|설명|
|--|--|
|-h, --help|make 명령에 대한 도움말 콘텐츠를 표시합니다. |
|--할당량-gb uint32|GiB(몫의 최대 크기)를 지정하는 0은 파일 서비스의 기본 할당량을 수락한다는 의미입니다.|

## <a name="options-inherited-from-parent-commands"></a>상위 명령에서 상속된 옵션

|옵션|설명|
|---|---|
|--캡 mbps uint32|초당 메가비트로 전송 속도를 한도 를 캡슐화합니다. 모멘트별 처리량은 캡과 약간 다를 수 있습니다. 이 옵션이 0으로 설정되어 있거나 생략된 경우 처리량은 제한되지 않습니다.|
|--출력 유형 문자열|명령 출력의 형식입니다. 선택 사항은 다음과 같습니다: 텍스트, json. 기본값은 "텍스트"입니다.|

## <a name="see-also"></a>참조

- [아즈카피](storage-ref-azcopy.md)
