---
title: Linux Virtual Machines에 대 한 Azure 보안 기준
description: Linux Virtual Machines 보안 기준은 Azure 보안 벤치 마크에 지정 된 보안 권장 사항을 구현 하기 위한 절차 지침과 리소스를 제공 합니다.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: e1569cf4c7e91fd0c90d407846a9165d4b7b3776
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100592952"
---
# <a name="azure-security-baseline-for-linux-virtual-machines"></a>Linux Virtual Machines에 대 한 Azure 보안 기준

Linux Virtual Machines에 대 한 Azure 보안 기준에는 배포의 보안 상태를 개선 하는 데 도움이 되는 권장 사항이 포함 되어 있습니다.

이 서비스의 기준은 [Azure Security Benchmark 버전 1.0](../../security/benchmarks/overview.md)에서 가져왔으며, 모범 사례 지침을 통해 Azure에서 클라우드 솔루션을 보호하는 방법에 대한 추천 사항을 제공합니다.

자세한 내용은 [Azure 보안 기준 개요](../../security/benchmarks/security-baselines-overview.md)를 참조하세요.

## <a name="network-security"></a>네트워크 보안

*자세한 내용은 [보안 제어: 네트워크 보안](../../security/benchmarks/security-control-network-security.md)을 참조하세요.*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: 가상 네트워크 내에서 Azure 리소스 보호

**지침**: Azure vm (가상 머신)을 만들 때 vnet (가상 네트워크)을 만들거나 기존 vnet을 사용 하 고 서브넷을 사용 하 여 VM을 구성 해야 합니다. 배포 된 모든 서브넷에 응용 프로그램의 신뢰할 수 있는 포트 및 원본에 특정 한 네트워크 액세스 제어와 함께 적용 되는 네트워크 보안 그룹이 있는지 확인 합니다.

또는 중앙 방화벽에 대 한 특정 사용 사례가 있는 경우 Azure 방화벽을 사용 하 여 이러한 요구 사항을 충족할 수도 있습니다.

* [Azure의 가상 네트워크 및 가상 머신](../network-overview.md)

* [Virtual Network를 만드는 방법](../../virtual-network/quick-create-portal.md)

* [보안 구성을 사용하여 NSG를 만드는 방법](../../virtual-network/tutorial-filter-network-traffic.md)

* [Azure 방화벽을 배포 및 구성 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: 가상 네트워크, 서브넷 및 네트워크 인터페이스의 구성 및 트래픽을 모니터링 하 고 기록 합니다.

**지침**: Azure Security Center을 사용 하 여 Azure에서 azure VM (가상 머신) 리소스를 보호 하는 데 도움이 되는 네트워크 보호 권장 사항을 확인 하 고 따릅니다. NSG 흐름 로그를 사용 하도록 설정 하 고 비정상적인 활동의 Vm에 대 한 트래픽 감사를 위해 저장소 계정에 로그를 보냅니다.

* [NSG 흐름 로그를 사용하도록 설정하는 방법](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Azure Security Center에서 제공 하는 네트워크 보안 이해](../../security-center/security-center-network-recommendations.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="13-protect-critical-web-applications"></a>1.3: 중요한 웹 애플리케이션 보호

**지침**: vm (가상 머신)을 사용 하 여 웹 응용 프로그램을 호스트 하는 경우 vm의 서브넷에 nsg (네트워크 보안 그룹)를 사용 하 여 통신할 수 있는 네트워크 트래픽, 포트 및 프로토콜을 제한 합니다. 응용 프로그램에 필요한 트래픽만 허용 하도록 NSGs를 구성 하는 경우 최소 권한 네트워크 접근 방식을 따릅니다.

들어오는 트래픽의 추가 검사를 위해 중요 한 웹 응용 프로그램 앞에 Azure WAF (웹 응용 프로그램 방화벽)를 배포할 수도 있습니다. WAF에 대해 진단 설정을 사용 하도록 설정 하 고 로그를 저장소 계정, 이벤트 허브 또는 Log Analytics 작업 영역에 수집 합니다.

* [Azure Portal을 사용하여 웹 애플리케이션 방화벽이 있는 애플리케이션 게이트웨이를 만듭니다.](../../web-application-firewall/ag/application-gateway-web-application-firewall-portal.md)

* [Azure의 가상 네트워크 및 가상 머신](../network-overview.md)

* [네트워크 보안 그룹에 대 한 정보](../../virtual-network/tutorial-filter-network-traffic.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: 알려진 악성 IP 주소와의 통신 거부

**지침**: 가상 네트워크에서 DDoS (분산 서비스 거부) 표준 보호를 사용 하도록 설정 하 여 DDoS 공격 으로부터 보호 합니다. Azure Security Center 통합 된 위협 인텔리전스를 사용 하 여 알려진 악성 IP 주소와의 통신을 모니터링할 수 있습니다. 위협 인텔리전스를 사용 하도록 설정 하 고 악의적인 네트워크 트래픽에 대해 "경고 및 거부"로 구성 하 여 각 Virtual Network 세그먼트에서 Azure 방화벽을 구성 합니다.

Azure Security Center의 Just-in-time 네트워크 액세스를 사용 하 여 제한 된 기간 동안 Linux Virtual Machines의 노출을 승인 된 IP 주소로 제한할 수 있습니다. 또한 적응 네트워크 강화 Azure Security Center 사용 하 여 실제 트래픽 및 위협 인텔리전스에 따라 포트와 원본 Ip를 제한 하는 NSG 구성을 권장 합니다.

* [DDoS 보호를 구성 하는 방법](../../ddos-protection/manage-ddos-protection.md)

* [Azure 방화벽을 배포 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

* [Azure Security Center 통합 위협 인텔리전스 이해](../../security-center/azure-defender.md)

* [적응 네트워크 강화 Azure Security Center 이해](../../security-center/security-center-adaptive-network-hardening.md)

* [Azure Security Center Just-in-time 네트워크 Access Control 이해](../../security-center/security-center-just-in-time.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="15-record-network-packets"></a>1.5: 네트워크 패킷을 기록 합니다.

**지침**: Azure Virtual Machines에 대 한 흐름 레코드를 생성 하기 위해 nsg 흐름 로그를 저장소 계정에 기록할 수 있습니다. 비정상적인 활동을 조사할 때 네트워크 트래픽을 비정상적이 고 예기치 않은 활동에 대해 검토할 수 있도록 Network Watcher 패킷 캡처를 사용 하도록 설정할 수 있습니다.

* [NSG 흐름 로그를 사용하도록 설정하는 방법](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Network Watcher를 사용하도록 설정하는 방법](../../network-watcher/network-watcher-create.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: 네트워크 기반 침입 감지/침입 방지 시스템 (IDS/IPS) 배포

**지침**: Network Watcher에서 제공 하는 패킷 캡처와 오픈 소스 id 도구를 결합 하 여 다양 한 위협에 대 한 네트워크 침입 검색을 수행할 수 있습니다. 또한 위협 인텔리전스를 사용 하도록 설정 하 고 악의적인 네트워크 트래픽에 대해 "경고 및 거부"로 구성 하 여 Virtual Network 세그먼트에 Azure 방화벽을 적절 하 게 배포할 수 있습니다.

* [Network Watcher 및 오픈 소스 도구를 사용 하 여 네트워크 침입 감지 수행](../../network-watcher/network-watcher-intrusion-detection-open-source-tools.md)

* [Azure 방화벽을 배포 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

* [Azure 방화벽을 사용 하 여 경고를 구성 하는 방법](../../firewall/threat-intel.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: 웹 애플리케이션에 대한 트래픽 관리

**지침**: 신뢰할 수 있는 인증서에 대해 HTTPS/SSL을 사용 하는 웹 응용 프로그램에 대 한 Azure 애플리케이션 게이트웨이를 배포할 수 있습니다. Azure 애플리케이션 Gateway를 사용 하 여 포트에 수신기를 할당 하 고, 규칙을 만들고, Linux 가상 머신과 같은 백 엔드 풀에 리소스를 추가 하 여 응용 프로그램 웹 트래픽을 특정 리소스로 보냅니다.

* [Application Gateway 배포 하는 방법](../../application-gateway/quick-create-portal.md)

* [HTTPS를 사용 하도록 Application Gateway를 구성 하는 방법](../../application-gateway/create-ssl-portal.md)

* [Azure 웹 응용 프로그램 게이트웨이를 사용 하 여 계층 7 부하 분산 이해](../../application-gateway/overview.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: 네트워크 보안 규칙의 복잡성 및 관리 오버헤드 최소화

**지침**: Virtual Network 서비스 태그를 사용 하 여 azure 가상 머신에 대해 구성 된 네트워크 보안 그룹 또는 azure 방화벽에서 네트워크 액세스 제어를 정의 합니다. 보안 규칙을 만들 때 특정 IP 주소 대신 서비스 태그를 사용할 수 있습니다. 서비스 태그 이름(예: ApiManagement)을 규칙의 적절한 원본 또는 대상 필드에 지정하면 해당 서비스에 대한 트래픽을 허용하거나 거부할 수 있습니다. Microsoft는 서비스 태그에 포함되는 주소 접두사를 관리하고 주소가 변경되면 서비스 태그를 자동으로 업데이트합니다.

* [서비스 태그 이해 및 사용](../../virtual-network/service-tags-overview.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: 네트워크 디바이스에 대한 표준 보안 구성 유지 관리

**지침**: Azure Policy를 사용 하 여 Azure VIRTUAL MACHINES (VM)에 대 한 표준 보안 구성을 정의 하 고 구현 합니다. 또한 Azure 청사진을 사용 하 여 단일 청사진 정의에서 Azure Resource Manager 템플릿, 역할 할당 및 Azure Policy 할당 등의 주요 환경 아티팩트를 패키지화 하 여 대규모 Azure VM 배포를 간소화할 수 있습니다. 청사진을 구독에 적용 하 고 청사진 버전 관리를 통해 리소스 관리를 사용 하도록 설정할 수 있습니다.

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [네트워킹에 대 한 Azure Policy 샘플](../../governance/policy/samples/built-in-policies.md#network)

* [Azure Blueprint를 만드는 방법](../../governance/blueprints/create-blueprint-portal.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: 트래픽 구성 규칙 문서화

**지침**: Azure 가상 머신에 대해 구성 된 네트워크 보안 및 트래픽 흐름과 관련 된 nsgs 및 기타 리소스에 대 한 태그를 사용할 수 있습니다. 개별 NSG 규칙에 대해 "설명" 필드를 사용 하 여 네트워크에서 들어오고 나가는 트래픽을 허용 하는 모든 규칙에 대 한 비즈니스 필요 및/또는 기간을 지정 합니다.

* [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

* [Virtual Network를 만드는 방법](../../virtual-network/quick-create-portal.md)

* [보안 구성을 사용하여 NSG를 만드는 방법](../../virtual-network/tutorial-filter-network-traffic.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: 자동화된 도구를 사용하여 네트워크 리소스 구성 모니터링 및 변경 내용 검색

**지침**: Azure 활동 로그를 사용 하 여 가상 머신과 관련 된 네트워크 리소스 구성에 대 한 변경 내용을 모니터링 합니다. 중요 네트워크 설정 또는 리소스가 변경 될 때 트리거되는 Azure Monitor 내에서 경고를 만듭니다.

Azure Policy를 사용 하 여 Linux Virtual Machines와 관련 된 네트워크 리소스에 대 한 구성을 확인 (및/또는 재구성) 합니다.

* [Azure 활동 로그 이벤트를 확인하고 검색하는 방법](../../azure-monitor/essentials/activity-log.md#view-the-activity-log)

* [Azure Monitor에서 경고를 만드는 방법](../../azure-monitor/alerts/alerts-activity-log.md)

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [네트워킹에 대 한 Azure Policy 샘플](../../governance/policy/samples/built-in-policies.md#network)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

## <a name="logging-and-monitoring"></a>로깅 및 모니터링

*자세한 내용은 [보안 제어: 로깅 및 모니터링](../../security/benchmarks/security-control-logging-monitoring.md)을 참조하세요.*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: 승인된 시간 동기화 원본 사용

**지침**: Microsoft는 Azure 리소스에 대 한 시간 원본을 유지 관리 하지만 Linux Virtual Machines에 대 한 시간 동기화 설정을 관리할 수 있는 옵션이 있습니다.

* [Azure 계산 리소스에 대 한 시간 동기화를 구성 하는 방법](./time-sync.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: 공유됨

### <a name="22-configure-central-security-log-management"></a>2.2: 중앙 보안 로그 관리 구성

**지침**: Azure Security Center는 Linux Virtual Machines에 대 한 보안 이벤트 로그 모니터링을 제공 합니다.

* [Azure Security Center에서 데이터 수집](../../security-center/security-center-enable-data-collection.md)

* [모니터링할 Syslog 데이터를 캡처하려면 Log Analytics 확장을 사용 하도록 설정 해야 합니다.](../../azure-monitor/vm/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure 리소스에 대한 감사 로깅 사용

**지침**: 활동 로그를 사용 하 여 가상 머신 리소스에 대해 수행 되는 작업 및 작업을 감사할 수 있습니다. 활동 로그에는 읽기 작업 (GET)을 제외 하 고 리소스에 대 한 모든 쓰기 작업 (PUT, POST, DELETE)이 포함 됩니다. 활동 로그를 사용 하 여 문제를 해결할 때 오류를 찾거나 조직의 사용자가 리소스를 수정한 방법을 모니터링할 수 있습니다.

Virtual Machines (VM)에 진단 확장을 배포 하 여 게스트 OS 진단 데이터 수집을 사용 하도록 설정 합니다. 진단 확장을 사용 하 여 Azure 가상 머신에서 응용 프로그램 로그 또는 성능 카운터와 같은 진단 데이터를 수집할 수 있습니다.

가상 머신에서 지 원하는 응용 프로그램 및 서비스에 대 한 고급 표시를 위해 VM용 Azure Monitor와 Application insights를 모두 사용 하도록 설정할 수 있습니다. Application Insights를 사용 하 여 응용 프로그램을 모니터링 하 고 HTTP 요청, 예외 등의 원격 분석을 캡처하여 Vm과 응용 프로그램 간의 문제를 상호 연결할 수 있습니다.

또한 이벤트 원본, 날짜, 사용자, 타임 스탬프, 원본 주소, 대상 주소 및 기타 유용한 요소를 포함 하는 감사 및 활동 로그에 대 한 액세스를 Azure Monitor 수 있습니다.

* [Azure Monitor를 사용 하 여 플랫폼 로그 및 메트릭을 수집 하는 방법](../../azure-monitor/essentials/diagnostic-settings.md)

* [Log analytics 에이전트 개요](../../azure-monitor/agents/log-analytics-agent.md)

* [Linux 용 Log analytics 가상 머신 확장](../extensions/oms-linux.md)

* [Azure 활동 로그 이벤트를 보고 검색하기](../../azure-monitor/essentials/activity-log.md#view-the-activity-log)

* [Application Insights 개요](../../azure-monitor/app/app-insights-overview.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: 운영 체제에서 보안 로그 수집

**지침**: Azure Security Center을 사용 하 여 Azure Virtual Machines에 대 한 보안 이벤트 로그 모니터링을 제공 합니다. 보안 이벤트 로그가 생성 하는 데이터의 양이 지정 된 경우 기본적으로 저장 되지 않습니다.

조직에서 가상 컴퓨터의 보안 이벤트 로그 데이터를 유지 하려는 경우 Azure Security Center 내에서 구성 된 원하는 데이터 컬렉션 계층의 Log Analytics 작업 영역에 저장할 수 있습니다.

* [Azure Security Center에서 데이터 수집](../../security-center/security-center-enable-data-collection.md)

* [모니터링할 Syslog 데이터를 캡처하려면 Log Analytics 확장을 사용 하도록 설정 해야 합니다.](../../azure-monitor/vm/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: 보안 로그 스토리지 보존 구성

**참고**: 가상 머신 로그를 저장 하는 데 사용 되는 저장소 계정 또는 Log Analytics 작업 영역에는 조직의 규정 준수 규정에 따라 설정 된 로그 보존 기간이 있는지 확인 합니다.

* [Azure에서 가상 머신을 모니터링하는 방법](../../azure-monitor/vm/monitor-vm-azure.md)

* [Log Analytics 작업 영역 보존 기간을 구성 하는 방법](../../azure-monitor/logs/manage-cost-storage.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: 로그를 모니터링 하 고 검토 합니다.

**지침**: MICROSOFT MONITORING AGENT (MMA) 또는 OMS Linux 에이전트 라고도 하는 Log Analytics 에이전트를 사용 하도록 설정 하 고 Log Analytics 작업 영역으로 로그를 보내도록 구성 합니다. Linux 에이전트는 다양 한 원본에서 수집 된 데이터를 Azure Monitor의 Log Analytics 작업 영역으로 보내고, 모니터링 솔루션에 정의 된 대로 고유한 로그 나 메트릭을 보냅니다.

비정상적인 동작에 대 한 로그를 분석 및 모니터링 하 고 정기적으로 결과를 검토 합니다. 로그를 검토 하 고 로그 데이터에 대 한 쿼리를 수행 하려면 Azure Monitor을 사용 합니다.

또는 Azure 센티널 또는 타사 SIEM에 대 한 온보드 데이터를 사용 하도록 설정 하 여 로그를 모니터링 하 고 검토할 수 있습니다.

* [Log analytics 에이전트 개요](../../azure-monitor/agents/log-analytics-agent.md)

* [Linux 용 Log analytics 가상 머신 확장](../extensions/oms-linux.md)

* [Azure Sentinel을 온보딩하는 방법](../../sentinel/quickstart-onboard.md)

* [Log Analytics 작업 영역 이해](../../azure-monitor/logs/log-analytics-tutorial.md)

* [Azure Monitor에서 사용자 지정 쿼리를 수행하는 방법](../../azure-monitor/logs/get-started-queries.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: 비정상적인 활동에 대해 경고를 사용 하도록 설정

**지침**: Azure Virtual Machines에 대 한 보안 로그 및 이벤트에 있는 비정상적인 활동을 모니터링 하 고 경고 하기 위해 Log Analytics 작업 영역으로 구성 된 Azure Security Center를 사용 합니다.

또는 Azure 센티널 또는 타사 SIEM에 대 한 온보드 데이터를 사용 하도록 설정 하 여 비정상 활동에 대 한 경고를 설정할 수 있습니다.

* [Azure Sentinel을 온보딩하는 방법](../../sentinel/quickstart-onboard.md)

* [Azure Security Center에서 경고를 관리 하는 방법](../../security-center/security-center-managing-and-responding-alerts.md)

* [Log analytics 로그 데이터를 경고 하는 방법](../../azure-monitor/alerts/tutorial-response.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: 맬웨어 방지 로깅 중앙 집중화

**지침**: Linux OS 내에서 맬웨어 방지 취약성 검색을 위한 타사 도구가 필요 합니다.

* [Azure Security center에 Linux 서버를 등록 하는 방법에 대 한 지침](../../security-center/quickstart-onboard-machines.md)

* [다음 링크는 선택한 취약점 소프트웨어의 조건 목록으로 사용할 수 있는 Microsoft 권장 보안 지침을 제공 합니다.](../security-recommendations.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="29-enable-dns-query-logging"></a>2.9: DNS 쿼리 로깅 사용

**지침**: DNS 로깅 솔루션에 대 한 Azure Marketplace에서 조직의 요구에 따라 타사 솔루션을 구현 합니다.

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="210-enable-command-line-audit-logging"></a>2.10: 명령줄 감사 로깅 사용

**지침**: 노드 단위로 콘솔 로깅을 수동으로 구성 하 고 syslog를 사용 하 여 데이터를 저장할 수 있습니다. 또한 Azure Monitor의 Log Analytics 작업 영역을 사용 하 여 Azure Virtual machines의 syslog 데이터에서 로그를 검토 하 고 쿼리를 수행 합니다.

* [Azure Monitor에서 사용자 지정 쿼리를 수행하는 방법](../../azure-monitor/logs/get-started-queries.md)

* [Azure Monitor의 Syslog 데이터 원본](../../azure-monitor/agents/data-sources-syslog.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

## <a name="identity-and-access-control"></a>ID 및 액세스 제어

*자세한 내용은 [보안 제어: ID 및 액세스 제어](../../security/benchmarks/security-control-identity-access-control.md)를 참조하세요.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: 관리 계정의 인벤토리 유지 관리

**지침**: Azure Active Directory는 사용자 액세스를 관리할 수 있는 권장 방법 이지만, Azure Virtual Machines에는 로컬 계정이 있을 수 있습니다. 로컬 및 도메인 계정은 일반적으로 최소한의 공간을 사용 하 여 검토 하 고 관리 해야 합니다. 또한 가상 컴퓨터 리소스에 액세스 하는 데 사용 되는 관리 계정에 대해 Azure Privileged Identity Management를 활용 합니다.

* [로컬 계정에 대 한 정보는에서 확인할 수 있습니다.](../../active-directory/devices/assign-local-admin.md#manage-the-device-administrator-role)

* [권한 있는 Id 관리자에 대 한 정보](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: 기본 암호 변경(해당하는 경우)

**지침**: Linux Virtual Machines 및 Azure Active Directory에는 기본 암호 개념이 없습니다. 기본 암호를 사용할 수 있는 타사 응용 프로그램 및 마켓플레이스 서비스를 담당 하는 고객입니다.

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: 전용 관리 계정 사용

**지침**: 가상 컴퓨터에 대 한 액세스 권한이 있는 전용 관리 계정의 사용에 대 한 표준 운영 절차를 만듭니다. Azure Security Center id 및 액세스 관리를 사용 하 여 관리 계정 수를 모니터링 합니다. Azure 가상 컴퓨터 리소스에 액세스 하는 데 사용 되는 모든 관리자 계정은 PIM (Azure Privileged Identity Management)에서 관리할 수도 있습니다. Azure Privileged Identity Management는 특정 시간 프레임에 대 한 사용 권한만 사용할 수 있도록 하 고 승인자가 필요한 경우에만 Just-in-time 권한 상승 등의 몇 가지 옵션을 제공 하 여 역할을 가정 하기 전에 Multi-Factor Authentication 필요 합니다.

* [Azure Security Center Id 및 액세스 이해](../../security-center/security-center-identity-access.md)

* [권한 있는 Id 관리자에 대 한 정보](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: SSO (Azure Active Directory Single Sign-On)를 사용 하십시오.

**지침**: 가능한 경우 고객은 서비스 별로 개별 독립 실행형 자격 증명을 구성 하는 대신 AZURE ACTIVE DIRECTORY에서 SSO를 사용할 수 있습니다. Azure Security Center Id 및 액세스 관리 권장 사항을 사용 합니다.

* [Azure AD를 사용 하 여 SSO 이해](../../active-directory/manage-apps/what-is-single-sign-on.md)

* [Azure Security Center 내에서 ID 및 액세스를 모니터링하는 방법](../../security-center/security-center-identity-access.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: 모든 Azure Active Directory 기반 액세스에 대해 multi-factor authentication을 사용 하십시오.

**지침**: Azure AD MFA를 사용하도록 설정하고 Azure Security Center ID 및 액세스 관리 권장 사항을 따릅니다.

* [Azure에서 MFA를 사용하도록 설정하는 방법](../../active-directory/authentication/howto-mfa-getstarted.md)

* [Azure Security Center 내에서 ID 및 액세스를 모니터링하는 방법](../../security-center/security-center-identity-access.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: 관리 작업에는 안전한 Azure 관리 워크스테이션 사용

**지침**: MFA가 구성된 PAW(Privileged Access Workstation)를 사용하여 Azure 리소스에 로그인하고 구성합니다.

* [권한 있는 액세스 워크스테이션에 대해 알아보기](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

* [Azure에서 MFA를 사용하도록 설정하는 방법](../../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: 관리 계정에서 의심 스러운 활동에 대 한 로그 및 경고

**지침**: 환경에서 의심 스러운 활동이 나 안전 하지 않은 활동이 발생 하는 경우 로그 및 경고를 생성 하는 데 PIM (Azure AD Privileged Identity Management)을 사용 합니다. Azure AD 위험 검색을 사용하여 위험한 사용자 동작에 대한 경고 및 보고서를 봅니다. 필요에 따라 고객은 Azure Security Center 위험 검색 경고를 Azure Monitor에 수집 하 고 작업 그룹을 사용 하 여 사용자 지정 경고/알림을 구성할 수 있습니다.

* [PIM(Privileged Identity Management)을 배포하는 방법](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

* [Azure Security Center 위험 탐지 이해 (의심 스러운 활동)](../../active-directory/identity-protection/overview-identity-protection.md)

* [Azure 활동 로그를 Azure Monitor에 통합하는 방법](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

* [사용자 지정 경고 및 알림에 대 한 작업 그룹을 구성 하는 방법](../../azure-monitor/alerts/action-groups.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: 승인된 위치에서만 Azure 리소스 관리

**지침**: Azure Active Directory 조건부 액세스 정책 및 명명 된 위치를 사용 하 여 IP 주소 범위 또는 국가/지역의 특정 논리적 그룹 에서만 액세스할 수 있도록 합니다.

* [Azure에서 명명된 위치를 구성하는 방법](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Azure Active Directory 사용

**지침**: Azure Active Directory (Azure AD)를 중앙 인증 및 권한 부여 시스템으로 사용 합니다. Azure AD는 강력한 암호화를 저장 데이터 및 전송 중 데이터에 사용하여 데이터를 보호합니다. 또한 Azure AD는 사용자 자격 증명을 솔트하고, 해시하고, 안전하게 저장합니다. 관리 id를 사용 하 여 코드에 자격 증명 없이 Key Vault를 포함 하 여 Azure AD 인증을 지 원하는 모든 서비스에 인증할 수 있습니다. 가상 머신에서 실행 되는 코드는 관리 되는 id를 사용 하 여 Azure AD 인증을 지 원하는 서비스에 대 한 액세스 토큰을 요청할 수 있습니다.

* [Azure AD 인스턴스를 만들고 구성하는 방법](../../active-directory-domain-services/tutorial-create-instance.md)

* [Azure 리소스에 대한 관리 ID 개요](../../active-directory/managed-identities-azure-resources/overview.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: 정기적으로 사용자 액세스 검토 및 조정

**지침**: Azure AD는 부실 계정을 검색하는 데 유용한 로그를 제공합니다. 또한 Azure Active Directory id 액세스 검토를 사용 하 여 그룹 멤버 자격, 엔터프라이즈 응용 프로그램에 대 한 액세스 및 역할 할당을 효율적으로 관리할 수 있습니다. 사용자의 액세스를 정기적으로 검토 하 여 적절 한 사용자만 계속 액세스할 수 있도록 할 수 있습니다. Azure 가상 컴퓨터를 사용 하는 경우 로컬 보안 그룹 및 사용자를 검토 하 여 시스템을 손상 시킬 수 있는 예기치 않은 계정이 없는지 확인 해야 합니다.

* [Azure ID 액세스 검토를 사용하는 방법](../../active-directory/governance/access-reviews-overview.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: 비활성화 되는 자격 증명에 대 한 액세스 시도를 모니터링 합니다.

**지침**: Azure Active Directory에 대 한 진단 설정을 구성 하 여 감사 로그 및 로그인 로그를 Log Analytics 작업 영역으로 보냅니다. 또한 Azure Monitor를 사용 하 여 Azure Virtual machines의 auth syslog 데이터에서 로그를 검토 하 고 쿼리를 수행 합니다.

* [Log Analytics 작업 영역 이해](../../azure-monitor/logs/log-analytics-tutorial.md)

* [Azure 활동 로그를 Azure Monitor에 통합하는 방법](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

* [Azure Monitor에서 사용자 지정 쿼리를 수행하는 방법](../../azure-monitor/logs/get-started-queries.md)

* [Azure Monitor의 Syslog 데이터 원본](../../azure-monitor/agents/data-sources-syslog.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: 계정 로그인 동작 편차에 대 한 경고

**지침**: Azure Active Directory의 위험 및 id 보호 기능을 사용 하 여 저장소 계정 리소스와 관련 된 검색 된 의심 스러운 작업에 대 한 자동화 된 응답을 구성할 수 있습니다. 조직의 보안 응답을 구현 하기 위해 Azure 센티널을 통해 자동화 된 응답을 사용 하도록 설정 해야 합니다.

* [Azure AD 위험한 로그인을 확인하는 방법](../../active-directory/identity-protection/overview-identity-protection.md)

* [ID 보호 위험 정책을 구성하고 사용하도록 설정하는 방법](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

* [Azure Sentinel을 온보딩하는 방법](../../sentinel/quickstart-onboard.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: 지원 시나리오에서 관련 고객 데이터에 대한 액세스 권한을 Microsoft에 제공

**지침**: 타사에서 고객 데이터에 액세스 해야 하는 경우 (예: 지원 요청 시) Azure virtual machines에 대 한 고객 Lockbox를 사용 하 여 고객 데이터 액세스 요청을 검토 하 고 승인 하거나 거부 합니다.

* [Microsoft Azure에 대한 고객 Lockbox](../../security/fundamentals/customer-lockbox-overview.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

## <a name="data-protection"></a>데이터 보호

*자세한 내용은 [보안 제어: 데이터 보호](../../security/benchmarks/security-control-data-protection.md)를 참조하세요.*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: 중요한 정보의 인벤토리 유지 관리

**지침**: 태그를 사용 하 여 중요 한 정보를 저장 하거나 처리 하는 Azure 가상 컴퓨터를 추적 하는 데 도움을 줍니다.

* [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: 중요한 정보를 저장하거나 처리하는 시스템 격리

**지침**: 개발, 테스트 및 프로덕션을 위한 별도의 구독 및/또는 관리 그룹을 구현합니다. 리소스는 가상 네트워크/서브넷으로 구분 되며, 적절 하 게 태그가 지정 되 고, NSG (네트워크 보안 그룹) 또는 Azure 방화벽 내에서 보호 되어야 합니다. 중요 한 데이터를 저장 하거나 처리 Virtual Machines 하는 경우 정책 및 프로시저를 구현 하 여 사용 하지 않을 때이를 해제 합니다.

* [추가 Azure 구독을 만드는 방법](../../cost-management-billing/manage/create-subscription.md)

* [관리 그룹을 만드는 방법](../../governance/management-groups/create-management-group-portal.md)

* [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

* [Virtual Network를 만드는 방법](../../virtual-network/quick-create-portal.md)

* [보안 구성을 사용하여 NSG를 만드는 방법](../../virtual-network/tutorial-filter-network-traffic.md)

* [Azure 방화벽을 배포 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

* [Azure 방화벽을 사용 하 여 경고 또는 경고 및 거부를 구성 하는 방법](../../firewall/threat-intel.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: 중요한 정보에 대한 무단 전송 모니터링 및 차단

**지침**: 네트워크 경계에서 타사 솔루션을 구현 하 여 중요 한 정보에 대 한 무단 전송을 모니터링 하 고 정보 보안 전문가에 게 경고 하는 동안 이러한 전송을 차단 합니다.

Microsoft에서 관리 하는 기본 플랫폼의 경우 Microsoft는 모든 고객 콘텐츠를 중요 한 것으로 간주 하 여 고객 데이터 손실 및 노출을 방지 합니다. Azure 내에서 고객 데이터를 안전하게 유지하기 위해 Microsoft는 강력한 데이터 보호 제어 및 기능 모음을 구현하고 유지 관리합니다.

* [Azure의 고객 데이터 보호 이해](../../security/fundamentals/protection-customer-data.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: 전송 중인 모든 중요한 정보 암호화

**지침**: Linux를 실행 하는 vm (Virtual Machines)에 전송 되는 데이터는 RDP 또는 SSH 세션에서 vm에 연결할 때와 같은 연결의 특성에 따라 여러 가지 방법으로 암호화 됩니다.

Microsoft는 TLS (전송 계층 보안) 프로토콜을 사용 하 여 클라우드 서비스와 고객 간에 이동할 때 데이터를 보호 합니다.

* [VM의 전송 중 암호화](../../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: 공유됨

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: 활성 검색 도구를 사용하여 중요한 데이터 식별

**지침**: 타사 활성 검색 도구를 사용 하 여 온사이트 또는 원격 서비스 공급자를 비롯 하 여 조직의 기술 시스템에서 저장, 처리 또는 전송 하는 중요 한 정보를 모두 식별 하 고 조직의 중요 한 정보 인벤토리를 업데이트 합니다.

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Azure RBAC를 사용하여 리소스에 대한 액세스 제어

**지침**: azure RBAC (역할 기반 액세스 제어)를 사용 하 여 팀 내에서 업무를 분리 하 고 사용자에 게 해당 작업을 수행 하는 데 필요한 만큼의 액세스 권한만 부여할 수 있습니다. VM에서 모든 사람에게 무제한 권한을 제공하는 대신 특정 작업만 허용할 수 있습니다. Azure CLI 또는 Azure PowerShell를 사용 하 여 Azure Portal에서 VM에 대 한 액세스 제어를 구성할 수 있습니다.

* [Azure RBAC](../../role-based-access-control/overview.md)

* [Azure 기본 제공 역할](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: 호스트 기반 데이터 손실 방지를 사용하여 액세스 제어 적용

**지침**: 자동화 된 호스트 기반 데이터 손실 방지 솔루션과 같은 타사 도구를 구현 하 여 데이터 위반 위험을 완화 하는 액세스 제어를 적용 합니다.

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: 중요한 저장 정보 암호화

**지침**: VM (Linux Virtual Machines)의 가상 디스크는 서버 쪽 암호화 또는 ADE (Azure disk encryption)를 사용 하 여 미사용으로 암호화 됩니다. Azure Disk Encryption은 Linux의 DM-Crypt 기능을 활용 하 여 게스트 VM 내에서 고객 관리 키를 사용 하 여 관리 디스크를 암호화 합니다. 고객 관리형 키를 사용하는 서버 쪽 암호화는 스토리지 서비스의 데이터를 암호화하여 VM에 대한 모든 OS 유형 및 이미지를 사용할 수 있도록 설정하여 ADE에서 향상됩니다.

* [Azure managed disks의 서버 쪽 암호화](../disk-encryption.md)

* [Linux VM용 Azure Disk Encryption](./disk-encryption-overview.md)

**Azure Security Center 모니터링**: 예

**책임**: 공유됨

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: 중요한 Azure 리소스에 대한 변경 내용 기록 및 경고

**지침**: Azure 활동 로그와 함께 Azure Monitor를 사용 하 여 가상 머신 및 관련 리소스에 대 한 변경 내용이 발생 하는 경우에 대 한 경고를 만듭니다.

* [Azure 활동 로그 이벤트에 대한 경고를 만드는 방법](../../azure-monitor/alerts/alerts-activity-log.md)

* [Azure 활동 로그 이벤트에 대한 경고를 만드는 방법](../../azure-monitor/alerts/alerts-activity-log.md)

* [Azure Storage 분석 로깅](../../storage/common/storage-analytics-logging.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

## <a name="vulnerability-management"></a>취약점 관리

*자세한 내용은 [보안 제어: 취약성 관리](../../security/benchmarks/security-control-vulnerability-management.md)를 참조하세요.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: 자동화된 취약성 검사 도구 실행

**지침**: Linux OS 내에서 맬웨어 방지 취약성 검색을 위한 타사 도구가 필요 합니다.

* [Azure Security center에 Linux 서버를 등록 하는 방법에 대 한 지침](../../security-center/quickstart-onboard-machines.md)

* [Microsoft 권장 보안 지침](../security-recommendations.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: 자동화된 운영 체제 패치 관리 솔루션 배포

**지침**: Azure 업데이트 관리 솔루션을 사용 하 여 가상 머신에 대 한 업데이트 및 패치를 관리할 수 있습니다. 업데이트 관리는 로컬로 구성 된 업데이트 리포지토리를 사용 하 여 지원 되는 시스템을 패치 합니다.

* [Azure의 업데이트 관리 솔루션](../../automation/update-management/overview.md)

* [Vm에 대 한 업데이트 및 패치 관리](../../automation/update-management/manage-updates-for-vm.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: 타사 소프트웨어 타이틀에 대 한 자동화 된 패치 관리 솔루션 배포

**지침**: 타사 패치 관리 솔루션을 사용할 수 있습니다. Azure 업데이트 관리 솔루션을 사용 하 여 가상 컴퓨터에 대 한 업데이트 및 패치를 관리할 수 있습니다. 업데이트 관리는 로컬로 구성 된 업데이트 리포지토리를 사용 하 여 지원 되는 시스템을 패치 합니다.

* [Azure의 업데이트 관리 솔루션](../../automation/update-management/overview.md)

* [Vm에 대 한 업데이트 및 패치 관리](../../automation/update-management/manage-updates-for-vm.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: 연속 취약성 검사 비교

**지침**: 검색 결과를 일관 된 간격으로 내보내고 결과를 비교 하 여 취약성이 수정 되었는지 확인 합니다. Azure Security Center에서 제안 하는 취약성 관리 권장 사항을 사용 하는 경우 고객은 선택한 솔루션의 포털로 피벗 하 여 기록 검색 데이터를 볼 수 있습니다.

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: 위험 등급 프로세스를 사용하여 검색된 취약성의 수정 우선 순위 지정

**지침**: Azure Security Center에서 제공 하는 기본 위험 등급 (보안 점수)을 사용 합니다.

* [보안 점수 Azure Security Center 이해](../../security-center/secure-score-security-controls.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

## <a name="inventory-and-asset-management"></a>인벤토리 및 자산 관리

*자세한 내용은 [보안 제어: 인벤토리 및 자산 관리](../../security/benchmarks/security-control-inventory-asset-management.md)를 참조하세요.*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: 자동화 된 asset discovery 솔루션 사용

**지침**: Azure 리소스 그래프를 사용 하 여 구독 내의 모든 리소스 (가상 머신 포함)를 쿼리하고 검색 합니다. 테넌트에서 적절한 권한(읽기)이 있는지 확인하고, 모든 Azure 구독 및 구독 내의 리소스를 열거할 수 있습니다.

* [Azure Graph를 사용하여 쿼리를 만드는 방법](../../governance/resource-graph/first-query-portal.md)

* [Azure 구독을 확인하는 방법](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 이해](../../role-based-access-control/overview.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: 자산 메타데이터 유지 관리

**지침**: Azure 리소스에 태그를 적용 하 여 분류에 따라 논리적으로 구성 하는 메타 데이터를 제공 합니다.

* [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: 권한 없는 Azure 리소스 삭제

**지침**: 적절 한 태그 지정, 관리 그룹 및 별도의 구독을 사용 하 여 가상 머신 및 관련 리소스를 구성 하 고 추적 합니다. 정기적으로 인벤토리를 조정하고, 구독에서 권한 없는 리소스가 적시에 삭제되도록 합니다.

* [추가 Azure 구독을 만드는 방법](../../cost-management-billing/manage/create-subscription.md)

* [관리 그룹을 만드는 방법](../../governance/management-groups/create-management-group-portal.md)

* [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: 승인 된 Azure 리소스의 인벤토리 정의 및 유지 관리

**지침**: 계산 리소스에 대해 승인 된 Azure 리소스 및 승인 된 소프트웨어의 인벤토리를 만들어야 합니다. 구성 된 컴퓨터 그룹에서 실행할 수 있는 응용 프로그램 집합을 정의 하는 데 도움이 되는 Azure Security Center 기능인 적응 응용 프로그램 컨트롤을 사용할 수도 있습니다. 이 기능은 Azure 및 비 Azure Windows (모든 버전, 클래식 또는 Azure Resource Manager) 및 Linux 컴퓨터에서 사용할 수 있습니다.

         

* [적응형 응용 프로그램 제어를 사용 하도록 설정 하는 방법](../../security-center/security-center-adaptive-application.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: 승인되지 않은 Azure 리소스 모니터링

**지침**: Azure Policy에서 다음 기본 제공 정책 정의를 사용한 고객 구독에 만들 수 있는 리소스 종류에 대한 제한을 설정할 수 있습니다.
- 허용되지 않는 리소스 종류
- 허용되는 리소스 유형

또한 Azure Resource Graph를 사용하여 구독 내에서 리소스를 쿼리/검색합니다. 이는 저장소 계정을 사용 하는 환경 등의 높은 보안 기반 환경에서 유용할 수 있습니다.

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [Azure Graph를 사용하여 쿼리를 만드는 방법](../../governance/resource-graph/first-query-portal.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: 컴퓨팅 리소스 내에서 승인되지 않은 소프트웨어 애플리케이션 모니터링

**지침**: Azure Automation는 작업 및 리소스의 배포, 작업 및 서비스 해제 중에 완전 한 제어를 제공 합니다. Azure Virtual Machine 인벤토리를 활용 하 여 Virtual Machines의 모든 소프트웨어에 대 한 정보 수집을 자동화할 수 있습니다. 참고: 소프트웨어 이름, 버전, 게시자 및 새로 고침 시간은 Azure Portal에서 사용할 수 있습니다. 메트릭 및 기타 정보에 대 한 액세스 권한을 얻으려면, 고객이 게스트 수준 진단을 사용 하도록 설정 하 고 syslog 정보를 지정 된 저장소 계정으로 보낼 수 있어야 합니다.

소프트웨어 응용 프로그램 모니터링을 위해 변경 내용 추적를 사용 하는 것 외에도 Azure Security Center의 적응 응용 프로그램 컨트롤은 기계 학습을 사용 하 여 컴퓨터에서 실행 되는 응용 프로그램을 분석 하 고이 인텔리전스에서 허용 목록을 만듭니다. 이 기능은 응용 프로그램 허용 목록 정책을 구성 하 고 유지 관리 하는 프로세스를 간소화 하 여 사용자 환경에서 원치 않는 소프트웨어를 사용 하지 않도록 할 수 있도록 합니다. 감사 모드를 구성 하거나 모드를 적용할 수 있습니다. 감사 모드는 보호 된 Vm의 활동만 감사 합니다. 적용 모드는 규칙을 적용 하 고 실행이 허용 되지 않은 응용 프로그램이 차단 되도록 합니다.

* [Linux 가상 머신에 대 한 진단 확장](../extensions/diagnostics-linux.md?toc=/azure/azure-monitor/toc.json)

* [Azure Automation 소개](../../automation/automation-intro.md)

* [Azure VM 인벤토리를 사용 하도록 설정 하는 방법](../../automation/automation-tutorial-installed-software.md)

* [적응 응용 프로그램 컨트롤 이해](../../security-center/security-center-adaptive-application.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: 승인되지 않은 Azure 리소스 및 소프트웨어 애플리케이션 제거

**지침**: Azure Automation는 작업 및 리소스의 배포, 작업 및 서비스 해제 중에 완전 한 제어를 제공 합니다. 변경 내용 추적를 사용 하 여 Virtual Machines에 설치 된 모든 소프트웨어를 식별할 수 있습니다. 사용자 고유의 프로세스를 구현 하거나 인증 되지 않은 소프트웨어를 제거 하기 위해 Azure Automation 상태 구성을 사용할 수 있습니다.

* [Azure Automation 소개](../../automation/automation-intro.md)

* [변경 내용 추적 이해](../../automation/change-tracking/overview.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="68-use-only-approved-applications"></a>6.8: 승인된 애플리케이션만 사용

**지침**: 적응 응용 프로그램 제어 Azure Security Center를 사용 하 여 권한 있는 소프트웨어만 실행 되 고 권한이 없는 모든 소프트웨어가 Azure Virtual Machines에서 실행 되지 않도록 차단 합니다.

* [Azure Security Center 적응 응용 프로그램 컨트롤을 사용 하는 방법](../../security-center/security-center-adaptive-application.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: 승인된 Azure 서비스만 사용

**지침**: Azure Policy에서 다음 기본 제공 정책 정의를 사용한 고객 구독에 만들 수 있는 리소스 종류에 대한 제한을 설정할 수 있습니다.
- 허용되지 않는 리소스 종류
- 허용되는 리소스 유형

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [Azure Policy를 사용하여 특정 리소스 종류를 거부하는 방법](../../governance/policy/samples/index.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: 승인 된 소프트웨어 타이틀의 인벤토리 유지 관리

**지침**: 적응 응용 프로그램 제어는 azure 및 비 azure 컴퓨터 (Windows 및 Linux)에서 실행할 수 있는 응용 프로그램을 제어 하는 데 도움이 되는 Azure Security Center의 지능적이 고 자동화 된 종단 간 솔루션입니다. 조직의 요구 사항에 맞지 않는 경우 타사 솔루션을 구현 합니다.

* [Azure Security Center 적응 응용 프로그램 컨트롤을 사용 하는 방법](../../security-center/security-center-adaptive-application.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: 사용자가 Azure Resource Manager 상호 작용할 수 있도록 제한

**지침**: "Microsoft Azure 관리" 앱에 대한 "액세스 차단"을 구성함으로써 Azure 조건부 액세스를 사용하여 Azure Resource Manager와 상호 작용하는 사용자 기능을 제한합니다.

* [Azure Resource Manager에 대한 액세스를 차단하도록 조건부 액세스를 구성하는 방법](../../role-based-access-control/conditional-access-azure-management.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: 사용자가 컴퓨팅 리소스 내에서 스크립트를 실행하는 기능 제한

**지침**: 스크립트 유형에 따라 운영 체제별 구성 또는 타사 리소스를 사용 하 여 Azure compute 리소스 내에서 스크립트를 실행 하는 사용자의 기능을 제한할 수 있습니다. Azure Security Center 적응 응용 프로그램 제어를 활용 하 여 권한 있는 소프트웨어만 실행 되 고 권한이 없는 모든 소프트웨어가 Azure Virtual Machines에서 실행 되지 않도록 차단할 수도 있습니다.

* [Azure Security Center 적응 응용 프로그램 컨트롤을 사용 하는 방법](../../security-center/security-center-adaptive-application.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: 위험 수준이 높은 애플리케이션을 물리적 또는 논리적으로 분리

**지침**: azure 환경에 배포 된 위험도 높은 응용 프로그램은 가상 네트워크, 서브넷, 구독, 관리 그룹을 사용 하 여 격리 될 수 있으며, azure 방화벽, Waf (웹 응용 프로그램 방화벽) 또는 nsg (네트워크 보안 그룹)를 사용 하 여 충분히 안전 하 게 보호할 수 있습니다.

* [Azure의 가상 네트워크 및 가상 머신](../network-overview.md)

* [Azure Firewall 개요](../../firewall/overview.md)

* [웹 애플리케이션 방화벽 개요](../../web-application-firewall/overview.md)

* [네트워크 보안 개요](../../virtual-network/network-security-groups-overview.md)

* [Azure Virtual Network 개요](../../virtual-network/virtual-networks-overview.md)

* [Azure 관리 그룹으로 리소스 구성](../../governance/management-groups/overview.md)

* [구독 관련 결정 가이드](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

## <a name="secure-configuration"></a>보안 구성

*자세한 내용은 [보안 제어: 보안 구성](../../security/benchmarks/security-control-secure-configuration.md)을 참조하세요.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: 모든 Azure 리소스에 대한 보안 구성 설정

**지침**: Azure Policy 또는 Azure Security Center를 사용 하 여 모든 Azure 리소스에 대 한 보안 구성을 유지 합니다. 또한 Azure Resource Manager은 구성이 회사의 보안 요구 사항을 충족 하거나 초과 하는지 확인 하기 위해 검토 해야 하는 JavaScript Object Notation (JSON)에서 템플릿을 내보낼 수 있습니다.

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [VM 템플릿을 다운로드 하는 방법에 대 한 정보](/previous-versions/azure/virtual-machines/windows/download-template)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: 보안 운영 체제 구성 설정

**지침**: 모든 계산 리소스에 대 한 보안 구성을 유지 하려면 Azure Security Center 권장 사항 [Virtual Machines 보안 구성의 취약성 수정]을 사용 하세요.

* [Azure Security Center 권장 사항을 모니터링 하는 방법](../../security-center/security-center-recommendations.md)

* [Azure Security Center 권장 사항을 수정 하는 방법](../../security-center/security-center-remediate-recommendations.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: 보안 Azure 리소스 구성 유지 관리

**지침**: Azure Resource Manager 템플릿 및 azure 정책을 사용 하 여 가상 머신과 연결 된 azure 리소스를 안전 하 게 구성 합니다. Azure Resource Manager 템플릿은 Azure 리소스와 함께 가상 머신을 배포 하는 데 사용 되는 JSON 기반 파일이 며 사용자 지정 템플릿은 유지 관리 해야 합니다. Microsoft는 기본 템플릿에 대 한 유지 관리를 수행 합니다. [거부] 및 [존재하지 않으면 배포] Azure 정책을 사용하여 보안 설정을 Azure 리소스 전체에 적용합니다.

* [Azure Resource Manager 템플릿 만들기에 대 한 정보](../windows/ps-template.md)

* [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

* [Azure Policy 효과 이해](../../governance/policy/concepts/effects.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: 보안 운영 체제 구성 유지 관리

**지침**: 배포를 위해 VM (Azure Virtual Machines)에 대 한 보안 구성을 유지 관리할 수 있는 몇 가지 옵션이 있습니다.

1 Azure Resource Manager 템플릿: Azure Portal에서 VM을 배포 하는 데 사용 되는 JSON 기반 파일이 며 사용자 지정 템플릿을 유지 관리 해야 합니다. Microsoft는 기본 템플릿에 대 한 유지 관리를 수행 합니다.

2-사용자 지정 VHD (가상 하드 디스크): 어떤 경우에는 다른 방법을 통해 관리할 수 없는 복잡 한 환경을 처리할 때와 같이 사용자 지정 VHD 파일을 사용 해야 할 수도 있습니다.

3-Azure Automation 상태 구성: 기본 OS가 배포 되 면이를 사용 하 여 보다 세부적인 설정을 제어 하 고 자동화 프레임 워크를 통해 적용할 수 있습니다.

대부분의 시나리오에서 Azure Automation 필요한 상태 구성과 결합 된 Microsoft 기본 VM 템플릿은 보안 요구 사항을 충족 하 고 유지 관리 하는 데 도움이 될 수 있습니다.

* [VM 템플릿을 다운로드 하는 방법에 대 한 정보](/previous-versions/azure/virtual-machines/windows/download-template)

* [ARM 템플릿 만들기에 대 한 정보](../windows/ps-template.md)

* [Azure에 사용자 지정 VM VHD를 업로드 하는 방법](/azure-stack/operator/azure-stack-add-vm-image?view=azs-1910&preserve-view=true)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure 리소스 구성을 안전하게 저장

**지침**: Azure devops/리포지토리를 사용 하 여 사용자 지정 Azure Policy 정의, Azure Resource Manager 템플릿, 필요한 상태 구성 스크립트 및 기타 코드와 같은 코드를 안전 하 게 저장 하 고 관리 합니다. 코드, 빌드 및 작업 추적과 같이 Azure DevOps에서 관리 하는 리소스에 액세스 하려면 해당 특정 리소스에 대 한 권한이 있어야 합니다. 사용 권한 및 액세스에 설명 된 대로 기본 제공 보안 그룹을 통해 대부분의 사용 권한을 부여 합니다. Azure DevOps와 통합 된 경우 Azure Active Directory (Azure AD)에 정의 된 특정 사용자, 기본 제공 보안 그룹 또는 그룹에 대 한 사용 권한을 부여 하거나 거부할 수 있습니다. 또는 TFS와 통합 된 경우 Active Directory 합니다.

* [Azure DevOps에 코드를 저장하는 방법](/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure DevOps의 사용 권한 및 그룹 정보](/azure/devops/organizations/security/about-permissions)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: 사용자 지정 운영 체제 이미지를 안전하게 저장

**지침**: 사용자 지정 이미지 (예: 가상 하드 디스크)를 사용 하는 경우 azure 역할 기반 액세스 제어 (azure RBAC)를 사용 하 여 권한 있는 사용자만 이미지에 액세스할 수 있도록 합니다.

* [Azure RBAC 이해](../../role-based-access-control/rbac-and-directory-admin-roles.md)

* [Azure RBAC를 구성 하는 방법](../../role-based-access-control/quickstart-assign-role-user-portal.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Azure 리소스에 대 한 구성 관리 도구 배포

**지침**: Azure Policy 활용 하 여 가상 머신에 대 한 시스템 구성을 경고, 감사 및 적용 합니다. 또한 정책 예외를 관리하는 프로세스와 파이프라인을 개발합니다.

* [Azure 정책을 구성 하 고 관리 하는 방법](../../governance/policy/tutorials/create-and-manage.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: 운영 체제용 구성 관리 도구를 배포 합니다.

**지침**: Azure Automation 상태 구성은 모든 클라우드 또는 온-프레미스 데이터 센터에서 DSC (Desired State configuration) 노드에 대 한 구성 관리 서비스입니다. 안전한 중앙 위치에서 수천 대의 컴퓨터를 빠르고 간편하게 확장할 수 있습니다. 컴퓨터를 쉽게 온보드하고, 컴퓨터에 선언적 구성을 할당하고, 사용자가 지정한 필요 상태에 대한 각 컴퓨터의 규정 준수를 나타내는 보고서를 확인할 수 있습니다.

* [Azure Automation 상태 구성을 통한 관리를 위한 머신 온보드](../../automation/automation-dsc-onboarding.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Azure 리소스에 대 한 자동화 된 구성 모니터링 구현

**지침**: Azure Security Center 활용 하 여 Azure 가상 머신에 대 한 기준 검색을 수행 합니다. 자동화 된 구성의 추가 방법에는 Azure Automation 상태 구성을 사용 하는 작업이 포함 됩니다.

* [Azure Security Center에서 권장 사항을 수정 하는 방법](../../security-center/security-center-remediate-recommendations.md)

* [Azure Automation 상태 구성 시작하기](../../automation/automation-dsc-getting-started.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: 운영 체제에 대한 자동화된 구성 모니터링 구현

**지침**: Azure Automation 상태 구성은 모든 클라우드 또는 온-프레미스 데이터 센터에서 DSC (Desired State configuration) 노드에 대 한 구성 관리 서비스입니다. 안전한 중앙 위치에서 수천 대의 컴퓨터를 빠르고 간편하게 확장할 수 있습니다. 컴퓨터를 쉽게 온보드하고, 컴퓨터에 선언적 구성을 할당하고, 사용자가 지정한 필요 상태에 대한 각 컴퓨터의 규정 준수를 나타내는 보고서를 확인할 수 있습니다.

* [Azure Automation 상태 구성을 통한 관리를 위한 머신 온보드](../../automation/automation-dsc-onboarding.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: 안전하게 Azure 비밀 관리

**지침**: Azure Key Vault와 함께 관리 서비스 ID를 사용 하 여 클라우드 응용 프로그램에 대 한 비밀 관리를 간소화 하 고 보호 합니다.

* [Azure 관리 Id와 통합 하는 방법](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

* [Key Vault를 만드는 방법](../../key-vault/general/quick-create-portal.md)

* [Key Vault에 인증 하는 방법](../../key-vault/general/authentication.md)

* [Key Vault 액세스 정책을 할당 하는 방법](../../key-vault/general/assign-access-policy-portal.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: 안전하게 자동으로 ID 관리

**지침**: 관리 되는 id를 사용 하 여 azure AD에서 자동으로 관리 되는 Id를 azure 서비스에 제공 합니다. 관리 ID를 사용하면 코드에 자격 증명 없이 Key Vault를 포함하여 Azure AD 인증을 지원하는 모든 서비스에 인증할 수 있습니다.

* [관리 Id를 구성 하는 방법](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: 의도하지 않은 자격 증명 노출 제거

**지침**: 자격 증명 스캐너를 구현하여 코드 내에서 자격 증명을 식별합니다. 또한 자격 증명 스캐너는 검색된 자격 증명을 더 안전한 위치(예: Azure Key Vault)로 이동하도록 추천합니다.

* [자격 증명 스캐너를 설정하는 방법](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

## <a name="malware-defense"></a>맬웨어 방어

*자세한 내용은 [보안 제어: 맬웨어 방어](../../security/benchmarks/security-control-malware-defense.md)를 참조하세요.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: 중앙에서 관리 하는 맬웨어 방지 소프트웨어 사용

**지침**: Azure Linux 가상 머신에서 맬웨어 방지 보호를 위한 타사 도구가 필요 합니다.

* [Cloud Services 및 Virtual Machines에 대해 Microsoft 맬웨어 방지 프로그램을 구성 하는 방법](../security-recommendations.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: 비 컴퓨팅 Azure 리소스에 업로드할 파일 미리 검사

**지침**: 계산 리소스 이므로 Azure Virtual machines에는 적용 되지 않습니다.

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: 해당 없음

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: 맬웨어 방지 소프트웨어 및 서명이 업데이트되었는지 확인

**지침**: Azure Linux 가상 머신에서 맬웨어 방지 보호를 위한 타사 도구가 필요 합니다.

* [Cloud Services 및 Virtual Machines에 대해 Microsoft 맬웨어 방지 프로그램을 구성 하는 방법](../security-recommendations.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

## <a name="data-recovery"></a>데이터 복구

*자세한 내용은 [보안 제어: 데이터 복구](../../security/benchmarks/security-control-data-recovery.md)를 참조하세요.*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: 정기 자동 백업 확인

**지침**: Azure Backup을 사용 하도록 설정 하 고 Azure VM (가상 머신)을 구성 하 고 자동 백업에 대 한 원하는 빈도 및 보존 기간을 구성 합니다.

* [Azure VM 백업 개요](../../backup/backup-azure-vms-introduction.md)

* [VM 설정에서 Azure VM 백업](../../backup/backup-azure-vms-first-look-arm.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: 전체 시스템 백업을 수행 하 고 고객 관리 키를 백업 합니다.

**지침**: POWERSHELL 또는 REST api를 사용 하 여 Azure 가상 머신 또는 해당 인스턴스에 연결 된 관리 디스크의 스냅숏을 만듭니다. Azure Key Vault 내에서 고객 관리 키를 백업 합니다.

원하는 주파수와 보존 기간 뿐만 아니라 Azure Backup 및 대상 Azure Virtual Machines (VM)을 사용 하도록 설정 합니다. 여기에는 전체 시스템 상태 백업이 포함 됩니다. Azure disk encryption을 사용 하는 경우 Azure VM 백업은 고객이 관리 하는 키의 백업을 자동으로 처리 합니다.

* [암호화를 사용 하는 Azure Vm에 백업](../../backup/backup-azure-vms-encryption.md)

* [Azure VM 백업 개요](../../backup/backup-azure-vms-introduction.md)

* [자동 백업 작동 방법 이해](../../backup/backup-azure-vms-encryption.md)

* [Azure에서 키 자격 증명 모음 키를 백업하는 방법](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: 고객 관리 키를 비롯 한 모든 백업 유효성 검사

**지침**: Azure Backup 내에서 콘텐츠의 데이터 복원을 정기적으로 수행할 수 있는지 확인 합니다. 필요한 경우 격리 된 가상 네트워크 또는 구독에 대 한 콘텐츠 복원을 테스트 합니다. 고객이 관리 하는 백업 키의 복원을 테스트 합니다.

Azure disk encryption을 사용 하는 경우 디스크 암호화 키를 사용 하 여 Azure VM을 복원할 수 있습니다. 디스크 암호화를 사용 하는 경우 디스크 암호화 키를 사용 하 여 Azure VM을 복원할 수 있습니다.

* [암호화를 사용 하는 Azure Vm에 백업](../../backup/backup-azure-vms-encryption.md)

* [Azure Virtual Machine 백업에서 파일을 복구 하는 방법](../../backup/backup-azure-restore-files-from-vm.md)

* [Azure에서 키 자격 증명 모음 키를 복원하는 방법](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

* [암호화 된 VM을 백업 및 복원 하는 방법](../../backup/backup-azure-vms-encryption.md)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: 백업 및 고객이 관리 하는 키를 보호 해야 합니다.

**지침**: Azure Backup을 사용 하 여 Azure vm을 백업 하는 경우 vm은 저장소 서비스 암호화 (SSE)를 사용 하 여 미사용 상태로 암호화 됩니다. Azure Backup Azure Disk Encryption를 사용 하 여 암호화 된 Azure Vm을 백업할 수도 있습니다. Azure Disk Encryption은 KEKs (Azure Key Vault 키 암호화 키)와도 통합 됩니다. Key Vault에서 일시 삭제를 사용하도록 설정하여 실수로 또는 악의적으로 삭제되지 않도록 키를 보호합니다. 

* [Vm에 대 한 일시 삭제](../../backup/soft-delete-virtual-machines.md)

* [Azure Key Vault 일시 삭제 개요](../../key-vault/general/soft-delete-overview.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

## <a name="incident-response"></a>사고 대응

*자세한 내용은 [보안 제어: 인시던트 대응](../../security/benchmarks/security-control-incident-response.md)을 참조하세요.*

### <a name="101-create-an-incident-response-guide"></a>10.1: 인시던트 대응 지침 만들기

**지침**: 조직에 대한 인시던트 대응 지침을 작성합니다. 탐지에서 인시던트 사후 검토까지의 인시던트 처리/관리 단계뿐만 아니라 담당자의 모든 역할도 정의하는 인시던트 대응 계획이 작성되어 있는지 확인합니다.

* [사용자 고유의 보안 인시던트 대응 프로세스를 구축하는 방법에 대한 지침](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft 보안 대응 센터의 인시던트 분석](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [또한 고객은 NIST의 컴퓨터 보안 인시던트 처리 가이드를 활용 하 여 고유한 인시던트 대응 계획을 만드는 데 도움이 될 수 있습니다.](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: 인시던트 점수 매기기 및 우선 순위 지정 절차 만들기

**지침**: Security Center에서 심각도를 각 경고에 할당하여 먼저 조사해야 하는 경고에 대한 우선 순위를 지정합니다. 심각도는 Security Center에서 경고를 실행하는 데 사용된 결과 또는 분석의 신뢰도 및 경고가 발생된 활동의 배후에 악의적인 의도가 있었음에 대한 신뢰 수준을 기준으로 합니다. 또한 태그를 사용하여 구독(예: 프로덕션, 비 프로덕션)을 명확하게 표시하고 Azure 리소스, 특히 중요한 데이터를 처리하는 리소스를 명확하게 식별하고 분류하는 명명 시스템을 만듭니다. 인시던트가 발생한 Azure 리소스 및 환경의 중요도에 따라 경고의 수정에 대한 우선 순위를 지정해야 합니다.

* [Azure Security Center의 보안 경고](../../security-center/security-center-alerts-overview.md)

* [태그를 사용하여 Azure 리소스 구성](../../azure-resource-manager/management/tag-resources.md).

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: 보안 대응 프로시저 테스트

**지침**: Azure 리소스를 보호하는 데 도움이 되도록 시스템의 인시던트 대응 기능을 정기적으로 테스트합니다.

* [필요에 따라 weak 점수와 격차 및 수정 계획을 식별 합니다. IT 계획 및 기능에 대 한 NIST의 게시: 테스트, 학습 및 연습 프로그램 가이드를 참조 하세요.](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: 보안 인시던트 연락처 세부 정보 제공 및 보안 인시던트에 대한 경고 알림 구성

**지침**: MSRC(Microsoft 보안 대응 센터)에서 불법적이거나 권한이 없는 당사자가 데이터에 액세스했다고 검색하는 경우 Microsoft에서 보안 인시던트 연락처 정보를 사용하여 사용자에게 연락합니다. 문제가 해결되었는지 확인하기 위해 사후에 인시던트를 검토합니다.

* [Azure Security Center 보안 연락처를 설정하는 방법](../../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center 모니터링**: 예

**책임**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: 보안 경고를 인시던트 대응 시스템에 통합

**지침**: Azure 리소스에 대한 위험을 식별하는 데 도움이 되도록 연속 내보내기 기능을 사용하여 Azure Security Center 경고 및 추천 사항을 내보냅니다. 연속 내보내기를 사용하면 경고 및 추천 사항을 수동으로 또는 지속적으로 내보낼 수 있습니다. Azure Security Center 데이터 커넥터를 사용하여 경고를 Azure Sentinel로 스트림할 수 있습니다.

* [연속 내보내기를 구성하는 방법](../../security-center/continuous-export.md)

* [경고를 Azure Sentinel로 스트림하는 방법](../../sentinel/connect-azure-security-center.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: 보안 경고에 대한 대응 자동화

**지침**: Azure Security Center의 Workflow Automation 기능을 사용 하 여 Azure 리소스를 보호 하기 위해 보안 경고 및 권장 사항에 대 한 "Logic Apps"를 통해 응답을 자동으로 트리거합니다.

* [워크플로 자동화 및 Logic Apps를 구성하는 방법](../../security-center/workflow-automation.md)

**Azure Security Center 모니터링**: 사용할 수 없음

**책임**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>침투 테스트 및 레드 팀 연습

*자세한 내용은 [보안 제어: 침투 테스트 및 레드 팀 연습](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md)을 참조하세요.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Azure 리소스에 대 한 정기적인 침투 테스트를 수행 하 고 모든 중요 한 보안 결과를 수정 하세요.

**지침**: Microsoft Engagement 규칙에 따라 침투 테스트가 microsoft 정책을 위반 하지 않는지 확인 합니다. Microsoft에서 관리 하는 클라우드 인프라, 서비스 및 응용 프로그램에 대 한 레드 팀 및 라이브 사이트 침투 테스트의 전략과 실행을 사용 합니다.

* [침투 테스트 시행 규칙](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft Cloud 레드 팀](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center 모니터링**: 해당 사항 없음

**책임**: 공유됨

## <a name="next-steps"></a>다음 단계

- [Azure 보안 벤치마크](../../security/benchmarks/overview.md)를 참조하세요.
- [Azure 보안 기준](../../security/benchmarks/security-baselines-overview.md)에 대해 자세히 알아보세요.