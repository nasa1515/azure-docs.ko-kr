---
title: 응용 프로그램 변경 분석에 대 한 시각화-Azure Monitor
description: Azure Monitor의 응용 프로그램 변경 분석에서 시각화를 사용 하는 방법에 대해 알아봅니다.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 02/11/2021
ms.openlocfilehash: 643645eb3b361cd68def1177ba7e8af89f1963bc
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100521090"
---
# <a name="visualizations-for-application-change-analysis-preview"></a>응용 프로그램 변경 분석 (미리 보기)에 대 한 시각화

## <a name="standalone-ui"></a>독립 실행형 UI

Azure Monitor에는 응용 프로그램 종속성 및 리소스에 대 한 정보를 포함 하는 모든 변경 내용을 볼 수 있는 독립 실행형 창이 있습니다.

Azure Portal의 검색 창에서 변경 분석을 검색 하 여 환경을 시작 합니다.

![Azure Portal에서 변경 분석 검색의 스크린샷](./media/change-analysis/search-change-analysis.png)

지난 24 시간의 변경 내용과 함께 선택한 구독의 모든 리소스가 표시 됩니다. 페이지 로드 성능을 최적화 하기 위해 서비스는 한 번에 10 개의 리소스를 표시 합니다. 다음 페이지를 선택 하 여 더 많은 리소스를 봅니다. 이 제한을 제거 하기 위해 노력 하 고 있습니다.

![Azure Portal의 변경 분석 블레이드 스크린샷](./media/change-analysis/change-analysis-standalone-blade.png)

리소스를 클릭 하 여 모든 변경 내용을 확인 합니다. 필요한 경우 변경 내용으로 드릴 다운 하 여 json 형식 변경 정보 및 정보를 확인 합니다.

![변경 세부 정보 스크린샷](./media/change-analysis/change-details.png)

사용자 의견은 사용자 의견 보내기 단추 또는 전자 메일을 사용 changeanalysisteam@microsoft.com 합니다.

![변경 분석 탭의 피드백 단추 스크린샷](./media/change-analysis/change-analysis-feedback.png)

### <a name="multiple-subscription-support"></a>여러 구독 지원

UI는 여러 구독을 선택 하 여 리소스 변경 내용을 볼 수 있도록 지원 합니다. 구독 필터를 사용 합니다.

![여러 구독 선택을 지 원하는 구독 필터 스크린샷](./media/change-analysis/multiple-subscriptions-support.png)

### <a name="web-app-diagnose-and-solve-problems"></a>웹 앱 문제 진단 및 해결

Azure Monitor에서 변경 분석은 셀프 서비스 **진단 및 문제 해결** 환경에도 기본적으로 제공 됩니다. App Service 응용 프로그램의 **개요** 페이지에서이 환경에 액세스 합니다.

!["개요" 단추의 스크린샷 및 "문제 진단 및 해결" 단추](./media/change-analysis/change-analysis.png)

## <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>문제 진단 및 해결 도구의 응용 프로그램 변경 분석

응용 프로그램 변경 분석은 웹 앱 진단 및 문제 해결 도구의 독립 실행형 탐지기입니다. 또한 **응용 프로그램 충돌** 및 **웹 앱 다운 감지기** 집계 됩니다. 진단 및 문제 해결 도구를 입력 하면 **Microsoft. ChangeAnalysis** 리소스 공급자가 자동으로 등록 됩니다. 웹 앱 게스트 변경 내용 추적을 사용 하도록 설정 하려면 다음 지침을 따르세요.

1. **가용성 및 성능을** 선택 합니다.

    !["가용성 및 성능" 문제 해결 옵션의 스크린샷](./media/change-analysis/availability-and-performance.png)

2. **응용 프로그램 변경 내용** 을 선택 합니다. **응용 프로그램 작동 중단** 에서도이 기능을 사용할 수 있습니다.

   !["응용 프로그램 작동 중단" 단추의 스크린샷](./media/change-analysis/application-changes.png)

3. 이 링크를 누르면 응용 프로그램 변경 분석 UI 범위가 웹 앱으로 지정 됩니다. 웹 앱에서 게스트 변경 내용 추적을 사용 하도록 설정 하지 않은 경우 배너를 따라 파일 및 앱 설정 변경 내용을 가져옵니다.

   !["응용 프로그램 작동 중단" 옵션 스크린샷](./media/change-analysis/enable-changeanalysis.png)

4. **변경 분석** 을 설정 하 고 **저장** 을 선택 합니다. 도구는 App Service 계획의 모든 웹 앱을 표시 합니다. 계획 수준 스위치를 사용 하 여 계획의 모든 웹 앱에 대 한 변경 분석을 켤 수 있습니다.

    !["변경 분석 설정" 사용자 인터페이스의 스크린샷](./media/change-analysis/change-analysis-on.png)

5. 변경 데이터는 **웹 앱 다운** 및 **응용 프로그램 크래시** 감지기 에서도 사용할 수 있습니다. 시간에 따른 변경 내용 유형과 해당 변경 내용에 대 한 세부 정보를 요약 하는 그래프가 표시 됩니다. 기본적으로 지난 24 시간 동안의 변경 내용은 즉각적인 문제를 해결 하기 위해 표시 됩니다.

     ![Diff 뷰 변경의 스크린샷](./media/change-analysis/change-view.png)

## <a name="virtual-machine-diagnose-and-solve-problems"></a>가상 컴퓨터 진단 및 문제 해결

가상 컴퓨터에 대 한 문제 진단 및 해결 도구로 이동 합니다.  **문제 해결 도구** 로 이동 하 여 페이지를 탐색 하 고 **최근 변경 내용 분석** 을 선택 하 여 가상 컴퓨터에 대 한 변경 내용을 확인 합니다.

![VM의 스크린샷 진단 및 문제 해결](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![문제 해결 도구의 analyzer 변경](./media/change-analysis/analyze-recent-changes.png)

## <a name="activity-log-change-history"></a>활동 로그 변경 기록

활동 로그의 [변경 기록 보기](../platform/activity-log.md#view-change-history) 기능은 응용 프로그램 변경 분석 서비스 백 엔드를 호출 하 여 작업과 연결 된 변경 내용을 가져옵니다. [Azure 리소스 그래프](../../governance/resource-graph/overview.md) 를 직접 호출 하는 데 사용 되는 **변경** 내용 이지만, 백 엔드가 교체 되어 응용 프로그램 변경 분석을 호출 하는 경우, 반환 되는 변경 내용에는 [azure 리소스 그래프](../../governance/resource-graph/overview.md)의 리소스 수준 변경, [Azure Resource Manager](../../azure-resource-manager/management/overview.md)의 리소스 속성 및 App Services 웹 앱과 같은 PaaS 서비스에서 게스트 변경 내용이 포함 됩니다. 응용 프로그램 변경 분석 서비스에서 사용자 구독의 변경 내용을 검색할 수 있도록 하려면 리소스 공급자를 등록 해야 합니다. **변경 기록** 탭을 처음으로 입력 하면 도구에서 자동으로 **Microsoft. changeanalysis** 리소스 공급자를 등록 하기 시작 합니다. 등록 후 **Azure 리소스 그래프** 의 변경 내용이 즉시 제공 되며 지난 14 일이 포함 됩니다. 구독을 등록 한 후 4 시간 이내에 다른 원본의 변경 내용을 사용할 수 있습니다.

![활동 로그 변경 기록 통합](./media/change-analysis/activity-log-change-history.png)

## <a name="vm-insights-integration"></a>VM Insights 통합

[VM Insights](../insights/vminsights-overview.md) 를 사용 하도록 설정 된 사용자는 CPU 또는 메모리와 같은 메트릭 차트에서 급증을 일으킬 수 있는 가상 머신에서 변경 된 내용을 볼 수 있습니다. 변경 데이터는 VM Insights 쪽 탐색 모음에 통합 되어 있습니다. 사용자는 VM에서 변경 된 내용이 있는지 확인 하 고 **변경 내용 조사** 를 선택 하 여 응용 프로그램 변경 분석 독립 실행형 UI에서 변경 정보를 볼 수 있습니다.

[![VM insights 통합](./media/change-analysis/vm-insights.png)](./media/change-analysis/vm-insights.png#lightbox)

## <a name="next-steps"></a>다음 단계

- [변경 분석의 문제를 해결](change-analysis-troubleshoot.md) 하는 방법 알아보기
