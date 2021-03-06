---
title: Azure SQL Database 관리 인스턴스로의 온라인 마이그레이션에 대한 알려진 문제 및 제한 사항
description: Azure SQL Database 관리 인스턴스로의 온라인 마이그레이션과 관련된 알려진 문제/마이그레이션 제한 사항에 대해 알아봅니다.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 88e2b5894686ee93caecf33e04940803eb75f394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648668"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database-managed-instance"></a>Azure SQL Database 관리 인스턴스로의 온라인 마이그레이션으로 알려진 문제/마이그레이션 제한

SQL Server에서 Azure SQL Database 관리 형 인스턴스로의 온라인 마이그레이션과 관련된 알려진 문제 및 제한 사항은 아래에 설명되어 있습니다.

> [!IMPORTANT]
> SQL Server를 Azure SQL Database로 온라인으로 마이그레이션하면 SQL_variant 데이터 형식의 마이그레이션이 지원되지 않습니다.

## <a name="backup-requirements"></a>백업 요구 사항

- **체크섬이 있는 백업**

    Azure Database 마이그레이션 서비스는 백업 및 복원 방법을 사용하여 온-프레미스 데이터베이스를 SQL Database 관리 인스턴스로 마이그레이션합니다. Azure 데이터베이스 마이그레이션 서비스는 체크섬을 사용하여 만든 백업만 지원합니다.

    [백업 또는 복원 중 백업 검사 사용 또는 비활성화(SQL 서버)](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)

    > [!NOTE]
    > 압축으로 데이터베이스 백업을 수행하면 명시적으로 사용하지 않도록 설정하지 않는 한 checksum이 기본 동작입니다.

    오프라인 마이그레이션을 사용하면 **Azure 데이터베이스 마이그레이션 서비스를 허용합니다.**

- **백업 미디어**

    별도의 백업 미디어(백업 파일)에서 모든 백업을 수행해야 합니다. Azure 데이터베이스 마이그레이션 서비스는 단일 백업 파일에 추가된 백업을 지원하지 않습니다. 전체 백업을 수행하 고 백업 파일을 기록 합니다.

## <a name="data-and-log-file-layout"></a>데이터 및 로그 파일 레이아웃

- **로그 파일 수**

    Azure 데이터베이스 마이그레이션 서비스는 여러 로그 파일이 있는 데이터베이스를 지원하지 않습니다. 여러 로그 파일이 있는 경우 축소하고 단일 트랜잭션 로그 파일로 재구성합니다. 비어 있지 않은 파일을 원격으로 로그할 수 없으므로 먼저 로그 파일을 백업해야 합니다.

## <a name="sql-server-features"></a>SQL Server 기능

- **파일 스트림/파일 테이블**

    SQL Database 관리 인스턴스는 현재 FileStream 및 FileTable을 지원하지 않습니다. 이러한 기능에 종속된 워크로드의 경우 Azure VM에서 실행되는 SQL Server를 Azure 대상으로 선택하는 것이 좋습니다.

- **메모리 내 테이블**

    인메모리 OLTP는 SQL Database 관리 인스턴스의 프리미엄 및 비즈니스 중요 계층에서 사용할 수 있습니다. 범용 계층은 인메모리 OLTP를 지원하지 않습니다.

## <a name="migration-resets"></a>마이그레이션 재설정

- **배포**

    SQL Database 관리 인스턴스는 자동 패치 및 버전 업데이트가 있는 PaaS 서비스입니다. SQL Database 관리 인스턴스를 마이그레이션하는 동안 중요하지 않은 업데이트는 최대 36시간 까지 도움이 됩니다. 이후에(및 중요한 업데이트의 경우) 마이그레이션이 중단되면 프로세스가 전체 복원 상태로 재설정됩니다.

    전체 백업이 복원되고 모든 로그 백업을 따라잡은 후에만 마이그레이션 컷오버를 호출할 수 있습니다. 프로덕션 마이그레이션 컷오버가 영향을 받는 경우 [Azure DMS 피드백 별칭에](mailto:dmsfeedback@microsoft.com)문의하십시오.
