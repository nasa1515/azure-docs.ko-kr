---
title: Azure Arc 사용 서버 에이전트 관리
description: 이 문서에서는 Azure Arc 사용 서버 연결 된 컴퓨터 에이전트의 수명 주기 동안 일반적으로 수행 하는 다양 한 관리 작업에 대해 설명 합니다.
ms.date: 02/10/2021
ms.topic: conceptual
ms.openlocfilehash: 36ae081f939cbf865db7755a2f766a7ccd87d619
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100587631"
---
# <a name="managing-and-maintaining-the-connected-machine-agent"></a>Connected Machine 에이전트 관리 및 유지 관리

Azure Arc 사용 서버에 연결 된 Windows 또는 Linux 용 컴퓨터 에이전트의 초기 배포 후 에이전트를 다시 구성 하거나, 업그레이드 하거나, 컴퓨터에서 제거 해야 할 수 있습니다. 이러한 일상적인 유지 관리 작업을 수동으로 또는 자동화를 통해 간단히 관리하여 조작 오류와 비용을 모두 줄일 수 있습니다.

## <a name="before-uninstalling-agent"></a>에이전트 제거 전

Arc 사용 서버에서 연결 된 컴퓨터 에이전트를 제거 하기 전에 다음 사항을 고려 하 여 예기치 않은 문제나 Azure 청구서에 추가 된 비용을 방지 합니다.

* Azure VM 확장을 사용 가능한 서버에 배포 하 고 연결 된 컴퓨터 에이전트를 제거 하거나 리소스 그룹에서 Arc 사용 서버를 나타내는 리소스를 삭제 하는 경우 해당 확장은 계속 실행 되 고 정상적인 작업을 수행 합니다.

* 리소스 그룹에서 Arc 사용 서버를 나타내는 리소스를 삭제 하지만 VM 확장을 제거 하지 않은 경우 컴퓨터를 다시 등록할 때 설치 된 VM 확장을 관리할 수 없습니다.

Azure Arc 사용 서버를 사용 하 여 더 이상 관리 하지 않으려는 서버 또는 컴퓨터의 경우 다음 단계를 수행 하 여 관리를 중지 해야 합니다.

1. 컴퓨터 또는 서버에서 VM 확장을 제거 합니다. 단계는 아래에 나와 있습니다.

2. 다음 방법 중 하나를 사용 하 여 Azure Arc에서 컴퓨터의 연결을 끊습니다.

    * `azcmagent disconnect`컴퓨터 또는 서버에서 명령을 실행 하 고 있습니다.

    * 위쪽 막대에서 **삭제** 를 선택 하 여 Azure Portal에서 선택한 등록 된 원호 사용 서버를 선택 합니다.

    * [Azure CLI](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-cli#delete-resource) 또는 [Azure PowerShell](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-powershell#delete-resource)를 사용 합니다. `ResourceType`매개 변수를 사용 `Microsoft.HybridCompute/machines` 합니다.

3. 아래 단계를 수행 하 여 컴퓨터 또는 서버에서 [에이전트를 제거](#remove-the-agent) 합니다.

## <a name="renaming-a-machine"></a>컴퓨터 이름 바꾸기

Azure Arc 사용 서버에 연결 된 Linux 또는 Windows 컴퓨터의 이름을 변경 하면 Azure의 리소스 이름이 변경 되지 않으므로 새 이름이 자동으로 인식 되지 않습니다. 다른 Azure 리소스와 마찬가지로 새 이름을 사용 하기 위해 리소스를 삭제 하 고 다시 만들어야 합니다.

Arc 사용 서버의 경우 컴퓨터 이름을 바꾸기 전에 먼저 VM 확장을 제거한 후 계속 해야 합니다.

> [!NOTE]
> 설치 된 확장은 계속 실행 되 고이 절차가 완료 된 후 정상적인 작업을 수행 하는 동안에는 관리할 수 없습니다. 컴퓨터에 확장을 다시 배포 하려고 하면 예기치 않은 동작이 발생할 수 있습니다.

> [!WARNING]
> 컴퓨터의 컴퓨터 이름을 바꾸는 것을 방지 하 고 반드시 필요한 경우에만이 절차를 수행 하는 것이 좋습니다.

1. 컴퓨터에 설치 된 VM 확장을 감사 하 고 [Azure CLI](manage-vm-extensions-cli.md#list-extensions-installed) 또는 [Azure PowerShell](manage-vm-extensions-powershell.md#list-extensions-installed)를 사용 하 여 구성을 확인 합니다.

2. [Azure CLI](manage-vm-extensions-cli.md#remove-an-installed-extension)또는 [Azure PowerShell](manage-vm-extensions-powershell.md#remove-an-installed-extension)를 사용 하 여 [Azure Portal](manage-vm-extensions-portal.md#uninstall-extension)에서 설치 된 VM 확장을 제거 합니다.

3. **Azcmagent** 도구를 [연결 끊기](manage-agent.md#disconnect) 매개 변수와 함께 사용 하 여 azure Arc에서 컴퓨터의 연결을 끊고 azure에서 컴퓨터 리소스를 삭제 합니다. Arc 사용 서버에서 컴퓨터 연결을 끊으면 연결 된 컴퓨터 에이전트는 제거 되지 않으므로이 프로세스의 일부로 에이전트를 제거할 필요가 없습니다. 대화형으로 로그온 하는 동안이를 수동으로 실행 하거나, 여러 에이전트를 등록 하는 데 사용한 것과 동일한 서비스 주체를 사용 하거나 Microsoft id 플랫폼 [액세스 토큰](../../active-directory/develop/access-tokens.md)을 사용 하 여 자동화할 수 있습니다. 서비스 주체를 사용 하 여 Azure Arc 사용 서버에 컴퓨터를 등록 하지 않은 경우 서비스 주체를 만들려면 다음 [문서](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) 를 참조 하세요.

4. 컴퓨터의 컴퓨터 이름 이름을 바꿉니다.

5. 연결 된 컴퓨터 에이전트를 Arc 사용 서버에 다시 등록 합니다. `azcmagent` [Connect](manage-agent.md#connect) 매개 변수를 사용 하 여이 도구를 실행 합니다.

6. 원래 컴퓨터에 배포 된 VM 확장을 Arc 사용 서버에서 다시 배포 합니다. Azure 정책을 사용 하 여 VM용 Azure Monitor (insights) 에이전트 또는 Log Analytics 에이전트를 배포한 경우 다음 [평가 주기](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers)후에 에이전트가 다시 배포 됩니다.

## <a name="upgrading-agent"></a>에이전트 업그레이드

Azure 연결 된 컴퓨터 에이전트는 버그 수정, 안정성 향상 및 새로운 기능을 해결 하기 위해 정기적으로 업데이트 됩니다. [Azure Advisor](../../advisor/advisor-overview.md) 는 최신 버전의 컴퓨터 에이전트를 사용 하지 않는 리소스를 식별 하 고 최신 버전으로 업그레이드 하는 것이 좋습니다. **개요** 페이지에 배너를 표시 하거나 Azure Portal 통해 Advisor에 액세스 하 여 Arc 사용 서버를 선택 하면 사용자에 게 알립니다.

Windows 및 Linux용 Azure Connected Machine 에이전트는 요구 사항에 따라 수동 또는 자동으로 최신 릴리스로 업그레이드할 수 있습니다.

다음 표에서는 에이전트 업그레이드를 수행하는 데 지원되는 방법을 설명합니다.

| 운영 체제 | 업그레이드 방법 |
|------------------|----------------|
| Windows | 수동<br> Windows 업데이트 |
| Ubuntu | [Apt](https://help.ubuntu.com/lts/serverguide/apt.html) |
| SUSE Linux Enterprise Server | [zypper](https://en.opensuse.org/SDB:Zypper_usage_11.3) |
| RedHat Enterprise, Amazon, CentOS Linux | [yum](https://wiki.centos.org/PackageManagement/Yum) |

### <a name="windows-agent"></a>Windows 에이전트

Windows용 Connected Machine 에이전트에 대한 업데이트 패키지는 다음 위치에서 제공됩니다.

* Microsoft Update

* [Microsoft Update 카탈로그](https://www.catalog.update.microsoft.com/Home.aspx)

* Microsoft 다운로드 센터에서 [Windows 에이전트 Windows Installer 패키지](https://aka.ms/AzureConnectedMachineAgent)를 다운로드합니다.

소프트웨어 업데이트 관리 프로세스를 지원 하기 위해 다양 한 방법으로 에이전트를 업그레이드할 수 있습니다. Microsoft 업데이트에서 가져오는 것 외에 명령 프롬프트를 사용하거나, 스크립트 또는 기타 자동화 솔루션을 사용하거나, UI 마법사에서 `AzureConnectedMachine.msi`를 실행하여 수동으로 다운로드하고 실행할 수 있습니다.

> [!NOTE]
> * 에이전트를 업그레이드하려면 관리자 권한이 있어야 합니다.
> * 수동으로 업그레이드하려면 먼저 설치 관리자 패키지를 다운로드하여 대상 서버의 폴더 또는 공유 네트워크 폴더에 복사해야 합니다. 

Windows Installer 패키지에 대한 명령줄 옵션에 익숙하지 않은 경우 [Msiexec 표준 명령줄](/windows/win32/msi/standard-installer-command-line-options) 및 [Msiexec 명령줄 옵션](/windows/win32/msi/command-line-options)을 검토하세요.

#### <a name="to-upgrade-using-the-setup-wizard"></a>설치 마법사를 사용하여 업그레이드하려면

1. 관리 권한이 있는 계정으로 컴퓨터에 로그인합니다.

2. **AzureConnectedMachineAgent.msi** 를 실행하여 설치 마법사를 시작합니다.

설치 마법사는 이전 버전이 있는지 검색한 다음 자동으로 에이전트 업그레이드를 수행합니다. 업그레이드가 완료되면 설치 마법사가 자동으로 닫힙니다.

#### <a name="to-upgrade-from-the-command-line"></a>명령줄에서 업그레이드하려면

1. 관리 권한이 있는 계정으로 컴퓨터에 로그인합니다.

2. 에이전트를 자동으로 업그레이드하고 `C:\Support\Logs` 폴더에 설치 로그 파일을 만들려면 다음 명령을 실행합니다.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentupgradesetup.log"
    ```

### <a name="linux-agent"></a>Linux 에이전트

Linux 머신의 에이전트를 최신 버전으로 업데이트하려면 두 개의 명령이 필요합니다. 로컬 패키지 인덱스를 리포지토리에서 사용 가능한 최신 패키지 목록으로 업데이트하는 명령과 로컬 패키지를 업그레이드하는 명령입니다.

Microsoft [패키지 리포지토리](https://packages.microsoft.com/)에서 최신 에이전트 패키지를 다운로드할 수 있습니다.

> [!NOTE]
> 에이전트를 업그레이드하려면 루트 액세스 권한이 있거나 Sudo를 사용하는 승격된 권한이 있는 계정을 사용해야 합니다.

#### <a name="upgrade-ubuntu"></a>Ubuntu 업그레이드

1. 로컬 패키지 인덱스를 리포지토리의 최신 변경 내용으로 업데이트하려면 다음 명령을 실행합니다.

    ```bash
    apt update
    ```

2. 시스템을 업그레이드하려면 다음 명령을 실행합니다.

    ```bash
    apt upgrade
    ```

패키지 설치 및 제거와 같은 [apt](https://help.ubuntu.com/lts/serverguide/apt.html) 명령의 동작은 `/var/log/dpkg.log` 로그 파일에 기록됩니다.

#### <a name="upgrade-red-hatcentosamazon-linux"></a>Red Hat/CentOS/Amazon Linux 업그레이드

1. 로컬 패키지 인덱스를 리포지토리의 최신 변경 내용으로 업데이트하려면 다음 명령을 실행합니다.

    ```bash
    yum check-update
    ```

2. 시스템을 업그레이드하려면 다음 명령을 실행합니다.

    ```bash
    yum update
    ```

패키지 설치 및 제거와 같은 [yum](https://access.redhat.com/articles/yum-cheat-sheet) 명령의 동작은 `/var/log/yum.log` 로그 파일에 기록됩니다. 

#### <a name="upgrade-suse-linux-enterprise"></a>SUSE Linux Enterprise 업그레이드

1. 로컬 패키지 인덱스를 리포지토리의 최신 변경 내용으로 업데이트하려면 다음 명령을 실행합니다.

    ```bash
    zypper refresh
    ```

2. 시스템을 업그레이드하려면 다음 명령을 실행합니다.

    ```bash
    zypper update
    ```

패키지 설치 및 제거와 같은 [zypper](https://en.opensuse.org/Portal:Zypper) 명령의 동작은 `/var/log/zypper.log` 로그 파일에 기록됩니다.

## <a name="about-the-azcmagent-tool"></a>Azcmagent 도구 정보

Azcmagent 도구 (Azcmagent.exe)는 설치 중에 Azure Arc 사용 서버 연결 된 컴퓨터 에이전트를 구성 하는 데 사용 되거나 설치 후 에이전트의 초기 구성을 수정 하는 데 사용 됩니다. Azcmagent.exe는 에이전트를 사용자 지정하고 해당 상태를 볼 수 있는 명령줄 매개 변수를 제공합니다.

* **Connect** - Azure Arc에 머신을 연결합니다.

* **Disconnect** - Azure Arc에서 머신의 연결을 끊습니다.

* **Show** - 에이전트와 관련된 문제를 해결할 때 도움이 될 수 있는 에이전트 상태 및 해당 구성 속성(리소스 그룹 이름, 구독 ID, 버전 등)을 표시합니다. `-j`매개 변수를 포함 하 여 JSON 형식으로 결과를 출력 합니다.

* **로그** -문제 해결에 도움이 되는 로그를 포함 하는 .zip 파일을 현재 디렉터리에 만듭니다.

* **버전** -연결 된 컴퓨터 에이전트 버전을 표시 합니다.

* **-h 또는 --help** - 사용 가능한 명령줄 매개 변수를 표시합니다.

    예를 들어 **Connect** 매개 변수에 대 한 자세한 도움말을 보려면을 입력 `azcmagent connect -h` 합니다. 

* **-v 또는 --verbose** - 자세한 정보 로깅을 사용합니다.

대화형으로 로그온 하는 동안 **연결** 및 **연결** 을 수동으로 수행 하거나, 여러 에이전트를 등록 하는 데 사용한 것과 동일한 서비스 주체를 사용 하거나 Microsoft id 플랫폼 [액세스 토큰](../../active-directory/develop/access-tokens.md)을 사용 하 여 자동화할 수 있습니다. 서비스 주체를 사용 하 여 Azure Arc 사용 서버에 컴퓨터를 등록 하지 않은 경우 서비스 주체를 만들려면 다음 [문서](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) 를 참조 하세요.

>[!NOTE]
>**Azcmagent** 를 실행 하려면 Linux 컴퓨터에 대 한 *루트* 액세스 권한이 있어야 합니다.

### <a name="connect"></a>연결

이 매개 변수는 Azure Resource Manager의 리소스를 지정하여 Azure에서 머신이 만들어짐을 나타냅니다. 리소스는 지정된 구독 및 리소스 그룹에 있으며, 머신에 대한 데이터는 `--location` 설정에 지정된 Azure 지역에 저장됩니다. 지정 하지 않으면 기본 리소스 이름은 컴퓨터의 호스트 이름입니다.

그런 다음 머신의 시스템 할당 ID에 해당하는 인증서를 다운로드하여 로컬로 저장합니다. 이 단계가 완료 되 면 Azure 연결 된 컴퓨터 Metadata Service 및 게스트 구성 에이전트가 Azure Arc 사용 서버와 동기화를 시작 합니다.

서비스 주체를 사용하여 연결하려면 다음 명령을 실행합니다.

`azcmagent connect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

액세스 토큰을 사용하여 연결하려면 다음 명령을 실행합니다.

`azcmagent connect --access-token <> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

관리자 권한으로 로그온한 자격 증명을 사용하여 연결하려면(대화형) 다음 명령을 실행합니다.

`azcmagent connect --tenant-id <TenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

### <a name="disconnect"></a>연결 끊기

이 매개 변수는 Azure Resource Manager의 리소스를 지정하여 머신이 Azure에서 삭제됨을 나타냅니다. 컴퓨터에서 에이전트를 제거 하지 않습니다. 에이전트는 별도로 제거 해야 합니다. 컴퓨터가 분리 된 후에 Azure Arc 사용 서버에 다시 등록 하려면 `azcmagent connect` azure에서 새 리소스를 만들도록를 사용 합니다.

> [!NOTE]
> 하나 이상의 Azure VM 확장을 Arc 사용 서버에 배포 하 고 Azure에서 해당 등록을 삭제 하는 경우 확장은 아직 설치 되어 있습니다. 설치 된 확장에 따라 함수를 적극적으로 수행 하는 것을 이해 하는 것이 중요 합니다. 사용 중지 되거나 Arc 사용 서버에서 더 이상 관리 되지 않는 컴퓨터는 먼저 Azure에서 등록을 제거 하기 전에 제거 된 확장이 있어야 합니다.

서비스 주체를 사용하여 연결을 끊으려면 다음 명령을 실행합니다.

`azcmagent disconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

액세스 토큰을 사용하여 연결을 끊으려면 다음 명령을 실행합니다.

`azcmagent disconnect --access-token <accessToken>`

관리자 권한으로 로그온한 자격 증명을 사용하여 연결을 끊으려면(대화형) 다음 명령을 실행합니다.

`azcmagent disconnect`

## <a name="remove-the-agent"></a>에이전트 제거

다음 방법 중 하나를 수행하여 머신에서 Windows 또는 Linux Connected Machine 에이전트를 제거합니다. 에이전트를 제거 해도 Arc 사용 서버를 사용 하 여 컴퓨터의 등록을 취소 하거나 설치 된 Azure VM 확장을 제거 하지 않습니다. Azure에서 컴퓨터를 더 이상 관리할 필요가 없고 에이전트를 제거 하기 전에 이러한 단계를 완료 해야 하는 경우 컴퓨터의 등록을 취소 하 고 설치 된 VM 확장을 별도로 제거 해야 합니다.

### <a name="windows-agent"></a>Windows 에이전트

다음 메서드는 모두 에이전트를 제거하지만 머신에서 *C:\Program Files\AzureConnectedMachineAgent* 폴더를 제거하지는 않습니다.

#### <a name="uninstall-from-control-panel"></a>제어판에서 제거

1. 머신에서 Windows 에이전트를 제거하려면 다음을 수행합니다.

    a. 관리자 권한이 있는 계정으로 컴퓨터에 로그인합니다.  
    b. **제어판** 에서 **프로그램 및 기능** 을 선택합니다.  
    다. **프로그램 및 기능** 에서 **Azure Connected Machine 에이전트**,  **제거**, **예** 를 차례로 선택합니다.  

    >[!NOTE]
    > **AzureConnectedMachineAgent.msi** 설치 관리자 패키지를 두 번 클릭하여 에이전트 설정 마법사를 실행할 수도 있습니다.

#### <a name="uninstall-from-the-command-line"></a>명령줄에서 제거

명령 프롬프트에서 에이전트를 수동으로 제거하거나 스크립트와 같은 자동화된 방법을 사용하려면 다음 예제를 사용할 수 있습니다. 먼저 운영 체제에서 제품 코드(애플리케이션 패키지의 기본 식별자인 GUID)를 검색해야 합니다. 제거는 Msiexec.exe 명령줄 `msiexec /x {Product Code}`를 사용하여 수행됩니다.

1. 레지스트리 편집기를 엽니다.

2. `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall` 레지스트리 키 아래에서 제품 코드 GUID를 찾아 복사합니다.

3. 그리고 나서 다음 예제에 따라 Msiexec.exe를 사용하여 에이전트를 제거할 수 있습니다.

   * 명령줄에서 다음을 입력합니다.

       ```dos
       msiexec.exe /x {product code GUID} /qn
       ```

   * PowerShell을 사용하여 동일한 단계를 수행할 수 있습니다.

       ```powershell
       Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
       Get-ItemProperty | `
       Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
       ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
       ```

### <a name="linux-agent"></a>Linux 에이전트

> [!NOTE]
> 에이전트를 제거하려면 루트 액세스 권한이 있거나 Sudo를 사용하는 승격된 권한이 있는 계정을 사용해야 합니다.

Linux 에이전트를 제거하려면 사용할 명령은 Linux 운영 체제에 따라 달라집니다.

- Ubuntu의 경우 다음 명령을 실행합니다.

    ```bash
    sudo apt purge azcmagent
    ```

- RHEL, CentOS 및 Amazon Linux의 경우 다음 명령을 실행합니다.

    ```bash
    sudo yum remove azcmagent
    ```

- SLES의 경우 다음 명령을 실행합니다.

    ```bash
    sudo zypper remove azcmagent
    ```

## <a name="unregister-machine"></a>머신 등록 취소

Azure에서 지원 서비스를 사용 하 여 컴퓨터를 관리 하지 않으려는 경우 다음 단계를 수행 하 여 Arc 사용 서버에서 컴퓨터의 등록을 취소 합니다. 머신에서 Connected Machine 에이전트를 제거하기 전이나 후에 이러한 단계를 수행할 수 있습니다.

1. [Azure Portal](https://aka.ms/hybridmachineportal)로 이동 하 여 Azure Arc 사용 서버를 엽니다.

2. 목록에서 머신을 선택하고, 줄임표 ( **...** )를 선택한 다음, **삭제** 를 선택합니다.

## <a name="update-or-remove-proxy-settings"></a>프록시 설정 업데이트 또는 제거

프록시 서버를 통해 서비스와 통신하도록 에이전트를 구성하거나 배포 후 이 구성을 제거하려면 다음 방법 중 하나를 사용하여 이 작업을 완료합니다.

> [!NOTE]
> Arc 사용 서버는 연결 된 컴퓨터 에이전트에 대 한 프록시로 [Log Analytics 게이트웨이](../../azure-monitor/agents/gateway.md) 를 사용 하는 것을 지원 하지 않습니다.
>

### <a name="windows"></a>Windows

프록시 서버 환경 변수를 설정하려면 다음 명령을 실행합니다.

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy","http://{proxy-url}:{proxy-port}","Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

프록시 서버를 통한 통신을 중지하도록 에이전트를 구성하려면 다음 명령을 실행하여 프록시 서버 환경 변수를 제거하고 에이전트 서비스를 다시 시작합니다.

```powershell
[Environment]::SetEnvironmentVariable("https_proxy",$null,"Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable removed.
Restart-Service -Name himds
```

### <a name="linux"></a>Linux

프록시 서버를 설정하려면 에이전트 설치 패키지를 다운로드한 디렉터리에서 다음 명령을 실행합니다.

```bash
# Reconfigure the connected machine agent and set the proxy server.
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

프록시 서버를 통한 통신을 중지하도록 에이전트를 구성하려면 다음 명령을 실행하여 프록시 구성을 제거합니다.

```bash
sudo azcmagent_proxy remove
```

## <a name="next-steps"></a>다음 단계

* 문제 해결 정보는 [연결 된 컴퓨터 에이전트 문제 해결 가이드](troubleshoot-agent-onboard.md)에서 찾을 수 있습니다.

* [Azure Policy](../../governance/policy/overview.md)를 사용하여 머신을 관리하는 방법을 알아봅니다(예: VM [게스트 구성](../../governance/policy/concepts/guest-configuration.md), 머신이 예상되는 Log Analytics 작업 영역에 보고되는지 확인, [VM을 사용한 Azure Monitor](../../azure-monitor/vm/vminsights-enable-policy.md)로 모니터링 등).

* [Log Analytics 에이전트](../../azure-monitor/agents/log-analytics-agent.md)에 대해 자세히 알아보세요. Windows 및 Linux 용 Log Analytics 에이전트는 운영 체제 및 워크 로드 모니터링 데이터를 수집 하거나, 자동화 runbook 또는 업데이트 관리 같은 기능을 사용 하 여 관리 하거나, [Azure Security Center](../../security-center/security-center-introduction.md)같은 다른 Azure 서비스를 사용 하려는 경우에 필요 합니다.