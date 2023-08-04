---
title: 보고서 작성 및 편집
description: 웹 사이트에서 고객을 위한 PDF 보고서를 생성하는 방법 알아보기
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8093
thumbnail: KT-8093.jpg
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1346'
ht-degree: 1%

---

# 보고서 작성 및 편집

![사례 메인 배너 사용](assets/UseCaseReportHero.jpg)

금융, 교육, 마케팅 등 다양한 업계에서 PDF을 사용하여 고객 및 이해 관계자와 데이터를 공유하고 있습니다. PDF을 사용하면 표, 그래픽, 인터랙티브한 콘텐츠를 통해 모든 사람이 볼 수 있는 포맷으로 풍부한 문서를 간편하게 공유할 수 있습니다. [!DNL Adobe Acrobat Services] 이러한 기업은 API를 사용하여 Microsoft Word, Microsoft Excel, 그래픽 및 기타 다양한 문서 포맷에서 공유 가능한 PDF 보고서를 생성할 수 있습니다.

말 해 [소셜 미디어 추적 회사 운영](https://www.adobe.io/apis/documentcloud/dcsdk/on-demand-report-creation.html). 고객이 사이트에서 암호로 보호된 부분에 로그인하여 캠페인 분석을 확인합니다. 종종 이러한 통계를 경영진, 주주, 기부자 또는 기타 이해관계자와 공유하려고 합니다. 다운로드 가능한 PDF 문서는 고객이 숫자, 그래프 등을 공유할 수 있는 탁월한 방법입니다.

통합에 의하여 [PDF 서비스 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) 를 웹 사이트로 이동하면 이동 중에 각 고객에 대한 PDF 보고서를 생성할 수 있습니다. PDF을 만든 다음 이를 하나의 편리한 보고서에 통합하여 고객이 다운로드하고 관련자에게 전달할 수 있습니다.

## 학습 내용

이 실습 튜토리얼에서는 Node.js 및 Express.js 환경(일부 JavaScript, HTML 및 CSS 포함)에서 PDF 서비스 SDK를 사용하여 기존 웹 사이트에 PDF 지향 기능을 빠르고 손쉽게 추가하는 방법을 살펴봅니다. 이 웹 사이트에는 관리자가 보고서를 업로드하는 페이지, 고객이 사용 가능한 보고서 목록을 보고 PDF으로 변환할 문서를 선택하는 영역, 시스템에서 생성된 PDF을 다운로드할 수 있는 유용한 끝점이 있습니다.

## 관련 API 및 리소스

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## 고객을 위한 캠페인 보고서 대시보드

>[!NOTE]
>
>이 자습서에서는 Node.js 모범 사례나 웹 응용 프로그램의 보안 방법에 대해 설명하지 않습니다. 웹 사이트의 일부 영역이 공개되어 사용할 수 있으며, 문서 이름 지정이 비생산적인 것일 수 있습니다. 이와 같은 시스템을 설계하기 위한 최선의 방법을 논의하려면 설계자와 엔지니어에게 문의하십시오.

기본 Express.js 웹 애플리케이션에는 고객 보고서 영역과 관리자 섹션이 있습니다. 이 응용 프로그램은 소셜 미디어 캠페인에 대한 보고서를 표시할 수 있습니다. 예를 들어 광고를 클릭한 횟수를 보여 줄 수 있습니다.

![개인화된 보고서를 가져오는 방법 스크린샷](assets/report_1.png)

이 프로젝트는 [GitHub 리포지토리](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools).

이제 보고서를 게시하는 방법을 살펴보죠.

## 보고서 업로드

파일 시스템 기반의 업로드 및 처리 방식만 사용하면 됩니다. Express.js에서 fs 모듈을 사용하여 디렉토리 아래에 사용 가능한 모든 파일을 나열할 수 있습니다.

동일한 페이지에서 관리자가 서버에 보고서 파일을 업로드하여 고객이 볼 수 있도록 합니다. 이러한 파일은 Microsoft Word, Microsoft Excel, HTML 및 [기타 데이터 형식]https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf)을 참조하십시오. 관리 페이지는 다음과 같습니다.

![관리 기능 스크린샷](assets/report_2.png)

>[!NOTE]
>
>URL을 암호로 보호하거나 npm의 Passport 패키지를 사용하여 인증 및 권한 부여 레이어 뒤에서 응용 프로그램을 보호합니다.

관리자가 파일을 선택하여 업로드하면 다른 사용자가 액세스할 수 있는 공용 저장소로 이동됩니다. 동일한 저장소를 사용하여 관리 페이지에서 문서를 게시하고 고객에 대해 사용 가능한 마케팅 보고서를 나열합니다. 이 코드는 다음과 같습니다.

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

이 코드는 모든 파일을 나열하고 파일 목록의 보기를 렌더링합니다.

## 보고서 선택

사용자 측에서는 고객이 소셜 미디어 캠페인 보고서에 포함할 문서를 선택할 수 있는 양식이 있습니다. 간단하게 보기 위해 예제 페이지에는 문서 이름과 문서를 선택할 수 있는 확인란만 표시됩니다. 고객은 단일 보고서 또는 여러 보고서를 선택하여 단일 PDF 문서에 결합할 수 있습니다.

고급 사용자 인터페이스의 경우 여기에 보고서의 미리 보기를 표시할 수도 있습니다.

![고객 역량 스크린샷](assets/report_3.png)

## PDF 보고서 생성

PDF 서비스 SDK를 사용하여 데이터 입력에서 PDF 보고서를 만듭니다. 데이터(위의 스크린샷에 나와 있음)는 Microsoft Word, Microsoft Excel, HTML, 그래픽 등 다양한 데이터 형식에서 가져올 수 있습니다. 먼저 PDF 서비스 SDK용 npm 패키지를 설치합니다.

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

시작하기 전에 API 자격 증명이 있어야 합니다. [Adobe에서 자유](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred). 사용자 [!DNL Acrobat Services] 계정 [6개월 무료 후 종량제](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 문서 트랜잭션당 \$0.05에 사용할 수 있습니다.

아카이브 파일을 다운로드하고 자격 증명과 개인 키를 위해 JSON 파일을 추출합니다. 샘플 프로젝트에서 src 디렉토리에 파일을 배치합니다.

![src 디렉터리 스크린샷](assets/report_4.png)

자격 증명을 설정했으므로 PDF 변환 작업을 작성할 수 있습니다. 이 데모에서는 응용 프로그램에서 수행해야 하는 두 가지 작업이 있습니다.

* 원시 문서를 PDF 파일로 변환

* 여러 PDF 파일을 단일 보고서에 결합

전체 절차는 작업을 실행할 때와 비슷합니다. 유일한 차이점은 사용하는 서비스뿐입니다. 다음 코드에서는 원시 문서를 PDF 파일로 변환합니다.

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

위의 코드에서는 자격 증명을 읽고 실행 컨텍스트를 만듭니다. PDF 서비스 SDK에서 요청을 인증하려면 실행 컨텍스트가 필요합니다.

그런 다음 Raw 문서를 PDF 형식으로 변환하는 PDF 만들기 작업을 실행합니다. 마지막으로 `outputPdf` 매개 변수를 사용하여 PDF 보고서를 복사합니다. 코드 샘플의 src/helpers/pdf.js 파일에서 이 코드를 찾습니다. 이 튜토리얼의 후반부에서 PDF 모듈을 가져와서 이 메서드를 호출합니다.

이전 섹션에서 설명한 대로 고객은 다음 페이지로 이동하여 PDF으로 변환할 보고서를 선택할 수 있습니다.

![고객 역량 스크린샷](assets/report_3.png)

고객이 이러한 보고서 중 하나 이상을 선택하면 PDF 파일이 생성됩니다.

먼저 하나의 PDF 파일이 실제로 작동하는 것을 살펴보겠습니다. 사용자가 단일 보고서를 선택하면 이를 PDF으로 변환하고 다운로드 링크를 제공하기만 하면 됩니다.

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

이 코드는 보고서를 생성하고 고객과 다운로드 URL을 공유합니다. 출력 웹 페이지는 다음과 같습니다.

![고객 다운로드 화면 스크린샷](assets/report_5.png)

다음은 출력 PDF입니다.

![일반 보고서의 스크린샷](assets/report_6.png)

고객은 여러 파일을 선택하여 결합된 보고서를 생성할 수 있습니다. 고객이 두 개 이상의 문서를 선택하면 다음 두 가지 작업을 수행합니다. 첫 번째 명령은 각 문서에 대해 부분 PDF을 만들고 두 번째 명령은 단일 PDF 보고서로 결합합니다.

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

이 방법은 src/helpers/pdf.js 파일에서 사용할 수 있으며 모듈 내보내기의 일부로 표시됩니다.

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

이 코드는 여러 입력 문서에 대해 컴파일된 보고서를 생성합니다. 추가된 함수는 `combinePdf` PDF 파일 경로 이름 목록을 가져오고 단일 출력 PDF을 반환하는 메서드입니다.

이제 소셜 미디어 대시보드 고객은 자신의 계정에서 관련 보고서를 선택하고 이를 하나의 편리한 PDF으로 다운로드할 수 있습니다. 이 대시보드를 사용하면 경영진 및 다른 이해관계자가 데이터, 표, 그래프로 캠페인의 성공 여부를 간편하게 볼 수 있는 형식으로 표시할 수 있습니다.

## 다음 단계

이 실습 위주의 튜토리얼에서는 PDF 서비스 API 사용 방법을 살펴보고 고객이 관련 보고서를 공유 가능한 PDF으로 다운로드할 수 있도록 지원했습니다. PDF 보고 및 읽기 서비스를 위한 강력한 PDF 서비스 API를 보여 주기 위해 Node.js 응용 프로그램을 만들었습니다. 이 애플리케이션은 고객이 단일 보고서 문서를 다운로드하거나 여러 문서를 단일 PDF 보고서로 결합하고 병합하는 방법을 시연했습니다.

이 Adobe 기반 응용 프로그램은 [소셜 미디어 대시보드 고객](https://www.adobe.io/apis/documentcloud/dcsdk/on-demand-report-creation.html) 수신자의 모든 디바이스에 Microsoft Office 또는 다른 소프트웨어가 설치되어 있는지 걱정하지 않고 필요한 보고서를 받고 공유할 수 있습니다. 자체 응용 프로그램에서 동일한 기술을 사용하여 사용자가 문서를 보고, 결합하고, 다운로드할 수 있습니다. 또한 Adobe의 다양한 API를 통해 서명을 추가하고 추적할 수 있습니다.

시작하려면 무료 체험판을 사용하세요 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 적합한 보고 경험을 만들어 직원과 고객을 만족시킬 수 있습니다. 그럼 6개월 동안 무료로 이용하세요 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 마케팅 활동이 증가함에 따라 문서 트랜잭션당 \$0.05에 불과합니다.
