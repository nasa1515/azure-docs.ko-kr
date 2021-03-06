---
title: 서비스 공급자 보기 및 관리
description: 고객은 Azure Portal의 서비스 공급자 페이지를 사용하여 서비스 공급자, 서비스 공급자 제공 및 위임된 리소스의 정보를 볼 수 있습니다.
ms.date: 02/16/2021
ms.topic: how-to
ms.openlocfilehash: f6ee5fb154d75ff715acf99c5184cd1652ccdb80
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100555583"
---
# <a name="view-and-manage-service-providers"></a>서비스 공급자 보기 및 관리

[Azure Portal](https://portal.azure.com) 의 **서비스 공급자** 페이지를 통해 고객은 [Azure Lighthouse](../overview.md)를 사용 하는 서비스 공급자를 제어 하 고 표시 유형을 확인할 수 있습니다. 고객은 서비스 공급자에 대 한 세부 정보를 보고, 특정 리소스를 위임 하 고, 새 제품을 찾고, 서비스 공급자 액세스를 제거할 수 있습니다.

Azure Portal에서 **서비스 공급자** 페이지를 보려면 **모든 서비스** 를 선택한 다음 **서비스 공급자** 를 검색 하 고 선택 합니다. Azure Portal 맨 위에 있는 검색 상자에 "서비스 공급자" 또는 "Azure Lighthouse"를 입력 하 여이 페이지를 찾을 수도 있습니다.

> [!NOTE]
> **서비스 공급자** 페이지를 보려면 고객 테 넌 트의 사용자에 게 [판독기 기본 제공 역할](../../role-based-access-control/built-in-roles.md#reader) (또는 읽기 권한자 액세스를 포함 하는 다른 기본 제공 역할)이 있어야 합니다.
>
> 제안을 추가 하거나 업데이트 하 고, 리소스를 위임 하 고, 제안을 제거 하려면 사용자에 게 `Microsoft.Authorization/roleAssignments/write` [소유자](../../role-based-access-control/built-in-roles.md#owner)와 같은 권한이 있는 역할이 있어야 합니다.

**서비스 공급자** 페이지에는 Azure Lighthouse를 통해 고객의 구독 또는 리소스 그룹에 대 한 액세스 권한이 있는 서비스 공급자에 대 한 정보만 표시 됩니다. 고객이 Azure Lighthouse를 사용 하지 않는 추가 서비스 공급자와 함께 작업 하는 경우 해당 서비스 공급자에 대 한 정보가 여기에 표시 되지 않습니다.

## <a name="view-service-provider-details"></a>서비스 공급자 세부 정보 보기

Azure Lighthouse를 사용 하 여 고객의 테 넌 트에 대해 작업 하는 현재 서비스 공급자에 대 한 세부 정보를 보려면 **서비스** 공급자 페이지의 왼쪽에서 **서비스 공급자 제안** 을 선택 합니다.

각 제품에 대해 서비스 공급자의 이름 및 연결 된 제품을 볼 수 있습니다. 서비스 공급자에 게 부여 된 역할 할당을 포함 하 여 설명 및 기타 세부 정보를 볼 수 있는 제품을 선택할 수 있습니다.

**위임 열에서** 해당 제품의 서비스 공급자에 게 위임 된 구독 및/또는 리소스 그룹의 수를 확인할 수 있습니다. 서비스 공급자는 제품에 지정된 액세스 수준에 따라 이러한 구독 및/또는 리소스 그룹을 액세스하고 관리할 수 있습니다.

## <a name="add-or-remove-service-provider-offers"></a>서비스 공급자 제품 추가 또는 제거

**서비스 공급자 제안** 페이지에서 새 서비스 공급자 제품을 추가 하려면 **제품 추가** 를 선택 합니다. **개인 제안** 을 선택 하 여 서비스 공급자가이 고객에 대해 게시 한 제품을 봅니다. 그런 다음 **개인 제품** 화면에서 해당 제품을 선택 하 고 **설정 + 구독** 을 선택할 수 있습니다.

해당 제품에 대 한 행에서 휴지통 아이콘을 선택 하 여 언제 든 지 서비스 공급자 제품을 제거할 수 있습니다. 삭제를 확인 한 후에는 해당 서비스 공급자가 이전에 해당 제품에 대해 위임 된 리소스에 더 이상 액세스할 수 없습니다.

## <a name="delegate-resources"></a>리소스 위임

서비스 공급자가 고객의 리소스에 액세스 하 고 관리할 수 있으려면 먼저 하나 이상의 특정 구독 및/또는 리소스 그룹을 위임 해야 합니다. 고객이 제안을 수락 했지만 아직 리소스를 위임 하지 않은 경우 **서비스 공급자 제안** 섹션의 맨 위에 메모를 표시 합니다. 이를 통해 고객은 서비스 공급자가 고객의 모든 리소스에 액세스하기 위해서는 먼저 작업을 수행해야 한다는 사실을 알 수 있습니다.

구독 또는 리소스 그룹을 제외하려면

1. 서비스 공급자, 제품 및 이름을 포함하는 행의 확인란을 선택합니다. 그런 다음, 화면 위쪽의 **리소스 위임** 을 선택합니다.
1. **리소스 위임** 페이지의 **제품 세부 정보** 섹션에서 서비스 공급자 및 제품에 대한 세부 정보를 검토합니다. 제품에 대한 역할 할당을 검토하려면 **선택한 제품에 대한 세부 정보를 보려면 여기를 클릭하세요.** 를 선택합니다.
1. **위임** 섹션에서 **구독 위임** 또는 **리소스 그룹 위임** 을 선택합니다.
1. 이 제품에 대해 위임하려는 구독 및/또는 리소스 그룹을 선택한 다음, **추가** 를 선택합니다.
1. 페이지 맨 아래에 있는 확인란을 선택하여 이 서비스 공급자에게 선택한 리소스에 대한 액세스 권한을 부여할 것인지 확인한 다음, **위임** 을 선택합니다.

## <a name="update-service-provider-offers"></a>서비스 공급자 제품 업데이트

고객이 제품을 추가한 후 서비스 공급자는 동일한 제품의 업데이트된 버전을 Azure Marketplace에 게시할 수 있습니다. 예를 들어, 새 역할 정의를 추가하려고 할 수 있습니다. 새 버전의 제품이 게시 된 경우 **서비스 공급자 제공** 페이지에 해당 제품에 대 한 행에 "업데이트" 아이콘이 표시 됩니다. 최신 버전의 제품 및 새 제품의 차이점을 확인 하려면이 아이콘을 선택 합니다.

 ![제품 업데이트 아이콘](../media/update-offer.jpg)

고객은 변경 내용을 검토한 후 새 버전으로 업데이트하도록 선택할 수 있습니다. 새 버전에 지정 된 권한 부여 및 기타 설정은 해당 제품에 대해 위임 된 모든 구독 및/또는 리소스 그룹에 적용 됩니다.

## <a name="view-delegations"></a>위임 보기

위임은 고객이 위임한 리소스에 대한 권한을 서비스 공급자에게 부여하는 역할 할당을 나타냅니다. 이 정보를 보려면 **서비스 공급자** 페이지의 왼쪽에서 **위임** 을 선택합니다.

페이지 맨 위에 있는 필터를 사용 하 여 위임 정보를 정렬 하 고 그룹화 할 수 있습니다. 특정 고객, 제품 또는 키워드를 기준으로 필터링 할 수도 있습니다.

> [!NOTE]
> Azure Portal 또는 Api를 통해 [위임 된 범위에 대 한 역할 할당을 볼](../../role-based-access-control/role-assignments-list-portal.md#list-role-assignments-at-a-scope) 때 이러한 역할이 부여 된 서비스 공급자 테 넌 트의 사용자 또는 이러한 역할 할당은 고객에 게 표시 되지 않습니다.

## <a name="audit-delegations-in-your-environment"></a>사용자 환경에서 위임 감사

고객은 Azure Lighthouse에 위임 된 구독 및/또는 리소스 그룹에 대 한 가시성을 확보 하고자 할 수 있습니다. 이는 많은 수의 구독이 있는 고객 또는 관리 작업을 수행 하는 많은 사용자에 게 특히 유용 합니다.

[관리 테 넌 트에 대 한 범위 위임을 감사](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/Lighthouse_Delegations_Audit.json)하는 [Azure Policy 기본 제공 정책 정의](../../governance/policy/samples/built-in-policies.md#lighthouse) 를 제공 합니다. 감사 하려는 모든 구독이 포함 된 관리 그룹에이 정책을 할당할 수 있습니다. 이 정책에 대 한 준수 여부를 확인 하는 경우 정책이 할당 된 관리 그룹 내에서 위임 된 구독 및/또는 리소스 그룹이 비규격 상태로 표시 됩니다. 그런 다음 결과를 검토 하 고 예기치 않은 위임이 있는지 확인할 수 있습니다.

다른 [기본 제공 정책 정의](../../governance/policy/samples/built-in-policies.md#lighthouse) 를 사용 하면 [특정 관리 테 넌 트로 위임을 제한할](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/AllowCertainManagingTenantIds_Deny.json)수 있습니다. 이 정책은 마찬가지로 위임을 제한 하려는 모든 구독이 포함 된 관리 그룹에도 적용할 수 있습니다. 정책이 배포 된 후에는 사용자가 지정 하는 항목의 외부에 있는 테 넌 트에 구독을 위임 하려는 시도는 거부 됩니다.

정책을 할당 하 고 준수 상태 결과를 확인 하는 방법에 대 한 자세한 내용은 [빠른 시작: 정책 할당 만들기](../../governance/policy/assign-policy-portal.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Lighthouse](../overview.md)에 대해 자세히 알아봅니다.
- [서비스 공급자 작업을 감사](view-service-provider-activity.md)하는 방법을 알아봅니다.
- 서비스 공급자가 Azure Portal **내 고객** 페이지에서 [고객을 보고 관리할](view-manage-customers.md) 수 있는 방법에 대해 알아봅니다.
- [여러 테 넌 트를 관리](../concepts/enterprise.md) 하는 기업이 Azure Lighthouse를 사용 하 여 관리 환경을 통합 하는 방법을 알아봅니다.

