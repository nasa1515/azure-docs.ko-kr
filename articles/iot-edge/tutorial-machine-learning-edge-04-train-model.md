---
title: '자습서: 모델 학습 및 배포 - Azure IoT Edge의 Machine Learning'
description: 이 자습서에서는 Azure Machine Learning을 사용하여 기계 학습 모델을 학습한 다음, Azure IoT Edge 모듈로 배포할 수 있는 컨테이너 이미지로 모델을 패키지화합니다.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/24/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2cc96db88d9a2aec02de5e2fc4ed18b445972e7b
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121147"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>자습서: Azure Machine Learning 모델 학습 및 배포

이 문서에서는 다음 작업을 수행합니다.

* Azure Machine Learning Studio를 사용하여 기계 학습 모델을 교육합니다.
* 학습된 모델을 컨테이너 이미지로 패키징합니다.
* 컨테이너 이미지를 Azure IoT Edge 모듈로 배포합니다.

Azure Machine Learning Studio는 기계 학습 모델을 실험, 학습 및 배포하는 데 사용되는 기본 블록입니다.

이 문서의 단계는 일반적으로 데이터 과학자가 수행할 수 있습니다.

자습서의 이 섹션에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Azure Machine Learning 작업 영역에 Jupyter Notebook을 만들어 기계 학습 모델을 학습합니다.
> * 학습된 기계 학습 모델을 컨테이너화합니다.
> * 컨테이너화된 기계 학습 모델에서 Azure IoT Edge 모듈을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서는 IoT Edge에서 Azure Machine Learning을 사용하는 방법에 대한 자습서 시리즈의 일부입니다. 시리즈의 각 문서는 이전 문서의 작업을 기반으로 합니다. 이 문서에 직접 도착한 경우 시리즈의 [첫 번째 문서](tutorial-machine-learning-edge-01-intro.md)를 방문하세요.

## <a name="set-up-azure-machine-learning"></a>Azure Machine Learning 설정 

Azure Machine Learning Studio를 사용하여 2개의 Jupyter Notebook 및 지원 파일을 호스팅합니다. 여기에서는 Azure Machine Learning 프로젝트를 만들고 구성합니다. Jupyter 및/또는 Azure Machine Learning Studio를 사용한 적이 없는 경우, 다음과 같은 소개 문서를 참조하세요.

* **Jupyter Notebook:** [Visual Studio Code에서 Jupyter Notebook 작업](https://code.visualstudio.com/docs/python/jupyter-support)
* **Azure Machine Learning:** [Jupyter Notebook에서 Azure Machine Learning 시작](../machine-learning/tutorial-1st-experiment-sdk-setup.md)


> [!NOTE]
> 일단 설치하면, Azure Machine Learning Service를 원하는 머신에서 액세스할 수 있습니다. 설치하는 동안, 필요한 파일이 모두 있는 개발 VM을 사용해야 합니다.

### <a name="install-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code 확장 설치
개발 VM의 VS Code에는 이 확장이 설치되어 있어야 합니다. 다른 인스턴스에서 실행 중인 경우 [여기](../machine-learning/tutorial-setup-vscode-extension.md)에 설명된 대로 확장을 다시 설치하세요.

### <a name="create-an-azure-machine-learning-account"></a>Azure Machine Learning 계정 만들기  
Azure에서 리소스를 프로비저닝하고 워크로드를 실행하려면 Azure 계정 자격 증명을 사용하여 로그인해야 합니다.

1. Visual Studio Code의 메뉴 모음에서 **보기** > **명령 팔레트** 를 차례로 선택하여 명령 팔레트를 엽니다. 

1. `Azure: Sign In` 명령을 명령 팔레트에 입력하여 로그인 프로세스를 시작합니다. 지침에 따라 로그인을 완료합니다. 

1. Azure ML Compute 인스턴스를 만들어 워크로드를 실행합니다. 명령 팔레트를 사용하여 `Azure ML: Create Compute` 명령을 입력합니다. 
1. Azure 구독 선택
1. **+ 새 Azure ML 작업 영역 만들기** 를 선택하고 이름 `turbofandemo`를 입력합니다.
1. 이 데모에 사용했던 리소스 그룹을 선택합니다.
1. VS Code 창의 오른쪽 아래에서 작업 영역을 만드는 과정을 볼 수 있습니다. **작업 영역 만들기: turobofandemo**(1~2분 정도 소요될 수 있음). 
1. 작업 영역이 성공적으로 만들어질 때까지 기다리세요. **Azure ML 작업 영역 turbofandemo 만들어짐** 이라고 표시되어야 합니다.


### <a name="upload-jupyter-notebook-files"></a>Jupyter Notebook 파일 업로드

샘플 Notebook 파일을 새 Azure ML 작업 영역에 업로드합니다.

1. ml.azure.com으로 이동하여 로그인합니다.
1. Microsoft 디렉터리, Azure 구독 및 새로 만든 Azure ML 작업 영역을 선택합니다.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-studio-workspace.png" alt-text="Azure ML 작업 영역을 선택합니다." :::

1. Azure ML 작업 공간에 로그인한 후 왼쪽 메뉴를 사용하여 **Notebooks** 섹션으로 이동합니다.
1. **내 파일** 탭을 선택합니다.

1. **업로드**(위쪽 화살표 아이콘) 선택 


1. **C:\source\IoTEdgeAndMlSample\AzureNotebooks** 로 이동합니다. 목록에서 모든 파일을 선택하고 **열기** 를 클릭합니다.

1. **이 파일의 콘텐츠를 신뢰합니다** 확인란을 선택합니다.

1. **업로드** 를 선택하여 업로드를 시작하고 프로세스가 완료되면 **완료** 를 선택합니다.

### <a name="jupyter-notebook-files"></a>Jupyter Notebook 파일

Azure ML 작업 영역에 업로드한 파일을 검토해 보겠습니다. 자습서의 이 부분에 나오는 활동은 몇 가지 지원 파일을 사용하는 2개의 Notebook 파일에 적용됩니다.

* **01-turbofan\_regression.ipynb:** 이 Notebook에서는 Machine Learning Service 작업 영역을 사용하여 기계 학습 실험을 생성 및 실행합니다. 이 Notebook은 대략적으로 다음 단계를 수행합니다.

  1. 디바이스 도구에서 생성한 Azure Storage 계정에서 데이터를 다운로드합니다.
  1. 데이터를 검색하고 준비한 다음, 데이터를 사용하여 분류자 모델을 학습시킵니다.
  1. 테스트 데이터 세트(Test\_FD003.txt)를 사용하여 실험에서 모델을 평가합니다.
  1. Machine Learning Service 작업 영역에 최상의 분류자 모델을 게시합니다.

* **02-turbofan\_deploy\_model.ipynb:** 이 Notebook은 이전 Notebook에서 만든 모델을 사용하여 Azure IoT Edge 디바이스에 배포할 준비가 된 컨테이너 이미지를 만듭니다. 이 Notebook은 다음 단계를 수행합니다.

  1. 모델에 대한 채점 스크립트를 만듭니다.
  1. Machine Learning Service 작업 영역에 저장된 분류자 모델을 사용하여 컨테이너 이미지를 생성합니다.
  1. 이미지를 Azure Container Instance에 웹 서비스로 배포합니다.
  1. 웹 서비스를 사용하여 모델과 이미지가 예상대로 작동하는지 확인합니다. 검사된 이미지는 이 자습서의 [사용자 지정 IoT Edge 모듈 만들기 및 배포](tutorial-machine-learning-edge-06-custom-modules.md) 부분에서 IoT Edge 디바이스에 배포됩니다.

* **Test\_FD003.txt:** 이 파일은 학습된 분류자의 유효성을 검사할 때 테스트 세트로 사용할 데이터를 포함합니다. 단순성을 위해, 원래 컨테스트에 제공된 테스트 데이터가 테스트 세트로 사용됩니다.

* **RUL\_FD003.txt:** 이 파일은 Test\_FD003.txt 파일에 있는 각 디바이스의 마지막 주기에 대한 RUL(잔여 수명)을 포함합니다. 데이터에 대한 자세한 설명은 C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan에서 readme.txt 및 Damage Propagation Modeling.pdf 파일을 참조하세요.

* **Utils.py:** 데이터 작업을 위한 일련의 Python 유틸리티 함수를 포함합니다. 첫 번째 Notebook에는 함수에 대한 자세한 설명이 있습니다.

* **README.md:** Notebook 사용을 설명하는 추가 정보입니다.  

## <a name="run-jupyter-notebooks"></a>Jupyter Notebook 실행

이제 작업 영역을 만들었으므로 Notebook을 실행할 수 있습니다. 

1. **내 파일** 페이지에서 **01-turbofan\_regression.ipynb** 를 선택합니다.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-turbofan-notebook.png" alt-text="실행할 첫 번째 Notebook을 선택합니다.":::

1. Notebook이 **신뢰할 수 없음** 으로 나열되면 Notebook의 오른쪽 위에서 **신뢰할 수 없음** 위젯을 클릭합니다. 대화 상자가 열리면, **신뢰** 를 선택합니다.

1. 최상의 결과를 위해 각 셀에 대한 설명서를 읽고 개별적으로 실행합니다. 도구 모음에서 **실행** 을 선택합니다. 나중에 여러 셀을 실행하는 것이 편리한 것을 알 수 있습니다. 업그레이드 및 사용 중단 경고는 무시해도 됩니다.

    셀이 실행 중이면 대괄호 사이에 별표가 표시됩니다([\*]). 셀 작업이 완료되면 별표가 숫자로 바뀌고 관련된 출력이 나타납니다. Notebook의 셀은 순차적으로 작성되며 한 번에 하나만 실행할 수 있습니다.

    또한 **셀** 메뉴의 실행 옵션을 사용할 수도 있습니다 `Ctrl` + `Enter`를 사용하여 셀을 실행할 수 있고, `Shift` + `Enter`를 사용하여 셀을 실행하고 다음 셀로 이동할 수 있습니다.

    > [!TIP]
    > 일관된 셀 작업을 위해 브라우저의 여러 탭에서 동일한 노트북을 실행하지 마세요.

1. **글로벌 속성 설정** 지침을 따르는 셀에서 Azure 구독, 설정 및 리소스에 값을 채웁니다. 그런 다음, 셀을 실행합니다.

    ![Notebook에서 글로벌 속성 설정](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. **Workspace details**(작업 영역 세부 정보) 이전의 셀에서, 실행 한 후 인증을 위해 로그인하도록 지시하는 링크를 찾습니다.

    ![디바이스 인증을 위한 로그인 프롬프트](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    링크를 열고 지정된 코드를 입력합니다. 이 로그인 절차는 Microsoft Azure 플랫폼 간 명령줄 인터페이스를 사용하여 Azure 리소스에 액세스하는 Jupyter Notebook을 인증합니다.  

    ![디바이스 확인 시 애플리케이션 인증](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. **Explore the results**(결과 탐색) 이전 셀에서 실행 ID의 값을 복사하고 **Reconstitute a run**(실행 재구성) 다음의 셀에 실행 ID에 대한 값을 붙여넣습니다.

   ![셀 사이 실행 ID 복사](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. 노트북의 나머지 셀을 실행합니다.

1. 노트북을 저장하고 프로젝트 페이지로 돌아갑니다.

1. **02-turbofan\_deploy\_model.ipynb** 를 열고 각 셀을 실행합니다. **작업 영역 구성** 다음에 나오는 셀에서 인증하려면 로그인해야 합니다.

1. 노트북을 저장하고 프로젝트 페이지로 돌아갑니다.

### <a name="verify-success"></a>성공 확인

Notebook이 성공적으로 완료되었는지 확인하려면 몇 가지 항목이 만들어졌는지 확인합니다.

1. Azure ML Notebooks **내 파일** 탭에서 **새로 고침** 을 선택합니다.

1. 다음 파일이 생성되었는지 확인합니다.

    | 파일 | Description |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Azure Machine Learning 작업 영역을 만드는 데 사용되는 구성 파일입니다. |
    | ./aml_config/model_config.json | Azure의 **turbofanDemo** Machine Learning 작업 영역에 모델을 배포하는 데 필요한 구성 파일입니다. |
    | myenv.yml| 배포된 Machine Learning 모델의 종속성에 대한 정보를 제공합니다.|

1. 다음 Azure 리소스가 생성되었는지 확인합니다. 일부 리소스 이름에는 임의의 문자가 추가됩니다.

    | Azure 리소스 | 속성 |
    | --- | --- |
    | Machine Learning 작업 영역 | turborfanDemo |
    | Container Registry | turbofandemoxxxxxxxx |
    | Application Insights | turbofaninsightxxxxxxxx |
    | Key Vault | turbofankeyvaultbxxxxxxxx |
    | 스토리지 | turbofanstoragexxxxxxxxx |

### <a name="debugging"></a>디버깅

디버깅을 위해 노트북에 Python 문(예: `print()` 명령)을 삽입하여 값을 표시할 수 있습니다. 정의되지 않은 변수 또는 개체가 표시되는 경우 처음으로 선언되거나 인스턴스화된 셀을 실행합니다.

노트북을 다시 실행해야 하는 경우 이전에 만든 파일과 Azure 리소스를 삭제해야 할 수도 있습니다.

## <a name="clean-up-resources"></a>리소스 정리

이 자습서는 각 문서가 이전 작업에서 수행된 작업을 기반으로 하는 집합의 일부입니다. 최종 자습서를 완료할 때까지 기다렸다가 리소스를 정리하세요.

## <a name="next-steps"></a>다음 단계

이 문서에서는 Azure ML Studio에서 실행되는 2개의 Jupyter Notebook을 통해 turbofan 디바이스의 데이터를 사용하여 RUL(잔여 수명) 분류자를 학습하고, 분류자를 모델로 저장하고, 컨테이너 이미지를 만들고, 이미지를 웹 서비스로 배포하여 테스트했습니다.

계속해서 다음 문서를 진행하면 IoT Edge 디바이스를 만들 수 있습니다.

> [!div class="nextstepaction"]
> [IoT Edge 디바이스 구성](tutorial-machine-learning-edge-05-configure-edge-device.md)