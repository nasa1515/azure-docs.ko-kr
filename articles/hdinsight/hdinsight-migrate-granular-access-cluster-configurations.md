---
title: 세부적인 역할 기반 액세스 Azure HDInsight 클러스터 구성
description: HDInsight 클러스터 구성에 대 한 세분화 된 역할 기반 액세스로 마이그레이션의 일부로 필요한 변경 사항에 대해 알아봅니다.
author: tylerfox
ms.author: tyfox
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/20/2020
ms.openlocfilehash: c6bbb389902c11239f665c6d0db787f61955a953
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100555819"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>클러스터 구성에 대한 세밀한 역할 기반 액세스로 마이그레이션

중요 한 정보를 얻기 위해 보다 세분화 된 역할 기반 액세스를 지원 하기 위해 몇 가지 중요 한 변경 사항을 소개 합니다. 이러한 변경의 일환으로 [영향을 받는 엔터티/시나리오](#am-i-affected-by-these-changes)중 하나를 사용 하는 경우 **2019 년 9 월 3 일** 에 일부 작업이 필요할 수 있습니다.

## <a name="what-is-changing"></a>변경되는 내용

이전에는 사용자가 사용 권한을 가진 모든 사용자가 사용할 수 있게 되었으므로 클러스터 사용자가 소유자, 참가자 또는 독자 [Azure 역할](../role-based-access-control/rbac-and-directory-admin-roles.md)을 처리 개체로 HDInsight API를 통해 암호를 가져올 수 있었습니다 `*/read` . 비밀은 사용자의 역할이 허용 해야 하는 것 보다 높은 수준의 액세스를 얻는 데 사용할 수 있는 값으로 정의 됩니다. 여기에는 클러스터 게이트웨이 HTTP 자격 증명, 저장소 계정 키, 데이터베이스 자격 증명 등의 값이 포함 됩니다.

2019 9 월 3 일부 터 이러한 비밀에 액세스 하려면 권한이 필요 합니다 `Microsoft.HDInsight/clusters/configurations/action` . 즉, 사용자가 읽기 권한자 역할을 사용 하 여 더 이상 액세스할 수 없습니다. 이 권한이 있는 역할은 참가자, 소유자 및 새 HDInsight 클러스터 운영자 역할 (아래 참조)입니다.

또한 참가자 또는 소유자의 관리 권한을 부여 하지 않고도 암호를 검색할 수 있는 새 [HDInsight 클러스터 운영자](../role-based-access-control/built-in-roles.md#hdinsight-cluster-operator) 역할을 도입 하 고 있습니다. 요약:

| 역할                                  | 이전의                                                                                        | 앞으로 이동       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| 판독기                                | -암호를 포함 한 읽기 권한입니다.                                                                   | -읽기 액세스, 비밀 **제외** |           |   |   |
| HDInsight 클러스터 운영자<br>(새 역할) | 해당 없음                                                                                              | -읽기/쓰기 액세스 (암호 포함)         |   |   |
| 참가자                           | -암호를 포함 하는 읽기/쓰기 액세스입니다.<br>-모든 유형의 Azure 리소스를 만들고 관리 합니다.<br>-스크립트 동작을 실행 합니다.     | 변경 내용 없음 |
| 소유자                                 | -암호를 포함 하는 읽기/쓰기 액세스입니다.<br>-모든 리소스에 대 한 모든 권한<br>-다른 사용자에 게 액세스 권한을 위임 합니다.<br>-스크립트 동작을 실행 합니다. | 변경 내용 없음 |

사용자에 게 클러스터 암호에 대 한 읽기/쓰기 액세스 권한을 부여 하는 HDInsight 클러스터 운영자 역할 할당을 추가 하는 방법에 대 한 자세한 내용은 아래 섹션에서 [사용자에 게 Hdinsight 클러스터 운영자 역할 할당 추가](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user)를 참조 하세요.

## <a name="am-i-affected-by-these-changes"></a>이러한 변경의 영향을 받습니까?

영향을 받는 엔터티 및 시나리오는 다음과 같습니다.

- [API](#api): 또는 끝점을 사용 하는 사용자 `/configurations` `/configurations/{configurationName}` 입니다.
- Visual Studio Code 버전 1.1.1 이하의 [Azure HDInsight 도구](#azure-hdinsight-tools-for-visual-studio-code)
- [Azure Toolkit for IntelliJ](#azure-toolkit-for-intellij) 버전 3.20.0 이상입니다.
- [Visual Studio 용 Azure Data Lake 및 Stream Analytics](#azure-data-lake-and-stream-analytics-tools-for-visual-studio) 버전 2.3.9000.1.
- [Azure Toolkit for Eclipse](#azure-toolkit-for-eclipse) 버전 3.15.0 이상입니다.
- [.NET용 SDK](#sdk-for-net)
    - [버전 1.x 또는](#versions-1x-and-2x)2.x: 사용자 `GetClusterConfigurations` `GetConnectivitySettings` `ConfigureHttpSettings` `EnableHttp` `DisableHttp` 는 ConfigurationsOperationsExtensions 클래스에서,, 또는 메서드를 사용 합니다.
    - [버전](#versions-3x-and-up)3.x `Get` `Update` 이상: `EnableHttp` 클래스에서,, 또는 `DisableHttp` 메서드 `ConfigurationsOperationsExtensions` 를 사용 하는 사용자입니다.
- [Python 용 SDK](#sdk-for-python): 클래스의 또는 메서드를 사용 하는 사용자 `get` `update` `ConfigurationsOperations` 입니다.
- [Java 용 SDK](#sdk-for-java): 클래스의 또는 메서드를 사용 하는 사용자 `update` `get` `ConfigurationsInner` 입니다.
- [Go 용 SDK](#sdk-for-go): 구조체의 또는 메서드를 사용 하는 사용자 `Get` `Update` `ConfigurationsClient` 입니다.
- [Az. HDInsight PowerShell](#azhdinsight-powershell) 버전 2.0.0.
시나리오에 대 한 마이그레이션 단계를 보려면 아래 섹션을 참조 하거나 위의 링크를 사용 하십시오.

### <a name="api"></a>API

다음 Api가 변경 되거나 더 이상 사용 되지 않습니다.

- [**/Configurations/{configurationName} 가져오기**](/rest/api/hdinsight/hdinsight-cluster#get-configuration) (중요 한 정보 제거)
    - 이전에는 개별 구성 유형 (암호 포함)을 가져오는 데 사용 되었습니다.
    - 2019 년 9 월 3 일부 터이 API 호출은 암호가 생략 된 개별 구성 유형을 반환 합니다. 비밀을 비롯 한 모든 구성을 얻으려면 새 사후/구성 호출을 사용 합니다. 게이트웨이 설정만 가져오려면 새 POST/getGatewaySettings 호출을 사용 합니다.
- [**/구성 가져오기**](/rest/api/hdinsight/hdinsight-cluster#get-configuration) (사용 되지 않음)
    - 이전에 모든 구성 (암호 포함)을 가져오는 데 사용 됨
    - 2019 년 9 월 3 일부 터이 API 호출은 더 이상 지원 되지 않으며 더 이상 지원 되지 않습니다. 앞으로 진행 되는 모든 구성을 가져오려면 새 사후/구성 호출을 사용 합니다. 중요 한 매개 변수가 생략 된 구성을 얻으려면 GET/configurations/{configurationName} 호출을 사용 합니다.
- [**POST/configurations/{configurationName}**](/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings) (사용 되지 않음)
    - 이전에 게이트웨이 자격 증명을 업데이트 하는 데 사용 되었습니다.
    - 2019 년 9 월 3 일부 터이 API 호출은 더 이상 지원 되지 않으며 더 이상 지원 되지 않습니다. 대신 새 POST/Vvgsettings를 사용 합니다.

다음과 같은 대체 Api가 추가 되었습니다.</span>

- [**사후/구성**](/rest/api/hdinsight/hdinsight-cluster#list-configurations)
    - 이 API를 사용 하 여 암호를 비롯 한 모든 구성을 가져옵니다.
- [**/GetGatewaySettings 게시**](/rest/api/hdinsight/hdinsight-cluster#get-gateway-settings)
    - 이 API를 사용 하 여 게이트웨이 설정을 가져옵니다.
- [**Rvgsettings 게시**](/rest/api/hdinsight/hdinsight-cluster#update-gateway-settings)
    - 이 API를 사용 하 여 게이트웨이 설정 (사용자 이름 및/또는 암호)을 업데이트 합니다.

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Visual Studio Code 용 Azure HDInsight 도구

버전 1.1.1이 하를 사용 하는 경우 중단을 방지 하려면 [최신 버전의 Azure HDInsight Tools for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) 로 업데이트 하세요.

### <a name="azure-toolkit-for-intellij"></a>IntelliJ용 Azure 도구 키트

버전 3.20.0를 사용 하는 경우 중단을 방지 하려면 [최신 버전의 Azure Toolkit for IntelliJ 플러그 인](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) 으로 업데이트 합니다.

### <a name="azure-data-lake-and-stream-analytics-tools-for-visual-studio"></a>Visual Studio 용 Azure Data Lake 및 Stream Analytics 도구

중단을 방지 하기 위해 Azure Data Lake의 버전 2.3.9000.1 이상 [및 Visual Studio 용 Stream Analytics 도구](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools&ssr=false#overview) 를 업데이트 합니다.  업데이트에 대 한 도움말은 [Visual Studio 용 업데이트 Data Lake 도구](./hadoop/apache-hadoop-visual-studio-tools-get-started.md#update-data-lake-tools-for-visual-studio)설명서를 참조 하세요.

### <a name="azure-toolkit-for-eclipse"></a>Eclipse용 Azure 도구 키트

버전 3.15.0를 사용 하는 경우 중단을 방지 하기 위해 [최신 버전의 Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) 을 업데이트 합니다.

### <a name="sdk-for-net"></a>.NET용 SDK

#### <a name="versions-1x-and-2x"></a>버전 1.x 및 2.x

HDInsight SDK for .NET의 [2.1.0 버전](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) 으로 업데이트 합니다. 이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.

- `ClusterOperationsExtensions.GetClusterConfigurations` 는 더 이상 저장소 키 (핵심 사이트) 또는 HTTP 자격 증명 (게이트웨이)과 같은 **중요 한 매개 변수를 반환 하지 않습니다** .
    - 중요 한 매개 변수를 비롯 한 모든 구성을 검색 하려면 `ClusterOperationsExtensions.ListConfigurations` 앞으로를 사용 합니다.  ' Reader ' 역할이 있는 사용자는이 메서드를 사용할 수 없습니다. 이렇게 하면 클러스터에 대 한 중요 한 정보에 액세스할 수 있는 사용자를 세부적으로 제어할 수 있습니다.
    - HTTP 게이트웨이 자격 증명만 검색 하려면를 사용 `ClusterOperationsExtensions.GetGatewaySettings` 합니다.

- `ClusterOperationsExtensions.GetConnectivitySettings` 는 이제 사용 되지 않으며로 대체 되었습니다 `ClusterOperationsExtensions.GetGatewaySettings` .

- `ClusterOperationsExtensions.ConfigureHttpSettings` 는 이제 사용 되지 않으며로 대체 되었습니다 `ClusterOperationsExtensions.UpdateGatewaySettings` .

- `ConfigurationsOperationsExtensions.EnableHttp` 및 `DisableHttp` 는 이제 사용 되지 않습니다. 이제 HTTP는 항상 사용 하도록 설정 되므로 이러한 메서드는 더 이상 필요 하지 않습니다.

#### <a name="versions-3x-and-up"></a>버전 3(sp3) 이상

.NET 용 HDInsight SDK [버전 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) 이상으로 업데이트 합니다. 이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.

- [`ConfigurationOperationsExtensions.Get`](/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet&preserve-view=true) 는 더 이상 저장소 키 (핵심 사이트) 또는 HTTP 자격 증명 (게이트웨이)과 같은 **중요 한 매개 변수를 반환 하지 않습니다** .
    - 중요 한 매개 변수를 비롯 한 모든 구성을 검색 하려면 [`ConfigurationOperationsExtensions.List`](/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.list?view=azure-dotnet&preserve-view=true) 앞으로를 사용 합니다.' Reader ' 역할이 있는 사용자는이 메서드를 사용할 수 없습니다. 이렇게 하면 클러스터에 대 한 중요 한 정보에 액세스할 수 있는 사용자를 세부적으로 제어할 수 있습니다. 
    - HTTP 게이트웨이 자격 증명만 검색 하려면를 사용 [`ClusterOperationsExtensions.GetGatewaySettings`](/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.getgatewaysettings?view=azure-dotnet&preserve-view=true) 합니다. 
- [`ConfigurationsOperationsExtensions.Update`](/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet&preserve-view=true) 는 이제 사용 되지 않으며로 대체 되었습니다 [`ClusterOperationsExtensions.UpdateGatewaySettings`](/dotnet/api/microsoft.azure.management.hdinsight.clustersoperationsextensions.updategatewaysettings?view=azure-dotnet&preserve-view=true) . 
- [`ConfigurationsOperationsExtensions.EnableHttp`](/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet&preserve-view=true) 및 [`DisableHttp`](/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet&preserve-view=true) 는 이제 사용 되지 않습니다. 이제 HTTP는 항상 사용 하도록 설정 되므로 이러한 메서드는 더 이상 필요 하지 않습니다.

### <a name="sdk-for-python"></a>Python용 SDK

Python 용 HDInsight SDK의 [버전 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) 이상으로 업데이트 합니다. 이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.

- [`ConfigurationsOperations.get`](/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) 는 더 이상 저장소 키 (핵심 사이트) 또는 HTTP 자격 증명 (게이트웨이)과 같은 **중요 한 매개 변수를 반환 하지 않습니다** .
    - 중요 한 매개 변수를 비롯 한 모든 구성을 검색 하려면 [`ConfigurationsOperations.list`](/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) 앞으로를 사용 합니다.' Reader ' 역할이 있는 사용자는이 메서드를 사용할 수 없습니다. 이렇게 하면 클러스터에 대 한 중요 한 정보에 액세스할 수 있는 사용자를 세부적으로 제어할 수 있습니다. 
    - HTTP 게이트웨이 자격 증명만 검색 하려면를 사용 [`ClusterOperations.get_gateway_settings`](/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) 합니다.
- [`ConfigurationsOperations.update`](/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurationsoperations#update-resource-group-name--cluster-name--configuration-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) 는 이제 사용 되지 않으며로 대체 되었습니다 [`ClusterOperations.update_gateway_settings`](/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clustersoperations#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) .

### <a name="sdk-for-java"></a>Java 용 SDK

Java 용 HDInsight SDK의 [버전 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/1.0.0/jar) 이상으로 업데이트 합니다. 이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.

- `ConfigurationsInner.get` 는 더 이상 저장소 키 (핵심 사이트) 또는 HTTP 자격 증명 (게이트웨이)과 같은 **중요 한 매개 변수를 반환 하지 않습니다** .
- `ConfigurationsInner.update` 는 이제 사용 되지 않습니다.

### <a name="sdk-for-go"></a>Go 용 SDK

Go 용 HDInsight SDK [버전 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight) 이상으로 업데이트 합니다. 이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight#ConfigurationsClient.Get) 는 더 이상 저장소 키 (핵심 사이트) 또는 HTTP 자격 증명 (게이트웨이)과 같은 **중요 한 매개 변수를 반환 하지 않습니다** .
    - 중요 한 매개 변수를 비롯 한 모든 구성을 검색 하려면 [`ConfigurationsClient.list`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight#ConfigurationsClient.List) 앞으로를 사용 합니다.' Reader ' 역할이 있는 사용자는이 메서드를 사용할 수 없습니다. 이렇게 하면 클러스터에 대 한 중요 한 정보에 액세스할 수 있는 사용자를 세부적으로 제어할 수 있습니다. 
    - HTTP 게이트웨이 자격 증명만 검색 하려면를 사용 [`ClustersClient.get_gateway_settings`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight#ClustersClient.GetGatewaySettings) 합니다.
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight#ConfigurationsClient.Update) 는 이제 사용 되지 않으며로 대체 되었습니다 [`ClustersClient.update_gateway_settings`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2015-03-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings) .

### <a name="azhdinsight-powershell"></a>Az. HDInsight PowerShell
중단을 방지 하려면 [Az PowerShell version 2.0.0](https://www.powershellgallery.com/packages/Az) 이상으로 업데이트 합니다.  이러한 변경의 영향을 받는 메서드를 사용 하는 경우 최소한의 코드를 수정 해야 할 수 있습니다.
- `Grant-AzHDInsightHttpServicesAccess` 는 이제 사용 되지 않으며 새 cmdlet으로 대체 되었습니다 `Set-AzHDInsightGatewayCredential` .
- `Get-AzHDInsightJobOutput` 는 저장소 키에 대 한 세분화 된 역할 기반 액세스를 지원 하도록 업데이트 되었습니다.
    - HDInsight 클러스터 운영자, 기여자 또는 소유자 역할이 있는 사용자는 영향을 받지 않습니다.
    - 읽기 권한자 역할만 있는 사용자는 `DefaultStorageAccountKey` 매개 변수를 명시적으로 지정 해야 합니다.
- `Revoke-AzHDInsightHttpServicesAccess` 는 이제 사용 되지 않습니다. 이제 HTTP는 항상 사용 하도록 설정 되므로이 cmdlet은 더 이상 필요 하지 않습니다.
 Az를 참조 하십시오 [. 자세한 내용은 HDInsight 마이그레이션 가이드를 참조](https://github.com/Azure/azure-powershell/blob/master/documentation/migration-guides/Az.2.0.0-migration-guide.md#azhdinsight) 하세요.

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>사용자에 게 HDInsight 클러스터 운영자 역할 할당 추가

[소유자](../role-based-access-control/built-in-roles.md#owner) 역할을 가진 사용자는 중요 한 hdinsight 클러스터 구성 값 (예: 클러스터 게이트웨이 자격 증명 및 저장소 계정 키)에 대 한 읽기/쓰기 권한을 부여 하려는 사용자에 게 [HDInsight 클러스터 운영자](../role-based-access-control/built-in-roles.md#hdinsight-cluster-operator) 역할을 할당할 수 있습니다.

### <a name="using-the-azure-cli"></a>Azure CLI 사용

이 역할 할당을 추가 하는 가장 간단한 방법은 Azure CLI의 명령을 사용 하는 것입니다 `az role assignment create` .

> [!NOTE]
> 이 명령은 소유자 역할을 가진 사용자만이 사용 권한을 부여할 수 있으므로 실행 해야 합니다. 는 `--assignee` HDInsight 클러스터 운영자 역할을 할당 하려는 사용자의 서비스 사용자 또는 전자 메일 주소의 이름입니다. 권한 부족 오류가 표시 되 면 아래 FAQ를 참조 하세요.

#### <a name="grant-role-at-the-resource-cluster-level"></a>리소스 (클러스터) 수준에서 역할을 부여 합니다.

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee <user@domain.com> --scope /subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.HDInsight/clusters/<ClusterName>
```

#### <a name="grant-role-at-the-resource-group-level"></a>리소스 그룹 수준에서 역할을 부여 합니다.

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

#### <a name="grant-role-at-the-subscription-level"></a>구독 수준에서 역할을 부여 합니다.

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

### <a name="using-the-azure-portal"></a>Azure Portal 사용

또는 Azure Portal를 사용 하 여 사용자에 게 HDInsight 클러스터 운영자 역할 할당을 추가할 수 있습니다. [Azure Portal를 사용 하 여 Azure 역할 할당](../role-based-access-control/role-assignments-portal.md)설명서를 참조 하세요.

## <a name="faq"></a>FAQ

### <a name="why-am-i-seeing-a-403-forbidden-response-after-updating-my-api-requests-andor-tool"></a>API 요청 및/또는 도구를 업데이트한 후 403(사용할 수 없음) 응답이 표시되는 이유는 무엇인가요?

클러스터 구성은 이제 세분화 된 역할 기반 액세스 제어를 사용 하 고 있으며이 `Microsoft.HDInsight/clusters/configurations/*` 를 액세스할 수 있는 권한이 필요 합니다. 이 사용 권한을 얻으려면 구성에 액세스 하려는 사용자 또는 서비스 사용자에 게 HDInsight 클러스터 운영자, 참가자 또는 소유자 역할을 할당 합니다.

### <a name="why-do-i-see-insufficient-privileges-to-complete-the-operation-when-running-the-azure-cli-command-to-assign-the-hdinsight-cluster-operator-role-to-another-user-or-service-principal"></a>Azure CLI 명령을 실행 하 여 다른 사용자 또는 서비스 주체에 HDInsight 클러스터 운영자 역할을 할당 하는 경우 "작업을 완료할 수 있는 권한이 부족 합니다."가 표시 되는 이유는 무엇 인가요?

이 명령을 실행 하는 사용자 또는 서비스 주체에 게 소유자 역할을 보유 하는 것 외에도 담당자의 개체 Id를 조회할 수 있는 충분 한 Azure AD 권한이 있어야 합니다. 이 메시지는 Azure AD 권한이 부족 함을 나타냅니다. `-–assignee`인수를로 바꾸고 `–assignee-object-id` , 담당자의 개체 id를 이름 대신 매개 변수로 제공 합니다 (또는 관리 id의 경우 보안 주체 id). 자세한 정보는 [az role 대입문 create 설명서](/cli/azure/role/assignment#az-role-assignment-create) 의 선택적 매개 변수 섹션을 참조 하세요.

그래도 문제가 해결 되지 않으면 Azure AD 관리자에 게 문의 하 여 올바른 사용 권한을 획득 합니다.

### <a name="what-will-happen-if-i-take-no-action"></a>작업을 수행 하지 않으면 어떻게 되나요?

2019 년 9 월 3 일부 터 `GET /configurations` `POST /configurations/gateway` 호출은 더 이상 정보를 반환 하지 않으며 `GET /configurations/{configurationName}` 호출은 저장소 계정 키 또는 클러스터 암호와 같은 중요 한 매개 변수를 더 이상 반환 하지 않습니다. 해당 SDK 메서드 및 PowerShell cmdlet의 경우에도 마찬가지입니다.

이전 버전의 도구 중 하나를 사용 하는 경우에는 위에서 언급 한 것 처럼 이전 버전의 도구를 사용 하는 경우를 업데이트할 때까지 더 이상 작동 하지 않습니다.

자세한 내용은 시나리오에 대 한이 문서의 해당 섹션을 참조 하세요.