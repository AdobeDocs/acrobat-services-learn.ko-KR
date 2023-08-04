---
title: 학생-교사 공동 작업
description: 교사와 학생이 PDF에서 리소스를 손쉽게 공유할 수 있는 온라인 학습 플랫폼을 만드는 방법에 대해 알아봅니다
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8091
thumbnail: KT-8091.jpg
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1485'
ht-degree: 1%

---

# 학생-교사 공동 작업

![사례 메인 배너 사용](assets/UseCaseStudentHero.jpg)

교육기관에서는 PDF 문서를 사용하여 학생들과 학습 자료를 공유합니다. PDF은 교사에게 상호 교환 가능한 문서 형식을 제공합니다.

통합 [Adobe PDF 서비스 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) 및 [Adobe PDF 임베드 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 하나의 앱으로 이동하여 교사와 학생이 하나의 플랫폼에서 학습할 수 있습니다. 예를 들어, 앱을 사용하면 학생들이 과제 또는 보고서 카드에 대해 질문을 하거나 그룹 과제를 공동 작업할 수 있습니다.

PDF 서비스 API에 액세스하기 위한 Node.js 응용 프로그램에 대한 공식 SDK가 있습니다. 이렇게 하면 Microsoft Word 또는 Microsoft Excel과 같은 문서를 PDF으로 변환할 수 있습니다. 또한 여러 보고서 결합, PDF 재정렬 및 페이지 보호와 같은 고급 작업을 수행할 수 있습니다. 자세한 내용은 다음을 참조하십시오. [제품 설명서](https://www.adobe.io/apis/documentcloud/dcsdk/).

## 학습 내용

이 실습 튜토리얼에서는 [교사와 학생이 리소스를 손쉽게 공유할 수 있습니다.](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html) PDF 이 자습서에서는 [학습 포털](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers) Node.js JavaScript 런타임(Node.js) 및 PDF 서비스를 사용하여 생성됩니다.

학습 포털에는 다음과 같은 기능이 있습니다.

* 교사가 리소스를 업로드할 수 있습니다.

* 학생들이 여러 문서를 선택하여 PDF으로 변환할 수 있도록 합니다.

* 문서를 PDF으로 변환할 수 있습니다.

* 웹 브라우저에서 학생들에게 PDF 미리 보기를 제공하고 추가 소프트웨어 없이 문서에 주석을 달 수 있도록 합니다

* 학생들이 주석을 남기고 컴퓨터에 다운로드할 수 있도록 합니다.

방법 살펴보기 [!DNL Adobe Acrobat Services] PDF을 통해 학생들에게 풍부한 경험을 제공하십시오. [!DNL Acrobat Services] API는 기존 애플리케이션에 긴밀하게 통합되어 있으므로 학생은 파일을 업로드, 변환 및 조회한 다음, 주석을 작성 및 저장할 수 있습니다. 이 모든 것이 현재 설정에서 가능합니다.

## 관련 API 및 리소스

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## 학습 포털에 리소스 업로드

학습 포털의 교사용 섹션에서 교사는 과제, 시험 등의 문서를 업로드할 수 있다. 문서는 Microsoft Word, Microsoft Excel, HTML, 다양한 이미지 형식 등 모든 형식이 될 수 있습니다.

![학습 포털의 교사 섹션 스크린샷](assets/edu_1.png)

업로드된 문서는 학생들이 웹 페이지를 열 때 저장되며 학생들에게 제공됩니다.

응용 프로그램이 파일을 업로드하는 방법에 대한 자세한 내용은 [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers).

## 문서를 PDF으로 변환

학생들은 Microsoft Word, Excel, PowerPoint뿐만 아니라 기타 널리 사용되는 텍스트 및 이미지 파일 유형 등 모든 유형의 단일 문서 또는 여러 문서를 PDF으로 변환할 수 있습니다. 학습 포털에서는 PDF 서비스를 사용하여 파일을 PDF으로 변환합니다.

학습 포털을 직접 만들려면 먼저 사용자 고유의 자격 증명을 만들어야 합니다. [등록](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) PDF 서비스 API를 6개월 무료 및 최대 1,000건의 문서 트랜잭션 그 이후로는 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 클래스가 할당을 확장할 때 문서 트랜잭션당 \$0.05에 불과합니다.

학생이 대시보드에서 문서를 선택하면 다음과 같이 표시됩니다.

![학습 포털의 학생 섹션 스크린샷](assets/edu_2.png)

변환용 문서를 선택하고 클릭하기만 하면 됩니다 **보고서 다운로드**.

학습 포털은 문서를 PDF으로 변환하고 PDF 파일 미리 보기와 함께 보고서 페이지를 표시합니다.

다음은 이 단계의 샘플 코드입니다.

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
            .serviceAccountCredentialsBuilder()
            .fromFile("./src/pdftools-api-credentials.json")
            .build();
 
            // Capture the credential from app and show create the context
            const executionContext = adobe.ExecutionContext.create(credentials),
            operation = adobe.CreatePDF.Operation.createNew();
 
            // Pass the content as input (stream)
            const input = adobe.FileRef.createFromLocalFile(rawFile);
            operation.setInput(input);
 
            // Async create the PDF
            let result = await operation.execute(executionContext);
            await result.saveAsFile(outputPdf);
    } catch (err) {
            console.log('Exception encountered while executing operation', err);
    }
}
```

샘플 코드는 `createPdf` PDF 생성을 위한 Express 경로 처리기 내의 메서드입니다.

이 메서드를 호출하는 방법에 대한 자세한 내용은 [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js).

## 학습 리소스 미리보기

사용자 인터페이스는 PDF 포함 API를 사용하여 웹 브라우저에서 PDF을 렌더링합니다. 이 API는 무료로 사용할 수 있습니다.

PDF 임베드 API는 PDF 서비스 API와 다른 자격 증명을 사용하므로 [자격 증명 만들기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
사용하기 전에 그런 다음 PDF 임베드를 완전히 무료로 사용할 수 있습니다.

토큰에 올바른 웹 사이트 URL을 입력해야 합니다. 그렇지 않으면 토큰을 사용하여 PDF을 렌더링할 수 없습니다.

사용자 인터페이스는 [핸들](https://handlebarsjs.com/) 템플릿 언어 웹 브라우저에 PDF이 표시됩니다.

다음은 이 단계에 대한 코드입니다.

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

이 코드는 아래 화면 캡처와 같이 PDF 출력과 PDF 보고서를 다운로드할 수 있는 링크를 표시합니다.

![학생 PDF 미리 보기 스크린샷](assets/edu_3.png)

학생들은 여기에서 보고서를 다운로드하거나 자료를 작업할 수 있습니다.

## PDF 문서에 주석 달기

학습 플랫폼은 기본 주석, 주석 및 PDF 내 토론을 지원해야 합니다. PDF 포함 API는 이러한 모든 기능을 제공합니다. 를 사용하여 주석 지원을 활성화합니다 `showAnnotationTools`교사와 학생이 문서에 주석을 달고 PDF의 일부로 주석을 보관할 수 있습니다.

PDF 문서에서 주석을 사용하려면 인수만 전달하면 됩니다 `showAnnotationTools` : 에 충실하 `previewFile` 있습니다. 그러면 PDF 미리 보기 장치에 주석 도구가 표시됩니다. 미리 보기의 오른쪽 상단에 있는 세 점 메뉴에서 이 도구에 액세스합니다.

![PDF의 주석 도구 스크린샷](assets/edu_4.png)

교사들이 업로드한 문서에서 학생들은 텍스트를 강조하고 댓글을 추가할 수 있습니다.

![PDF에 댓글 추가 스크린샷](assets/edu_5.png)

위의 화면 캡처에서 사용자는 &quot;Guest&quot;라는 레이블이 지정되지만 학생 및 교사와 같은 사용자에 대한 프로필을 구성할 수 있습니다.

학생이 주석을 적용하면 PDF 포함 API에 **저장** 단추를 클릭합니다. 저장하면 파일에 주석이 추가됩니다. 클릭 시도 **저장** 파일에 보고서에 포함된 주석이 포함된 상태로 저장되는 방법을 확인합니다.

학생들은 주석을 사용하여 학습 자료에 대한 질문을 하거나 의견을 공유할 수 있습니다.

## 문서 사용 추적

교사와 학교에서는 학생들이 어떻게 온라인 플랫폼을 사용하고 있는지 보는 것이 중요하다. 이것은 교사들이 학생들의 과제를 더 잘 수행하는 데 도움이 되는 자원을 지원하는데 도움을 준다. PDF 임베드 API는 사용자가 문서를 열고, 읽고, 닫는 등의 모든 이벤트를 측정하는 데 사용할 수 있는 분석과 통합됩니다. PDF 서비스 API를 통해 교사는 학업 무결성을 유지하기 위해 인쇄, 다운로드 및 파일 수정을 비활성화할 수도 있습니다.

만약 [Adobe Analytics](https://www.adobe.io/apis/experiencecloud/analytics.html) 라이선스를 통해 [즉시 사용 가능한 통합](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#adobe-analytics). 그렇지 않은 경우, 콜백을 사용하여 PDF 서비스를 다음과 같은 다른 분석 공급자와 통합합니다. [Google](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#google-analytics).

문서 이벤트를 측정하려면 `registerCallback` Adobe DC View 인스턴스가 있는 메서드입니다. 콘솔에서 문서 열기 또는 페이지 읽기와 같은 기본 측정 단위를 표시할 수 있습니다. 지표를 로그에 저장하거나 다른 분석 저장소에 게시할 수도 있습니다.

다음은 이벤트 핸들러 연결을 위한 샘플 코드입니다.

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

교사는 얼마나 많은 학생들이 과제를 보았는지, 얼마나 많은 학생들이 노트의 모든 페이지를 통과했는지, 그리고 다른 중요한 세부 사항들을 볼 수 있다.

다음은 웹 브라우저 콘솔의 화면 캡처입니다.

![웹 브라우저 콘솔 스크린샷](assets/edu_6.png)

이 화면 캡처를 통해 학생들은 할당 파일을 열고 첫 번째 페이지를 읽습니다. 추가 페이지로 스크롤하지 않았거나 문서에 한 페이지만 있는 경우 파일을 다운로드했습니다. 이러한 지표를 수집하여 분석을 수행하고 학생의 행동을 연구할 수 있습니다.

또한 [Adobe Analytics](https://business.adobe.com/products/analytics/adobe-analytics.html) PDF 포함 API와 통합되므로 Adobe Analytics 제품군을 구독하는 경우 구독에 측정 단위를 게시할 수 있습니다. Adobe Analytics에서 측정 단위를 게시하려면 suite ID를 PDF 포함 API 생성자에 전달하기만 하면 됩니다. (PDF 서비스 API 자격 증명이 아닌 PDF 포함 API 자격 증명을 사용해야 합니다.)

다음은 PDF 포함 API 생성자에 suite ID를 전달하는 방법을 보여 주는 샘플 코드입니다.

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## 다음 단계

이 실습 튜토리얼에서는 PDF 서비스 API 및 PDF 포함 API를 사용하여 학습 포털을 만들어 효과적인 방법을 검토했습니다 [학생과 교사 간의 공동 작업](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html). 이 포털을 통해 교사는 모든 형식의 학습 자료를 업로드하고 PDF 서비스 API를 사용하여 PDF으로 변환할 수 있습니다. 이후 학생들은 PDF 임베드 API를 사용하여 이러한 PDF을 미리 볼 수 있습니다.

PDF 보고서에 주석을 달고, 주석을 보관하고, PDF 보고서 사용을 추적하는 방법을 이해하셨다면 이제 직접 프로젝트에 이러한 솔루션을 구현할 수 있습니다.

사용할 수 있습니다. [!DNL Adobe Acrobat Services] 웹 사이트에서 사용자 친화적이고 인터랙티브한 PDF 경험을 제작하기 위한 API 6개월간 무료로 Adobe PDF 서비스 API를 사용할 수 있습니다. [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (AWS 또는 직접 계약을 통해) 문서 트랜잭션당 \$0.05에 대해서만 적용됩니다. Adobe PDF 임베드 무료(시간 제한 없음) 사용 무료 계정 만들기 [시작하기](https://www.adobe.com/go/dcsdks_credentials) 미래
