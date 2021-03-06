---
title: 여러 컨테이너의 파일 복사
description: Azure Data Factory를 사용 하 여 솔루션 템플릿을 사용 하 여 여러 컨테이너의 파일을 복사 하는 방법을 알아봅니다.
author: dearandyxu
ms.author: yexu
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/1/2018
ms.openlocfilehash: ec7af1e81e0b295491420597636c8443f4d36512
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100376091"
---
# <a name="copy-multiple-folders-with-azure-data-factory"></a>Azure Data Factory를 사용 하 여 여러 폴더 복사

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

이 문서에서는 여러 복사 작업을 사용 하 여 파일 기반 저장소 간에 컨테이너 또는 폴더를 복사 하는 방법에 대해 설명 합니다. 각 복사 작업은 단일 컨테이너 또는 폴더를 복사 해야 합니다. 

> [!NOTE]
> 단일 컨테이너에서 파일을 복사 하려면 [데이터 복사 도구](copy-data-tool.md) 를 사용 하 여 단일 복사 작업으로 파이프라인을 만드는 것이 더 효율적입니다. 이 문서에서는 이러한 간단한 시나리오에 대 한 템플릿이 필요 합니다.

## <a name="about-this-solution-template"></a>이 솔루션 템플릿 정보

이 템플릿은 원본 저장소 저장소의 지정 된 부모 폴더에서 폴더를 열거 합니다. 그런 다음 각 폴더를 대상 저장소에 복사 합니다.

이 템플릿은 다음 세 가지 작업을 포함합니다.
- **GetMetadata** 는 원본 저장소 저장소를 검색 하 고 지정 된 부모 폴더에서 하위 폴더 목록을 가져옵니다.
- **ForEach** **GetMetadata** 작업에서 하위 폴더 목록을 가져온 다음 목록을 반복 하 고 각 폴더를 복사 작업에 전달 합니다.
- **복사** 는 원본 저장소 저장소의 각 폴더를 대상 저장소에 복사 합니다.

템플릿은 다음 매개 변수를 정의합니다.
- *Sourcefilefolder* 는 데이터 원본 저장소의 부모 폴더 경로에 속합니다. *sourcefilefolder/sourcefilefolder* 는 하위 폴더의 목록을 가져올 수 있습니다. 
- *Sourcefiledirectory* 는 데이터 원본 저장소의 부모 폴더 경로에 속합니다. *sourcefiledirectory/sourcefiledirectory* 는 하위 폴더의 목록을 가져올 수 있습니다. 
- *Destinationfilefolder* 는 부모 폴더 경로입니다. *destinationfilefolder/destinationfilefolder* 는 파일이 대상 저장소로 복사 됩니다. 
- *Destinationfiledirectory* 는 파일이 대상 저장소로 복사 되는 *destinationfiledirectory/destinationfiledirectory* 의 부모 폴더 경로에 속합니다. 

저장소 저장소의 루트 폴더에 있는 여러 컨테이너를 복사 하려는 경우 네 가지 매개 변수를 모두로 입력할 수 있습니다 */* . 이렇게 하면 저장소 저장소 간의 모든 것을 복제 합니다.

## <a name="how-to-use-this-solution-template"></a>이 솔루션 템플릿을 사용하는 방법

1. **파일 저장 템플릿 간에 여러 파일 복사 컨테이너** 로 이동 합니다. 원본 저장소 저장소에 대 한 **새** 연결을 만듭니다. 원본 저장소 저장소는에서 여러 컨테이너의 파일을 복사 하려는 위치입니다.

    ![원본에 대한 새 연결 만들기](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. 대상 저장소 저장소에 대 한 **새** 연결을 만듭니다.

    ![대상에 대한 새 연결 만들기](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. **이 템플릿 사용** 을 선택합니다.

    ![이 템플릿 사용](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. 다음 예제와 같이 파이프라인이 표시 됩니다.

    ![파이프라인 표시](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. **디버그** 를 선택하고 **매개 변수** 를 입력한 다음, **마침** 을 선택합니다.

    ![파이프라인 실행](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. 결과를 검토합니다.

    ![결과 검토](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>다음 단계

- [Azure Data Factory 있는 컨트롤 테이블을 사용 하 여 데이터베이스에서 대량 복사](solution-template-bulk-copy-with-control-table.md)

- [Azure Data Factory를 사용하여 여러 컨테이너에서 파일 복사](solution-template-copy-files-multiple-containers.md)
