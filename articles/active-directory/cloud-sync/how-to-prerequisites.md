---
title: Azure AD에서 Azure AD Connect 클라우드 동기화를 위한 필수 구성 요소
description: 이 문서에서는 클라우드 동기화에 필요한 필수 구성 요소 및 하드웨어 요구 사항을 설명 합니다.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/11/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b83c9b0ece933ad71810c50e89ae296aa218ec75
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98613665"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-sync"></a>Azure AD Connect 클라우드 동기화를 위한 필수 구성 요소
이 문서에서는 id 솔루션으로 Azure AD (Azure Active Directory) 연결 클라우드 동기화를 선택 하 고 사용 하는 방법에 대 한 지침을 제공 합니다.

## <a name="cloud-provisioning-agent-requirements"></a>클라우드 프로비저닝 에이전트 요구 사항
Azure AD Connect 클라우드 동기화를 사용 하려면 다음이 필요 합니다.

- 에이전트 서비스를 실행 하는 Azure AD Connect Cloud Sync gMSA (그룹 관리 서비스 계정)를 만드는 데 필요한 도메인 관리자 또는 엔터프라이즈 관리자 자격 증명입니다. 
- 게스트 사용자가 아닌 Azure AD 테 넌 트에 대 한 하이브리드 id 관리자 계정입니다.
- Windows 2012 R2 이상 버전을 사용하는 프로비저닝 에이전트에 대한 온-프레미스 서버  이 서버는 [Active Directory 관리 계층 모델](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)을 기반으로 하는 계층 0 서버 여야 합니다.
- 온-프레미스 방화벽 구성

## <a name="group-managed-service-accounts"></a>Group Managed Service Accounts
그룹 관리 서비스 계정은 자동 암호 관리, 간소화 된 SPN (서비스 사용자 이름) 관리, 다른 관리자에 게 관리를 위임 하는 기능 및 여러 서버에서이 기능을 확장 하는 관리 되는 도메인 계정입니다.  Azure AD Connect 클라우드 동기화는 에이전트를 실행 하는 데 gMSA을 지원 하 고 사용 합니다.  이 계정을 만들기 위해 설치 중에 관리자 자격 증명을 입력 하 라는 메시지가 표시 됩니다.  계정이 (domain\provAgentgMSA $)로 표시 됩니다.  GMSA에 대 한 자세한 내용은 [그룹 관리 서비스 계정](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) 을 참조 하세요. 

### <a name="prerequisites-for-gmsa"></a>GMSA에 대 한 필수 구성 요소:
1.  GMSA 도메인 포리스트의 Active Directory 스키마를 Windows Server 2012로 업데이트 해야 합니다.
2.  도메인 컨트롤러의 [POWERSHELL RSAT 모듈](/windows-server/remote/remote-server-administration-tools)
3.  도메인에 있는 하나 이상의 도메인 컨트롤러가 Windows Server 2012를 실행 해야 합니다.
4.  에이전트가 설치 되는 도메인에 가입 된 서버는 Windows Server 2012 이상 이어야 합니다.

### <a name="custom-gmsa-account"></a>사용자 지정 gMSA 계정
사용자 지정 gMSA 계정을 만드는 경우 계정에 다음 사용 권한이 있는지 확인 해야 합니다.

|Type |속성 |액세스 권한 |적용 대상| 
|-----|-----|-----|-----|
|Allow |gMSA 계정 |모든 속성 읽기 |하위 디바이스 개체| 
|Allow |gMSA 계정|모든 속성 읽기 |하위 InetOrgPerson 개체| 
|Allow |gMSA 계정 |모든 속성 읽기 |하위 Computer 개체| 
|Allow |gMSA 계정 |모든 속성 읽기 |하위 foreignSecurityPrincipal 개체| 
|Allow |gMSA 계정 |모든 권한 |하위 Group 개체| 
|Allow |gMSA 계정 |모든 속성 읽기 |하위 User 개체| 
|Allow |gMSA 계정 |모든 속성 읽기 |하위 Contact 개체| 
|Allow |gMSA 계정 |사용자 개체 만들기/삭제|이 개체 및 모든 하위 개체| 

GMSA 계정을 사용 하도록 기존 에이전트를 업그레이드 하는 방법에 대 한 단계는 [그룹 관리 서비스 계정](how-to-install.md#group-managed-service-accounts)을 참조 하세요.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory 관리 센터에서

1. Azure AD 테 넌 트에서 클라우드 전용 하이브리드 id 관리자 계정을 만듭니다. 이러한 방식으로 온-프레미스 서비스가 실패하거나 사용할 수 없게 되면 테넌트의 구성을 관리할 수 있습니다. [클라우드 전용 하이브리드 id 관리자 계정을 추가](../fundamentals/add-users-azure-active-directory.md)하는 방법에 대해 알아봅니다. 테넌트에서 잠기지 않도록 하려면 이 단계를 완료하는 것이 중요합니다.
1. Azure AD 테넌트에 [사용자 지정 도메인 이름](../fundamentals/add-custom-domain.md)을 하나 이상 추가합니다. 사용자는 이러한 도메인 이름 중 하나로 로그인할 수 있습니다.

### <a name="in-your-directory-in-active-directory"></a>Active Directory의 디렉터리에서

[IdFix 도구](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix)를 실행하여 동기화를 위한 디렉터리 특성을 준비합니다.

### <a name="in-your-on-premises-environment"></a>온-프레미스 환경에서

1. 4GB 이상의 RAM 및 .NET 4.7.1 이상의 런타임을 사용하여 Windows Server 2012 R2 이상을 실행하는 도메인 조인 호스트 서버를 식별합니다.

2. 로컬 서버에 대한 PowerShell 실행 정책을 Undefined 또는 RemoteSigned로 설정해야 합니다.

3. 서버와 Azure AD 사이에 방화벽이 있는 경우 다음 항목을 구성합니다.
    - 에이전트에서 다음 포트를 통해 Azure AD에 대한 *아웃바운드* 요청을 수행할 수 있는지 확인합니다.

      | 포트 번호 | 사용 방법 |
      | --- | --- |
      | **80** | TLS/SSL 인증서의 유효성을 검사하는 동안 CRL(인증서 해지 목록)을 다운로드합니다.  |
      | **443** | 서비스와의 모든 아웃바운드 통신을 처리합니다. |
      | **8080**(선택 사항) | 443 포트를 사용할 수 없는 경우 에이전트는 8080 포트를 통해 10분마다 해당 상태를 보고합니다. 이 상태는 Azure AD 포털에 표시됩니다. |

    - 방화벽이 원래 사용자에 따라 규칙에 적용되는 경우 네트워크 서비스로 실행하는 Windows 서비스의 트래픽에 대해 이러한 포트를 엽니다.
    - 방화벽 또는 프록시를 통해 안전한 접미사를 지정할 수 있으면 \*.msappproxy.net 및 \*.servicebus.windows.net에 대한 연결을 추가합니다. 그렇지 않으면 매주 업데이트되는 [Azure 데이터 센터 IP 범위](https://www.microsoft.com/download/details.aspx?id=41653)에 액세스하도록 허용합니다.
    - 에이전트는 초기 등록을 위해 login.windows.net 및 login.microsoftonline.com에 액세스해야 합니다. 이러한 URL에 대한 방화벽도 엽니다.
    - 인증서 유효성 검사를 위해 mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 및 www\.microsoft.com:80 URL을 차단 해제합니다. 이러한 URL은 다른 Microsoft 제품과의 인증서 유효성 검사에 사용되므로 이러한 URL을 이미 차단 해제했을 수 있습니다.

    >[!NOTE]
    > Windows Server Core에 클라우드 프로저닝 에이전트를 설치하는 것은 지원되지 않습니다.

### <a name="additional-requirements"></a>추가 요구 사항

- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS 요구 사항

> [!NOTE]
> TLS(전송 계층 보안)는 보안 통신을 지원하는 프로토콜입니다. TLS 설정을 변경하면 전체 포리스트에 영향을 줍니다. 자세한 내용은 [TLS 1.1 및 TLS 1.2를 Windows의 WinHTTP에서 기본 보안 프로토콜로 사용하는 업데이트](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi)를 참조하세요.

Azure AD Connect 클라우드 프로비저닝 에이전트를 호스트하는 Windows Server를 설치하기 전에 TLS 1.2를 사용하도록 설정해야 합니다.

TLS 1.2를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. 다음 레지스트리 키를 설정합니다.

    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. 서버를 다시 시작합니다.

## <a name="known-limitations"></a>알려진 제한 사항

알려진 제한 사항은 다음과 같습니다.

### <a name="delta-synchronization"></a>델타 동기화

- 델타 동기화에 대 한 그룹 범위 필터링은 1500 개 이상의 멤버를 지원 하지 않습니다.
- 그룹 범위 지정 필터의 일부로 사용 되는 그룹을 삭제 하는 경우 그룹의 멤버인 사용자는 삭제 되지 않습니다. 
- 범위에 있는 OU 또는 그룹의 이름을 바꾸면 델타 동기화가 사용자를 제거 하지 않습니다.

### <a name="provisioning-logs"></a>프로비저닝 로그
- 프로 비전 로그는 생성 및 업데이트 작업을 명확 하 게 구분 하지 않습니다.  만들기에 대 한 업데이트 및 업데이트 작업에 대 한 만들기 작업이 표시 될 수 있습니다.

### <a name="group-re-naming-or-ou-re-naming"></a>그룹 이름 다시 지정 또는 OU 다시 명명
- 지정 된 구성의 범위에 속하는 그룹 또는 OU의 이름을 지정 하는 경우 클라우드 동기화 작업은 AD의 이름 변경을 인식할 수 없습니다. 작업은 격리로 이동 하지 않으며 정상 상태로 유지 됩니다.


## <a name="next-steps"></a>다음 단계 

- [프로비저닝이란?](what-is-provisioning.md)
- [Azure AD Connect 클라우드 동기화 란?](what-is-cloud-sync.md)