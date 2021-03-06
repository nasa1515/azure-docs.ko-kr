---
title: 앱을 게시자 확인으로 표시 - Microsoft ID 플랫폼 | Azure
description: 앱을 게시자 확인으로 표시하는 방법에 대해 설명합니다. 애플리케이션이 게시자 확인으로 표시되어 있으면 게시자가 Microsoft 파트너 네트워크 계정을 사용하여 확인 프로세스를 완료했으며 애플리케이션 등록과 연결된 MPN 계정과 연결된 ID를 확인하고 있는 것입니다.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: 2b658cdc58777274bb14f9e8069cef2facdb0479
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92363456"
---
# <a name="mark-your-app-as-publisher-verified"></a>앱을 게시자 확인으로 표시

앱 등록에 확인 된 게시자가 있는 경우 앱 게시자는 해당 Microsoft 파트너 네트워크 (MPN) 계정을 사용 하 여 id를 [확인](/partner-center/verification-responses) 하 고이 MPN 계정을 앱 등록과 연결 했음을 의미 합니다. 이 문서에서는 [게시자 확인](publisher-verification-overview.md) 프로세스를 완료 하는 방법을 설명 합니다.

## <a name="quickstart"></a>빠른 시작
이미 Microsoft 파트너 네트워크 (MPN)에 등록되어 있고 [필수 조건](publisher-verification-overview.md#requirements)을 충족한 경우 바로 시작할 수 있습니다. 

1. [Multi-factor authentication](../fundamentals/concept-fundamentals-mfa-get-started.md) 을 사용 하 여 [앱 등록 포털](https://aka.ms/PublisherVerificationPreview) 에 로그인

1. 앱을 선택하고 **브랜딩**을 클릭합니다. 

1. **MPN ID를 추가하여 게시자 확인**을 클릭하고 나열된 요구사항을 검토합니다.

1. MPN ID를 입력하고 **확인 및 저장**을 클릭합니다.

구체적인 혜택, 요구 사항, 질문과 대답에 대한 자세한 내용은 [개요](publisher-verification-overview.md)를 확인하십시오.


## <a name="mark-your-app-as-publisher-verified"></a>앱을 게시자 확인으로 표시
[필수 조건](publisher-verification-overview.md#requirements)을 충족하는지 확인하고 다음 단계에 따라 앱을 게시자 확인으로 표시합니다.  

1. 파트너 센터의 MPN 계정에 게시자 확인으로 표시 하려는 앱을 변경할 수 있는 권한이 있는 조직 (Azure AD) 계정에 대 한 [multi-factor authentication](../fundamentals/concept-fundamentals-mfa-get-started.md) 을 사용 하 여 로그인 했는지 확인 합니다.

    - Azure AD에서이 사용자는 응용 프로그램 관리, 클라우드 응용 프로그램 관리자, 전역 관리자 [역할](../roles/permissions-reference.md)중 하나의 멤버 여야 합니다. 

    - 파트너 센터에서 이 사용자는 [역할](/partner-center/permissions-overview)로 MPN 관리자, 계정 관리자 또는 글로벌 관리자(Azure AD의 공유 역할) 중 하나가 할당되어야 합니다. 

1. 앱 등록 포털로 이동 합니다.  

1. 게시자 확인으로 표시하고자 하는 앱을 클릭하고 브랜딩 블레이드를 엽니다. 

1. 앱의 [게시자 도메인이](howto-configure-publisher-domain.md) 설정 되어 있는지 확인 합니다. 

1. 테 넌 트의 게시자 도메인 또는 DNS 확인 [사용자 지정 도메인이](../fundamentals/add-custom-domain.md) MPN 계정에 대 한 확인 프로세스 중에 사용 된 전자 메일 주소의 도메인과 일치 하는지 확인 합니다.

1. 페이지 아래쪽의 **MPN ID를 추가하여 게시자 확인**을 클릭합니다. 

1. **MPN ID**를 입력합니다. 이 MPN ID는 

    - 확인 프로세스를 완료한 유효한 Microsoft 파트너 네트워크 계정이어야 합니다.  

    - 조직의 PGA(파트너 글로벌 계정)이어야 합니다. 

1. **확인 및 저장**을 클릭합니다. 

1. 프로세스 요청을 기다려 주세요. 이 작업은 몇 분 정도 걸릴 수 있습니다. 

1. 확인에 성공하면 게시자 확인 창이 닫히고 브랜딩 블레이드로 돌아갑니다. 확인된 **게시자 표시 이름** 옆에 파란색 확인 배지가 표시됩니다. 

1. 시스템 전체에 복제되는 데에는 다소 시간이 걸릴 수 있지만 성공적으로 프로세스를 마치면 앱에 동의한 사용자에게도 배지가 표시됩니다. 

1. 이 기능을 확인하려면 애플리케이션에 로그인하고 동의 화면에 확인 배지가 표시되는지 확인합니다. 이미 앱에 동의를 부여한 사용자로 로그인하면 *prompt=consent* 쿼리 매개 변수를 사용하여 동의 확인 프롬프트가 나타나도록 할 수 있습니다. 이 매개 변수는 테스트 전용 이며 앱의 요청에 하드 코딩 되지 않습니다.

1. 배지가 표시되어야 하는 다른 앱에서도 필요에 따라 이 프로세스를 반복합니다. 대량으로 이를 수행하려면 Microsoft Graph를 사용할 수도 있으며 PowerShell cmdlets는 곧 제공될 예정입니다. 자세한 내용은 [Microsoft API Graph 호출](troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls)을 확인하십시오. 

끝났습니다. 일반적인 프로세스, 결과, 기능에 대한 피드백이 있다면 알려주십시오. 

## <a name="next-steps"></a>다음 단계
문제가 발생할 경우 [문제 해결 정보](troubleshoot-publisher-verification.md)를 읽어보십시오.
