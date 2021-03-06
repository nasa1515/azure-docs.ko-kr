---
title: IoT Hub에 대 한 사용자 지정 보안 경고
description: IoT Hub의 기능 및 서비스에 대해 Defender를 사용 하 여 사용자 지정 가능한 보안 경고 및 권장 해결 방법에 대해 알아봅니다.
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
ms.openlocfilehash: 04198432f2b600a3c703d5e4f253656f116000db
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636529"
---
# <a name="defender-for-iot-hub-custom-security-alerts"></a>Defender IoT Hub 사용자 지정 보안 경고

IoT 용 Defender는 고급 분석 및 위협 인텔리전스를 사용 하 여 IoT 솔루션을 지속적으로 분석 하 여 악의적인 활동을 경고 합니다.

사용자가 고유한 조직 배포 및 환경에서 잠재적인 손상에 대 한 가장 효율적인 지표로 경고를 제공 하도록 예상 되는 장치 동작에 대 한 지식을 바탕으로 사용자 지정 경고를 만드는 것이 좋습니다.

다음 IoT 경고 목록은 예상 IoT Hub 동작에 따라 사용자가 정의할 수 있습니다. 각 경고를 사용자 지정 하는 방법에 대 한 자세한 내용은 [사용자 지정 경고 만들기](quickstart-create-custom-alerts.md)를 참조 하세요.

## <a name="built-in-custom-alerts-in-the-iot-hub"></a>IoT Hub에서 기본 제공 되는 사용자 지정 경고

| 심각도 | 경고 이름 | 데이터 원본 | 설명 | 제안된 수정 사항 |
|--|--|--|--|--|
| 낮음 | 사용자 지정 경고-AMQP 프로토콜의 클라우드-장치 메시지 수가 허용 되는 범위를 벗어남 | IoT Hub | 특정 기간 내의 클라우드-장치 메시지 수 (AMQP 프로토콜)가 현재 구성 된 범위 및 허용 되는 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-AMQP 프로토콜에서 거부 된 클라우드-장치 메시지 수가 허용 되는 범위를 벗어남 | IoT Hub | 특정 기간 내에 장치에서 거부 한 클라우드-장치 메시지 (AMQP 프로토콜)의 수가 현재 구성 된 범위 및 허용 되는 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-AMQP 프로토콜의 장치-클라우드 메시지 수가 허용 되는 범위를 벗어남 | IoT Hub | 특정 시간 범위 내의 디바이스-클라우드 메시지 수(AMQP 프로토콜)가 현재 구성된 허용 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-직접 메서드 호출 수가 허용 된 범위를 벗어났습니다. | IoT Hub | 특정 시간 범위 내의 직접 메서드 호출 수가 현재 구성된 허용 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-파일 업로드 수가 허용 된 범위를 벗어남 | IoT Hub | 특정 시간 범위 내의 파일 업로드 수가 현재 구성된 허용 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-HTTP 프로토콜의 클라우드-장치 메시지 수가 허용 된 범위를 벗어남 | IoT Hub | 시간 범위에 속한 클라우드-디바이스 메시지(HTTP 프로토콜)의 양이 구성된 허용 범위를 벗어났습니다. |
| 낮음 | 사용자 지정 경고-HTTP 프로토콜에서 거부 된 클라우드-장치 메시지 수가 허용 되는 범위에 없습니다. | IoT Hub | 특정 시간 범위 내의 클라우드-디바이스 메시지 수(HTTP 프로토콜)가 현재 구성된 허용 범위를 벗어납니다. |
| 낮음 | 사용자 지정 경고-HTTP 프로토콜의 장치-클라우드 메시지 수가 허용 되는 범위를 벗어남 | IoT Hub | 특정 기간 내에 있는 장치-클라우드 메시지 (HTTP 프로토콜)의 양은 현재 구성 된 범위 및 허용 되는 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-MQTT 프로토콜의 클라우드-장치 메시지 수가 허용 된 범위를 벗어남 | IoT Hub | 특정 시간 범위 내의 클라우드-디바이스 메시지 수(MQTT 프로토콜)가 현재 구성된 허용 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-MQTT 프로토콜에서 거부 된 클라우드-장치 메시지 수가 허용 되는 범위를 벗어남 | IoT Hub | 디바이스에서 거부한 특정 시간 범위 내의 클라우드-디바이스 메시지 수(MQTT 프로토콜)가 현재 구성된 허용 범위를 벗어납니다. |
| 낮음 | 사용자 지정 경고-MQTT 프로토콜의 장치-클라우드 메시지 수가 허용 된 범위를 벗어남 | IoT Hub | 특정 시간 범위 내의 디바이스-클라우드 메시지 수(MQTT 프로토콜)가 현재 구성된 허용 범위를 벗어납니다. |
| 낮음 | 사용자 지정 경고-허용 된 범위를 벗어난 명령 큐 제거의 수 | IoT Hub | 특정 시간 범위 내의 명령 큐 제거 수가 현재 구성된 허용 범위를 벗어납니다. |  |
| 낮음 | 사용자 지정 경고-모듈 쌍 업데이트 수가 허용 된 범위를 벗어남 | IoT Hub | 특정 시간 범위 내의 모듈 쌍 업데이트 수가 현재 구성된 허용 범위를 벗어납니다. |
| 낮음 | 사용자 지정 경고-허용 된 범위를 벗어난 권한 없는 작업 수입니다. | IoT Hub | 특정 시간 범위 내의 권한 없는 작업 수가 현재 구성된 허용 범위를 벗어납니다. |

## <a name="next-steps"></a>다음 단계

- [경고를 사용자 지정](quickstart-create-custom-alerts.md) 하는 방법 알아보기
- IoT 서비스용 Defender [개요](overview.md)
- [보안 데이터에 액세스](how-to-security-data-access.md) 하는 방법 알아보기
- [장치 조사](how-to-investigate-device.md) 에 대 한 자세한 정보
