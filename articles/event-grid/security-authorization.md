---
title: Azure Event Grid 보안 및 인증
description: Azure Event Grid 및 해당 개념을 설명합니다.
ms.topic: conceptual
ms.date: 02/12/2021
ms.openlocfilehash: 326fa00645302eb4b9c9bc59f17c1ca153bdb0b7
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100371723"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Event Grid 리소스에 대 한 액세스 권한 부여
Azure Event Grid를 사용 하면 다양 한 사용자가 이벤트 구독 나열, 새 구독 만들기 및 키 생성과 같은 다양 한 **관리 작업** 을 수행 하는 데 지정 된 액세스 수준을 제어할 수 있습니다. Azure RBAC (역할 기반 액세스 제어)를 사용 하 Event Grid입니다.

> [!NOTE]
> EventGrid는 토픽 또는 도메인 Event Grid에 이벤트를 게시 하기 위해 Azure RBAC를 지원 하지 않습니다. SAS (공유 액세스 서명) 키 또는 토큰을 사용 하 여 이벤트를 게시 하는 클라이언트를 인증 합니다. 자세한 내용은 [게시 클라이언트 인증](security-authenticate-publishing-clients.md)을 참조 하세요. 

## <a name="operation-types"></a>작업 형식
Azure Event Grid에서 지원 되는 작업 목록은 다음 Azure CLI 명령을 실행 합니다. 

```azurecli-interactive
az provider operation show --namespace Microsoft.EventGrid
```

다음 작업은 일반 읽기 작업에서 필터링 되는 잠재적 비밀 정보를 반환 합니다. 이 작업에 대한 액세스를 제한하는 것이 좋습니다. 

* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action


## <a name="built-in-roles"></a>기본 제공 역할

Event Grid는 이벤트 구독을 관리하기 위한 두 가지 기본 제공 역할을 제공합니다. 이벤트 도메인의 토픽을 구독 하는 데 필요한 사용 권한을 사용자에 게 부여 하므로 [이벤트 도메인](event-domains.md) 을 구현 하는 경우 중요 합니다. 이러한 역할은 이벤트 구독에 중점을 두며, 토픽 만들기와 같은 작업에 대한 액세스 권한을 부여하지 않습니다.

[사용자 또는 그룹에 이러한 역할을 할당](../role-based-access-control/quickstart-assign-role-user-portal.md)할 수 있습니다.

**Eventgrid Eventgrid 참여자**: Event Grid 구독 작업 관리

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/systemtopics/eventsubscriptions/*",
          "Microsoft.EventGrid/partnertopics/eventsubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**Eventgrid Eventgrid Reader**: Event Grid 구독 읽기

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="custom-roles"></a>사용자 지정 역할

기본 제공 역할이 아닌 다른 사용 권한을 지정해야 할 경우 사용자 지정 역할을 만들 수 있습니다.

사용자가 다른 동작을 수행할 수 있는 샘플 Event Grid 역할 정의는 다음과 같습니다. 이러한 사용자 지정 역할은 이벤트 구독보다 더 광범위한 액세스 권한을 부여하기 때문에 기본 제공 역할과는 다릅니다.

**EventGridReadOnlyRole.json**: 읽기 전용 작업만을 허용합니다.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: 제한된 게시 작업을 허용하지만 삭제 동작을 허용하지 않습니다.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: 모든 Event Grid 작업을 허용합니다.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

[PowerShell](../role-based-access-control/custom-roles-powershell.md), [Azure CLI](../role-based-access-control/custom-roles-cli.md) 및 [REST](../role-based-access-control/custom-roles-rest.md)를 사용하여 사용자 지정 역할을 만들 수 있습니다.



### <a name="encryption-at-rest"></a>휴지 상태의 암호화

Event Grid 서비스에서 디스크에 기록하는 모든 이벤트 또는 데이터는 Microsoft에서 관리하는 키로 암호화되어 암호화된 상태로 유지됩니다. 또한 이벤트 또는 데이터를 보존하는 최대 기간은 [Event Grid 재시도 정책](delivery-and-retry.md)을 준수하는 24시간입니다. Event Grid는 24시간 후에 모든 이벤트 또는 데이터를 자동으로 삭제하거나, 더 적은 이벤트 Time-to-Live를 삭제합니다.

## <a name="permissions-for-event-subscriptions"></a>이벤트 구독에 대 한 사용 권한
WebHook(예: 이벤트 허브 또는 Queue Storage)이 아닌 이벤트 처리기를 사용하는 경우 해당 리소스에 대한 쓰기 권한이 필요합니다. 이 권한 검사는 권한 없는 사용자가 리소스에 이벤트를 전송하지 못하도록 합니다.

이벤트 소스인 리소스에 **Microsoft.EventGrid/EventSubscriptions/Write** 권한이 있어야 합니다. 리소스의 범위에서 새 구독을 작성하기 때문에 이 권한이 있어야 합니다. 필요한 리소스는 시스템 항목 또는 사용자 지정 항목을 구독하는지 여부에 따라 다릅니다. 두 형식은 모두 이 섹션에 설명되어 있습니다.

### <a name="system-topics-azure-service-publishers"></a>시스템 항목(Azure 서비스 게시자)
시스템 항목의 경우 원본 리소스의 소유자 또는 참가자가 아닌 경우 이벤트를 게시 하는 리소스의 범위에서 새 이벤트 구독을 쓸 수 있는 권한이 있어야 합니다. 리소스의 형식은 다음과 같습니다. `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

예를 들어 **myacct** 라는 스토리지 계정에 이벤트를 구독하려면 `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`에 대한 Microsoft.EventGrid/EventSubscriptions/Write 권한이 필요합니다.

### <a name="custom-topics"></a>사용자 지정 항목
사용자 지정 항목의 경우 Event Grid 항목의 범위에서 새 이벤트 구독을 쓸 수 있는 권한이 필요합니다. 리소스의 형식은 다음과 같습니다. `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

예를 들어 **mytopic** 이라는 사용자 지정 항목을 구독하려면 `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`에 대한 Microsoft.EventGrid/EventSubscriptions/Write 권한이 필요합니다.



## <a name="next-steps"></a>다음 단계

* Event Grid에 대한 소개는 [Event Grid 정보](overview.md)를 참조하세요.
