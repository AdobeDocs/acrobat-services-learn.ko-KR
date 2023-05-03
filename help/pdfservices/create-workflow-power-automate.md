---
title: Microsoft Power Automate에서 간단한 워크플로우 만들기
description: Microsoft Power Automate에서 Adobe PDF Services 커넥터를 사용하는 방법에 대해 알아봅니다
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-10379.jpg
kt: 10379
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 2%

---


# Microsoft Power Automate에서 간단한 흐름 만들기

에서 첫 번째 플로우를 만드는 방법 알아보기 [Microsoft Power Automate](https://flow.microsoft.com) 사용 [Adobe PDF 서비스](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) 있습니다.

이 실습 튜토리얼에서는 다음과 같은 방법을 살펴봅니다.

* Word 문서를 PDF으로 변환
* PDF 문서를 하나의 PDF으로 결합
* 암호로 PDF 문서 Protect

## 준비

### 필요한 기능

* **Adobe PDF 서비스에 대한 평가판 또는 프로덕션 자격 증명**
Microsoft Power Automate에서 자격 증명을 가져오고 구성하는 방법에 대한 자세한 내용 [여기](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).
* **Microsoft Power Automate와 프리미엄 커넥터**
Power Automate의 라이선스 수준을 확인하는 방법 알아보기 [여기](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types).
* **OneDrive**
이 자습서에서는 OneDrive 저장소 커넥터를 사용하지만 모든 저장소 커넥터를 대체할 수 있습니다.

### 샘플 파일

두 가지가 있습니다 [샘플 파일](assets/sample-assets.zip) 압축을 풀고 OneDrive에 업로드해야 하는 경우:

* WordDocument01.docx
* WordDocument02.docx

### 자격 증명을 가져오는 중

이 자습서를 완료하려면 Adobe PDF Services용 Microsoft Power Automate에 자격 증명이 이미 구성되어 있어야 합니다. 이 단계를 완료하지 않은 경우 [지침](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).

## 1부: 새 흐름 만들기 및 Word를 PDF으로 변환

### 흐름 만들기

이 부분에서는 [Microsoft Power Automate](https://flow.microsoft.com) 인스턴트 흐름을 사용하여 매개 변수를 추가하고, OneDrive에서 파일을 가져오고, 파일을 PDF으로 변환합니다.

1. 다음으로 이동 [Microsoft Power Automate](https://flow.microsoft.com) 자격 증명으로 로그인합니다.
1. 사이드바에서 **[!UICONTROL 만들기]**.

   ![만들기 단추](assets/createButtonPowerAutomate.png)

1. 선택 **[!UICONTROL 즉각적인 흐름]**.
1. 플로우에 이름을 지정합니다.
1. 아래 *이 흐름을 트리거하는 방법 선택*, 선택 **[!UICONTROL 수동으로 흐름 트리거]**.
1. **[!UICONTROL 생성]**&#x200B;을 선택합니다.

### 파일의 파일 내용 가져오기

다음으로 샘플 파일의 파일 내용을 가져옵니다.

>[!PREREQUISITES]
>
>파일을 업로드하지 않은 경우 [샘플 파일](assets/sample-assets.zip) 압축을 풀고 업로드합니다.


1. In [Power Automate](https://flow.microsoft.com), 선택 **[!UICONTROL + 새 단계]**.
1. 검색 대상 *OneDrive* 를 클릭합니다.
1. 다음을 선택하여 작업 또는 개인 OneDrive 계정을 선택합니다. **[!UICONTROL 비즈니스용 OneDrive]** 또는 **[!UICONTROL OneDrive]**.
1. 검색 대상 *파일 내용 가져오기* 를 클릭합니다.
1. 에 **[!UICONTROL 파일]** 필드에서 폴더 아이콘을 선택하여 *WordDocument01.docx* 파일을 추가합니다.

   ![Microsoft Power Automate에서 파일 콘텐츠 OneDrive 동작 가져오기](assets/getFileContentOneDrive.png)

### 파일을 PDF으로 변환

파일 콘텐츠가 완성되었다면 이제 문서를 PDF으로 변환할 수 있습니다.

1. In [Power Automate](https://flow.microsoft.com), 선택 **[!UICONTROL + 새 단계]**.
1. 검색 대상 *Adobe PDF 서비스* 를 클릭합니다.
1. 선택 **[!UICONTROL Adobe PDF 서비스]**.
1. 검색 대상 *Word를 PDF으로 변환* 를 클릭합니다.
1. In **[!UICONTROL 파일 이름]**&#x200B;에서 파일 이름을 원하는 대로 지정합니다. 이 이름은 로 끝나야 합니다. *.docx*. 이 확장은 문서를 Word에서 PDF으로 변환하는 데 필요합니다.
1. 커서를 **[!UICONTROL 파일 내용]** 필드에 표시됩니다.
1. 사용 **[!UICONTROL 다이내믹 콘텐츠]** 패널, 선택 **[!UICONTROL 파일 내용]**.

   ![Microsoft Power Automate에서 Word를 PDF 동작으로 변환](assets/convertWordToPDFActionPowerAutomate.png)

### 파일을 OneDrive에 저장

문서가 생성되면 파일을 다시 OneDrive에 저장합니다.

1. In [Microsoft Power Automate](https://flow.microsoft.com), 선택 **[!UICONTROL + 새 단계]**.
1. 검색 대상 *OneDrive* 를 클릭합니다.
1. 다음을 선택하여 작업 또는 개인 OneDrive 계정을 선택합니다. **[!UICONTROL 비즈니스용 OneDrive]** 또는 **[!UICONTROL OneDrive]**.
1. 검색 대상 *파일 내용 가져오기* 를 클릭합니다.
1. 검색 대상 *파일 만들기* 를 클릭합니다.
1. 선택 **[!UICONTROL 파일 만들기]**.
1. 에 **[!UICONTROL 폴더 경로]** 필드에서 폴더 아이콘을 선택하여 OneDrive에서 파일을 저장할 위치를 지정합니다.
1. In **[!UICONTROL 파일 이름]**&#x200B;에서 파일 이름을 원하는 대로 지정합니다. 이 이름은 로 끝나야 합니다. *.docx*. 이 확장은 문서를 Word에서 PDF으로 변환하는 데 필요합니다.
1. 에 **[!UICONTROL 파일 내용]** 필드, 사용 **[!UICONTROL 다이내믹 콘텐츠]** 패널 - PDF 파일 내용 변수를 삽입합니다.

### Try Flow

1. 왼쪽 상단에서 **[!UICONTROL 제목 없음]** 플로우의 이름을 바꿉니다.
1. **[!UICONTROL 저장]**&#x200B;을 선택합니다.
1. 선택 **[!UICONTROL 테스트]**.
1. 선택 **[!UICONTROL 수동]** 그리고 **[!UICONTROL 저장 및 테스트]**.
1. **[!UICONTROL 계속]**&#x200B;을 선택합니다.
1. 선택 **[!UICONTROL 실행 흐름]**.

이제 OneDrive 폴더에 변환된 PDF이 표시됩니다.

![OneDrive에서 선택한 변환된 PDF 문서](assets/selectedGeneratedFileInOneDrive.png)

## 2부: 템플릿에서 동적 문서 생성

다음 부분은 1부에서 빌드되고 *Word에서 문서 생성* 템플릿으로 데이터를 문서에 동적으로 병합할 수 있습니다.

### 문서 템플릿 검토

열기 *WordDocument02_.docx* 있습니다. Word 문서에는 데이터가 문서에 채워지는 위치를 나타내는 여러 텍스트 태그가 있습니다.

### 트리거에 매개 변수 추가

동적 데이터를 문서에 푸시하려면 트리거에 대해 값을 묻는 몇 가지 매개 변수를 만들어야 합니다.

1. 플로우를 편집할 때 **[!UICONTROL 수동으로 흐름 트리거]** 를 클릭하여 동작을 확장합니다.
1. 선택 **[!UICONTROL 입력 추가]**.
1. 선택 **[!UICONTROL 텍스트]**.
1. 필드 이름 지정 *이름*.

2-4단계를 반복하여 다음 필드를 추가합니다.

* 성
* 급여

![매개 변수 필드를 사용하여 Power Automate에서 트리거](assets/triggerParametersInPowerAutomate.png)

### 템플릿의 파일 내용 가져오기

문서를 생성하려면 먼저 Word 템플릿의 파일 내용을 가져와야 합니다.

1. Power Automate에서 + **[!UICONTROL 새 단계]**.
1. 검색 대상 *OneDrive* 를 클릭합니다.
1. 다음을 선택하여 작업 또는 개인 OneDrive 계정을 선택합니다. **[!UICONTROL 비즈니스용 OneDrive]** 또는 **[!UICONTROL OneDrive]**.
1. 검색 대상 *파일 내용 가져오기* 를 클릭합니다.
1. 에 **[!UICONTROL 파일]** 필드에서 폴더 아이콘을 선택하여 *WordDocument02.docx* 파일을 추가합니다.

![Microsoft Power Automate의 OneDrive에서 파일 콘텐츠 동작 가져오기](assets/getFileContentAction02.png)

### 템플릿에서 문서 생성

1. Power Automate에서 **[!UICONTROL + 새 단계]**.
1. 검색 대상 *Adobe PDF 서비스* 를 클릭합니다.
1. 선택 **[!UICONTROL Adobe PDF 서비스]**.
1. 다음을 선택합니다. **[!UICONTROL Word 템플릿에서 문서 생성]** 액션 .
1. 에 **[!UICONTROL 템플릿 파일 이름]** 필드에 파일 이름을 지정합니다. 이 이름은 로 끝나야 합니다. *.docx*.

#### 데이터 병합

사용 *Word 템플릿에서 문서 생성* 액션을 사용하면 [동적 내용]을 사용하여 이전에 흐름에 있던 다른 변수의 데이터를 문서에 병합할 수 있습니다.

아래 JSON 데이터를 **데이터 병합** 필드:

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. 필드에 커서를 놓고 *이름* 값입니다.
1. 사용 **[!UICONTROL 다이내믹 콘텐츠]** 패널, 삽입 *이름* 수동으로 흐름 액션을 트리거하기 값

   ![JSON에서 데이터 태그를 사용하여 문서 생성](assets/generateDocumentJSONAction.png)

1. 에 대해 7-8단계를 반복합니다. **[!UICONTROL 성]** 및 **[!UICONTROL 급여]** 필드에 입력할 수 있습니다.
1. 에 **[!UICONTROL 템플릿 파일 내용]** 필드, **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 **[!UICONTROL 파일 내용]** 값 *파일 내용 가져오기* 있습니다.

![모든 값이 완료된 상태에서 Power Automate의 Word 템플릿에서 문서 생성](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>추가 *Word 템플릿에서 문서 생성* 작업에서 Adobe 문서 생성 API를 사용합니다. 템플릿을 만드는 방법에 대해 자세히 알아보려면 다음 리소스를 참조하십시오.
>
>* [Adobe 문서 생성에 대해 더 알아보기](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [Microsoft Word용 Adobe 문서 생성 태거](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [Adobe 문서 생성 API 설명서](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)


### 파일을 OneDrive에 저장

문서가 생성되면 OneDrive에 파일을 다시 저장할 수 있습니다.

1. Power Automate에서 **+ [!UICONTROL 새 단계]**.
1. 검색 대상 *OneDrive* 를 클릭합니다.
1. 다음을 선택하여 작업 또는 개인 OneDrive 계정을 선택합니다. **[!UICONTROL 비즈니스용 OneDrive]** 또는 **[!UICONTROL OneDrive]**.
1. 검색 대상 *파일 만들기* 를 클릭합니다.
1. 선택 **[!UICONTROL 파일 만들기]**.
1. 에 **[!UICONTROL 폴더 경로]** 필드에서 폴더 아이콘을 선택하여 OneDrive에서 파일을 저장할 위치를 지정합니다.
1. 에 **[!UICONTROL 파일 이름]** 필드에 파일 이름을 설정합니다. 출력은 PDF 이므로 파일 이름은 .pdf 확장자로 끝나야 합니다.
1. 사용 **[!UICONTROL 다이내믹 콘텐츠]** PDF 파일 내용 변수를 **[!UICONTROL 파일 내용]** 필드에 표시됩니다.

### Try Flow

![Microsoft Power Automate의 실행 흐름 화면에서 입력 내용 확인](assets/runFlowParameters.png)

1. **[!UICONTROL 저장]**&#x200B;을 선택합니다.
1. 선택 **[!UICONTROL 테스트]**.
1. 선택 **[!UICONTROL 수동]** 그리고 **[!UICONTROL 저장 및 테스트]**.
1. **[!UICONTROL 계속]**&#x200B;을 선택합니다.
1. 값 입력 *이름*, *성*&#x200B;및 *급여*.
1. 선택 **[!UICONTROL 실행 흐름]**.

이제 OneDrive 폴더에 Word 문서에서 생성된 PDF이 표시됩니다. OneDrive에서 PDF 문서를 열면 데이터가 텍스트 태그 위치에 병합됩니다.


## 파트 3: PDF을 하나로 결합

이제 Word 문서를 생성하여 PDF으로 변환했으므로 이제 여러 PDF 문서를 함께 결합할 수 있습니다.

>[!NOTE]
>
>이전 작업에서는 OneDrive에서 문서 사본을 파일로 저장했습니다. 병합 PDF과 같은 도구를 사용하려면 파일을 OneDrive에 저장할 필요가 없습니다. 대신 한 작업에서 다음 작업으로 출력을 직접 전달할 수 있습니다. 이 방법은 각 작업 후에 OneDrive에 저장하는 것보다 좋습니다. 하지만 데모용으로 이러한 파일을 OneDrive에 저장합니다.

### 병합 PDF 추가 단계

1. 플로우를 편집할 때 **[!UICONTROL + 다음 단계]** 를 클릭하여 플로우의 끝에 액션을 추가합니다.
1. 검색 대상 *Adobe PDF 서비스* 를 클릭합니다.
1. 선택 **[!UICONTROL Adobe PDF 서비스]**.
1. 다음을 선택합니다. **[!UICONTROL PDF 병합]** 액션입니다.
1. 에 **[!UICONTROL 병합 PDF 파일 이름]** 필드에 원하는 파일 이름(예:*CombinedDocument.pdf*).
1. 에 **[!UICONTROL 파일 내용 -1]** 필드, **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 *PDF 파일 내용* 값 **[!UICONTROL Word를 PDF으로 변환]** 있습니다.
1. 다음 문서를 추가하려면 **+ [!UICONTROL 새 항목 추가]**.
1. 에 **[!UICONTROL 파일 내용 - 2]** 필드, **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 **[!UICONTROL 출력 파일 내용]** 값 *Word 템플릿에서 문서 생성* 있습니다.

![Microsoft Power Automate에서 PDF 동작 병합](assets/mergePDFAction.png)

### 병합된 PDF을 OneDrive에 저장

문서가 결합되면 문서를 다시 OneDrive에 저장할 수 있습니다.

1. Power Automate에서 **+ [!UICONTROL 새 단계]**.
1. 검색 대상 *OneDrive* 를 클릭합니다.
1. 다음을 선택하여 작업 또는 개인 OneDrive 계정을 선택합니다. **[!UICONTROL 비즈니스용 OneDrive]** 또는 **[!UICONTROL OneDrive]**.
1. 검색 대상 *파일 만들기* 를 클릭합니다.
1. 선택 **[!UICONTROL 파일 만들기]**.
1. 에 **[!UICONTROL 폴더 경로]** 필드에서 폴더 아이콘을 선택하여 OneDrive에서 파일을 저장할 위치를 지정합니다.
1. 에 **[!UICONTROL 파일 이름]** 필드에 파일 이름을 설정합니다. 출력은 PDF 이므로 파일 이름은 .pdf로 끝나야 합니다.
1. 에 **[!UICONTROL 파일 내용]** 필드, 사용 **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 *PDF 파일 내용* 값 **[!UICONTROL PDF 병합]** 있습니다.

   ![Microsoft Power Automate의 플로우 개요](assets/flowOverviewSavedMergedDocument.png)

### Try Flow

1. **[!UICONTROL 저장]**&#x200B;을 선택합니다.
1. 선택 **[!UICONTROL 테스트]**.
1. 선택 **[!UICONTROL 수동]** 그리고 **[!UICONTROL 저장 및 테스트]**.
1. **[!UICONTROL 계속]**&#x200B;을 선택합니다.
1. 값 입력 *이름*, *성*&#x200B;및 *급여*.
1. 선택 **[!UICONTROL 실행 흐름]**.

OneDrive 폴더에는 첫 번째 및 두 번째 문서의 PDF과 결합된 페이지가 표시됩니다.

## 4부: Protect PDF 문서

문서를 생성한 후 OneDrive에 저장하기 전에 추가 단계를 포함하여 편집되지 않도록 보호할 수 있습니다.

### PDF 보호

1. Power Automate에서 플로우를 편집하는 동안 **+** 사이에 **[!UICONTROL PDF 병합]** 및 **[!UICONTROL 파일 만들기 3]** 액션입니다.

   ![두 액션 사이에 새 액션을 추가하는 더하기 기호](assets/addActionToProtect.png)

1. 선택 **[!UICONTROL 동작 추가]**.
1. 검색 대상 *Adobe PDF 서비스* 를 클릭합니다.
1. 선택 **[!UICONTROL Adobe PDF 서비스]**.
1. 다음을 선택합니다. **[!UICONTROL 보기의 Protect PDF]** 액션입니다.
1. 에 **[!UICONTROL 파일 이름]** 필드에 확장명이 .pdf인 한, 이름을 원하는 이름으로 설정합니다.
1. 설정 **[!UICONTROL 암호]** 필드에 암호를 입력하여 문서를 엽니다.
1. 에 **[!UICONTROL 파일 내용]** 필드, **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 *PDF 파일 내용* 값 **[!UICONTROL PDF 병합]** 있습니다.

### OneDrive에 저장 업데이트

문서가 보호되면 OneDrive에 파일을 다시 저장할 수 있습니다. 이 예제에서는 기존 **파일 만들기 3** 새로운 기능이 포함된 작업 *파일 내용* 값입니다.

1. 에서 커서를 선택합니다. **[!UICONTROL 파일 내용]** 필드에 **[!UICONTROL 파일 만들기 3]** 액션입니다.
1. 사용 **[!UICONTROL 다이내믹 콘텐츠]** 삽입할 패널 *PDF 파일 내용* 값 **보기의 Protect PDF** 있습니다.

### Try Flow

1. **[!UICONTROL 저장]**&#x200B;을 선택합니다.
1. 선택 **[!UICONTROL 테스트]**.
1. 선택 **[!UICONTROL 수동]** 그리고 **[!UICONTROL 저장 및 테스트]**.
1. **[!UICONTROL 계속]**&#x200B;을 선택합니다.
1. 값 입력 *이름*, *성*&#x200B;및 *급여*.
1. 선택 **[!UICONTROL 실행 흐름]**.

OneDrive 폴더에는 문서를 보기 위해 암호를 입력하라는 메시지가 표시되는 결합된 PDF이 표시됩니다.

## 다음 단계

이 자습서에서는 Word 문서를 PDF으로 변환하고, 데이터를 기반으로 문서를 생성하고, 문서를 병합하고, 암호로 보호했습니다. 자세한 내용을 보려면 Microsoft Power Automate의 Adobe PDF Services 커넥터에서 사용할 수 있는 다른 작업을 살펴보십시오.

* Microsoft Power Automate에서 사용할 수 있는 사전 제작된 템플릿을 확인합니다.
* 학습 출처 [기사](https://medium.com/adobetech/tagged/microsoft-power-automate) Adobe 기술 블로그
* 검토 [설명서](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) Adobe 문서 생성 API의 경우
