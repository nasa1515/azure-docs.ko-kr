---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/11/2020
ms.author: mikben
ms.openlocfilehash: 90bc9def9c1a78799f4931b58ad0a661602985e3
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98013195"
---
## <a name="setting-up"></a>설치

### <a name="create-a-new-nodejs-application"></a>새 Node.js 애플리케이션 만들기

터미널 또는 명령 창을 열어 앱에 대한 새 디렉터리를 만들고 해당 디렉터리로 이동합니다.

```console
mkdir calling-quickstart && cd calling-quickstart
```

`npm init -y`를 실행하여 기본 설정으로 **package.json** 파일을 만듭니다.

```console
npm init -y
```

### <a name="install-the-package"></a>패키지 설치

`npm install` 명령을 사용하여 JavaScript용 Azure Communication Services 통화 클라이언트 라이브러리를 설치합니다.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

이 빠른 시작에는 다음 버전의 webpack이 권장됩니다.

```console
"webpack": "^4.42.0",
"webpack-cli": "^3.3.11",
"webpack-dev-server": "^3.10.3"
```

`--save` 옵션은 라이브러리를 **package.json** 파일의 종속성으로 나열합니다.

### <a name="set-up-the-app-framework"></a>앱 프레임워크 설정

이 빠른 시작에서는 webpack을 사용하여 애플리케이션 자산을 번들로 묶습니다. 다음 명령을 실행하여 webpack, webpack-cli 및 webpack-dev-server npm 패키지를 설치하고 **package.json** 에서 개발 종속성으로 나열합니다.

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

프로젝트의 루트 디렉터리에 **index.html** 파일을 만듭니다. 이 파일을 사용하여 사용자가 호출을 수행할 수 있도록 하는 기본 레이아웃을 구성합니다.
