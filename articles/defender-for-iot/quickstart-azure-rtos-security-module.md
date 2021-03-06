---
title: '빠른 시작: Azure RTOS용 보안 모듈 구성 및 사용'
description: Azure IoT Hub에서 Azure RTOS용 보안 모듈 서비스를 온보드하고 활성화하는 방법에 대해 알아봅니다.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: a11a8ec2d8eb82950cf0ab55eb6ca1913b41c84a
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522940"
---
# <a name="quickstart-security-module-for-azure-rtos-preview"></a>빠른 시작: Azure RTOS용 보안 모듈(미리 보기)

이 문서에서는 시작하기 전에 필수 구성 요소에 대해 설명하고 IoT Hub에서 Azure RTOS용 보안 모듈 서비스를 활성화하는 방법을 설명합니다. 현재 IoT Hub가 없는 경우 시작하려면 [Azure Portal을 사용하여 IoT Hub 만들기](../iot-hub/iot-hub-create-through-portal.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소 

### <a name="supported-devices"></a>지원되는 디바이스

- ST STM32F746G 검색 키트
- NXP i.MX RT1060 EVK
- Microchip SAM E54 Xplained Pro EVK

[Azure RTOS GitHub용 보안 모듈 리소스](https://github.com/azure-rtos/azure-iot-preview/releases)에서 선택한 특정 보드 및 도구(IAR, 세미 IDE 또는 PC)에 대한 .zip 파일 중 하나를 다운로드, 컴파일 및 실행합니다.

### <a name="azure-resources"></a>Azure 리소스

시작하기 위한 다음 단계는 Azure 리소스를 준비하는 것입니다. IoT Hub가 필요하며 Log Analytics 작업 영역을 제안합니다. IoT Hub의 경우 디바이스에 연결하기 위한 IoT Hub 연결 문자열이 필요합니다. 
  
### <a name="iot-hub-connection"></a>IoT Hub 연결

시작하려면 IoT Hub 연결이 필요합니다. 

1. Azure Portal에서 **IoT Hub** 를 엽니다.

1. **IoT 디바이스** 로 이동합니다.

1. **만들기** 를 선택합니다.

1. IoT 연결 문자열을 [구성 파일](how-to-azure-rtos-security-module.md)에 복사합니다.

연결 자격 증명은 사용자 애플리케이션 구성 **HOST_NAME**, **DEVICE_ID** 및 **DEVICE_SYMMETRIC_KEY** 에서 가져옵니다.

Azure RTOS용 보안 모듈은 **MQTT** 프로토콜을 기반으로 하는 Azure IoT 미들웨어 연결을 사용합니다.

## <a name="next-steps"></a>다음 단계

다음 문서로 이동하여 솔루션 구성 및 사용자 지정을 완료합니다.

> [!div class="nextstepaction"]
> [Azure RTOS용 보안 모듈 구성](how-to-azure-rtos-security-module.md)
