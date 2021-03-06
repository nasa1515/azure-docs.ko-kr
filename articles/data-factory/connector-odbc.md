---
title: Azure Data Factory를 사용하여 ODBC 데이터 저장소 간 데이터 복사
description: Azure Data Factory 파이프라인에서 복사 작업을 사용 하 여 ODBC 데이터 저장소에서 데이터를 복사 하는 방법에 대해 알아봅니다.
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: jingwang
ms.openlocfilehash: 9b73e10b0ed539879e9a32d3961b6375828cc153
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100389623"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Azure Data Factory를 사용하여 ODBC 데이터 저장소 간 데이터 복사
> [!div class="op_single_selector" title1="사용 중인 Data Factory 서비스 버전을 선택합니다."]
> * [버전 1](v1/data-factory-odbc-connector.md)
> * [현재 버전](connector-odbc.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

이 문서에서는 Azure Data Factory의 복사 작업을 사용하여 ODBC 데이터 저장소 간에 데이터를 복사하는 방법을 설명합니다. 이 문서는 복사 작업에 대한 일반적인 개요를 제공하는 [복사 작업 개요](copy-activity-overview.md) 문서를 기반으로 합니다.

## <a name="supported-capabilities"></a>지원되는 기능

이 ODBC 커넥터는 다음과 같은 작업에 대해 지원 됩니다.

- [지원되는 원본/싱크 매트릭스](copy-activity-overview.md)를 사용한 [복사 작업](copy-activity-overview.md)
- [조회 작업](control-flow-lookup-activity.md)

ODBC 원본에서 지원되는 싱크 데이터 저장소로 또는 지원되는 원본 데이터 저장소에서 ODBC 싱크로 데이터를 복사할 수 있습니다. 복사 작업의 원본/싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats) 표를 참조하세요.

특히 이 ODBC 커넥터는 **Basic** 또는 **Anonymous** 인증을 사용하여 **ODBC 호환 데이터 저장소** 간의 데이터 복사를 지원합니다. **64 비트 ODBC 드라이버가** 필요 합니다. ODBC 싱크의 경우 ADF는 ODBC 버전 2.0 표준을 지원 합니다.

## <a name="prerequisites"></a>사전 요구 사항

ODBC 커넥터를 사용하려면 다음을 수행해야 합니다.

- 자체 호스팅 Integration Runtime을 설정합니다. 자세한 내용은 [자체 호스팅 Integration Runtime](create-self-hosted-integration-runtime.md) 문서를 참조 하세요.
- Integration Runtime 컴퓨터의 데이터 저장소에 대 한 64 비트 ODBC 드라이버를 설치 합니다.

## <a name="getting-started"></a>시작

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 ODBC 커넥터에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

ODBC 연결된 서비스에 다음 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 형식 속성은 **Odbc** 로 설정해야 합니다. | 예 |
| connectionString | 자격 증명 부분을 제외한 연결 문자열입니다. `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`와 같은 패턴으로 연결 문자열을 지정하거나 `"DSN=<name of the DSN on IR machine>;"`을 통해 Integration Runtime 컴퓨터에 설정한 시스템 DSN(데이터 원본 이름)을 사용할 수 있습니다(이에 따라 연결된 서비스에서 자격 증명 부분도 계속 지정해야 함).<br>Azure Key Vault에 암호를 입력 하 고 연결 문자열에서 구성을 끌어올 수도 있습니다 `password` . 자세한 내용은 [Azure Key Vault에 자격 증명 저장](store-credentials-in-key-vault.md) 을 참조 하세요.| 예 |
| authenticationType | ODBC 데이터 저장소에 연결하는 데 사용되는 인증 형식입니다.<br/>허용되는 값은 **Basic** 및 **Anonymous** 입니다. | 예 |
| userName | 기본 인증을 사용하는 경우 사용자 이름을 지정합니다. | 예 |
| password | userName에 지정한 사용자 계정의 암호를 지정합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 예 |
| 자격 증명(credential) | 드라이버 관련 속성 값 형식에 지정된 연결 문자열의 액세스 자격 증명 부분입니다. 예: `"RefreshToken=<secret refresh token>;"`. 이 필드를 SecureString으로 표시합니다. | 예 |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md)입니다. [필수 조건](#prerequisites)에 설명된 대로 자체 호스팅 Integration Runtime이 필요합니다. |Yes |

**예제 1: 기본 인증 사용**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
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

**예제 2: 익명 인증 사용**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
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

데이터 세트 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 세트](concepts-datasets-linked-services.md) 문서를 참조하세요. 이 섹션에서는 ODBC 데이터 세트에서 지원하는 속성의 목록을 제공합니다.

ODBC 호환 데이터 저장소에서 데이터를 복사 하려면 다음 속성이 지원 됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 집합의 type 속성은 **OdbcTable** 로 설정 해야 합니다. | 예 |
| tableName | ODBC 데이터 저장소에 있는 테이블의 이름입니다. | 원본: 아니요(작업 원본에서 "query"가 지정된 경우)<br/>싱크: 예 |

**예제**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

`RelationalTable` 형식의 데이터 세트를 사용하고 있는 경우 현재까지 지원되지만 앞으로는 새 형식을 사용하는 것이 좋습니다.

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 ODBC 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="odbc-as-source"></a>ODBC를 원본으로

ODBC 호환 데이터 저장소에서 데이터를 복사 하려면 복사 작업 **원본** 섹션에서 다음 속성을 지원 합니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 type 속성은 **OdbcSource** 로 설정 해야 합니다. | 예 |
| Query | 사용자 지정 SQL 쿼리를 사용하여 데이터를 읽습니다. 예: `"SELECT * FROM MyTable"` | 아니요(데이터 세트의 "tableName"이 지정된 경우) |

**예:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
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
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

`RelationalSource` 형식의 원본을 사용하고 있는 경우 현재까지 지원되지만 앞으로는 새 형식을 사용하는 것이 좋습니다.

### <a name="odbc-as-sink"></a>ODBC를 싱크로

ODBC 호환 데이터 저장소에 데이터를 복사하려면 복사 작업의 싱크 형식을 **OdbcSink** 로 설정합니다. 복사 작업 **sink** 섹션에서 다음 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 싱크의 형식 속성은 **OdbcSink** 로 설정해야 합니다. | Yes |
| writeBatchTimeout |시간이 초과되기 전에 완료하려는 배치 삽입 작업을 위한 대기 시간입니다.<br/>허용되는 값은 시간 범위입니다. 예제: "00:30:00"(30분). |예 |
| writeBatchSize |버퍼 크기가 writeBatchSize에 도달하는 경우 SQL 테이블에 데이터 삽입<br/>허용되는 값은 정수(행 수)입니다. |아니요(기본값: 0 - 자동 검색됨) |
| preCopyScript |각 실행 시 데이터 저장소에 데이터를 쓰기 전에 실행할 복사 작업에 대한 SQL 쿼리를 지정합니다. 이 속성을 사용하여 미리 로드된 데이터를 정리할 수 있습니다. |예 |

> [!NOTE]
> "WriteBatchSize"의 경우 (자동 검색) 설정 되지 않은 경우 복사 작업은 먼저 드라이버에서 일괄 처리 작업을 지원 하는지 여부를 검색 하 고,이를 1만로 설정 하 고, 그렇지 않은 경우 1로 설정 합니다. 0 이외의 값을 명시적으로 설정 하는 경우 복사 작업은 값을 인식 하 고 드라이버에서 일괄 처리 작업을 지원 하지 않는 경우 런타임에 실패 합니다.

**예:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>조회 작업 속성

속성에 대한 자세한 내용을 보려면 [조회 작업](control-flow-lookup-activity.md)을 확인하세요.

## <a name="troubleshoot-connectivity-issues"></a>연결 문제 해결

연결 문제를 해결하려면 **통합 런타임 구성 관리자** 의 **진단** 탭을 사용합니다.

1. **통합 런타임 구성 관리자** 를 시작합니다.
2. **진단** 탭으로 전환 합니다.
3. "연결 테스트" 섹션에서 데이터 저장소(연결된 서비스)의 **형식** 을 선택합니다.
4. 데이터 저장소에 연결하는 데 사용되는 **연결 문자열** 을 지정하고 **인증** 을 선택하고 **사용자 이름**, **암호** 및/또는 **자격 증명** 을 입력합니다.
5. **연결 테스트** 를 클릭하여 데이터 저장소에 대한 연결을 테스트합니다.

## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats)를 참조하세요.
