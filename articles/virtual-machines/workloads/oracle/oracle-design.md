---
title: Azure에서 Oracle 데이터베이스 설계 및 구현 | Microsoft Docs
description: Azure 환경에서 Oracle 데이터베이스를 설계하고 구현합니다.
author: dbakevlar
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 12/17/2020
ms.author: kegorman
ms.reviewer: tigorman
ms.openlocfilehash: 0b6f4e652ca8fef7bee4165bcd0673be2fa11eac
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98890767"
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Azure에서 Oracle 데이터베이스 설계 및 구현

## <a name="assumptions"></a>Assumptions

- 온-프레미스에서 Azure로 Oracle 데이터베이스 마이그레이션할 계획입니다.
- 마이그레이션하려는 Oracle Database에 대 한 [진단 팩](https://docs.oracle.com/cd/E11857_01/license.111/e11987/database_management.htm) 또는 [자동 워크 로드 리포지토리가](https://www.oracle.com/technetwork/database/manageability/info/other-manageability/wp-self-managing-database18c-4412450.pdf) 있습니다.
- Oracle의 다양 한 메트릭에 대해 이해 하 고 있습니다.
- 애플리케이션 성능 및 플랫폼 사용률에 대해 기본적으로 이해하고 있습니다.

## <a name="goals"></a>목표

- Azure에서 Oracle 배포를 최적화하는 방법을 이해합니다.
- Azure 환경에서 Oracle 데이터베이스에 대한 성능 튜닝 옵션을 탐색합니다.
- 아키텍처와 이점 또는 데이터베이스 코드 (SQL) 및 전체 데이터베이스 디자인의 논리적 튜닝을 통해 물리적 튜닝의 제한을 명확 하 게 기대 합니다.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>온-프레미스와 Azure 구현 간의 차이점 

다음은 온-프레미스 애플리케이션을 Azure로 마이그레이션할 때 유의해야 할 몇 가지 중요 사항입니다. 

중요한 차이점 중 하나는 Azure 구현에서 VM, 디스크 및 가상 네트워크와 같은 리소스가 다른 클라이언트와 공유된다는 것입니다. 또한 리소스는 요구 사항에 따라 제한될 수도 있습니다. Azure에서는 실패 방지(MTBF)에 집중하는 대신 실패 극복(MTTR)에 더 많이 집중합니다.

다음 표에는 Oracle 데이터베이스의 온-프레미스 구현과 Azure 구현 간의 차이점이 나열되어 있습니다.


|  | 온-프레미스 구현 | Azure 구현 |
| --- | --- | --- |
| **네트워킹** |LAN/WAN  |SDN(소프트웨어 방식 네트워킹)|
| **보안 그룹** |IP/포트 제한 도구 |[NSG (네트워크 보안 그룹)](https://azure.microsoft.com/blog/network-security-groups) |
| **복원력** |MTBF(평균 고장 간격) |MTTR(평균 복구 시간)|
| **계획된 유지 보수** |패치/업그레이드|[가용성 집합](/previous-versions/azure/virtual-machines/windows/infrastructure-example)(Azure에서 관리되는 패치/업그레이드) |
| **리소스** |전용  |다른 클라이언트와 공유|
| **지역** |데이터 센터 |[지역 쌍](../../regions.md#region-pairs)|
| **스토리지** |SAN/실제 디스크 |[Azure 관리 스토리지](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
| **크기 조정** |수직적 확장 |수평적 확장|


### <a name="requirements"></a>요구 사항

- Oracle이 코어에서 사용이 허가 됨에 따라 실제 CPU 사용량을 결정 하 고, vCPU 요구 사항의 크기를 조정 하는 것이 중요 합니다. 
- 데이터베이스 크기, 백업 저장소 및 증가율을 결정 합니다.
- Oracle Statspack 및 AWR 보고서 또는 OS 수준 저장소 모니터링 도구를 기준으로 예측할 수 있는 IO 요구 사항을 확인 합니다.

## <a name="configuration-options"></a>구성 옵션

Azure 환경에서 성능을 향상시키기 위해 조정할 수 있는 잠재적인 네 가지 영역이 있습니다.

- 가상 머신 크기
- 네트워크 처리량
- 디스크 형식 및 구성
- 디스크 캐시 설정

### <a name="generate-an-awr-report"></a>AWR 보고서 생성

Oracle Enterprise Edition 데이터베이스가 기존에 있고 Azure로 마이그레이션할 계획인 경우 몇 가지 옵션이 있습니다. Oracle 인스턴스에 대 한 [진단 팩](https://www.oracle.com/technetwork/oem/pdf/511880.pdf) 이 있는 경우 oracle AWR 보고서를 실행 하 여 메트릭 (IOPS, Mbps, gid 등)을 가져올 수 있습니다. 진단 팩 라이선스 또는 Standard Edition 데이터베이스를 사용 하지 않는 데이터베이스의 경우, 수동 스냅숏이 수집 된 후에도 Statspack 보고서를 사용 하 여 동일한 중요 한 메트릭을 수집할 수 있습니다.  이러한 두 보고 방법의 주요 차이점은 AWR이 자동으로 수집 되 고 데이터베이스에 대 한 자세한 정보를 제공 하는 것입니다 .이는 Statspack의 선행 작업 보고 옵션입니다.

일반 및 최대 워크로드 모두에서 AWR 보고서를 실행하여 비교하는 것이 좋을 수도 있습니다. 보다 정확한 워크 로드를 수집 하려면 한 주의 확장 된 창 보고서와 24 시간 실행을 고려 하 고, AWR은 보고서에서 계산의 일부로 평균을 제공 합니다.  데이터 센터 마이그레이션의 경우 프로덕션 시스템의 크기를 조정 하기 위한 보고서를 수집 하 고 사용자 테스트, 테스트, 개발 등에 사용 되는 나머지 데이터베이스 복사본 (UAT 프로덕션, 테스트 및 개발 50%의 프로덕션 크기 조정 등)을 예측 하는 것이 좋습니다.

기본적으로 AWR 리포지토리는 8 일간의 데이터를 유지 하 고 시간 간격으로 스냅숏을 생성 합니다.  명령줄에서 AWR 보고서를 실행 하려면 터미널에서 다음을 수행할 수 있습니다.

```bash
$ sqlplus / as sysdba
SQL> @$ORACLE_HOME/rdbms/admin/awrrpt.sql;
```

### <a name="key-metrics"></a>주요 메트릭

보고서에 다음 정보를 묻는 메시지가 표시 됩니다.
- 보고서 유형: HTML 또는 텍스트, (12.1의 HTML 및 텍스트 형식 보다 추가 정보 제공)
- 스냅숏이 표시 되는 기간 (일)입니다. 1 시간 간격의 경우 1 주일 보고서는 스냅숏 Id에서 168입니다.
- 보고서 창의 시작 SnapshotID입니다.
- 보고서 창의 끝 SnapshotId입니다.
- AWR 스크립트로 만들 보고서의 이름입니다.

RAC (실제 응용 프로그램 클러스터)에서 AWR를 실행 하는 경우 명령줄 보고서는 awrrpt가 아닌 awrgrpt입니다.  "G" 보고서는 단일 보고서에서 RAC 데이터베이스의 모든 노드에 대 한 보고서를 만들고 각 RAC 노드에서 하나를 실행 해야 합니다.

다음은 AWR 보고서에서 얻을 수 있는 메트릭입니다.

- 데이터베이스 이름, 인스턴스 이름 및 호스트 이름
- 데이터베이스 버전, (Oracle의 지원 가능성)
- CPU/코어
- SGA/이상, (그리고의 크기가 부족 인 경우 사용자에 게 알릴 수 있는 관리자)
- 전체 메모리(GB)
- CPU 사용률 (%)
- DB Cpu
- IOPs (읽기/쓰기)
- MBPs (읽기/쓰기)
- 네트워크 처리량
- 네트워크 대기 시간 비율(낮음/높음)
- 상위 대기 이벤트 
- 데이터베이스에 대 한 매개 변수 설정
- 데이터베이스 RAC, Exadata, 고급 기능 또는 구성 사용

### <a name="virtual-machine-size"></a>가상 머신 크기

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. AWR 보고서의 CPU, 메모리 및 i/o 사용량에 따라 VM 크기를 예상 합니다.

시스템 병목 현상이 발생하는 위치를 나타내는 상위 5개 시간 지정 전경 이벤트를 확인할 수 있습니다.

예를 들어 다음 다이어그램에서는 로그 파일 동기화가 맨 위에 있습니다. LGWR에서 로그 버퍼를 다시 실행 로그 파일에 쓰기 전에 필요한 대기 수를 나타냅니다. 이러한 결과는 더 효율적으로 수행되는 스토리지 또는 디스크가 필요함을 나타냅니다. 또한 다이어그램에서는 CPU(코어) 수와 메모리 양도 보여 줍니다.

![테이블 맨 위에 있는 로그 파일 동기화를 보여 주는 스크린샷](./media/oracle-design/cpu_memory_info.png)

다음 다이어그램에서는 읽기 및 쓰기의 총 I/O 수를 보여 줍니다. 보고서 시간 동안 59GB의 읽기 및 247.3GB의 쓰기가 있었습니다.

![읽기 및 쓰기의 총 i/o를 보여 주는 스크린샷](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. VM 선택

다음 단계에서는 AWR 보고서에서 수집한 정보에 따라 요구 사항을 충족하는 비슷한 크기의 VM을 선택합니다. [메모리 최적화](../../sizes-memory.md) 문서에서 사용 가능한 VM 목록을 찾을 수 있습니다.

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. ACU에 따라 유사한 VM 시리즈를 사용 하 여 VM 크기를 미세 조정 합니다.

VM을 선택한 후에는 해당 VM에 대한 ACU에 주의해야 합니다. 요구 사항에 더 적합한 ACU 값에 따라 다른 VM을 선택할 수도 있습니다. 자세한 내용은 [Azure 컴퓨팅 단위](../../acu.md)를 참조하세요.

![ACU 단위 페이지의 스크린샷](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>네트워크 처리량

다음 다이어그램에서는 처리량과 IOPS 간의 관계를 보여 줍니다.

![처리량 스크린샷](./media/oracle-design/throughput.png)

총 네트워크 처리량은 다음 정보를 기반으로 하여 예측합니다.
- SQL*Net 트래픽
- MBps x 서버 수(Oracle Data Guard와 같은 아웃바운드 스트림)
- 애플리케이션 복제와 같은 다른 요소

![SQL*Net 처리량의 스크린샷](./media/oracle-design/sqlnet_info.png)

네트워크 대역폭 요구 사항에 따라 다양한 게이트웨이 유형을 선택할 수 있습니다. 여기에는 기본, VpnGw 및 Azure ExpressRoute가 포함됩니다. 자세한 내용은 [VPN Gateway 가격 페이지](https://azure.microsoft.com/pricing/details/vpn-gateway/?v=17.23h)를 참조하세요.

**권장 사항**

- 네트워크 대기 시간은 온-프레미스 배포에 비해 더 높습니다. 네트워크 왕복 수를 줄이면 성능이 크게 향상될 수 있습니다.
- 왕복을 줄이려면 동일한 가상 머신에서 트랜잭션이 많은 애플리케이션 또는 "채팅 가능한(chatty)" 앱을 통합합니다.
- 네트워크 성능을 향상 시키려면 [가속화 된 네트워킹](../../../virtual-network/create-vm-accelerated-networking-cli.md) 을 사용 하는 Virtual Machines를 사용 합니다.
- 특정 Linux 배포판의 경우 [트리밍/매핑 해제 지원을](/previous-versions/azure/virtual-machines/linux/configure-lvm#trimunmap-support)사용 하도록 설정 하는 것이 좋습니다.
- 별도의 가상 컴퓨터에 [Oracle Enterprise Manager](https://www.oracle.com/technetwork/oem/enterprise-manager/overview/index.html) 를 설치 합니다.
- 기본적으로 큰 페이지는 linux에서 사용 하도록 설정 되지 않습니다. 큰 페이지를 사용 하도록 설정 하 고 Oracle DB에 설정 하는 것이 좋습니다 `use_large_pages = ONLY` . 이렇게 하면 성능을 향상 시킬 수 있습니다. 자세한 내용은 [여기](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/refrn/USE_LARGE_PAGES.html#GUID-1B0F4D27-8222-439E-A01D-E50758C88390)를 참조하세요.

### <a name="disk-types-and-configurations"></a>디스크 형식 및 구성

- *기본 OS 디스크*: 이러한 디스크 유형은 영구 데이터 및 캐싱을 제공합니다. 시작 시 OS 액세스에 최적화되며, 트랜잭션 또는 데이터 웨어하우스(분석) 워크로드용으로는 설계되지 않았습니다.

- *관리 디스크*: Azure에서 VM 디스크에 사용하는 스토리지 계정을 관리합니다. 디스크 유형 (가장 자주 Oracle 워크 로드의 경우 premium SSD) 및 필요한 디스크 크기를 지정 합니다. Azure에서 사용자에게 맞는 디스크를 만들고 관리합니다.  프리미엄 SSD 관리 디스크는 메모리 최적화 및 특별히 설계 된 VM 시리즈에만 사용할 수 있습니다. 특정 VM 크기를 선택하면 해당 VM 크기를 기반으로 하여 사용할 수 있는 Premium Storage SKU만 메뉴에 표시됩니다.

![관리되는 디스크 페이지의 스크린샷](./media/oracle-design/premium_disk01.png)

VM에서 스토리지를 구성한 후 데이터베이스를 만들기 전에 디스크를 로드하여 테스트할 수 있습니다. 대기 시간 및 처리량 모두와 관련된 I/O 속도를 알고 있으면 VM에서 대기 시간 목표로 예상되는 처리량을 지원할 수 있는지 확인하는 데 도움이 됩니다.

Oracle Orion, Systool, SLOB 및 Fio와 같은 응용 프로그램 부하 테스트를 위한 다양 한 도구가 있습니다.

Oracle 데이터베이스를 배포한 후에 부하 테스트를 다시 실행합니다. 일반 및 최대 워크로드를 시작합니다. 그러면 결과에서 사용자 환경의 초기 계획을 보여 줍니다.  워크 로드 테스트에서 현실적인 일은 현실에서 VM에 대해 실행 하는 것과 같은 작업을 실행 하는 것이 적합 하지 않습니다.

Oracle은 많은 IO 집약적 데이터베이스 이므로 저장소 크기가 아닌 IOPS 속도로 저장소 크기를 조정 하는 것이 매우 중요 합니다. 예를 들어 필요한 IOPS가 5,000이지만 200GB만 필요한 경우 200GB 이상의 스토리지가 있어도 P30 클래스 프리미엄 디스크를 받을 수도 있습니다.

IOPS 속도는 AWR 보고서에서 얻을 수 있으며, 다시 실행 로그, 물리적 읽기 및 쓰기 속도로 결정됩니다.  항상 선택한 VM 시리즈에는 작업의 IO 요청을 처리할 수 있는 기능이 있는지 확인 합니다.  VM의 IO 제한이 저장소 보다 낮으면 VM에서 최대 제한 값을 설정 합니다.

![AWR 보고서 페이지의 스크린샷](./media/oracle-design/awr_report.png)

예를 들어 다시 실행 크기는 초당 12,200,000바이트이며, 11.63MBps와 같습니다.
IOPS는 12,200,000 / 2,358 = 5,174입니다.

I/O 요구 사항에 대해 명확히 알고 있으면 이러한 요구 사항에 가장 적합한 드라이브 조합을 선택할 수 있습니다.

**권장 사항**

- 데이터 테이블스페이스의 경우 관리되는 스토리지 또는 Oracle ASM을 사용하여 I/O 워크로드를 여러 디스크에 분산합니다.
- Oracle 고급 압축을 사용 하 여 i/o (데이터 및 인덱스 모두)를 줄입니다.
- 별도의 데이터 디스크에서 다시 실행 로그, 임시 및 실행 테이블을 분리 합니다.
- 기본 OS 디스크(/dev/sda)에 애플리케이션 파일을 배치하지 않습니다. 이러한 디스크는 빠른 VM 부팅 시간에 맞게 최적화되지 않으며, 애플리케이션에 좋은 성능을 제공하지 않을 수 있습니다.
- Premium storage에서 M 시리즈 Vm을 사용 하는 경우 redo logs 디스크에서 [쓰기 가속기](../../how-to-enable-write-accelerator.md) 를 사용 하도록 설정 합니다.
- 대기 시간이 긴 다시 실행 로그를 울트라 디스크로 이동 하는 것이 좋습니다.

### <a name="disk-cache-settings"></a>디스크 캐시 설정

호스트 캐싱에는 세 가지 옵션이 있지만 Oracle 데이터베이스의 경우 데이터베이스 작업에는 읽기 전용 캐싱이 권장 됩니다.  ReadWrite는 데이터 파일에 대 한 중요 한 취약성을 발생 시킬 수 있습니다. 즉, 데이터베이스 작성 목표는 정보를 캐시 하지 않고 데이터 파일에 기록 하는 것입니다.

데이터베이스의 경우 파일 시스템 또는 응용 프로그램과 달리 호스트 캐싱에 대 한 권장 사항은 *읽기 전용* 입니다. 모든 요청은 이후 읽기에 대해 캐시 됩니다. 모든 쓰기는 디스크에 계속 기록 됩니다.

**권장 사항**

처리량을 최대화 하려면 가능 하면 항상 호스트 캐싱에 대해 **ReadOnly** 로 시작 하는 것이 좋습니다. Premium Storage **읽기 전용** 옵션을 사용 하 여 파일 시스템을 탑재 하는 경우 "장벽"을 사용 하지 않도록 설정 해야 합니다. /etc/fstab 파일을 UUID로 디스크에 업데이트합니다.

![ReadOnly 및 None 옵션을 보여 주는 관리 디스크 페이지의 스크린샷](./media/oracle-design/premium_disk02.png)

- OS 디스크의 경우 기본 **읽기/쓰기** 캐싱을 사용 하 고 Oracle 워크 로드 vm에 프리미엄 SSD를 사용 합니다.  또한 교환에 사용 되는 볼륨이 프리미엄 SSD에 있는지도 확인 합니다.
- 모든 데이터 파일의 경우 캐싱에 **ReadOnly** 를 사용 합니다. 읽기 전용 캐싱은 프리미엄 관리 디스크 P30 이상 에서만 사용할 수 있습니다.  ReadOnly 캐싱에 사용할 수 있는 4095GiB 볼륨의 제한이 있습니다.  더 큰 할당은 기본적으로 호스트 캐싱을 사용 하지 않습니다.

작업 및 저녁 마다 작업이 크게 다르고 IO 워크 로드에서 지원할 수 있는 경우 P20 프리미엄 SSD 버스트를 사용 하면 야간 시간 일괄 처리 로드 또는 제한 된 IO 수요 중에 필요한 성능을 제공할 수 있습니다.  

## <a name="security"></a>보안

Azure 환경을 설정하고 구성한 후의 다음 단계는 네트워크를 보호하는 것입니다. 몇 가지 권장 사항입니다.

- *NSG 정책*: NSG는 서브넷 또는 NIC에서 정의될 수 있습니다. 응용 프로그램 방화벽과 같은 항목에 대 한 보안 및 강제 라우팅을 위해 서브넷 수준에서 액세스를 제어 하는 것이 더 간단 합니다.

- *Jumpbox*: 더 안전한 액세스를 위해 관리자는 애플리케이션 서비스 또는 데이터베이스에 직접 연결하면 안됩니다. Jumpbox는 관리자 컴퓨터와 Azure 리소스 간 미디어로 사용됩니다.
![Jumpbox 토폴로지 페이지의 스크린샷](./media/oracle-design/jumpbox.png)

    관리자 컴퓨터는 Jumpbox에 대해 IP가 제한된 액세스만 제공해야 합니다. Jumpbox에는 애플리케이션과 데이터베이스에 대한 액세스 권한이 있어야 합니다.

- *사설망*(서브넷): NSG 정책에 따라 더 나은 제어를 설정할 수 있도록 애플리케이션 서비스와 데이터베이스를 별도의 서브넷에 두는 것이 좋습니다.


## <a name="additional-reading"></a>추가 참조 항목

- [Oracle ASM 구성](configure-oracle-asm.md)
- [Oracle Data Guard 구성](configure-oracle-dataguard.md)
- [Oracle Golden Gate 구성](configure-oracle-golden-gate.md)
- [Oracle 백업 및 복구](./oracle-overview.md)

## <a name="next-steps"></a>다음 단계

- [자습서: 고가용성 VM 만들기](../../linux/create-cli-complete.md)
- [VM 배포 Azure CLI 샘플 탐색](https://github.com/Azure-Samples/azure-cli-samples/tree/master/virtual-machine)
