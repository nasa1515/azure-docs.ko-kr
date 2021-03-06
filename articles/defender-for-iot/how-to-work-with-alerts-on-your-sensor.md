---
title: 센서에 대 한 경고 사용
description: 경고를 사용 하 여 네트워크의 보안 및 작업을 향상 시킬 수 있습니다.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/30/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 3ae17ebce564de465a91739a210ae7f18f86a3b9
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523552"
---
# <a name="work-with-alerts-on-your-sensor"></a>센서에 대 한 경고 사용

경고를 사용 하 여 네트워크의 보안 및 작업을 향상 시킬 수 있습니다. 경고는 다음에 대 한 정보를 제공 합니다.

- 승인 된 네트워크 작업의 편차

- 프로토콜 및 작동 변칙

- 의심 되는 맬웨어 트래픽

:::image type="content" source="media/how-to-work-with-alerts-sensor/address-scan-detected-screen.png" alt-text="주소 검색을 검색 합니다.":::

사용자는 경고 관리 옵션을 사용 합니다.

- 승인 된 트래픽으로 검색 된 작업을 학습 하려면 센서를 지시 합니다.

- 경고 검토를 승인 합니다.

- 센서에 동일한 장치 및 동급 트래픽을 검색 하는 이벤트를 음소거 하도록 지시 합니다.

경고 조사를 개선 하 고 신속 하 게 진행 하는 데 도움이 되는 추가 도구를 사용할 수 있습니다. 예를 들면 다음과 같습니다.

  - 경고 검토자에 대 한 지침 설명을 추가 합니다.

  - SOC 솔루션에 표시할 경고 그룹을 만듭니다. 

  - 특정 경고를 검색 합니다. 관련 PCAP 파일 검토 장치 맵에서 검색 된 장치 및 기타 연결 된 장치를 보거나 파트너 시스템에 경고 세부 정보를 보냅니다.

  - 파트너 공급 업체에 게 알림 전달: SIEM systems, MSSP 시스템 등

## <a name="alerts-and-engines"></a>경고 및 엔진

센서 엔진이 네트워크 트래픽 및 주의가 필요한 동작의 변경 내용을 검색할 때 경고가 트리거됩니다. 이 문서에서는 각 엔진이 트리거하는 경고의 종류를 설명 합니다.

| 경고 유형 | Description |
|-|-|
| 정책 위반 경고 | 정책 위반 엔진이 이전에 학습 한 트래픽의 편차를 감지한 경우에 트리거됩니다. 예를 들면 다음과 같습니다. <br /> -새 장치가 검색 되었습니다.  <br /> -장치에서 새 구성이 검색 됩니다. <br /> -프로그래밍 장치로 정의 되지 않은 장치는 프로그래밍 변경을 수행 합니다. <br /> -펌웨어 버전이 변경 되었습니다. |
| 프로토콜 위반 경고 | 프로토콜 위반 엔진이 프로토콜 사양을 따르지 않는 패킷 구조 또는 필드 값을 검색 하는 경우에 트리거됩니다. | 
| 운영 경고 | 운영 엔진이 네트워크 작동 인시던트 또는 장치가 제대로 작동 하지 않는 것을 감지 하면 트리거됩니다. 예를 들어 네트워크 장치가 Stop PLC 명령을 통해 중지 되었거나 센서의 모니터링 트래픽 모니터링이 중지 되었습니다. |
| 맬웨어 경고 | 맬웨어 엔진이 악의적인 네트워크 작업을 감지 하면 트리거됩니다. 예를 들어 엔진은 Conficker와 같은 알려진 공격을 검색 합니다. |
| 변칙 경고 | 변칙 엔진이 편차를 감지한 경우 트리거됩니다. 예를 들어 장치에서 네트워크 검색을 수행 하지만 검색 장치로 정의 되어 있지 않습니다. |

도구를 사용 하 여 센서 엔진을 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 사용 하지 않도록 설정 된 엔진에서는 경고가 트리거되지 않습니다. [모니터링 되는 트래픽 제어를](how-to-control-what-traffic-is-monitored.md)참조 하세요.

## <a name="alerts-and-sensor-reporting"></a>경고 및 센서 보고

경고에 반영 된 활동을 계산 하 여 데이터 마이닝, 위험 평가 및 공격 벡터 보고서를 생성할 수 있습니다. 이러한 이벤트를 관리 하면 센서가 보고서를 업데이트 합니다.

예를 들면 다음과 같습니다.

  - 정의 된 서브넷의 장치와 서브넷 외부에 있는 장치 간의 무단 연결이 (공용) 데이터 마이닝 *인터넷 활동* 보고서 및 위험 평가 *인터넷 연결* 섹션에 표시 됩니다. 이러한 장치에 대 한 권한이 부여 되 면 (학습 된) 이러한 보고서를 생성 하 여 계산 됩니다.

  - 네트워크 장치에서 검색 된 맬웨어 이벤트는 위험 평가 보고서에 보고 됩니다. 맬웨어 이벤트에 대 한 경고가 *음소거* 되 면 위험 평가 보고서에서 영향을 받는 장치가 계산 되지 않습니다.

## <a name="next-steps"></a>다음 단계

[학습 및 스마트 IT 학습 모드](how-to-control-what-traffic-is-monitored.md#learning-and-smart-it-learning-modes) 
 경고에 제공 [된 정보 보기](how-to-view-information-provided-in-alerts.md) 
 [경고 이벤트 관리](how-to-manage-the-alert-event.md) 
 [경고 워크플로 가속화](how-to-accelerate-alert-incident-response.md)
