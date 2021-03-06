---
title: '알려진 문제: PostgreSQL에 대 한 Azure 데이터베이스에 온라인 마이그레이션'
titleSuffix: Azure Database Migration Service
description: Azure 데이터베이스 마이그레이션 서비스를 사용하여 PostgreSQL에서 PostgreSQL용 Azure 데이터베이스로의 온라인 마이그레이션을 통해 알려진 문제 및 마이그레이션 제한 사항에 대해 알아봅니다.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom:
- seo-lt-2019
- seo-dt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 3d1bc627ccb8814ab2dfb61fb0653ef0ac644038
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235258"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-postgresql-to-azure-db-for-postgresql"></a>PostgreSQL에서 PostgreSQL에 대한 Azure DB로의 온라인 마이그레이션으로 알려진 문제/마이그레이션 제한

다음 섹션에서는 PostgreSQL에서 Azure Database for PostgreSQL로의 온라인 마이그레이션과 관련된 알려진 문제 및 제한 사항에 대해 설명합니다.

## <a name="online-migration-configuration"></a>온라인 마이그레이션 구성

- 소스 PostgreSQL 서버는 버전 9.4, 9.5, 9.6, 10 또는 11을 실행중이어야 합니다. 자세한 내용은 [지원되는 PostgreSQL 데이터베이스 버전](../postgresql/concepts-supported-versions.md) 문서를 참조하세요.
- 동일하거나 더 높은 버전으로의 마이그레이션만 지원됩니다. 예를 들어 PostgreSQL 9.5를 PostgreSQL 9.6 또는 10에 대한 Azure 데이터베이스로 마이그레이션하는 것이 지원되지만 PostgreSQL 11에서 PostgreSQL 9.6으로 마이그레이션하는 것은 지원되지 않습니다.
- **원본 PostgreSQL postgresql.conf** 파일에서 논리 복제를 사용하도록 설정하려면 다음 매개 변수를 설정합니다.
  - **wal_level** = logical
  - **max_replication_slots** = [마이그레이션을 위한 데이터베이스의 최소 개수]; 네 개의 데이터베이스를 마이그레이션하려면 값을 4개 이상으로 설정합니다.
  - **max_wal_senders** = [동시에 실행되는 데이터베이스 수]. 권장 값은 10입니다.
- 소스 PostgreSQL pg_hba.conf에 DMS 에이전트 IP를 추가합니다.
  1. Azure 데이터베이스 마이그레이션 서비스의 인스턴스 프로비저닝을 완료한 후 DMS IP 주소를 기록합니다.
  2. 아래에 나와 있는 것처럼 pg_hba.conf 파일에 IP 주소를 추가합니다.

      ```
          host  all     172.16.136.18/10    md5
          host  replication postgres    172.16.136.18/10    md5
      ```

- 사용자는 원본 데이터베이스를 호스팅하는 서버에 REPLICATION 역할이 있어야 합니다.
- 원본 및 대상 데이터베이스 스키마가 일치해야 합니다.
- PostgreSQL-Single 서버에 대한 대상 Azure 데이터베이스의 스키마에는 외래 키가 없어야 합니다. 외래 키를 삭제하려면 다음 쿼리를 사용합니다.

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    쿼리 결과에서 외래 키 삭제(두 번째 열)를 실행합니다.

- PostgreSQL-Single 서버에 대한 대상 Azure 데이터베이스의 스키마에는 트리거가 없어야 합니다. 대상 데이터베이스에서 트리거를 사용하지 않도록 설정하려면 다음 명령을 사용합니다.

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>데이터 형식 제한 사항

  **제한**: 테이블에 기본 키가 없는 경우 변경 내용이 대상 데이터베이스에 동기화되지 않을 수 있습니다.

  **해결 방법**: 마이그레이션을 계속할 수 있도록 일시적으로 테이블에 대한 기본 키를 설정합니다. 데이터 마이그레이션이 완료된 후 기본 키를 제거할 수 있습니다.

## <a name="limitations-when-migrating-online-from-aws-rds-postgresql"></a>AWS RDS PostgreSQL에서 온라인으로 마이그레이션할 때의 제한 사항

AWS RDS PostgreSQL에서 PostgreSQL용 Azure 데이터베이스로의 온라인 마이그레이션을 수행하려고 하면 다음과 같은 오류가 발생할 수 있습니다.

- **오류**: 데이터베이스 '{데이터베이스}'의 테이블 '{table}'에서 '{열}' 열의 기본값은 소스 및 대상 서버에서 다릅니다. 원본은 '{value on source}'이고 대상은 '{value on target}'입니다.

  **제한**: 이 오류는 열 스키마의 기본값이 원본 데이터베이스와 대상 데이터베이스 간에 다를 때 발생합니다.
  **해결:** 대상의 스키마가 소스의 스키마와 일치하는지 확인합니다. 스키마 마이그레이션에 대한 자세한 내용은 [Azure PostgreSQL 온라인 마이그레이션 설명서를](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)참조하십시오.

- **오류**: 대상 데이터베이스 '{데이터베이스}'에는 소스 데이터베이스 '{데이터베이스}' 테이블이 있는 '{테이블 수}' 테이블이 있습니다. 원본과 대상 데이터베이스의 테이블 수가 같아야 합니다.

  **제한**: 이 오류는 테이블 수가 원본 데이터베이스와 대상 데이터베이스 간에 다를 때 발생합니다.

  **해결:** 대상의 스키마가 소스의 스키마와 일치하는지 확인합니다. 스키마 마이그레이션에 대한 자세한 내용은 [Azure PostgreSQL 온라인 마이그레이션 설명서를](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)참조하십시오.

- **오류:** 원본 데이터베이스 {데이터베이스}가 비어 있습니다.

  **제한**: 원본 데이터베이스가 비어 있을 때 이 오류가 발생합니다. 잘못된 데이터베이스를 원본으로 선택했기 때문일 수 있습니다.

  **해결:** 마이그레이션을 위해 선택한 원본 데이터베이스를 다시 확인한 다음 다시 시도합니다.

- **오류:** 대상 데이터베이스 {데이터베이스}가 비어 있습니다. 스키마를 마이그레이션하세요.

  **제한**: 이 오류는 대상 데이터베이스에 스키마가 없을 때 발생합니다. 대상스키마가 소스에서 스키마와 일치하는지 확인합니다.
  **해결:** 대상의 스키마가 소스의 스키마와 일치하는지 확인합니다. 스키마 마이그레이션에 대한 자세한 내용은 [Azure PostgreSQL 온라인 마이그레이션 설명서를](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)참조하십시오.

## <a name="other-limitations"></a>기타 제한 사항

- 데이터베이스 이름은 세미콜론(;)을 포함할 수 없습니다.
- 캡처된 테이블에는 기본 키가 있어야 합니다. 테이블에 기본 키가 없는 경우 DELETE 및 UPDATE 레코드 작업의 결과를 예측할 수 없습니다.
- 기본 키 세그먼트 업데이트는 무시됩니다. 이 경우 해당 업데이트를 적용하면 대상이 업데이트 작업을 모든 행을 업데이트하지 않은 업데이트로 식별하므로 예외 테이블에 레코드가 작성됩니다.
- 이름은 같고 대/소문자만 다른 여러 테이블(예: table1, TABLE1, Table1)을 마이그레이션하면 예기치 않은 동작이 발생할 수 있으므로 해당 마이그레이션은 지원되지 않습니다.
- [CREATE | 변경 처리 알터 | 드롭 | TRUNCATE] 테이블 DDL은 지원되지 않습니다.
- Azure 데이터베이스 마이그레이션 서비스에서 단일 마이그레이션 작업은 최대 4개의 데이터베이스만 수용할 수 있습니다.
