---
title: Azure Monitor 로그 쿼리의 app() 식 | Microsoft Docs
description: 앱 식은 동일한 리소스 그룹, 다른 리소스 그룹 또는 다른 구독에 있는 특정 Application Insights 앱에서 데이터를 검색 하기 위해 Azure Monitor 로그 쿼리에서 사용 됩니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/09/2019
ms.openlocfilehash: cbfc9c8a32eb5345cfc69f51f40e0634e05bd978
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100614229"
---
# <a name="app-expression-in-azure-monitor-query"></a>Azure Monitor 쿼리에서 app() 식

`app` 식은 동일한 리소스 그룹, 다른 리소스 그룹 또는 다른 구독의 특정 Application Insights 앱에서 데이터를 검색하기 위해 Azure Monitor 쿼리에서 사용됩니다. 이 식은 Azure Monitor 로그 쿼리에 애플리케이션 데이터를 포함하고 Application Insights 쿼리를 통해 여러 애플리케이션의 데이터를 쿼리하는 데 유용합니다.

> [!IMPORTANT]
> 응용 프로그램 () 식은 [작업 영역 기반 Application Insights 리소스](../app/create-workspace-resource.md) 를 사용 하는 경우에는 로그 데이터가 Log Analytics 작업 영역에 저장 되므로 사용 되지 않습니다. Log () 식을 사용 하 여 여러 작업 영역에서 응용 프로그램을 포함 하는 쿼리를 작성 합니다. 동일한 작업 영역에 있는 여러 응용 프로그램의 경우에는 상호 작업 영역 쿼리가 필요 하지 않습니다.

## <a name="syntax"></a>Syntax

`app(`*한정자*`)`


## <a name="arguments"></a>인수

- *Identifier*: 아래 표에 있는 형식 중 하나를 사용하여 앱을 식별합니다.

| ID | Description | 예제
|:---|:---|:---|
| 리소스 이름 | 사람이 읽을 수 있는 앱 이름 ("구성 요소 이름"이 라고도 함) | app(“fabrikamapp”) |
| 정규화된 이름 | “subscriptionName/resourceGroup/componentName” 형식으로 된 앱의 전체 이름 | app(‘AI-Prototype/Fabrikam/fabrikamapp’) |
| ID | 앱의 GUID | app(“988ba129-363e-4415-8fe7-8cbab5447518”) |
| Azure 리소스 ID | Azure 리소스의 식별자 |app(“/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp”) |


## <a name="notes"></a>참고

* 애플리케이션에 대한 읽기 권한이 있어야 합니다.
* 애플리케이션을 이름으로 식별하면 액세스 가능한 모든 구독에서 고유한 것으로 가정합니다. 지정된 이름의 애플리케이션이 여러 개 있으면 모호성으로 인해 쿼리가 실패합니다. 이런 경우 다른 식별자 중 하나를 사용해야 합니다.
* 관련된 식 [작업 영역](../logs/workspace-expression.md)을 사용하여 Log Analytics 작업 영역 전체를 쿼리합니다.
* 앱() 식은 현재 Application Insights 애플리케이션이 경고 규칙에 대한 리소스로 사용되지 않는 한 Azure Portal을 사용하여 [사용자 지정 로그 검색 경고 규칙](../alerts/alerts-log.md)을 만들 때 검색 쿼리에서 지원되지 않습니다.

## <a name="examples"></a>예제

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>다음 단계

- Log Analytics 작업 영역을 가리키는 [작업 영역 식](../logs/workspace-expression.md)을 참조하세요.
- [Azure Monitor 데이터](../log-query/log-query-overview.md)가 저장되는 방법에 대해 알아보세요.
- [ 쿼리 언어](/azure/kusto/query/)에 대한 전체 문서에 액세스합니다.

