---
title: NDA 작성
description: 공동 작업을 위한 다이내믹한 NDA PDF을 제작하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
type: Tutorial
feature: Use Cases
thumbnail: KT-8098.jpg
jira: KT-8098
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 3%

---

# NDA 작성

![사례 메인 배너 사용](assets/UseCaseNDAHero.jpg)

조직은 서비스 및 제품을 빌드하기 위해 외부 콘텐츠 작가와 공동 작업합니다. 비밀 유지 계약(NDA)은 이러한 공동 작업의 중요한 부분입니다. 이는 모든 당사자가 두 기관 중 어느 한 기관을 손상시킬 수 있는 기밀 정보를 공개하지 못하도록 구속합니다.

가장 널리 사용되는 NDA 형식은 PDF 문서입니다. 조직은 NDA를 작성하여 모든 당사자에게 보냅니다. 그런 다음 모든 사람이 서명하면 계약을 시작합니다. 빠른 속도로 진행하는 팀에서는 수동 PDF 제작으로 작업 속도가 느려집니다.

## 학습 내용

이 실습 튜토리얼에서는 기업을 위한 특별한 Microsoft Word NDA 템플릿을 만드는 방법을 설명합니다. Microsoft Word용 Adobe 무료 추가 기능, [Adobe 문서 생성 태거](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)에서 &quot;태그&quot;를 삽입하여 동적 값을 입력합니다. JSON 데이터를 PDF에 전달하고 동적 템플릿을 만드는 방법을 살펴봅니다. 결과 PDF은 비즈니스 요구 사항 및 목표에 따라 브라우저에서 공동 작업자에게 이메일로 전송되거나 표시될 수 있습니다. 따라서 Node.js, JavaScript, Express.js, HTML 및 CSS에 대한 간단한 경험만 있으면 됩니다.

## 관련 API 및 리소스

포함 [!DNL Adobe Acrobat Services]동적 데이터를 사용하여 PDF 문서를 신속하게 생성할 수 있습니다. [!DNL Acrobat Services] 에서는 자동화를 위한 Adobe 문서 생성 API를 비롯한 PDF 툴 세트를 제공합니다. [NDA 작성](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html).

* [Adobe 문서 생성 API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe 문서 생성 태거](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] 키](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## JSON 모델 만들기

Microsoft Word 템플릿은 JSON 모델에 따라 달라지므로 먼저 만듭니다. 튜토리얼에서는 연락처 정보와 같은 회사 세부 정보가 포함된 기본 JSON 구조를 사용합니다.

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

Microsoft Word 내에서 이 구조를 사용하여 템플릿을 생성합니다. 이 데이터는 JSON 형식이라면 모든 데이터 소스에서 가져올 수 있습니다. 단순하게 Node.js 애플리케이션 내에서 여러 파일을 만들지만, 사용 사례에서 공급업체 정보를 가져오기 위해 데이터베이스 연결이 필요할 수 있습니다.

## Microsoft Word 템플릿 만들기

Microsoft Word 문서에서 NDA 템플릿을 만듭니다. Adobe PDF 서비스 API에서는 Microsoft Word 문서에 서비스가 JSON 문서에서 값을 삽입할 수 있는 태그가 포함되어 있을 것으로 예상합니다. Adobe은 템플릿에 대한 모든 요청에서 동일하지만 JSON의 동적 데이터는 변경됩니다. 이러한 태그는 이러한 경우 단일 Microsoft Word 템플릿을 사용하여 모든 공급업체에 맞는 PDF 문서를 만들고 NDA 문서 생성을 자동화하여 프로세스를 가속화하는 데 도움이 됩니다.

설치 [무료 문서 생성 Tagger 추가 기능](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) Microsoft Word로 이동합니다. 조직에 속한 경우 Microsoft Office 관리자에게 모든 사용자를 위한 무료 추가 기능을 설치하도록 요청할 수 있습니다.

추가 기능이 설치되면 Adobe 범주 아래의 홈 탭에서 찾을 수 있습니다. 탭을 열려면 **문서 생성**:

![Word의 문서 생성 추가 기능 스크린샷](assets/nda_1.png)

탭 내에서 샘플 JSON 문서를 업로드할 수 있습니다. 이 문서는 Microsoft Word 템플릿을 만드는 데만 사용하기 때문에 샘플이 될 수 있습니다.

![문서 생성 추가 기능의 샘플 데이터 스크린샷](assets/nda_2.png)

선택 **태그 생성** 템플릿 내에서 사용할 수 있는 항목을 보려면 다음은 템플릿에서 사용할 준비가 된 JSON 구조에서 추출된 속성입니다.

![문서 생성 추가 기능의 텍스트 태그 스크린샷](assets/nda_3.png)

다음은 `authorizedSigner` 필드에 표시됩니다. 다른 필드는 래핑되며 Microsoft Word에서 보기를 확장할 수 있습니다. 추가 기능은 표, 목록, 계산된 값 등과 같은 고급 데이터 옵션도 제공합니다.

## 태그 만들기

템플릿을 만들거나 [기존 템플릿](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) Microsoft Word로 변환할 수 있습니다. 문서를 설정한 후 추가 기능에서 해당 토큰을 클릭하여 각 필드에 태그를 추가합니다.

Microsoft Word 파일의 템플릿:

![샘플 템플릿의 스크린샷](assets/nda_4.png)

이 파일에는 여러 태그가 포함되어 있습니다. 프로그램을 실행하면 이 필드들은 공급업체 정보로 채워집니다.

문서 생성 태거는 Adobe Sign API와 통합됩니다. 이러한 통합 덕분에 서명 텍스트 태그를 자동으로 만들어 생성된 문서를 서명을 위해 Adobe Sign으로 보낼 수 있습니다.

## 공급업체용 NDA 생성

샘플 응용 프로그램 내에서 입력 및 출력을 위한 폴더를 준비했습니다. 앞서 언급했듯이 JSON 파일을 사용하므로 시스템에서 사용 가능한 공급업체를 보여주는 파일이 두 개 있습니다. 파일은 브라우저에 인쇄되는 양식에 표시됩니다.

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

이 코드는 브라우저에서 다음 UI(사용자 인터페이스)를 생성합니다.

![NDA 사용자 인터페이스 만들기 스크린샷](assets/nda_5.png)

책임자가 인물을 선택하면 앱은 Adobe PDF 서비스를 사용하여 이동 중에 NDA를 생성합니다.

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials);
// create the operation
const documentMerge = adobe.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);
const operation = documentMerge.Operation.createNew(options);
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(inputFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Express 라우터 내에서 다음 코드를 사용하십시오.

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

다음을 볼 수 있습니다 [완전한 샘플 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) 단축할 수 있습니다.

이 코드는 JSON 문서와 Microsoft Word 템플릿을 사용하여 [!DNL Adobe Acrobat Services] SDK. 이에 대한 응답으로 출력을 받아 앱의 파일 시스템에 저장합니다. 생성된 문서를 전자 메일을 통해 클라이언트에게 전달하거나 무료 [Adobe PDF 임베드 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

이 호출은 다음과 같은 NDA 문서를 작성합니다.

![NDA 문서 미리 보기 스크린샷](assets/nda_6.png)

[!DNL Adobe Acrobat Services] API가 내용을 삽입하여 PDF 문서를 만듭니다. 이러한 도구를 사용하지 않으면 Office 문서를 처리하고 원시 PDF 파일 형식으로 작업하기 위한 코드를 작성해야 할 수 있습니다. Adobe PDF 서비스의 지원을 통해 단일 API 호출로 이러한 모든 단계를 수행할 수 있습니다.

지금 사용 [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html) NDA에 대한 서명을 요청하고 서명된 최종 문서를 모든 당사자에게 전달할 수 있습니다. Adobe Sign에서 알림 [Webhook 사용](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md). 이 Webhook을 들으면 NDA 상태를 가져올 수 있습니다.

Adobe Sign 프로세스에 대한 자세한 설명을 보려면 [설명서를 참조하십시오](https://www.adobe.io/apis/documentcloud/sign/docs.html) 또는 심층적인 블로그 게시물 을 읽어 보십시오.

## 다음 단계

이 실습 튜토리얼에서는 Adobe 문서 생성 태거를 사용하여 Microsoft Word 템플릿과 JSON 데이터 파일을 사용하여 PDF 문서를 동적으로 생성했습니다. 추가 기능이 [자동으로 NDA 작성](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html) 각 당사자에 대해 사용자 정의된 다음 Sign API를 사용하여 서명을 수집합니다.

이러한 기술을 사용하여 자신만의 NDA나 다른 문서를 동적으로 만들 수 있으므로 팀은 생산적인 작업에 집중할 수 있습니다. 살펴보기 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) 원하는 언어 및 런타임에 대한 API와 SDK를 찾아 PDF 함수를 응용 프로그램에 직접 추가하여 PDF 문서를 빠르게 만들 수 있습니다. [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 6개월 무료 체험판 이용
[사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 문서 트랜잭션당 $0.05에 불과합니다.
