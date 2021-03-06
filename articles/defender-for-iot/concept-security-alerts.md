---
title: 기본 제공 & 사용자 지정 경고 목록
description: IoT Hub의 기능 및 서비스에 대해 Defender를 사용 하 여 보안 경고 및 권장 수정에 대해 알아봅니다.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/16/2021
ms.author: shhazam
ms.openlocfilehash: ef33851600c576494e4e0903c6ab8ffefc9a1a59
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636495"
---
# <a name="defender-for-iot-hub-security-alerts"></a>Defender IoT Hub 보안 경고

IoT 용 Defender는 고급 분석 및 위협 인텔리전스를 사용 하 여 IoT 솔루션을 지속적으로 분석 하 여 악의적인 활동을 경고 합니다.
또한 예상 된 장치 동작에 대 한 정보를 바탕으로 사용자 지정 경고를 만들 수 있습니다.
경고는 잠재적 손상을 나타내는 표시기 역할을 하며 조사 하 고 수정 해야 합니다.

이 문서에는 IoT Hub에서 트리거될 수 있는 기본 제공 경고 목록이 나와 있습니다.
기본 제공 경고 외에도 IoT 용 Defender를 사용 하면 예상 된 IoT Hub 및/또는 장치 동작에 따라 사용자 지정 경고를 정의할 수 있습니다.
자세한 내용은 [사용자 지정 가능한 경고](concept-customizable-security-alerts.md)를 참조 하세요.

## <a name="built-in-alerts-for-iot-hub"></a>IoT Hub에 대 한 기본 제공 경고

| 심각도 | 속성 | 설명 | 제안된 수정 사항 |
|--|--|--|--|
| **중간** 심각도 |  |  |  |
| IoT Hub에 추가 된 새 인증서 | 중간 | 이름이 \' % {DescCertificateName} 인 인증서가 \' IoT Hub \' % {DescIoTHubName}에 추가 되었습니다 \' . 권한이 없는 당사자가이 작업을 수행한 경우 악성 작업을 나타낼 수 있습니다. | 1. 인증 된 파티에서 인증서를 추가 했는지 확인 합니다. <br> 2. 권한이 부여 된 당사자가 추가 하지 않은 경우 인증서를 제거 하 고 조직 보안 팀에 경고를 에스컬레이션 합니다. |
| IoT Hub에서 인증서가 삭제 됨 | 중간 | 이름이 \' % {DescCertificateName} 인 인증서가 \' IoT Hub \' % {DescIoTHubName}에서 삭제 되었습니다 \' . 권한이 없는 당사자가이 작업을 수행한 경우 악의적인 활동을 나타낼 수 있습니다. | 1. 인증 된 파티에서 인증서를 제거 했는지 확인 합니다. <br> 2. 인증 된 파티에서 인증서를 제거 하지 않은 경우 인증서를 다시 추가 하 고 조직 보안 팀에 경고를 에스컬레이션 합니다. |
| IoT Hub에 인증서를 추가 하는 데 실패 했습니다. | 중간 | % { \' DescCertificateName} 인증서 \' 를 IoT Hub \' % {DescIoTHubName}에 추가 하지 못했습니다 \' . 권한이 없는 당사자가이 작업을 수행한 경우 악성 작업을 나타낼 수 있습니다. | 인증서 변경 권한이 인증 된 파티에만 부여 되었는지 확인 합니다. |
| IoT Hub에서 인증서를 삭제 하는 데 실패 했습니다. | 중간 | \' \' IoT Hub \' % {DescIoTHubName} 에서% {DescCertificateName} 인증서를 삭제 하지 못했습니다 \' . 권한이 없는 당사자가이 작업을 수행한 경우 악성 작업을 나타낼 수 있습니다. | 인증서 변경 권한이 인증 된 파티에만 부여 되었는지 확인 합니다. |
| x.509 장치 인증서 지문 불일치 | 중간 | x.509 장치 인증서 지문이 구성과 일치 하지 않습니다. | 장치에서 경고를 검토 합니다. 추가 작업이 필요하지 않습니다. |
| x.509 인증서가 만료 됨 | 중간 | X.509 장치 인증서가 만료 되었습니다. | 만료 된 인증서가 있는 합법적인 장치 이거나 합법적인 장치를 가장할 수 있습니다. 합법적인 장치가 현재 올바르게 통신 하 고 있는 경우 가장을 시도할 수 있습니다. |
| **낮은** 심각도 |  |  |  |
| 검색 된 IoT Hub의 진단 설정을 추가 하거나 편집 하려고 합니다. | 낮음 | IoT Hub의 진단 설정을 추가 하거나 편집 하려고 했습니다. 진단 설정을 사용 하면 보안 인시던트가 발생 하거나 네트워크가 손상 된 경우 조사를 위해 활동 기록을 다시 만들 수 있습니다. 권한이 부여 된 당사자가이 작업을 수행 하지 않은 경우 악성 작업을 나타낼 수 있습니다. | 1. 인증 된 파티에서 인증서를 제거 했는지 확인 합니다.<br> 2. 인증 된 파티에서 인증서를 제거 하지 않은 경우 인증서를 다시 추가 하 고 정보 보안 팀에 경고를 에스컬레이션 합니다. |
| 검색 된 IoT Hub에서 진단 설정을 삭제 하려고 합니다. | 낮음 | % {DescAttemptStatusMessage}이 (가) \' \' \' IoT Hub \' % {DescIoTHubName}의 진단 설정% {DescDiagnosticSettingName}을 (를) 추가 하거나 편집 하려고 했습니다 \' . 진단 설정을 사용 하면 보안 인시던트가 발생 하거나 네트워크가 손상 된 경우 조사를 위해 활동 기록을 다시 만들 수 있습니다. 권한이 부여 된 당사자가이 작업을 수행 하지 않은 경우 악의적인 활동을 나타낼 수 있습니다. | 진단 설정 변경 권한이 인증 된 파티에만 부여 되었는지 확인 합니다. |
| 만료 된 SAS 토큰 | 낮음 | 장치에서 사용 하는 만료 된 SAS 토큰 | 만료 된 토큰이 있는 합법적인 장치 이거나 합법적인 장치를 가장할 수 있습니다. 합법적인 장치가 현재 올바르게 통신 하 고 있는 경우 가장을 시도할 수 있습니다. |
| 잘못 된 SAS 토큰 서명 | 낮음 | 장치에서 사용 하는 SAS 토큰에 잘못 된 서명이 있습니다. 서명이 기본 키 또는 보조 키와 일치 하지 않습니다. | 장치에서 경고를 검토 합니다. 추가 작업이 필요하지 않습니다. |

## <a name="next-steps"></a>다음 단계

- IoT 서비스용 Defender [개요](overview.md)
- [보안 데이터에 액세스](how-to-security-data-access.md) 하는 방법 알아보기
- [장치 조사](how-to-investigate-device.md) 에 대 한 자세한 정보
