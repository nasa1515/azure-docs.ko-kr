---
title: Azure Data Factory를 프로그래밍 방식으로 모니터링
description: 다른 SDK(소프트웨어 개발 키트)를 사용하여 Data Factory에서 파이프라인을 모니터링하는 방법을 알아봅니다.
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/16/2018
author: dcstwh
ms.author: weetok
ms.custom: devx-track-python
ms.openlocfilehash: 038da033c2bdf78a0a2547cc713944bc11bf093d
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379899"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Azure Data Factory를 프로그래밍 방식으로 모니터링

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

이 문서에서는 다른 SDK(소프트웨어 개발 키트)를 사용하여 Data Factory에서 파이프라인을 모니터링하는 방법을 설명합니다. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>데이터 범위

Data Factory는 45일 동안 파이프라인 실행 데이터를 저장하기만 합니다. Data Factory 파이프라인 실행에 대한 데이터를 프로그래밍 방식으로 쿼리할 때(예: PowerShell 명령 `Get-AzDataFactoryV2PipelineRun` 사용) 선택적 `LastUpdatedAfter` 및 `LastUpdatedBefore` 매개 변수에 대한 최대 날짜가 없습니다. 그러나 지난 해에 대 한 데이터를 쿼리 하는 경우 오류는 발생 하지 않지만 파이프라인은 지난 45 일 동안의 데이터만 실행 합니다.

파이프라인 실행 데이터를 45 일 넘게 유지 하려면 [Azure Monitor](monitor-using-azure-monitor.md)를 사용 하 여 고유한 진단 로깅을 설정 합니다.

## <a name="pipeline-run-information"></a>파이프라인 실행 정보

파이프라인 실행 속성은 [PIPELINERUN API 참조](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/get#pipelinerun)를 참조 하세요. 파이프라인 실행은 수명 주기 중에 다른 상태를 가지 며, 가능한 실행 상태 값은 다음과 같습니다.

* Queued
* InProgress
* 성공
* Failed
* 취소 중
* 취소됨

## <a name="net"></a>.NET
.NET SDK를 사용 하 여 파이프라인을 만들고 모니터링 하는 전체 연습에 대해서는 [.net을 사용 하 여 데이터 팩터리 및 파이프라인 만들기](quickstart-create-data-factory-dot-net.md)를 참조 하세요.

1. 다음 코드를 추가하여 데이터 복사가 완료될 때까지 지속적으로 파이프라인 실행의 상태를 검사합니다.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress" || pipelineRun.Status == "Queued")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. 복사 작업 실행 세부 정보(예: 읽고/쓴 데이터의 크기)를 검색하는 다음 코드를 추가합니다.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

.NET SDK에 대한 전체 설명서를 보려면 [Data Factory .NET SDK 참조](/dotnet/api/microsoft.azure.management.datafactory)를 참조하세요.

## <a name="python"></a>Python
Python SDK를 사용 하 여 파이프라인을 만들고 모니터링 하는 전체 연습에 대해서는 [python을 사용 하 여 데이터 팩터리 및 파이프라인 만들기](quickstart-create-data-factory-python.md)를 참조 하세요.

파이프라인 실행을 모니터링하려면 다음 코드를 추가합니다.

```python
# Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(
    rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(
    rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Python SDK에 대한 전체 설명서를 보려면 [Data Factory Python SDK 참조](/python/api/overview/azure/datafactory)를 참조하세요.

## <a name="rest-api"></a>REST API
REST API를 사용 하 여 파이프라인을 만들고 모니터링 하는 전체 연습을 보려면 [REST API를 사용 하 여 데이터 팩터리 및 파이프라인 만들기](quickstart-create-data-factory-rest-api.md)를 참조 하세요.
 
1. 다음 스크립트를 실행하여 데이터 복사가 완료될 때까지 지속적으로 파이프라인 실행 상태를 검사합니다.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ( ($response.Status -eq "InProgress") -or ($response.Status -eq "Queued") ) {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. 복사 활동 실행 세부 정보(예: 읽고/쓴 데이터의 크기)를 검색하는 다음 스크립트를 실행합니다.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

REST API에 대한 전체 설명서를 보려면 [Data Factory REST API 참조](/rest/api/datafactory/)를 참조하세요.

## <a name="powershell"></a>PowerShell
PowerShell을 사용 하 여 파이프라인을 만들고 모니터링 하는 전체 연습을 보려면 [powershell을 사용 하 여 데이터 팩터리 및 파이프라인 만들기](quickstart-create-data-factory-powershell.md)를 참조 하세요.

1. 다음 스크립트를 실행하여 데이터 복사가 완료될 때까지 지속적으로 파이프라인 실행 상태를 검사합니다.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ( ($run.Status -ne "InProgress") -and ($run.Status -ne "Queued") ) {
                Write-Output ("Pipeline run finished. The status is: " +  $run.Status)
                $run
                break
            }
            Write-Output ("Pipeline is running...status: " + $run.Status)
        }

        Start-Sleep -Seconds 30
    }
    ```
2. 복사 활동 실행 세부 정보(예: 읽고/쓴 데이터의 크기)를 검색하는 다음 스크립트를 실행합니다.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

PowerShell cmdlet에 대한 전체 설명서를 보려면 [Data Factory PowerShell cmdlet 참조](/powershell/module/az.datafactory)를 참조하세요.

## <a name="next-steps"></a>다음 단계
Azure Monitor를 사용하여 Data Factory 파이프라인을 모니터링하는 방법에 대한 자세한 내용은 [Azure Monitor를 사용하여 파이프라인 모니터링](monitor-using-azure-monitor.md) 문서를 참조하세요. 

