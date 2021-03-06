---
title: 시계열 예측 모델 자동 학습
titleSuffix: Azure Machine Learning
description: Azure Machine Learning을 사용하여 자동화된 기계 학습으로 시계열 예측 회귀 모델을 학습하는 방법을 알아봅니다.
services: machine-learning
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, contperf-fy21q1, automl
ms.date: 08/20/2020
ms.openlocfilehash: 6e686c7b22eb834a096cdd7a67beb6d8d291ef20
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100392326"
---
# <a name="auto-train-a-time-series-forecast-model"></a>시계열 예측 모델 자동 학습


이 문서에서는 [PYTHON SDK Azure Machine Learning](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py)에서 자동화 된 machine Learning, automl을 사용 하 여 시계열 예측 회귀 모델을 구성 하 고 학습 하는 방법에 대해 알아봅니다. 

이렇게 하려면 다음을 수행합니다. 

> [!div class="checklist"]
> * 시계열 모델링을 위한 데이터를 준비 합니다.
> * 개체의 특정 시계열 매개 변수를 구성 [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) 합니다.
> * 시계열 데이터를 사용 하 여 예측을 실행 합니다.

최소한의 코드를 사용하는 환경의 경우 [자습서: 자동화된 기계 학습으로 수요 예측](tutorial-automated-ml-forecast.md)을 통해 [Azure Machine Learning Studio](https://ml.azure.com/)에서 자동화된 기계 학습을 사용하는 시계열 예측 예제를 참조하세요.

기존 시계열 메서드와 달리 자동화 된 ML에서 과거 시계열 값은 다른 예측 변수와 함께 회귀 변수의 추가 차원이 되도록 "피벗" 됩니다. 이 방법은 학습 중에 여러 컨텍스트 변수와 각 변수 간 관계를 통합합니다. 여러 요인이 예측에 영향을 줄 수 있으므로 이 방법은 실제 예측 시나리오에 적합합니다. 예를 들어 판매를 예측 하는 경우 과거 추세, 환율 및 가격의 상호 작용은 판매 결과를 모두 공동으로 구동 합니다. 

## <a name="prerequisites"></a>사전 요구 사항

이 문서에는 다음이 필요 합니다. 

* Azure Machine Learning 작업 영역 작업 영역을 만들려면 [Azure Machine Learning 작업 영역 만들기](how-to-manage-workspace.md)를 참조하세요.

* 이 문서에서는 자동화 된 machine learning 실험을 설정 하는 방법을 잘 알고 있다고 가정 합니다. [자습서](tutorial-auto-train-models.md) 또는 [방법에](how-to-configure-auto-train.md) 따라 자동화 된 기본 기계 학습 실험 디자인 패턴을 볼 수 있습니다.

## <a name="preparing-data"></a>데이터 준비

AutoML 내에서 예측 회귀 태스크 유형과 회귀 작업 유형 간의 가장 중요 한 차이점은 유효한 시계열을 나타내는 데이터의 기능을 포함 하는 것입니다. 정규 시계열에는 잘 정의되고 일관된 빈도가 있으며 연속 시간 범위의 모든 샘플 요소에 값이 있습니다. 

`sample.csv` 파일의 다음 스냅샷을 살펴보세요.
이 데이터 집합은 두 개의 다른 매장, A 및 B가 있는 회사의 일일 판매 데이터입니다. 

또한 다음과 같은 기능을 제공 합니다.

 *  `week_of_year`: 모델에서 주간 계절성를 검색할 수 있습니다.
* `day_datetime`: 일별 빈도로 정리 시계열을 나타냅니다.
* `sales_quantity`: 예측을 실행 하기 위한 대상 열입니다. 

```output
day_datetime,store,sales_quantity,week_of_year
9/3/2018,A,2000,36
9/3/2018,B,600,36
9/4/2018,A,2300,36
9/4/2018,B,550,36
9/5/2018,A,2100,36
9/5/2018,B,650,36
9/6/2018,A,2400,36
9/6/2018,B,700,36
9/7/2018,A,2450,36
9/7/2018,B,650,36
```


Pandas 데이터 프레임으로 데이터를 읽어온 다음, `to_datetime` 함수를 사용하여 시계열이 `datetime` 형식인지 확인합니다.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

이 경우 데이터는 이미 시간 필드 `day_datetime`의 오름차순으로 정렬됩니다. 그러나 실험을 설정하는 경우 올바른 시계열을 작성하기 위해 원하는 시간 열이 오름차순으로 정렬되어 있는지 확인합니다. 

다음 코드 
* 데이터에 1000 레코드가 포함 되어 있다고 가정 하 고 데이터를 명확 하 게 분할 하 여 학습 및 테스트 데이터 집합을 만듭니다. 
* 레이블 열을로 식별 합니다 `sales_quantity` .
* 레이블 필드를에서 `test_data` 로 구분 하 여 집합을 구성 `test_target` 합니다.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!IMPORTANT]
> 미래 가치를 예측하기 위해 모델을 학습하는 경우 학습에 사용된 모든 기능을 의도한 구간에 대해 예측을 실행할 때도 사용할 수 있는지 확인합니다. <br> <br>예를 들어, 수요 예측을 만들 때 현재 주가에 대한 기능을 포함하면 학습 정확도가 크게 높아질 수 있습니다. 그러나 장기 구간을 사용하여 예측하려는 경우 미래 시계열 요소에 해당하는 선물 주가를 정확하게 예측할 수 없으며 모델 정확성이 떨어질 수 있습니다.

<a name="config"></a>

## <a name="training-and-validation-data"></a>학습 및 유효성 검사 데이터

개체에서 별도의 학습 및 유효성 검사 집합을 직접 지정할 수 있습니다 `AutoMLConfig` .   [AutoMLConfig](#configure-experiment)에 대해 자세히 알아보세요.

시계열을 예측 하는 경우에는 기본 유효성 검사에 **ROCV (롤링 원본 교차 유효성 검사)** 만 사용 됩니다. 학습 및 유효성 검사 데이터를 함께 전달 하 고의 매개 변수를 사용 하 여 교차 유효성 검사 접기 수를 설정 합니다 `n_cross_validations` `AutoMLConfig` . ROCV는 원본 시간 요소를 사용하여 계열을 학습 및 유효성 검사 데이터로 나눕니다. 원본을 시간에 따라 슬라이딩하여 교차 유효성 검사 겹을 생성합니다. 이 전략은 시계열 데이터 무결성을 유지 하 고 데이터 누출의 위험을 제거 합니다.

![롤링 원본 교차 유효성 검사](./media/how-to-auto-train-forecast/ROCV.svg)

사용자 고유의 유효성 검사 데이터를 가져와서 [AutoML에서 데이터 분할 및 교차 유효성 검사 구성](how-to-configure-cross-validation-data-splits.md#provide-validation-data)에서 자세히 알아볼 수도 있습니다.


```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```

AutoML이 교차 유효성 검사를 적용 하 여 [오버 맞춤 모델을 방지](concept-manage-ml-pitfalls.md#prevent-over-fitting)하는 방법에 대해 자세히 알아보세요.

## <a name="configure-experiment"></a>실험 구성

[`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?preserve-view=true&view=azure-ml-py) 개체는 자동화된 기계 학습 태스크에 필요한 설정 및 데이터를 정의합니다. 예측 모델에 대 한 구성은 표준 회귀 모델을 설정 하는 것과 유사 하지만 특정 모델, 구성 옵션 및 기능화 단계는 특히 시계열 데이터를 위해 존재 합니다. 

### <a name="supported-models"></a>지원되는 모델
자동화 된 machine learning은 모델 생성 및 조정 프로세스의 일부로 다른 모델 및 알고리즘을 자동으로 시도 합니다. 사용자는 알고리즘을 지정할 필요가 없습니다. 예측 실험의 경우 기본 시계열 및 심층 학습 모델은 모두 권장 사항 시스템의 일부입니다. 다음 표에서는 이러한 모델의 하위 집합을 요약 합니다. 

>[!Tip]
> 기존 회귀 모델은 예측 실험에 대 한 권장 사항 시스템의 일부로도 테스트 됩니다. 모델의 전체 목록은 [지원 되는 모델 표](how-to-configure-auto-train.md#supported-models) 를 참조 하세요. 

모델| Description | 이점
----|----|---
Prophet(미리 보기)|Prophet은 강력한 계절적 효과와 여러 계절의 기록 데이터를 포함하는 시계열에서 가장 잘 작동합니다. 이 모델을 활용 하려면를 사용 하 여 로컬에 설치 `pip install fbprophet` 합니다. | 시계열의 이상값, 누락된 데이터 및 획기적인 변경에 정확하고 빠르고 강력하게 작동합니다.
Auto-ARIMA(미리 보기)|자동 회귀 통합 이동 평균 (ARIMA)은 데이터가 고정 되어 있을 때 가장 잘 수행 됩니다. 즉, 평균 및 편차와 같은 통계 속성이 전체 세트에서 일정하게 유지됩니다. 예를 들어 동전을 대칭 이동 하는 경우 오늘, 내일 또는 다음 연도의 대칭 이동 여부와 관계 없이 헤드를 가져오는 확률은 50%입니다.| 과거의 값을 사용하여 미래의 값을 예측하므로 단변량 계열에 적합합니다.
ForecastTCN(미리 보기)| ForecastTCN은 가장 까다로운 예측 태스크를 처리하고 데이터의 비선형 로컬 및 전역 추세 뿐만 아니라 시계열 간 관계를 캡처하도록 디자인된 신경망 모델입니다.|데이터의 복잡한 추세를 활용하고 가장 큰 데이터 세트로 쉽게 확장할 수 있습니다.

### <a name="configuration-settings"></a>구성 설정

회귀 문제와 마찬가지로 태스크 유형, 반복 횟수, 학습 데이터 및 교차 유효성 검사 수와 같은 표준 학습 매개 변수를 정의합니다. 예측 작업의 경우 실험에 영향을 주는 추가 매개 변수를 설정해야 합니다. 

다음 표에서는 이러한 추가 매개 변수를 요약 합니다. 구문 디자인 패턴에 대해서는 [ForecastingParameter 클래스 참조 설명서](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters?preserve-view=true&view=azure-ml-py) 를 참조 하세요.

| 매개 변수&nbsp;이름 | Description | 필수 |
|-------|-------|-------|
|`time_column_name`|시계열을 작성하고 해당 빈도를 유추하는 데 사용되는 날짜/시간 열을 입력 데이터에 지정하는 데 사용됩니다.|✓|
|`forecast_horizon`|예측 하려는 기간을 정의 합니다. 수평은 시계열 빈도의 단위입니다. 단위는 예측자가 예측해야 하는 학습 데이터의 시간 간격(예: 매월, 매주)을 기준으로 합니다.|✓|
|`enable_dnn`|[DNNs 예측을 사용 하도록 설정]()합니다.||
|`time_series_id_column_names`|타임 스탬프를 사용 하는 여러 행이 있는 데이터의 시계열을 고유 하 게 식별 하는 데 사용 되는 열 이름입니다. 시계열 식별자가 정의 되지 않은 경우 데이터 집합은 하나의 시계열으로 간주 됩니다. 단일 시계열에 대한 자세한 내용은 [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand)을 참조하세요.||
|`freq`| 시계열 데이터 집합 빈도입니다. 이 매개 변수는 매일, 매주, 매년 등 이벤트가 발생 하는 기간을 나타냅니다. 빈도는 [pandas 오프셋 별칭](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects)이어야 합니다.||
|`target_lags`|데이터의 빈도에 따라 대상 값을 지연시킬 행 수입니다. 지연은 목록 또는 단일 정수로 표시됩니다. 지연은 독립 변수와 종속 변수 간 관계가 일치하지 않거나 기본적으로 상관 관계가 없는 경우에 사용해야 합니다. ||
|`feature_lags`| 지연 되는 기능은 `target_lags` 가 설정 되 고 `feature_lags` 가로 설정 된 경우 자동 ML에 의해 자동으로 결정 됩니다 `auto` . 기능 지연을 사용 하면 정확도를 향상 시키는 데 도움이 될 수 있습니다. 기능 지연은 기본적으로 사용 되지 않습니다. ||
|`target_rolling_window_size`|예측 값(학습 세트 크기 이하)을 생성하는 데 사용할 *n* 개 기록 기간입니다. 생략하면 *n* 은 전체 학습 세트 크기입니다. 모델을 학습시킬 때 특정한 양의 기록만 고려하려는 경우 이 매개 변수를 지정합니다. [대상 롤링 창 집계](#target-rolling-window-aggregation)에 대해 자세히 알아보세요.||
|`short_series_handling_config`| 데이터 부족으로 인 한 학습 중에 오류가 발생 하지 않도록 짧은 시계열 처리를 사용 합니다. Short 시리즈 처리는 기본적으로로 설정 됩니다 `auto` . [Short 시리즈 처리](#short-series-handling)에 대해 자세히 알아보세요.|


다음 코드 
* 클래스를 활용 [`ForecastingParameters`](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters?preserve-view=true&view=azure-ml-py) 하 여 실험 학습에 대 한 예측 매개 변수를 정의 합니다.
* 을 `time_column_name` `day_datetime` 데이터 집합의 필드로 설정 합니다. 
* `time_series_id_column_names`에 대 한 매개 변수를 정의 `"store"` 합니다. 이렇게 하면 데이터에 대해 **두 개의 개별 시계열 그룹이** 만들어집니다. 저장소 A와 B에 대 한 하나입니다.
* `forecast_horizon`전체 테스트 집합을 예측 하기 위해를 50로 설정 합니다. 
* 를 사용 하 여 예측 기간을 10 개로 설정 합니다. `target_rolling_window_size`
* 매개 변수를 사용 하 여 두 기간의 대상 값에 대 한 단일 지연을 지정 합니다 `target_lags` . 
* 가 `target_lags` 이 값을 자동으로 검색 하는 권장 되는 "자동" 설정으로 설정 합니다.

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecasting_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                               forecast_horizon=50,
                                               time_series_id_column_names=["store"],
                                               freq='W',
                                               target_lags='auto',
                                               target_rolling_window_size=10)
                                              
```

`forecasting_parameters`그런 다음 `AutoMLConfig` `forecasting` 작업 유형, 기본 메트릭, 종료 조건 및 학습 데이터와 함께 표준 개체에 전달 됩니다. 

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **forecasting_parameters)
```

자동화 된 ML을 사용 하 여 예측 모델을 학습 하는 데 필요한 데이터 양은 `forecast_horizon` 를 `n_cross_validations` `target_lags` `target_rolling_window_size` 구성할 때 지정 된,, 및 값 `AutoMLConfig` 의 영향을 받습니다. 

다음 수식은 시계열 기능을 생성 하는 데 필요한 기록 데이터의 양을 계산 합니다.

필요한 최소 기록 데이터: (2x `forecast_horizon` ) + # `n_cross_validations` + max (max ( `target_lags` ), `target_rolling_window_size` )

지정 된 관련 설정에 대해 필요한 기록 데이터 양을 충족 하지 않는 데이터 집합의 모든 계열에 대해 오류 예외가 발생 합니다. 

### <a name="featurization-steps"></a>기능화 단계

자동화 된 모든 기계 학습 실험에서 자동 크기 조정 및 정규화 기술이 기본적으로 데이터에 적용 됩니다. 이러한 기술은 다양 한 규모의 기능에 중요 한 *특정* 알고리즘을 지 원하는 **기능화** 의 유형입니다. [AutoML에서 기능화](how-to-configure-auto-features.md#automatic-featurization) 의 기본 기능화 단계에 대 한 자세한 정보

그러나 다음 단계는 작업 형식에 대해서만 수행 됩니다 `forecasting` .

* 시계열 샘플 빈도(예: 매시간, 매일, 매주)를 검색하고 없는 시간 요소의 새 레코드를 만들어 계열이 연속되도록 합니다.
* 누락된 값을 대상(전방 채우기를 통해) 및 기능 열(중앙값 열 값 사용)에 귀속
* 시계열 식별자를 기반으로 하는 기능을 만들어 여러 계열에 걸쳐 고정 효과 사용
* 계절적 패턴을 학습하는 데 도움이 되는 시간 기반 기능 만들기
* 범주 변수를 숫자 수량으로 인코딩

이러한 단계의 결과로 생성 되는 기능에 대 한 요약 정보는 [기능화 투명도](how-to-configure-auto-features.md#featurization-transparency) 를 참조 하세요.

> [!NOTE]
> 자동화된 Machine Learning 기능화 단계(기능 정규화, 누락된 데이터 처리, 텍스트를 숫자로 변환 등)는 기본 모델의 일부가 됩니다. 예측에 모델을 사용하는 경우 학습 중에 적용되는 동일한 기능화 단계가 입력 데이터에 자동으로 적용됩니다.

#### <a name="customize-featurization"></a>기능화 사용자 지정

또한 ML 모델을 학습 하는 데 사용 되는 데이터 및 기능으로 인해 관련 예측이 발생 하도록 기능화 설정을 사용자 지정 하는 옵션도 있습니다. 

작업에 대해 지원 되는 사용자 지정은 `forecasting` 다음과 같습니다.

|사용자 지정|정의|
|--|--|
|**열 용도 업데이트**|지정 된 열에 대해 자동 검색 된 기능 유형을 재정의 합니다.|
|**변환기 매개 변수 업데이트** |지정 된 변환기에 대 한 매개 변수를 업데이트 합니다. 현재는 fill_value 및 중앙값 *을 지원 합니다* .|
|**삭제 열** |기능화에서 삭제할 열을 지정 합니다.|

SDK를 사용 하 여 featurizations를 사용자 지정 하려면 `"featurization": FeaturizationConfig` 개체에를 지정 `AutoMLConfig` 합니다. [사용자 지정 featurizations](how-to-configure-auto-features.md#customize-featurization)에 대해 자세히 알아보세요.

>[!NOTE]
> **열 삭제** 기능은 SDK 버전 1.19에서 사용 되지 않습니다. 자동 ML 실험에서 사용 하기 전에 데이터를 정리 하는 과정에서 데이터 집합의 열을 삭제 합니다. 

```python
featurization_config = FeaturizationConfig()

# `logQuantity` is a leaky feature, so we remove it.
featurization_config.drop_columns = ['logQuantitity']

# Force the CPWVOL5 feature to be of numeric type.
featurization_config.add_column_purpose('CPWVOL5', 'Numeric')

# Fill missing values in the target column, Quantity, with zeroes.
featurization_config.add_transformer_params('Imputer', ['Quantity'], {"strategy": "constant", "fill_value": 0})

# Fill mising values in the `INCOME` column with median value.
featurization_config.add_transformer_params('Imputer', ['INCOME'], {"strategy": "median"})
```

실험을 위해 Azure Machine Learning studio를 사용 하는 경우 [studio에서 기능화을 사용자 지정 하는 방법](how-to-use-automated-ml-for-ml-models.md#customize-featurization)을 참조 하세요.

## <a name="optional-configurations"></a>선택적 구성

심층 학습을 사용 하도록 설정 하 고 대상 롤링 창 집계를 지정 하는 등의 예측 태스크에 대 한 추가 옵션 구성을 사용할 수 있습니다. 

### <a name="enable-deep-learning"></a>딥 러닝 사용

> [!NOTE]
> 자동화 Machine Learning의 예측에 대 한 DNN 지원은 **미리 보기** 상태 이며 로컬 실행을 지원 하지 않습니다.

또한 심층 신경망, DNNs를 사용 하 여 심층 학습을 활용 하 여 모델의 점수를 향상할 수 있습니다. 자동화된 ML의 딥 러닝을 사용하여 단변량 및 다변량 시계열 데이터를 예측할 수 있습니다.

딥 러닝 모델에는 다음과 같은 세 가지 기능이 내장되어 있습니다.
1. 입력에서 출력으로의 임의 매핑에서 학습할 수 있습니다.
1. 여러 입력 및 출력을 지원합니다.
1. 긴 시퀀스에 걸쳐 있는 입력 데이터의 패턴을 자동으로 추출할 수 있습니다. 

심층 학습을 사용 하려면 개체에서을 설정 합니다 `enable_dnn=True` `AutoMLConfig` .

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **forecasting_parameters)
```
> [!Warning]
> SDK를 사용 하 여 만든 실험에 대해 DNN를 사용 하도록 설정 하면 [최상의 모델 설명이](how-to-machine-learning-interpretability-automl.md) 사용 되지 않습니다.

Azure Machine Learning studio에서 만든 AutoML 실험에 대해 DNN를 사용 하도록 설정 하려면 [studio 방법에서 작업 형식 설정](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment)을 참조 하세요.

DNN을 활용하는 자세한 코드 예제는 [음료 생산 예측 Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)을 참조하세요.

### <a name="target-rolling-window-aggregation"></a>대상 이동 기간 집계
종종 예측자가 보유할 수 있는 최상의 정보는 대상의 최신 값입니다.  대상 롤링 창 집계를 사용 하면 데이터 값의 롤링 집계를 기능으로 추가할 수 있습니다. 이러한 추가 기능을 생성하고 추가 컨텍스트 데이터로 사용하면 학습 모델의 정확도를 높이는 데 도움이 됩니다.

예를 들어 에너지 수요를 예측 하려는 경우를 가정해 보겠습니다. 3 일의 이동 창 기능을 추가 하 여 열이 아닌 공간의 열 변경에 대해 고려해 볼 수 있습니다. 이 예제에서는 생성자에서을 설정 하 여이 창을 만듭니다 `target_rolling_window_size= 3` `AutoMLConfig` . 

표는 창 집계가 적용 될 때 발생 하는 결과 기능 엔지니어링을 보여 줍니다. **최소값, 최대값** 및 **합계** 에 대 한 열은 정의 된 설정에 따라 3의 슬라이딩 윈도우에서 생성 됩니다. 각 행에는 새로운 계산 된 기능이 있습니다 .이 기능은 9 월 8 일에 대 한 타임 스탬프의 경우 2017 4:00am의 경우 최대, 최소 및 합계 값은 9 월 8 일 2017 1:00AM-3:00AM의 **수요 값** 을 사용 하 여 계산 됩니다. 이 3개의 기간은 함께 이동하면서 나머지 행의 데이터를 채웁니다.

![대상 롤링 창](./media/how-to-auto-train-forecast/target-roll.svg)

[대상 이동 기간 집계 기능](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)을 활용하는 Python 코드 예제를 확인해 보세요.

### <a name="short-series-handling"></a>간단한 시리즈 처리

자동화 된 ML은 모델 개발의 학습 및 유효성 검사 단계를 수행 하기에 충분 한 데이터 요소가 없는 경우 시계열을 **짧은 계열로** 간주 합니다. 데이터 요소 수는 각 실험에 따라 달라 지 며, max_horizon, 교차 유효성 검사 분할의 수, 모델 lookback의 길이 (시계열 기능을 생성 하는 데 필요한 기록의 최대값)에 따라 달라 집니다. 정확한 계산은 [short_series_handling_configuration 참조 설명서](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters?preserve-view=true&view=azure-ml-py#short-series-handling-configuration)를 참조 하세요.

자동화 된 ML은 개체의 매개 변수를 사용 하 여 기본적으로 짧은 계열 처리를 제공 `short_series_handling_configuration` `ForecastingParameters` 합니다. 

Short 시리즈 처리를 사용 하려면 `freq` 매개 변수도 정의 해야 합니다. 매시간 빈도를 정의 하려면를 설정 `freq='H'` 합니다. [여기](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects)에서 빈도 문자열 옵션을 봅니다. 기본 동작을 변경 하려면 `short_series_handling_configuration = 'auto'` `short_series_handling_configuration` 개체에서 매개 변수를 업데이트 `ForecastingParameter` 합니다.  

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecast_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                            forecast_horizon=50,
                                            short_series_handling_configuration='auto',
                                            freq = 'H',
                                            target_lags='auto')
```
다음 표에는에 대 한 사용 가능한 설정이 요약 되어 `short_series_handling_config` 있습니다.
 
|설정|설명
|---|---
|`auto`| 다음은 간단한 시리즈 처리의 기본 동작입니다. <li> *모든 계열이 짧으면* 데이터를 채웁니다. <br> <li> *모든 계열이 짧으면* short 시리즈를 삭제 합니다. 
|`pad`| 인 경우 `short_series_handling_config = pad` 자동화 된 ML은 찾은 각 짧은 계열에 임의 값을 추가 합니다. 다음 목록에는 열 형식과 해당 열이 채워져 있는 항목이 나와 있습니다. <li>Nan를 사용 하는 개체 열 <li> 0으로 숫자 열 <li> False를 사용 하는 부울/논리 열 <li> 대상 열은 0과 표준 편차가 1 인 임의 값으로 채워집니다. 
|`drop`| 이면 `short_series_handling_config = drop` 자동화 된 ML이 짧은 계열을 삭제 하 고 학습 또는 예측에 사용 되지 않습니다. 이러한 계열에 대 한 예측은 NaN을 반환 합니다.
|`None`| 패딩 되거나 삭제 된 계열이 없습니다.

>[!WARNING]
>채우기는 결과 모델의 정확도에 영향을 줄 수 있습니다 .이는 인공 데이터를 도입 하 여 오류 없이 이전 학습을 가져오는 것 이기 때문입니다. <br> <br> 많은 계열이 짧으면 explainability 결과에 약간의 영향을 줄 수 있습니다.

## <a name="run-the-experiment"></a>실험 실행 

개체가 준비 되 면 `AutoMLConfig` 실험을 제출할 수 있습니다. 모델이 완료된 후 최상의 실행 반복을 검색합니다.

```python
ws = Workspace.from_config()
experiment = Experiment(ws, "Tutorial-automl-forecasting")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```
 
## <a name="forecasting-with-best-model"></a>최적 모델로 예측

최상의 모델 반복을 사용하여 테스트 데이터 세트의 값을 예측합니다.

`forecast()`함수는 `predict()` 일반적으로 분류 및 회귀 작업에 사용 되는와 달리 예측이 시작 되어야 하는 경우의 사양을 허용 합니다.

다음 예제에서는 먼저 `y_pred`의 모든 값을 `NaN`으로 바꿉니다. 이 경우 예측 원본은 학습 데이터의 끝에 있습니다. 그러나 `y_pred`의 두 번째 절반을 `NaN`으로 바꾸면 이 함수는 처음 절반의 숫자 값을 수정하지 않은 상태로 유지하지만 두 번째 절반의 `NaN` 값을 예측합니다. 이 함수는 예측된 값과 정렬된 기능을 모두 반환합니다.

`forecast()` 함수에서 `forecast_destination` 매개 변수를 사용하여 지정된 날짜까지의 값을 예측할 수도 있습니다.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

`actual_labels`실제 값과의 예측 값 사이에서 RMSE (제곱 평균 제곱 오차)를 계산 합니다 `predict_labels` .

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```

전체적인 모델 정확도를 확인했으므로 가장 현실적인 다음 단계는 모델을 사용하여 알 수 없는 미래 가치를 예측하는 것입니다. 

테스트 세트 `test_data`와 형식은 같지만 날짜/시간이 미래인 데이터 세트를 제공하면 결과 예측 세트는 각 시계열 단계에 대해 예측된 값입니다. 데이터 세트의 마지막 시계열 레코드가 2018년 12월 31일에 대한 것이라고 가정합니다. 다음 날(또는 예측해야 하는 `forecast_horizon` 이하 기간)의 수요를 예측하려면 2019년 1월 1일의 각 매장에 대한 단일 시계열 레코드를 만듭니다.

```output
day_datetime,store,week_of_year
01/01/2019,A,1
01/01/2019,A,1
```

필요한 단계를 반복하여 이 미래 데이터를 데이터 프레임에 로드하고 `best_run.predict(test_data)`를 실행하여 미래 가치를 예측합니다.

> [!NOTE]
> `target_lags`및/또는을 사용 하도록 설정한 경우에는 자동화 된 ML을 사용 하 여 예측에 대 한 샘플에서 예측을 지원 하지 않습니다 `target_rolling_window_size` .


## <a name="example-notebooks"></a>노트북 예제
다음을 포함하는 고급 예측 구성의 자세한 코드 예제는 [예측 샘플 Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)을 참조하세요.

* [휴일 검색 및 기능화](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [이동 원본 교차 유효성 검사](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [구성 가능한 지연](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [롤링 기간 집계 기능](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

## <a name="next-steps"></a>다음 단계

* [모델 배포 방법 및 위치](how-to-deploy-and-where.md)에 대해 자세히 알아봅니다.
* [Interpretability: 자동화 된 machine learning (미리 보기)의 모델](how-to-machine-learning-interpretability-automl.md)설명에 대해 알아봅니다. 
* 여러 [모델 솔루션 가속기](https://aka.ms/many-models)에서 automl을 사용 하 여 여러 모델을 학습 하는 방법에 대해 알아봅니다.
* 자동화 된 기계 학습으로 실험을 만드는 방법에 대 한 종단 간 예제는 [자습서](tutorial-auto-train-models.md) 를 따르세요.
