---
title: Azure AD Multi-Factor Authentication 버전 및 소비 계획
description: Azure AD Multi-Factor Authentication 클라이언트 및 사용 가능한 다양 한 방법과 버전에 대해 알아봅니다.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63d880146a0b068a5d097c452c14b28db4907098
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96743958"
---
# <a name="features-and-licenses-for-azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication의 기능 및 라이선스

조직의 사용자 계정을 보호하려면 다단계 인증을 사용해야 합니다. 이 기능은 리소스에 대한 액세스 권한이 있는 계정에 특히 중요합니다. 기본 multi-factor authentication 기능은 추가 비용 없이 Azure AD (Microsoft 365 및 Azure Active Directory) 관리자에 게 제공 됩니다. 관리자의 기능을 업그레이드 하거나 multi-factor authentication을 나머지 사용자로 확장 하려는 경우 여러 가지 방법으로 Azure AD Multi-Factor Authentication를 구매할 수 있습니다.

> [!IMPORTANT]
> 이 문서에서는 Azure AD Multi-Factor Authentication를 사용 하 고 사용 하는 다양 한 방법에 대해 자세히 설명 합니다. 가격 책정 및 청구에 대 한 자세한 내용은 [AZURE AD Multi-Factor Authentication 가격 책정 페이지](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)를 참조 하세요.

## <a name="available-versions-of-azure-ad-multi-factor-authentication"></a>사용 가능한 Azure AD Multi-Factor Authentication 버전

Azure AD Multi-Factor Authentication는 조직의 요구에 따라 몇 가지 방법으로 사용 하 고 사용이 허가 될 수 있습니다. 현재 azure AD, EMS 또는 Microsoft 365 라이선스에 따라 Azure AD Multi-Factor Authentication를 사용 하도록 이미 자격이 있을 수 있습니다. 다음 표에서는 Azure AD Multi-Factor Authentication 및 각 기능에 대 한 몇 가지 기능 및 사용 사례를 가져오는 다양 한 방법을 자세히 설명 합니다.

| 제품 사용자 | 기능 및 사용 사례 |
| --- | --- |
| Microsoft 365 Business 프리미엄 및 EMS 또는 Microsoft 365 E3 및 E5 | EMS E3, Microsoft 365 E3 및 Microsoft 365 Business 프리미엄에는 Azure AD Premium P1이 포함 됩니다. EMS E5 또는 Microsoft 365 E5에는 Azure AD Premium P2가 포함됩니다. 다음 섹션에서 설명한 것과 동일한 조건부 액세스 기능을 사용하여 사용자에게 다단계 인증을 제공할 수 있습니다. |
| Azure AD Premium P1 | [Azure AD 조건부 액세스](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)를 사용하여 비즈니스 요구 사항에 맞게 특정 시나리오 또는 이벤트 중에 사용자에게 다단계 인증을 사용할지 묻는 메시지를 표시할 수 있습니다. |
| Azure AD Premium P2 | 가장 강력한 보안 위치 및 향상된 사용자 환경을 제공합니다. Azure AD Premium P1 기능에 사용자의 패턴에 맞게 조정되고 Multi-Factor Authentication 프롬프트를 최소화하는 [위험 기반 조건부 액세스](../conditional-access/howto-conditional-access-policy-risk.md)를 추가합니다. |
| 모든 Microsoft 365 계획 | Azure AD Multi-Factor Authentication는 [사용자 단위로 활성화](howto-mfa-userstates.md)하거나 [보안 기본값](../fundamentals/concept-fundamentals-security-defaults.md)을 사용 하는 모든 사용자에 대해 사용 하거나 사용 하지 않도록 설정할 수 있습니다. Azure AD Multi-Factor Authentication 관리는 Microsoft 365 포털을 통해 관리 됩니다. 향상된 사용자 환경을 위해 Azure AD Premium P1 또는 P2로 업그레이드하고 조건부 액세스를 사용합니다. 자세한 내용은 [multi-factor authentication을 사용 하 여 리소스 Microsoft 365 보안](/microsoft-365/admin/security-and-compliance/set-up-multi-factor-authentication)을 참조 하세요. |
| Azure AD Free | [보안 기본값](../fundamentals/concept-fundamentals-security-defaults.md) 을 사용 하 여 모든 사용자에 대해 multi-factor authentication을 사용 하도록 설정할 수 있습니다. 설정된 사용자 또는 시나리오를 세부적으로 제어할 수는 없지만 추가 보안 단계를 제공합니다.<br /> 보안 기본값을 사용하여 모든 사용자에 대해 다단계 인증을 사용하도록 설정하지 않는 경우에도 Azure AD 전역 관리자 역할이 할당된 사용자는 다단계 인증을 사용하도록 구성할 수 있습니다. 무료 계층의 이 기능을 사용하면 중요한 관리자 계정이 다단계 인증에 의해 보호됩니다. |

## <a name="feature-comparison-of-versions"></a>버전 기능 비교

다음 표에서는 다양 한 버전의 Azure AD Multi-Factor Authentication에서 사용할 수 있는 기능 목록을 제공 합니다. 사용자 인증을 보호하기 위한 요구 사항을 계획하고 해당 요구 사항을 충족하는 방법을 결정합니다. 예를 들어 Azure AD Free는 Azure AD Multi-Factor Authentication를 제공 하는 보안 기본값을 제공 하지만 휴대폰 통화 또는 SMS가 아닌 인증 프롬프트에는 모바일 authenticator 앱만 사용할 수 있습니다. 이 방법은 사용자의 개인 디바이스에 모바일 인증 앱이 설치되어 있는지 확인할 수 없는 경우에 제한이 있을 수 있습니다.

| 기능 | Azure AD Free - 보안 기본값 | Azure AD Free - Azure AD 전역 관리자 | Microsoft 365 앱 | Azure AD Premium P1 또는 P2 |
| --- |:---:|:---:|:---:|:---:|
| MFA를 사용하여 Azure AD 테넌트 계정 보호 | ● | ● (Azure AD 전역 관리자 계정에만 해당) | ● | ● |
| 두 번째 단계로 모바일 앱 | ● | ● | ● | ● |
| 두 번째 단계로 전화 통화 | | ● | ● | ● |
| 두 번째 단계로 SMS | | ● | ● | ● |
| 확인 방법에 대한 관리자 제어 | | ● | ● | ● |
| 사기 행위 경고 | | | | ● |
| MFA 보고서 | | | | ● |
| 전화 통화에 대한 사용자 지정 인사말 | | | | ● |
| 전화 통화에 대한 사용자 지정 발신자 번호 | | | | ● |
| 신뢰할 수 있는 IP | | | | ● |
| 신뢰할 수 있는 디바이스에 대한 MFA 기억 | | ● | ● | ● |
| 온-프레미스 애플리케이션에 대한 MFA | | | | ● |

## <a name="purchase-and-enable-azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication 구매 및 사용

Azure AD Multi-Factor Authentication를 사용 하려면 적격 Azure AD 계층에 등록 하거나 구매 합니다. Azure AD는 Free, Microsoft 365 apps, Premium P1 및 Premium P2의 네 가지 버전으로 제공 됩니다.

Free 버전은 Azure 구독에 포함되어 있습니다. 보안 기본값을 사용하는 방법에 대한 자세한 내용 또는 Azure AD 전역 관리자 역할로 계정을 보호하는 방법에 대한 자세한 내용은 [아래 섹션](#azure-ad-free-tier)을 참조하세요.

Azure AD Premium 버전은 Microsoft 담당자, [오픈 볼륨 라이선스 프로그램](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx) 및 [클라우드 솔루션 공급자 프로그램](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)을 통해 사용할 수 있습니다. Azure 및 Microsoft 365 구독자는 온라인에서 Azure Active Directory Premium P1 및 P2를 구입할 수도 있습니다. 구매하려면 [로그인](https://portal.office.com/Commerce/Catalog.aspx)하세요.

필요한 Azure AD 계층을 구매한 후 [AZURE ad Multi-Factor Authentication를 계획 하 고 배포](howto-mfa-getstarted.md)합니다.

### <a name="azure-ad-free-tier"></a>Azure AD Free 계층

Azure AD Free 테 넌 트의 모든 사용자는 보안 기본값을 사용 하 여 Azure AD Multi-Factor Authentication를 사용할 수 있습니다. 모바일 인증 앱은 Azure AD Free 보안 기본값을 사용 하는 경우 Azure AD Multi-Factor Authentication에 사용할 수 있는 유일한 방법입니다.

* [Azure AD 보안 기본값에 대한 자세한 정보](../fundamentals/concept-fundamentals-security-defaults.md)
* [Azure AD Free의 사용자에 대한 보안 기본값 사용](../fundamentals/concept-fundamentals-security-defaults.md#enabling-security-defaults)

모든 사용자에 대해 Azure AD Multi-Factor Authentication를 사용 하도록 설정 하지 않으려는 경우 *AZURE Ad 전역 관리자* 역할을 사용 하 여 사용자 계정만 보호 하도록 선택할 수 있습니다. 이 방법은 중요한 관리자 계정에 대한 추가 인증 프롬프트를 제공합니다. 사용 하는 계정 유형에 따라 다음 방법 중 하나를 사용 하 여 Azure AD Multi-Factor Authentication를 사용 하도록 설정 합니다.

* Microsoft 계정을 사용하는 경우 [다단계 인증에 등록](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)합니다.
* Microsoft 계정을 사용하지 않는 경우 [Azure AD에서 사용자 또는 그룹에 대한 다단계 인증을 설정](howto-mfa-userstates.md)합니다.

## <a name="next-steps"></a>다음 단계

* 비용에 대 한 자세한 내용은 [AZURE AD Multi-Factor Authentication 가격 책정](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)을 참조 하세요.
* [조건부 액세스란?](../conditional-access/overview.md)

