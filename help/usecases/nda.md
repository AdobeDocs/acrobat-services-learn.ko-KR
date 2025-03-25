---
title: NDA 만들기
description: 공동 작업을 위한 동적 NDA PDF을 만드는 방법 알아보기
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---

# NDA 만들기

![사례 영웅 배너 사용](assets/UseCaseNDAHero.jpg)

조직은 외부 콘텐츠 작가와 협력하여 서비스 및 제품을 구축합니다. 비공개 계약(NDA)은 이러한 공동 작업 중 중요합니다. 그것은 모든 당사자들이 어느 한쪽 실체를 손상시킬 수 있는 기밀 정보를 발표하지 못하도록 구속한다.

가장 널리 사용되는 NDA 형식은 PDF 문서입니다. 조직에서 NDA를 준비하여 모든 당사자에게 보냅니다. 그런 다음 모든 사람이 서명하면 계약을 시작합니다. 고속 팀에서는 수동 PDF 생성으로 진행률이 느려집니다.

## 학습 내용

이 실습용 튜토리얼에서는 회사에 특화된 Microsoft Word NDA 템플릿을 만드는 방법을 설명합니다. Microsoft Word용 Adobe의 무료 추가 기능 [Adobe 문서 생성 Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)에서는 &quot;태그&quot;를 삽입하여 동적 값을 입력합니다. JSON 데이터를 PDF으로 전달하고 동적 템플릿을 만드는 방법을 알아봅니다. 비즈니스 요구 사항 및 목표에 따라 최종 PDF을 이메일로 전송하거나 브라우저에서 공동 작업자에게 표시할 수 있습니다. 따라 하려면 Node.js, JavaScript, Express.js, HTML 및 CSS에 대한 약간의 환경이 필요합니다.

## 관련 API 및 리소스

[!DNL Adobe Acrobat Services]을(를) 사용하면 동적 데이터를 사용하여 즉석에서 PDF 문서를 생성할 수 있습니다. [!DNL Acrobat Services]은(는) [NDA 생성](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation)을 자동화하는 Adobe 문서 생성 API를 포함한 PDF 도구 모음을 제공합니다.

* [Adobe 문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [Adobe 문서 생성 Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] 키](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## JSON 모델 생성

Microsoft Word 템플릿은 JSON 모델에 따라 다르므로, 먼저 해당 템플릿을 만들어야 합니다. 튜토리얼에서는 연락처 정보 등의 회사 세부 정보가 포함된 기본 JSON 구조를 사용합니다.

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

Microsoft Word 내에서 이 구조를 사용하여 템플릿을 생성합니다. 이 데이터는 JSON 포맷인 경우 모든 데이터 소스에서 가져올 수 있습니다. 간소화를 위해 Node.js 애플리케이션 내에 여러 파일을 만들지만 사용 사례에서는 공급업체 정보를 가져오기 위해 데이터베이스 연결이 필요할 수 있습니다.

## Microsoft Word 템플릿 만들기

Microsoft Word 문서에서 NDA 템플릿을 만듭니다. Adobe PDF 서비스 API는 Microsoft Word 문서에 서비스가 JSON 문서의 값을 삽입할 수 있는 태그가 포함되어 있을 것으로 예상합니다. 템플릿이 모든 Adobe 요청에 대해 동일하더라도 JSON의 동적 데이터가 변경됩니다. 이러한 태그는 단일 Microsoft Word 템플릿을 사용하고 NDA 문서 생성을 자동화하여 프로세스를 가속화함으로써 이 경우 모든 공급업체의 PDF 문서를 만드는 데 도움이 됩니다.

Microsoft Word에 [무료 문서 생성 Tagger 추가 기능](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)을 설치할 수 있습니다. 조직의 일원인 경우 Microsoft Office 관리자에게 모든 사용자를 위한 무료 추가 기능을 설치하도록 요청할 수 있습니다.

추가 기능을 설치하고 나면 Adobe 범주 아래 홈 탭에서 해당 추가 기능을 찾을 수 있습니다. 탭을 열려면 **문서 생성**&#x200B;을 선택합니다.

Word에서 문서 생성 추가 기능의 ![스크린샷](assets/nda_1.png)

탭 내에서 샘플 JSON 문서를 업로드할 수 있습니다. 이 문서는 Microsoft Word 템플릿을 만드는 데만 사용하므로 샘플이 될 수 있습니다.

문서 생성 추가 기능의 ![샘플 데이터 스크린샷](assets/nda_2.png)

템플릿 내에서 사용할 수 있는 항목을 보려면 **태그 생성**&#x200B;을 선택하세요. 다음은 JSON 구조에서 추출한 속성으로, 템플릿에서 사용할 수 있는 준비입니다.

문서 생성 추가 기능의 ![텍스트 태그 스크린샷](assets/nda_3.png)

`authorizedSigner` 필드의 기능입니다. 다른 필드는 래핑되어 Microsoft Word에서 보기를 확장할 수 있습니다. 추가 기능은 표, 목록, 계산된 값 등과 같은 고급 데이터 옵션도 제공합니다.

## 태그 만들기

언제든지 템플릿을 만들거나 [기존 템플릿](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)을 Microsoft Word로 가져올 수 있습니다. 문서를 설정한 후에는 추가 기능에서 해당 토큰을 클릭하여 각 필드에 태그를 추가합니다.

Microsoft Word 파일의 다음 템플릿:

![샘플 템플릿의 스크린샷](assets/nda_4.png)

이 파일에는 여러 태그가 포함되어 있습니다. 프로그램을 실행하면 이러한 필드는 공급업체 정보로 채워집니다.

문서 생성 Tagger는 Adobe Sign API와 통합됩니다. 이러한 통합으로 인해 서명 텍스트 태그를 자동으로 생성하여 생성된 문서를 서명을 위해 Adobe Sign으로 보낼 수 있습니다.

## 공급업체에 대한 NDA 생성

샘플 응용 프로그램 내에서 입력 및 출력을 위한 폴더를 준비했습니다. 앞에서 언급했듯이 JSON 파일을 사용하여 시스템에서 사용 가능한 벤더를 표시하는 두 개의 파일이 있습니다. 해당 파일은 브라우저에 인쇄되는 양식 안에 표시됩니다.

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

이 코드는 브라우저에 다음 UI(사용자 인터페이스)를 생성합니다.

![NDA 사용자 인터페이스 만들기 스크린샷](assets/nda_5.png)

관리자가 사람을 선택하면 앱에서 Adobe PDF 서비스를 사용하여 이동 중에 NDA를 생성합니다.

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

Express 라우터 내에서 다음 코드 사용:

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

GitHub에서 [전체 샘플 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)를 볼 수 있습니다.

이 코드는 [!DNL Adobe Acrobat Services] SDK에 대한 API 호출에서 JSON 문서와 Microsoft Word 템플릿을 사용합니다. 이에 대한 응답으로 출력을 수신하고 앱의 파일 시스템에 저장합니다. 생성된 문서를 전자 메일을 통해 클라이언트에게 전달하거나 무료 [Adobe PDF Embed API](https://developer.adobe.com/document-services/apis/pdf-embed)를 사용하여 브라우저 내에서 미리 보기를 표시할 수 있습니다.

이 호출은 다음 NDA 문서를 만듭니다.

NDA 문서 미리 보기의 ![스크린샷](assets/nda_6.png)

[!DNL Adobe Acrobat Services] API가 콘텐츠를 삽입하여 PDF 문서를 만듭니다. 이러한 도구가 없으면 Office 문서를 처리하고 원시 PDF 파일 형식으로 작업할 코드를 작성해야 할 수 있습니다. Adobe PDF Services를 사용하면 단일 API 호출로 이러한 모든 단계를 수행할 수 있습니다.

이제 [Adobe Sign API](https://developer.adobe.com/adobesign-api/)를 사용하여 NDA에서 서명을 요청하고 마지막으로 서명된 문서를 모든 당사자에게 전달합니다. Adobe Sign은 Webhook을 사용하여 [알림을 보냅니다](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md). 이 Webhook을 듣고 NDA 상태를 가져올 수 있습니다.

Adobe Sign 프로세스에 대한 자세한 설명은 [설명서를 참조하거나](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html) 이 자세한 블로그 게시물을 읽어 보십시오.

## 다음 단계

이 실습용 튜토리얼에서는 Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿 및 JSON 데이터 파일을 사용하여 PDF 문서를 동적으로 생성했습니다. 추가 기능을 사용하면 각 당사자에 대해 사용자 지정된 [NDA](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation)를 자동으로 만든 다음 Sign API를 사용하여 서명을 수집할 수 있습니다.

이러한 기술을 사용하여 자신의 NDA 또는 기타 문서를 동적으로 생성하여 팀이 생산적인 작업에 집중할 수 있도록 합니다. [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/apis/pdf-services)을(를) 탐색하여 선택한 언어 및 런타임에 대한 API와 SDK를 찾으면 PDF 함수를 응용 프로그램에 직접 추가하여 PDF 문서를 빠르게 만들 수 있습니다. 6개월 무료 체험판으로 [시작](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
문서 트랜잭션당 $0.05에 대해서만 [종량제](https://developer.adobe.com/document-services/pricing/main)를 받으세요.
