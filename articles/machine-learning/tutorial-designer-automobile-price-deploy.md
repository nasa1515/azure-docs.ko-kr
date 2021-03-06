---
title: '자습서: 디자이너를 사용하여 ML 모델 배포'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 디자이너에서 예측 분석 솔루션을 빌드합니다. 끌어서 놓기 모듈을 사용하여 기계 학습 모델을 학습하고, 점수를 매기고, 배포합니다.
author: likebupt
ms.author: keli19
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 01/15/2021
ms.custom: designer
ms.openlocfilehash: e93f912915303ce903a32ceba4f079593657a4ac
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99822468"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer"></a>자습서: 디자이너를 사용하여 Machine Learning 모델 배포


다른 사용자가 사용할 수 있도록 [이 자습서의 1부](tutorial-designer-automobile-price-train-score.md)에서 개발한 예측 모델을 배포할 수 있습니다. 1부에서는 모델을 학습했습니다. 이제는 사용자 입력에 기반한 새 예측을 생성해야 합니다. 이 자습서의 이번 회차에서는 다음과 같은 일을 합니다.

> [!div class="checklist"]
> * 실시간 유추 파이프라인을 만듭니다.
> * 유추 클러스터를 만듭니다.
> * 실시간 엔드포인트를 배포합니다.
> * 실시간 엔드포인트를 테스트합니다.

## <a name="prerequisites"></a>필수 구성 요소

[이 자습서의 1부](tutorial-designer-automobile-price-train-score.md)를 완료하여 디자이너에서 Machine Learning 모델을 학습하고 채점하는 방법을 알아보세요.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-real-time-inference-pipeline"></a>실시간 유추 파이프라인 만들기

파이프라인을 배포하려면 먼저 학습 파이프라인을 실시간 유추 파이프라인으로 변환해야 합니다. 이 프로세스는 학습 모듈을 제거하고 요청을 처리하기 위해 웹 서비스 입력 및 출력을 추가합니다.

### <a name="create-a-real-time-inference-pipeline"></a>실시간 유추 파이프라인 만들기

1. 파이프라인 캔버스의 위쪽에서 **유추 파이프라인 만들기** > **실시간 유추 파이프라인** 을 차례로 선택합니다.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png"alt-text="파이프라인 만들기 단추를 찾을 수 있는 위치를 보여주는 스크린샷":::

    이제 파이프라인이 다음과 비슷할 것입니다. 

   ![배포 준비 후 예상되는 파이프라인 구성을 보여주는 스크린샷](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    **유추 파이프라인 만들기** 를 선택하면 다음과 같은 여러 작업이 수행됩니다.
    
    * 학습된 모델은 모듈 팔레트에 **데이터 세트** 모듈로 저장됩니다. **내 데이터 세트** 아래에서 찾을 수 있습니다.
    * **모델 학습** 및 **데이터 분할** 과 같은 학습 모듈이 제거됩니다.
    * 저장된 학습된 모델이 파이프라인에 다시 추가됩니다.
    * **웹 서비스 입력** 및 **웹 서비스 출력** 모듈이 추가됩니다. 이러한 모듈은 사용자 데이터가 파이프라인에 입력되는 위치와 데이터가 반환되는 위치를 보여줍니다.

    > [!NOTE]
    > 기본적으로 **웹 서비스 입력** 은 예측 파이프라인을 만드는 데 사용되는 학습 데이터와 동일한 데이터 스키마를 예상합니다. 이 시나리오에서 가격은 스키마에 포함됩니다. 그러나 예측 중에는 가격이 요소로 사용되지 않습니다.
    >

1. **제출** 을 선택하고 1부에서 사용한 것과 동일한 컴퓨팅 대상 및 실험을 사용합니다.

    첫 번째 실행인 경우 파이프라인이 실행을 완료하는 데 최대 20분이 걸릴 수 있습니다. 기본 컴퓨팅 설정의 최소 노드 크기는 0입니다. 즉, 디자이너가 유휴 상태가 된 후에 리소스를 할당해야 합니다. 컴퓨팅 리소스가 이미 할당되었기 때문에 반복되는 파이프라인 실행은 시간이 덜 걸립니다. 또한 디자이너는 각 모듈에 대해 캐시된 결과를 사용하여 효율성을 더욱 향상시킵니다.

1. **배포** 를 선택합니다.

## <a name="create-an-inferencing-cluster"></a>유추 클러스터 만들기

표시되는 대화 상자에서 기존 AKS(Azure Kubernetes Service) 클러스터에서 선택하여 모델을 배포할 수 있습니다. AKS 클러스터가 없는 경우 다음 단계에 따라 클러스터를 만듭니다.

1. 표시되는 대화 상자에서 **컴퓨팅** 을 선택하여 **컴퓨팅** 페이지로 이동합니다.

1. 탐색 리본에서 **유추 클러스터** >  **+ 새로 만들기** 를 선택합니다.

    ![새 유추 클러스터 창으로 이동하는 방법을 보여주는 스크린샷](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)
   
1. 유추 클러스터 창에서 새 Kubernetes 서비스를 구성합니다.

1. **컴퓨팅 이름** 에 *aks-compute* 를 입력합니다.
    
1. **지역** 에 사용할 수 있는 주변 지역을 선택합니다.

1. **만들기** 를 선택합니다.

    > [!NOTE]
    > 새 AKS 클러스터를 만드는 데 약 15분이 걸립니다. **유추 클러스터** 페이지에서 프로비저닝 상태를 확인할 수 있습니다.
    >

## <a name="deploy-the-real-time-endpoint"></a>실시간 엔드포인트 배포

AKS 서비스 프로비저닝이 완료된 후에는 실시간 유추 파이프라인으로 돌아가서 배포를 완료합니다.

1. 캔버스 위에서 **배포** 를 선택합니다.

1. **새 실시간 엔드포인트 배포** 를 선택합니다. 

1. 만든 AKS 클러스터를 선택합니다.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png"alt-text="새 실시간 엔드포인트를 설정하는 방법을 보여주는 스크린샷":::

    실시간 엔드포인트에 대한 **고급** 설정을 변경할 수도 있습니다.
    
    |고급 설정|Description|
    |---|---|
    |Application Insights 진단 및 데이터 수집 사용| 배포된 엔드포인트에서 데이터를 수집하도록 Azure Application Ingishts를 사용할지 여부를 지정합니다. </br> 기본값: false |
    |점수 매기기 시간 제한| 웹 서비스 호출에 대한 점수 매기기에 적용할 시간 제한(밀리초)입니다.</br>기본적으로: 60000|
    |자동 크기 조정 사용|   자동 크기 조정을 웹 서비스에 사용하도록 설정할지 여부입니다.</br>기본값: true|
    |최소 복제본| 이 웹 서비스의 크기를 자동으로 조정할 때 사용할 최소 컨테이너 수입니다.</br>기본적으로: 1|
    |최대 복제본| 이 웹 서비스의 크기를 자동으로 조정할 때 사용할 최대 컨테이너 수입니다.</br> 기본적으로: 10|
    |대상 사용률|자동 크기 조정기에서 이 웹 서비스에 대해 유지하려고 시도해야 하는 목표 사용률(백분율)입니다.</br> 기본적으로: 70|
    |새로 고침 기간|자동 크기 조정기에서 이 웹 서비스의 크기를 조정하려고 시도하는 빈도(초)입니다.</br> 기본적으로: 1|
    |CPU 예약 용량|이 웹 서비스에 할당할 CPU 코어 수입니다.</br> 기본적으로: 0.1|
    |메모리 예약 용량|이 웹 서비스에 할당할 메모리 양(GB)입니다.</br> 기본적으로: 0.5|
        

1. **배포** 를 선택합니다. 

    배포가 완료된 후 캔버스 위의 성공 알림이 나타납니다. 몇 분 정도 걸릴 수 있습니다.

> [!TIP]
> 실시간 엔드포인트 설정 상자에서 **컴퓨팅 형식** 에 대해 **Azure Container Instance** 를 선택하는 경우 **ACI(Azure Container Instance)** 에 배포할 수도 있습니다.
> Azure Container Instance는 테스트 또는 개발에 사용됩니다. 48GB 미만의 RAM이 필요한 소규모 CPU 기반 워크로드의 경우 ACI를 사용합니다.

## <a name="test-the-real-time-endpoint"></a>실시간 엔드포인트 테스트

배포가 완료된 후 **엔드포인트** 페이지로 이동하여 실시간 엔드포인트를 볼 수 있습니다.

1. **엔드포인트** 페이지에서 배포한 엔드포인트를 선택합니다.

    **세부 정보** 탭에서 REST URI, Swagger 정의, 상태 및 태그와 같은 추가 정보를 볼 수 있습니다.

    **사용** 탭에서 샘플 사용 코드, 보안 키를 찾아 인증 방법을 설정할 수 있습니다.

    **배포 로그** 탭에서 실시간 엔드포인트의 자세한 배포 로그를 찾을 수 있습니다.

1. 엔드포인트를 테스트하려면 **테스트** 탭으로 이동합니다. 여기에서 테스트 데이터를 입력하고 **테스트** 를 선택하여 엔드포인트의 출력을 확인할 수 있습니다.

웹 서비스 사용에 대한 자세한 내용은 [웹 서비스로 배포된 모델 사용](how-to-consume-web-service.md)을 참조하세요.

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>다음 단계

이 자습서에서는 디자이너에서 기계 학습 모델을 만들고, 배포하고, 사용하는 방법에 대한 주요 단계를 알아보았습니다. 디자이너를 사용하는 방법에 대해 자세히 알아보려면 다음 링크를 참조하세요.

+ [디자이너 샘플](samples-designer.md): 디자이너를 사용하여 다른 유형의 문제를 해결하는 방법을 알아봅니다.
+ [Azure 가상 네트워크에서 Azure Machine Learning Studio를 사용](how-to-enable-studio-virtual-network.md)합니다.