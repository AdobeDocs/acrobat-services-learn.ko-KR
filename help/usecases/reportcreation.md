---
title: 보고서 생성 및 편집
description: 고객을 위해 웹 사이트에서 PDF 보고서를 생성하는 방법에 대해 알아보십시오
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8093
thumbnail: KT-8093.jpg
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1292'
ht-degree: 0%

---

# 보고서 생성 및 편집

![사례 영웅 배너 사용](assets/UseCaseReportHero.jpg)

금융, 교육, 마케팅 및 기타 산업에서는 PDF을 사용하여 고객 및 이해 관계자와 데이터를 공유합니다. PDF을 사용하면 표, 그래픽 및 대화형 콘텐츠가 포함된 리치 문서를 누구나 볼 수 있는 형식으로 쉽게 공유할 수 있습니다. [!DNL Adobe Acrobat Services] API를 사용하면 이러한 회사에서 Microsoft Word, Microsoft Excel, 그래픽 및 기타 다양한 문서 형식에서 공유 가능한 PDF 보고서를 생성할 수 있습니다.

[소셜 미디어 추적 회사를 운영](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation)한다고 가정해 보세요. 고객은 사이트의 암호로 보호된 부분에 로그인하여 캠페인 분석을 봅니다. 종종 경영진, 주주, 기부자 또는 기타 이해 관계자들과 이러한 통계를 공유하려고 합니다. 다운로드 가능한 PDF 문서는 고객이 숫자, 그래프 등을 공유할 수 있는 좋은 방법입니다.

[PDF 서비스 API](https://developer.adobe.com/document-services/apis/pdf-services)를 웹 사이트에 통합하면 이동 중에 각 고객에 대한 PDF 보고서를 생성할 수 있습니다. PDF을 만든 다음 이를 하나의 편리한 보고서로 결합하여 고객이 다운로드하고 관련자에게 전달할 수 있습니다.

## 학습 내용

이 실습용 튜토리얼에서는 Node.js 및 Express.js 환경(일부 JavaScript, HTML 및 CSS만 사용)에서 PDF 서비스 SDK를 사용하여 PDF 지향 기능을 기존 웹 사이트에 빠르고 쉽게 추가하는 방법을 살펴보세요. 이 웹 사이트에는 관리자가 보고서를 업로드하는 페이지, 고객이 사용 가능한 보고서 목록을 보고 PDF으로 변환할 문서를 선택하는 영역, 시스템에서 생성한 PDF을 다운로드하는 데 유용한 끝점이 있습니다.

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## 고객용 캠페인 보고서 대시보드

>[!NOTE]
>
>이 튜토리얼에서는 Node.js 모범 사례나 웹 애플리케이션 보안 방법에 대해 다루지 않습니다. 웹사이트의 일부 영역은 공개적으로 사용할 수 있도록 노출되며 문서 이름 지정은 비생산적일 수 있습니다. 이와 같은 시스템을 설계하는 최선의 방법을 논의하려면 설계자와 엔지니어에게 문의하십시오.

여기에서는 고객 보고서 영역과 관리자 섹션이 있는 기본 Express.js 웹 애플리케이션을 제공합니다. 이 애플리케이션은 소셜 미디어 캠페인에 대한 보고서를 보여줄 수 있습니다. 예를 들어 광고를 클릭한 횟수를 나타낼 수 있습니다.

![개인 맞춤화된 보고서를 가져오는 방법에 대한 스크린샷](assets/report_1.png)

[GitHub 저장소](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools)에서 이 프로젝트를 다운로드할 수 있습니다.

이제 보고서를 게시하는 방법을 살펴보겠습니다.

## 보고서 업로드 중

간단하게 유지하려면 파일 시스템 기반 업로드 및 처리만 사용합니다. Express.js에서 fs 모듈을 사용하여 디렉터리 아래의 사용 가능한 모든 파일을 나열할 수 있습니다.

동일한 페이지에서 관리자가 고객에게 보고서 파일을 볼 수 있도록 서버에 업로드할 수 있습니다. 이러한 파일은 Microsoft Word, Microsoft Excel, HTML 및 그래픽 파일을 포함한 [기타 데이터 형식](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf)과 같은 다양한 형식으로 제공됩니다. 관리 페이지는 다음과 같이 표시됩니다.

![관리자 기능 스크린샷](assets/report_2.png)

>[!NOTE]
>
>암호로 URL을 보호하거나 npm의 Passport 패키지를 사용하여 인증 및 권한 부여 계층 뒤에서 응용 프로그램을 보호합니다.

관리자가 파일을 선택하고 업로드하면 다른 사용자가 액세스할 수 있는 공용 저장소로 이동합니다. 동일한 저장소를 사용하여 관리 페이지에서 문서를 게시하고 고객에 대해 사용 가능한 마케팅 보고서를 나열합니다. 이 코드는 다음과 같습니다.

```
router.get('/', (req, res) => {
try {
let files = fs.readdirSync('./public/documents/raw') // read the files
res.status(200).render("reports", { page: 'reports', files: files });
} catch (error) {
res.status(500).render("crash", { error: error });
}
});
```

이 코드는 모든 파일을 나열하고 파일 목록 보기를 렌더링합니다.

## 보고서 선택

사용자 측면에서는 고객이 소셜 미디어 캠페인 보고서에 포함할 문서를 선택할 수 있는 양식이 제공됩니다. 간소화를 위해 예제 페이지에는 문서 이름과 문서를 선택할 수 있는 확인란만 표시됩니다. 고객은 단일 보고서 또는 여러 보고서를 선택하여 단일 PDF 문서에 결합할 수 있습니다.

고급 사용자 인터페이스를 사용하려면 보고서 미리 보기를 여기에 표시할 수도 있습니다.

![고객 기능 스크린샷](assets/report_3.png)

## PDF 보고서 생성

PDF 서비스 SDK를 사용하여 데이터 입력에서 PDF 보고서를 만듭니다. 위 스크린샷에 나와 있는 것과 같은 데이터는 Microsoft Word, Microsoft Excel, HTML, 그래픽 등과 같은 다양한 데이터 형식에서 가져올 수 있습니다. PDF 서비스 SDK용 npm 패키지를 설치하여 시작합니다.

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

시작하기 전에 API 자격 증명이 있어야 합니다. [Adobe에서 무료](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred). 문서 트랜잭션당 \$0.05만 지불하고 [!DNL Acrobat Services] 계정을 6개월 동안 [무료 사용한 다음 선불 결제](https://developer.adobe.com/document-services/pricing/main)하세요.

아카이브 파일을 다운로드하고 자격 증명과 개인 키에 대한 JSON 파일의 압축을 풉니다. 샘플 프로젝트에서 파일을 src 디렉토리에 배치합니다.

![src 디렉터리 스크린샷](assets/report_4.png)

자격 증명을 설정했으므로 PDF 변환 작업을 작성할 수 있습니다. 이 데모에서는 응용 프로그램에서 수행해야 하는 두 가지 작업이 있습니다.

* Raw 문서를 PDF 파일로 변환

* 하나의 보고서에 여러 PDF 파일 결합

전체 절차는 작업 실행과 유사합니다. 유일한 차이점은 사용하는 서비스입니다. 다음 코드에서는 Raw 문서를 PDF 파일로 변환합니다.

```
async function createPdf(rawFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
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

위의 코드에서 자격 증명을 읽고 실행 컨텍스트를 만듭니다. PDF 서비스 SDK를 사용하려면 요청을 인증하기 위한 실행 컨텍스트가 필요합니다.

그런 다음 Raw 문서를 PDF 형식으로 변환하는 PDF 만들기 작업을 실행합니다. 마지막으로 `outputPdf` 매개 변수를 사용하여 PDF 보고서를 복사합니다. 코드 샘플에서는 src/helpers/pdf.js 파일 아래에서 이 코드를 찾을 수 있습니다. 이 자습서의 뒷부분에서 PDF 모듈을 가져와 이 메서드를 호출합니다.

이전 섹션에서 설명한 대로 고객은 다음 페이지로 이동하여 PDF으로 변환할 보고서를 선택할 수 있습니다.

![고객 기능 스크린샷](assets/report_3.png)

고객이 이러한 보고서 중 하나 이상을 선택하면 PDF 파일이 생성됩니다.

먼저 작동하는 단일 PDF 파일을 살펴보겠습니다. 사용자가 단일 보고서를 선택하면 다운로드로 변환하고 PDF 링크를 제공하기만 하면 됩니다.

```
try {
console.log(`[INFO] generating the report...`);
await pdf.createPdf(`./public/documents/raw/${reports}`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

이 코드는 보고서를 만들고 다운로드 URL을 고객과 공유합니다. 다음은 출력 웹 페이지입니다.

![고객 다운로드 화면 스크린샷](assets/report_5.png)

출력 PDF:

![일반 보고서의 스크린샷](assets/report_6.png)

고객은 여러 파일을 선택하여 결합된 보고서를 생성할 수 있습니다. 고객이 두 개 이상의 문서를 선택하면 두 가지 작업이 수행됩니다. 첫 번째 작업은 각 문서에 대한 부분 PDF을 만들고 두 번째 작업은 해당 문서를 단일 PDF 보고서로 결합합니다.

```
async function combinePdf(pdfs, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CombineFiles.Operation.createNew();
// Pass the PDF content as input (stream)
for (let pdf of pdfs) {
const source = adobe.FileRef.createFromLocalFile(pdf);
operation.addInput(source);
}
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

이 메서드는 src/helpers/pdf.js 파일에서 사용할 수 있으며 모듈 내보내기의 일부로 표시됩니다.

```
try {
console.log(`[INFO] creating a batch report...`);
// Create a batch report and send it back
let partials = [];
for (let index in reports) {
const name = `partial-${index}-${reports[index]}`;
await pdf.createPdf(`./public/documents/raw/${reports[index]}`, `./public/documents/processed/${name}`);
partials.push(`./public/documents/processed/${name.replace('docx', 'pdf').replace('xlsx', 'pdf')}`);
}
await pdf.combinePdf(partials, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the combined report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

이 코드는 여러 입력 문서에 대해 컴파일된 보고서를 생성합니다. 추가된 유일한 함수는 PDF 파일 PDF 이름 목록을 가져오고 단일 출력 경로를 반환하는 `combinePdf` 메서드입니다.

이제 소셜 미디어 대시보드 고객은 계정에서 관련 보고서를 선택하고 하나의 편리한 PDF으로 다운로드할 수 있습니다. 이 대시보드를 통해 관리 및 기타 관련자들에게 데이터, 표 및 그래프를 통해 캠페인의 성공을 보편적으로 쉽게 열 수 있는 형식으로 표시할 수 있습니다.

## 다음 단계

이 실습에서는 고객이 PDF 서비스 API를 사용하여 공유하기 쉬운 PDF으로 관련 보고서를 다운로드하는 방법을 살펴보았습니다. PDF 보고 및 읽기 서비스를 위한 PDF 서비스 API의 강력한 기능을 보여 주기 위해 Node.js 애플리케이션을 만들었습니다. 이 애플리케이션은 고객이 단일 보고서 문서를 다운로드하거나 여러 문서를 결합하여 단일 PDF 보고서로 병합하는 방법을 시연했습니다.

이 Adobe 기반 응용 프로그램은 [소셜 미디어 대시보드 고객](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation)이 장치에 Microsoft Office 또는 기타 소프트웨어가 설치되어 있는지 여부에 관계없이 필요한 보고서를 가져오고 공유할 수 있도록 도와줍니다. 자신의 응용 프로그램에서도 동일한 기술을 사용하여 사용자가 문서를 보고, 결합하고, 다운로드하는 데 도움을 줄 수 있습니다. 또는 Adobe의 다른 많은 API를 확인하여 서명 등을 추가하고 추적해 보세요.

시작하려면 무료 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 계정을 요청한 다음 직원 및 고객을 위한 매력적인 보고 경험을 만드십시오. 6개월 동안 무료로 계정을 사용한 다음 [종량제](https://developer.adobe.com/document-services/pricing/main)를 통해 마케팅 활동을 늘리세요. 문서 트랜잭션당 단 \$0.05입니다.
