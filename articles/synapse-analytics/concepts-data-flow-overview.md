---
title: 데이터 흐름
description: Azure Synapse Analytics의 데이터 흐름에 대 한 개요
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: synapse-analytics
ms.topic: conceptual
ms.custom: references_regions
ms.date: 12/16/2020
ms.openlocfilehash: 18322afbdca94b12ef142f6e37c4d2a22170436b
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97592670"
---
# <a name="data-flows-in-azure-synapse-analytics"></a>Azure Synapse Analytics의 데이터 흐름

## <a name="what-are-data-flows"></a>데이터 흐름 이란?

데이터 흐름은 Azure Synapse Analytics에서 시각적으로 디자인 된 데이터 변환입니다. 데이터 흐름을 통해 데이터 엔지니어는 코드를 작성 하지 않고도 데이터 변환 논리를 개발할 수 있습니다. 결과 데이터 흐름은 확장 Apache Spark 클러스터를 사용 하는 Azure Synapse Analytics 파이프라인 내에서 작업으로 실행 됩니다. 데이터 흐름 작업은 기존 Azure Synapse 분석 일정, 제어, 흐름 및 모니터링 기능을 사용 하 여 조작 가능한 수 있습니다.

데이터 흐름은 코딩 하지 않아도 되는 완전 한 시각적 환경을 제공 합니다. 데이터 흐름은 확장 된 데이터 처리를 위해 Synapse로 관리 되는 실행 클러스터에서 실행 됩니다. Azure Synapse Analytics는 모든 코드 변환, 경로 최적화 및 데이터 흐름 작업의 실행을 처리 합니다.

## <a name="getting-started"></a>시작

데이터 흐름은 Synapse studio의 개발 창에서 만듭니다. 데이터 흐름을 만들려면 **개발** 옆에 있는 더하기 기호를 선택 하 고 **데이터 흐름** 을 선택 합니다. 

![새 데이터 흐름](media/data-flow/new-data-flow.png)

이 작업을 수행 하면 변환 논리를 만들 수 있는 데이터 흐름 캔버스로 이동 합니다. 원본 **추가** 를 선택 하 여 원본 변환 구성을 시작 합니다. 자세한 내용은 [원본 변환](../data-factory/data-flow-source.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)을 참조 하세요.

## <a name="authoring-data-flows"></a>데이터 흐름 작성

데이터 흐름에는 변환 논리를 쉽게 작성할 수 있도록 설계 된 고유한 제작 캔버스가 있습니다. 데이터 흐름 캔버스는 위쪽 막대, 그래프 및 구성 패널의 세 부분으로 구분 됩니다. 

![스크린샷 위쪽 막대, 그래프 및 구성 패널이 표시 된 데이터 흐름 캔버스를 표시 합니다.](media/data-flow/canvas-1.png)

### <a name="graph"></a>그래프

그래프는 변환 스트림을 표시 합니다. 하나 이상의 싱크로 전달 되는 원본 데이터의 계보를 보여 줍니다. 새 원본을 추가 하려면 **원본 추가** 를 선택 합니다. 새 변환을 추가 하려면 기존 변환의 오른쪽 아래에 있는 더하기 기호를 선택 합니다. [데이터 흐름 그래프를 관리](../data-factory/concepts-data-flow-manage-graph.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)하는 방법에 대해 자세히 알아보세요.

![스크린샷 검색 텍스트 상자를 사용 하 여 캔버스의 그래프 부분을 보여 줍니다.](media/data-flow/canvas-2.png)

### <a name="configuration-panel"></a>구성 패널

구성 패널에는 현재 선택한 변환과 관련 된 설정이 표시 됩니다. 선택 된 변환이 없는 경우 데이터 흐름이 표시 됩니다. 전체 데이터 흐름 구성에서는 **매개 변수** 탭을 통해 매개 변수를 추가할 수 있습니다. 자세한 내용은 [데이터 흐름 매개 변수](../data-factory/parameters-data-flow.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)를 참조 하세요.

각 변환에는 네 개 이상의 구성 탭이 포함 되어 있습니다.

#### <a name="transformation-settings"></a>변환 설정

각 변환의 구성 창에 있는 첫 번째 탭에는 해당 변환과 관련 된 설정이 포함 되어 있습니다. 자세한 내용은 해당 변형의 설명서 페이지를 참조 하세요.

![원본 설정 탭](media/data-flow/source-1.png)

#### <a name="optimize"></a>최적화

**최적화** 탭에는 파티션 구성표를 구성 하는 설정이 포함 되어 있습니다. 데이터 흐름을 최적화 하는 방법에 대 한 자세한 내용은 [데이터 흐름 매핑 성능 가이드](../data-factory/concepts-data-flow-performance.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)를 참조 하세요.

![최적화 탭을 보여 주는 스크린샷](media/data-flow/optimize.png)

#### <a name="inspect"></a>검사

**검사** 탭은 변환 중인 데이터 스트림의 메타 데이터에 대 한 뷰를 제공 합니다. 열 개수, 변경 된 열, 추가 된 열, 데이터 형식, 열 순서 및 열 참조를 볼 수 있습니다. **검사** 는 메타 데이터의 읽기 전용 뷰입니다. **검사** 창에서 메타 데이터를 보려면 디버그 모드를 사용 하도록 설정 하지 않아도 됩니다.

![검사 탭](media/data-flow/inspect.png)

변환을 통해 데이터의 모양을 변경 하면 **검사** 창에 메타 데이터 변경 흐름이 표시 됩니다. 원본 변환에 정의 된 스키마가 없는 경우 **검사** 창에 메타 데이터가 표시 되지 않습니다. 메타 데이터 부족은 스키마 드리프트 시나리오에서 일반적입니다.

#### <a name="data-preview"></a>데이터 미리 보기

디버그 모드가 on 이면 **데이터 미리 보기** 탭에서 각 변환 시 데이터의 대화형 스냅숏을 제공 합니다. 자세한 내용은 [디버그 모드에서 데이터 미리 보기](../data-factory/concepts-data-flow-debug-mode.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#data-preview)를 참조 하세요.

### <a name="top-bar"></a>위쪽 막대

위쪽 막대는 유효성 검사 및 디버그 설정과 같은 전체 데이터 흐름에 영향을 주는 작업을 포함 합니다. 변환 논리의 기본 JSON 코드 및 데이터 흐름 스크립트도 볼 수 있습니다.

## <a name="available-transformations"></a>사용 가능한 변환

사용 가능한 변환 목록을 가져오려면 [데이터 흐름 매핑 변환 개요](../data-factory/data-flow-transformation-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 를 참조 하세요.

## <a name="data-flow-activity"></a>데이터 흐름 작업

데이터 흐름은 [데이터 흐름 활동](../data-factory/control-flow-execute-data-flow-activity.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)을 사용 하 여 Azure Synapse Analytics 파이프라인 내에서 조작 가능한 됩니다. 모든 사용자는 사용할 통합 런타임을 지정 하 고 매개 변수 값을 전달 해야 합니다. 자세한 내용은 [Azure 통합 런타임에](../data-factory/concepts-integration-runtime.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#azure-integration-runtime)대해 알아보세요.

## <a name="debug-mode"></a>디버그 모드

디버그 모드를 사용 하면 데이터 흐름을 빌드하고 디버그 하는 동안 각 변환 단계의 결과를 대화형으로 볼 수 있습니다. 디버그 세션은 데이터 흐름 논리를 빌드하고 데이터 흐름 작업을 사용 하 여 파이프라인 디버그를 실행 하는 경우에 사용할 수 있습니다. 자세히 알아보려면 [디버그 모드 설명서](../data-factory/concepts-data-flow-debug-mode.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)를 참조 하세요.

## <a name="monitoring-data-flows"></a>데이터 흐름 모니터링

데이터 흐름은 기존 Azure Synapse Analytics 모니터링 기능과 통합 됩니다. 데이터 흐름 모니터링 결과를 이해 하는 방법에 대 한 자세한 내용은 [데이터 흐름 매핑 모니터링](../data-factory/concepts-data-flow-monitoring.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)을 참조 하세요.

Azure Synapse Analytics 팀은 비즈니스 논리를 구축한 후 데이터 흐름의 실행 시간을 최적화 하는 데 도움이 되는 [성능 조정 가이드](../data-factory/concepts-data-flow-performance.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 를 만들었습니다.

## <a name="next-steps"></a>다음 단계

* [원본 변환을](../data-factory/data-flow-source.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)만드는 방법을 알아봅니다.
* [디버그 모드](../data-factory/concepts-data-flow-debug-mode.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)에서 데이터 흐름을 빌드하는 방법에 대해 알아봅니다.
