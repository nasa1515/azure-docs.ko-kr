---
title: Azure 센티널에 Okta Single Sign-On 데이터 연결 | Microsoft Docs
description: Azure 센티널에 Okta Single Sign-On 데이터를 연결 하는 방법에 대해 알아봅니다.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: e76aea8a3fc59827664900a6d5686e2e725e258d
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100093118"
---
# <a name="connect-your-okta-single-sign-on-to-azure-sentinel-with-azure-function"></a>Azure Function을 사용 하 여 Azure 센티널에 Okta Single Sign-On 연결

> [!IMPORTANT]
> Azure 센티널의 Okta Single Sign-On 데이터 커넥터는 현재 공개 미리 보기로 제공 됩니다.
> 이 기능은 서비스 수준 계약 없이 제공 되며 프로덕션 워크 로드에는 권장 되지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

Okta SSO (Single Sign-On) 커넥터를 사용 하면 모든 [Okta sso (single Sign-On](https://www.okta.com/products/single-sign-on/) ) 보안 솔루션 로그를 Azure 센티널과 쉽게 연결 하 고, 대시보드를 보고, 사용자 지정 경고를 만들고, 조사를 개선할 수 있습니다. Okta Single Sign-On와 Azure 센티널 간의 통합을 통해 REST API를 사용 하 여 로그 데이터를 끌어오는 Azure Functions를 사용 합니다.

> [!NOTE]
> 데이터는 Azure 센티널을 실행 하는 작업 영역의 지리적 위치에 저장 됩니다.

## <a name="configure-and-connect-okta-single-sign-on"></a>Okta Single Sign-On 구성 및 연결

Azure Functions은 Okta 단일 Sign-On에서 직접 이벤트와 로그를 통합 하 고 가져와 Azure 센티널에 전달할 수 있습니다.

1. Azure 센티널 포털에서 **데이터 커넥터** 를 클릭 하 고 **Okta Single sign-on** 커넥터를 선택 합니다.

1. **커넥터 페이지 열기** 를 선택 합니다.

1. **Okta Single sign-on** 페이지의 지침을 따릅니다.

## <a name="find-your-data"></a>데이터 찾기

연결이 설정 되 면 데이터가 **Okta_CL** 테이블 아래 Log Analytics에 나타납니다.

## <a name="validate-connectivity"></a>연결 유효성 검사

로그가 Log Analytics 나타날 때까지 최대 20 분이 걸릴 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 Azure 함수 앱을 사용 하 여 Azure 센티널에 Okta Single Sign-On을 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.

- [데이터에 대한 가시성을 얻고 재적 위협을 확인](quickstart-get-visibility.md)하는 방법을 알아봅니다.
- [Azure Sentinel을 사용하여 위협 검색](tutorial-detect-threats-built-in.md)을 시작합니다.
- [통합 문서를 사용](tutorial-monitor-your-data.md)하여 데이터를 모니터링합니다.

