---
title: Azure 서비스 패브릭 서비스 모델 XML 스키마 단순 유형
description: Service Fabric 서비스 모델 XML 스키마의 단순 형식을 설명합니다.
ms.topic: reference
ms.date: 12/10/2018
ms.openlocfilehash: 3ed028bc255fa9561316a655da6edfacceff1c55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75466223"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>서비스 모델 XML 스키마 단순 형식

## <a name="fabricuri-simpletype"></a>FabricUri simpleType
Microsoft Azure Service Fabric 명명 시스템에서 서비스의 안정적 ID로 사용하는 URI입니다. 

### <a name="xml-source"></a>XML 원본
```xml
<xs:simpleType xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
