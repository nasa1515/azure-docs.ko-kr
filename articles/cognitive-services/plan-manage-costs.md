---
title: Azure Cognitive Services에 대 한 비용 관리 계획
description: Azure Portal에서 비용 분석을 사용 하 여 Azure Cognitive Services에 대 한 비용을 계획 하 고 관리 하는 방법을 알아봅니다.
author: erhopf
ms.author: erhopf
ms.custom: subject-cost-optimization
ms.service: cognitive-services
ms.topic: how-to
ms.date: 12/15/2020
ms.openlocfilehash: 661af89ad223978abbefd71dd8008577475c5875
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99221386"
---
# <a name="plan-and-manage-costs-for-azure-cognitive-services"></a>Azure Cognitive Services에 대 한 비용 계획 및 관리

이 문서에서는 Azure Cognitive Services에 대 한 비용을 계획 하 고 관리 하는 방법을 설명 합니다. 먼저 Azure 가격 계산기를 사용 하 여 비용을 예측 하는 서비스에 대 한 리소스를 추가 하기 전에 Cognitive Services 비용을 계획할 수 있습니다. 그런 다음 Azure 리소스를 추가할 때 예상 비용을 검토 합니다. Cognitive Services 리소스 (예: Speech, Computer Vision, LUIS, Text Analytics, Translator 등) 사용을 시작한 후에는 Cost Management 기능을 사용 하 여 예산을 설정 하 고 비용을 모니터링할 수 있습니다. 예상 비용을 검토 하 고 지출 추세를 파악 하 여 작업할 수 있는 영역을 식별할 수도 있습니다. Cognitive Services 비용은 Azure 청구서의 월별 비용 중 일부에 불과합니다. 이 문서에서는 Cognitive Services에 대 한 비용을 계획 하 고 관리 하는 방법을 설명 하지만, 타사 서비스를 비롯 하 여 Azure 구독에 사용 되는 모든 Azure 서비스 및 리소스에 대해 요금이 청구 됩니다.

## <a name="prerequisites"></a>전제 조건

Cost Management의 비용 분석은 대부분의 Azure 계정 유형을 지원 하지만 일부는 지원 하지 않습니다. 지원되는 계정 유형의 전체 목록을 보려면 [Cost Management 데이터 이해](../cost-management-billing/costs/understand-cost-mgt-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)를 참조하세요. 비용 데이터를 보려면 최소한 Azure 계정에 대 한 읽기 권한이 있어야 합니다. Azure Cost Management 데이터에 액세스하는 방법에 대한 정보는 [데이터에 대한 액세스 할당](../cost-management-billing/costs/assign-access-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)을 참조하세요.

<!--Note for Azure service writer: If you have other prerequisites for your service, insert them here -->

## <a name="estimate-costs-before-using-cognitive-services"></a>Cognitive Services를 사용 하기 전에 비용 추정

Cognitive Services를 추가 하기 전에 [Azure 가격 계산기](https://azure.microsoft.com/pricing/calculator/) 를 사용 하 여 비용을 예측 합니다.

:::image type="content" source="media/cognitive-services-pricing-calculator.png" alt-text="Cognitive Services에 대 한 Azure 가격 계산기" border="true":::

작업 영역에 새 리소스를 추가할 때이 계산기로 돌아가서 동일한 리소스를 추가 하 여 비용 추정치를 업데이트 합니다.

자세한 내용은 [Azure Cognitive Services 가격 책정](https://azure.microsoft.com/pricing/details/cognitive-services/)을 참조 하세요.

## <a name="understand-the-full-billing-model-for-cognitive-services"></a>Cognitive Services에 대 한 전체 청구 모델 이해

Cognitive Services는 Azure 인프라에서 실행 되며 새 리소스를 배포할 때 [비용이](https://azure.microsoft.com/pricing/details/cognitive-services/) 발생 합니다. 추가 인프라가 비용을 계산 하는 것을 이해 하는 것이 중요 합니다. 배포 된 리소스를 변경할 때이 비용을 관리 해야 합니다. 

### <a name="costs-that-typically-accrue-with-cognitive-services"></a>일반적으로 Cognitive Services으로 계산 되는 비용

일반적으로 Azure 리소스를 배포한 후에는 가격 책정 계층 및 끝점에 대 한 API 호출에 따라 비용이 결정 됩니다. 사용 중인 서비스에 약정 계층이 있는 경우 계층에서 할당 된 호출을 초과분 요금은 청구 될 수 있습니다.

이러한 서비스를 사용 하는 경우 추가 비용이 발생할 수 있습니다.

#### <a name="qna-maker"></a>QnA Maker

QnA Maker에 대 한 리소스를 만들 때 다른 Azure 서비스에 대 한 리소스를 만들 수도 있습니다. 다음과 같은 변경 내용이 해당됩니다.

- [Azure App Service (런타임)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (데이터)](https://azure.microsoft.com/pricing/details/search/)
 
### <a name="costs-that-might-accrue-after-resource-deletion"></a>리소스 삭제 후 발생할 수 있는 비용

#### <a name="qna-maker"></a>QnA Maker

QnA Maker 리소스를 삭제 한 후에는 다음 리소스가 계속 존재할 수 있습니다. 삭제할 때까지 비용이 계속 해 서 계산 됩니다.

- [Azure App Service (런타임)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Cognitive Search (데이터)](https://azure.microsoft.com/pricing/details/search/)

### <a name="using-azure-prepayment-credit-with-cognitive-services"></a>Cognitive Services Azure 선불 크레딧을 사용

Azure 선불 (이전에는 금액 약정 이라고 함) 크레딧을 사용 하 여 Cognitive Services 요금을 지불할 수 있습니다. 그러나 Azure 선불 크레딧을 사용 하 여 Azure Marketplace에서 사용 하는 타사 제품 및 서비스에 대 한 요금을 지불할 수 없습니다.

## <a name="monitor-costs"></a>비용 모니터링

<!-- Note to Azure service writer: Modify the following as needed for your service. Replace example screenshots with ones taken for your service. If you need assistance capturing screenshots, ask banders for help. -->

Cognitive Services에서 Azure 리소스를 사용 하는 경우 비용이 발생 합니다. Azure 리소스 사용 단위 비용은 시간 간격 (초, 분, 시간 및 일) 또는 단위 사용 (바이트, 메가바이트 등)에 따라 달라 집니다. 인식 서비스 (또는 Cognitive Services)를 사용 하는 즉시 비용이 발생 하며 비용 [분석](../cost-management/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)에서 비용을 볼 수 있습니다.

비용 분석을 사용 하면 서로 다른 시간 간격에 대 한 그래프 및 테이블의 Cognitive Services 비용을 볼 수 있습니다. 몇 가지 예로는 일, 현재 및 이전 달 및 연도가 있습니다. 또한 예산 및 예상 비용에 대 한 비용도 볼 수 있습니다. 시간이 지남에 따라 더 긴 뷰로 전환 하면 지출 추세를 식별 하는 데 도움이 됩니다. 그리고 과도 한 지출이 발생 한 위치를 확인할 수 있습니다. 예산을 만든 경우에는 해당 위치를 쉽게 확인할 수도 있습니다.

비용 분석에서 Cognitive Services 비용을 보려면 다음을 수행 합니다.

1. Azure Portal에 로그인합니다.
2. Azure Portal에서 범위를 열고 메뉴에서 **비용 분석** 을 선택 합니다. 예를 들어 **구독** 으로 이동 하 여 목록에서 구독을 선택한 다음 메뉴에서  **비용 분석** 을 선택 합니다. 비용 분석에서 다른 범위로 전환 하려면 **범위** 를 선택 합니다.
3. 기본적으로 서비스 비용은 첫 번째 도넛형 차트에 표시 됩니다. Cognitive Services 레이블 차트에서 영역을 선택 합니다.

처음에는 비용 분석을 열 때 실제 월별 비용이 표시 됩니다. 모든 월간 사용 비용을 보여 주는 예제는 다음과 같습니다.

:::image type="content" source="./media/cost-management/all-costs.png" alt-text="구독에 대 한 누적 비용을 보여 주는 예제":::

- Cognitive Services와 같은 단일 서비스에 대 한 비용을 좁히려면 **필터 추가** 를 선택 하 고 **서비스 이름** 을 선택 합니다. 그런 다음 **Cognitive Services** 를 선택 합니다.

Cognitive Services에 대 한 비용을 보여 주는 예제는 다음과 같습니다.

:::image type="content" source="./media/cost-management/cognitive-services-costs.png" alt-text="Cognitive Services에 대 한 누적 비용을 보여 주는 예제":::

앞의 예제에서 서비스에 대 한 현재 비용이 표시 됩니다. Azure 지역 (위치) 및 리소스 그룹별 비용 Cognitive Services 비용도 표시 됩니다. 여기에서 비용을 직접 살펴볼 수 있습니다.

## <a name="create-budgets"></a>예산 만들기

[예산을](../cost-management-billing/costs/tutorial-acm-create-budgets.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) 만들어 비용을 관리 하 고 관련자에 게 잘못 된 지출 및 과도 한 지출 위험을 자동으로 알리는 [경고](../cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) 를 만들 수 있습니다. 경고는 예산 및 비용 임계값에 따른 지출을 기준으로 합니다. Azure 구독 및 리소스 그룹에 대 한 예산과 경고가 생성 되므로 전체 비용 모니터링 전략의 일부로 유용 합니다. 

모니터링에 세분성이 더 필요한 경우 Azure의 특정 리소스 또는 서비스에 대 한 필터를 사용 하 여 예산을 만들 수 있습니다. 필터를 통해 추가 비용을 절감할 수 있는 새 리소스를 실수로 만들지 않도록 할 수 있습니다. 예산 생성 시의 필터 옵션에 대 한 자세한 내용은 [그룹 및 필터 옵션](../cost-management-billing/costs/group-filter.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)을 참조 하세요.

## <a name="export-cost-data"></a>비용 데이터 내보내기

[비용 데이터](../cost-management-billing/costs/tutorial-export-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) 를 저장소 계정으로 내보낼 수도 있습니다. 이는 또는 다른 사용자가 비용에 대 한 추가 데이터 분석을 수행 해야 하는 경우에 유용 합니다. 예를 들어 재무 팀은 Excel 또는 Power BI를 사용 하 여 데이터를 분석할 수 있습니다. 매일, 매주 또는 매월 일정에 대 한 비용을 내보내고 사용자 지정 날짜 범위를 설정할 수 있습니다. 비용 데이터를 내보내는 것은 비용 데이터 집합을 검색 하는 데 권장 되는 방법입니다.

## <a name="next-steps"></a>다음 단계

- [Azure Cost Management를 사용 하 여 클라우드 투자를 최적화 하는 방법을](../cost-management-billing/costs/cost-mgt-best-practices.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)알아봅니다.
- [비용 분석](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)으로 비용 관리에 대해 자세히 알아보세요.
- [예기치 않은 비용을 방지](../cost-management-billing/cost-management-billing-overview.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)하는 방법에 대해 알아봅니다.
- [Cost Management](/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) 단계별 학습 과정을 수행 하세요.