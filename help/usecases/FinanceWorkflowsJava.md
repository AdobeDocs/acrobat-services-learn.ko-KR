---
title: Java에서 재무 문서 워크플로우 관리
description: "[!DNL Adobe Acrobat Services] PDF 재무 문서에서 데이터를 처리하고 추출하는 데 필요한 모든 도구, 서비스 및 기능을 제공합니다."
type: Tutorial
role: Developer
level: Intermediate
feature: Use Cases
thumbnail: KT-7482.jpg
jria: KT-7482
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Java에서 재무 문서 워크플로우 관리

![사례 메인 배너 사용](assets/UseCaseFinancialHero.jpg)

금융 업계는 PDF 파일을 광범위하게 사용하여 데이터를 교환하는데, 이는 문서 형식, 디자인, 구조를 유지하는 데 도움이 되기 때문입니다. 이러한 강력한 형식을 통해 재무 분석가와 자문은 고객이 정보에 입각한 의사 결정을 내리는 데 도움을 줄 수 있습니다.

그러나 PDF 형식은 특히 여러 데이터 소스를 결합할 때 처리 및 자동화하기가 어려울 수 있으며, 이는 금융 업계에서 일반적으로 사용하는 기능입니다. PDF 문서를 처리할 맞춤 솔루션을 구축하는 것도 방법이지만, 소프트웨어와 인프라에 너무 많은 시간과 비용을 투자할 필요는 없습니다. [!DNL Adobe Acrobat Services] PDF 문서에서 데이터를 처리하고 추출하는 데 필요한 모든 도구, 서비스 및 기능을 제공합니다.

## 학습 내용

실습 튜토리얼에서 사용 방법을 살펴보세요 [!DNL Adobe Acrobat Services] API [!DNL Java Spring Boot] 있습니다. PDF 문서에서 내용을 추출하고, Excel과 같은 다른 데이터 형식으로 변환하고, 여러 PDF을 결합하고, 암호를 사용하여 리소스를 보호하는 MVC(모델-보기-컨트롤러) 앱을 빌드합니다. 이 자습서에서는 Adobe을 사용하여 PDF 문서를 처리하고 웹 사이트에 표시하는 방법을 설명합니다 [PDF 포함 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

## 관련 API 및 리소스

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [프로젝트 샘플](https://github.com/adobe/pdftools-java-sdk-samples)

## 설정

[!DNL Adobe Acrobat Services] 인증 시스템을 사용하여 리소스 액세스를 제어합니다. 서비스에 액세스하려면 조직 또는 응용 프로그램의 Adobe에서 API 키를 요청해야 합니다. API 키가 있는 경우 다음 섹션으로 진행합니다. 새 API 키를 만들려면 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 에 [!DNL Acrobat Services] 사이트로 이동합니다. 최대 6개월 동안 사용할 수 있는 1,000개의 문서 트랜잭션을 제공하는 무료 평가판을 사용하여 키를 만들 수 있습니다.

이 튜토리얼을 따라 하려면 두 개의 API 키 세트가 필요합니다.

* Adobe PDF 서비스 — PDF 문서를 처리하는 데 사용됩니다.

* Adobe PDF 임베드 API

자격 증명을 만든 후 PDF 서비스 API 자격 증명과 개인 키를 [!DNL Spring Boot] 응용 프로그램을 선택할 수 있습니다. 자세한 내용은 [Maven 및 Gradle 라이브러리 및 종속성](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) 에 [!DNL Adobe Acrobat Services] 있습니다. 계속하기 전에 필요한 모든 패키지와 라이브러리를 설정해야 합니다.

![PDF 서비스 API 자격 증명을 위한 디렉터리 위치 스크린샷](assets/FAWJ_1.png)

로깅 서비스를 구성하려면 [Adobe 설명서](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) 로깅 섹션으로 스크롤합니다.

>[!NOTE]
>
> 프로덕션 환경에서는 버전 제어에 개인 키를 저장하지 마십시오. 자격 증명의 무단 사용을 방지하려면 항상 비밀 보관소나 키 삽입 서비스를 사용하십시오.

이제 [!DNL Spring Boot] 애플리케이션이 구성되어 있으면 PDF 처리 및 고객에 대한 보고서 생성을 계속할 수 있습니다.

## 보고서 데이터 제출

Adobe PDF 서비스 API를 사용하려면 먼저 `ExecutionContext` 사용자가 제공한 자격 증명을 사용합니다. 응용 프로그램 내에 자격 증명이 있으므로 다음과 같이 파일에서 자격 증명을 읽고 컨텍스트를 작성할 수 있습니다.

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

다음으로 PDF 문서를 처리할 컨텍스트를 가져옵니다. 수행할 수 있는 작업은 다음과 같습니다.

* PDF 문서를 Excel, Word 또는 그래픽 유형으로 변환

* PDF 문서 만들기(HTML, Excel, Word 등에서)

* 여러 PDF 문서 결합

* PDF 문서 Protect 및 보호 해제(암호가 있어야 함)

* 네트워크에서 전달할 PDF 문서 최적화

이 모든 샘플은 [GitHub 샘플](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples) 저장소에 저장됩니다.

다음 [!DNL Spring Boot]에서는 파일이 업로드되는 문자열 경로 또는 스트림을 사용하여 파일을 가져올 수 있습니다. 수행하는 모든 작업을 초기화하고 입력 파일 경로를 설정해야 합니다. 이 튜토리얼에서는 [블랙록](https://www.blackrock.com/us/individual/products/investment-funds). 자체 보고서를 포함하여 다른 소스를 사용할 수 있습니다.

먼저 [FileRef](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/io/FileRef.html) 객체를 반환합니다. 단순하게 만들기 위해 문자열 경로로 파일에 집중합니다. 아래에서 파일의 경로를 PDF에서 Excel로 변환하는 작업을 만듭니다.

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

이 단계가 끝나면 PDF에서 첫 번째 작업을 실행할 준비가 됩니다. 그런 다음 작업을 실행하고 Excel 시트에서 결과를 가져옵니다.

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

이 시나리오는 PDF 파일을 하나만 처리합니다. 여러 PDF 파일로 시작하여 하나의 파일로 결합할 수도 있습니다. 포괄적인 보고서를 제공하려면 여러 소스에서 자금을 처리해야 하므로 재무 데이터 보고에서 여러 파일을 사용하는 것이 일반적입니다.

## 보고서 생성

[!DNL Adobe Acrobat Services] 는 Excel 문서 처리를 지원하지 않지만 커뮤니티 프레임워크 및 라이브러리를 사용하여 내용을 처리할 수 있습니다.

예를 들어, [아파치 POI](https://poi.apache.org/) Excel(또는 기타 Microsoft 문서)을 [!DNL Java Spring Boot] 또는 Excel 파일에서 다른 수동 또는 자동화된 작업을 수행할 수 있습니다.

이 예에서는 PDF 문서로 시작하여 세 자금에 대한 순 자산 값을 추출하고 이를 테이블에 표시합니다. 요구 사항 및 사용 가능한 데이터에 따라 차트 및 표와 같은 다른 정보도 가져올 수 있습니다. 다른 소스의 데이터도 가져올 수 있습니다.

Excel 형식의 보고서가 생성되면 Adobe PDF Services 작업을 사용하여 보고서를 PDF 문서로 다시 변환하고 보호할 수 있습니다.

보고서를 Excel 형식에서 PDF 문서로 변환하려면 다음 작업을 사용하십시오.

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
CreatePDFOperation exportOperation = CreatePDFOperation.createNew();

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_EXCEL);
exportOperation.setInput(inputPdf);

try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_PDF);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

>[!TIP]
>
> 요청이 올 때마다 개체를 다시 만들지 않으려면 Spring의 종속성 삽입을 사용하여 `ExecutionContext` 있습니다.

이 코드는 Excel 형식의 보고서에서 PDF 문서를 생성합니다.

이 PDF을 고객에게 전달하기 전에 암호로 보호할 수 있습니다. 이 보호를 처리하는 다른 작업을 만듭니다. [ProtectPDFOperation](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/ProtectPDFOperation.html), 사용 [ProtectPDFOptions](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/options/protectpdf/package-summary.html) 문서에 암호를 추가합니다.

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

그런 다음 입력을 지정하고 작업을 실행합니다. 무단 액세스를 방지하기 위해 결과 파일에 암호가 있어야 합니다.

## 보고서 표시

PDF 보고서가 생성되면 Adobe PDF 포함 API를 사용하여 웹 사이트에 보고서를 표시할 수 있습니다. 이 JavaScript API를 통해 웹 개발자는 기본적으로 웹 브라우저에서 PDF 문서를 로드하고 렌더링할 수 있습니다.

>[!NOTE]
>
> 이때 두 번째 자격 증명 토큰인 클라이언트 ID가 필요합니다.

에 [!DNL Spring Boot] 응용 프로그램에서 PDF 보고서를 렌더링할 위치에 다음 HTML 조각을 추가합니다.

```
<div id="pdf-viewer"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function()
    {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-client-id-here>", divId: "pdf-viewer" });
        adobeDCView.previewFile(
        {
            content: {
                location: {
                    url: "<your-document.pdf>"
                }
            },
            metaData: {
                fileName: "<document-name.pdf>"
            }
        });
    });
</script>
```

이 스크립트는 PDF 문서를 로드하며 뷰어가 문서에 주석을 달고 주석을 달 수 있도록 합니다. 다음은 Firefox에 표시된 이 포함 API의 보기입니다.

![Firefox의 PDF 문서 스크린샷](assets/FAWJ_2.png)

PDF 포함 API는 PDF을 미리 보고 보고서에 주석을 추가하는 데 필요한 모든 도구를 제공합니다.

## 다음 단계

이 실습 튜토리얼에서는 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) API를 통해 이러한 서비스를 사용하여 PDF 데이터를 처리하고 재무 의사 결정을 위한 보고서를 생성하는 방법에 대해 논의했습니다. 이 비디오에서는 [!DNL Java Spring Boot] 예제 프레임워크로, PDF 문서를 빠르게 처리하는 방법을 보여 줍니다.

살펴보기 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) Adobe PDF 서비스를 통해 얻을 수 있는 이점을 확인해 보십시오. SDK에서 사용할 수 있는 기능에 대한 자세한 내용은 [GitHub 리포지토리](https://github.com/adobe/pdftools-java-sdk-samples) 샘플 및 방법 [PDF 포함 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 응용 프로그램 내에서 PDF을 빠르게 표시할 수 있습니다.

손쉽게 문서를 결합하고 조작하여 재무 클라이언트를 위한 유용한 PDF 보고서를 생성하려면 먼저 무료로 등록하십시오 [Adobe 개발자 계정](https://www.adobe.io/apis/documentcloud/dcsdk/) 미래
