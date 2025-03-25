---
title: 법적 계약 관리
description: 사용자 정의 데이터 입력으로 법적 문서를 자동으로 생성 및 보호하는 방법에 대해 알아보십시오.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8097
thumbnail: KT-8097.jpg
exl-id: e0c32082-4f8f-4d8b-ab12-55d95b5974c5
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1890'
ht-degree: 0%

---

# 법적 계약 관리

![사례 영웅 배너 사용](assets/UseCaseLegalHero.jpg)

디지털화에는 문제가 따릅니다. 현재 대부분의 조직에는 작성, 편집, 승인 및 다른 당사자가 서명해야 하는 [법률 계약](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts)의 다양한 유형이 있습니다. 이러한 법적 계약은 종종 고유한 사용자 정의 및 브랜딩을 필요로 합니다. 조직이 보안을 유지하기 위해 로그인한 후 보호된 형식으로 저장해야 할 수도 있습니다. 이러한 모든 작업을 수행하려면 강력한 문서 생성 및 관리 솔루션이 필요합니다.

많은 솔루션은 일부 문서 생성을 제공하지만 특정 시나리오에만 적용되는 조항과 같은 데이터 입력 및 조건부 논리를 사용자 정의할 수 없습니다. 이러한 문서가 광범위해짐에 따라 기업의 법적 템플릿을 수동으로 업데이트하는 작업은 까다롭고 오류가 발생하기 쉽습니다. 이러한 프로세스를 자동화해야 할 필요성이 상당합니다.

## 학습 내용

이 실습형 튜토리얼에서는 문서에 사용자 지정 입력 필드를 생성하는 [[!DNL Adobe Acrobat Services] API](https://developer.adobe.com/document-services/apis/doc-generation)의 기능을 살펴봅니다. 또한 생성된 문서를 보호된 휴대용 문서 형식(PDF)으로 쉽게 변환하여 데이터를 조작하지 못하도록 하는 방법을 살펴봅니다.

이 튜토리얼에서는 약정의 PDF 변환을 살펴볼 때 약간의 프로그래밍을 제공합니다. 효과적으로 따라 하려면 [Microsoft Word](https://www.microsoft.com/en-us/download/office.aspx) 및 [Node.js](https://nodejs.org/)이(가) PC에 설치되어 있어야 합니다. Node.js 및 [ES6 구문](https://www.w3schools.com/js/js_es6.asp)에 대한 기본 이해도 권장합니다.

## 관련 API 및 리소스

* [Adobe 문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation)

* [PDF Embed API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [프로젝트 코드](https://github.com/agavitalis/adobe_legal_contracts.git)

## 템플릿 문서 만들기

Microsoft Word 응용 프로그램을 사용하거나 Adobe의 [샘플 Word 템플릿](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)을 다운로드하여 법률 문서를 만들 수 있습니다. 여전히 Microsoft Word용 [Adobe 문서 생성 Tagger 추가 기능](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)과 같은 일부 도우미 도구를 사용하지 않고 입력을 사용자 정의하고 이러한 문서에 디지털 서명하는 것은 쉽지 않습니다.

Document Generation Tagger는 태그를 사용하여 문서를 매끄럽게 사용자 지정할 수 있도록 만들어진 Microsoft Word 추가 기능입니다. JSON 데이터를 사용하여 동적으로 채워지는 문서 템플릿에서 동적 필드를 만들 수 있습니다.

![Word에서 Adobe Document Generation Tagger를 추가하는 방법에 대한 스크린샷](assets/legal_1.png)

Document Generation Tagger 사용을 설명하기 위해 이 추가 기능을 설치한 다음 간단한 법적 계약 문서의 태그 지정에 사용되는 JSON 데이터 모델을 만듭니다.

**삽입** 탭을 클릭한 다음 추가 기능 그룹에서 **내 추가 기능**&#x200B;을 클릭하여 Word에서 문서 생성 Tagger를 설치합니다. Office 추가 기능 메뉴에서 &quot;Adobe 문서 생성&quot;을 검색한 다음 **추가**&#x200B;를 클릭하고 프로세스를 따르십시오. 위의 화면 캡처에서 이러한 단계를 확인할 수 있습니다.

Word용 Document Generation Tagger 추가 기능을 설치한 후 간단한 JSON 데이터 모델을 만들어 법적 문서에 태그를 지정합니다.

계속하려면 원하는 편집기를 열고 Agreement.json이라는 파일을 만든 다음, 아래 코드 스니펫을 만든 JSON 파일에 붙여넣으십시오.

```
{
"Agreement": {
"Date": "1/24/2021",
"Prime Contractor Name": "Ogbonna Vitalis Corp",
"Prime State": "Lagos",
"Address": "Maryland Ave, Lagos State, Ng",
"Sub Contractor Name": "Vivvaa Soln",
"Sub Contractor State": "California",
"Sub Contractor Address": "Molusi Avenue, Dallas Texas, CA",
"Agreement Date": "1/24/2021",
"Length": 5
}
}
```

이 JSON 문서를 저장한 후 Document Generation Tagger 추가 기능으로 가져옵니다. 아래 화면 캡처와 같이 Word 화면 오른쪽 상단의 Adobe 그룹에서 **문서 생성**&#x200B;을 클릭하여 문서를 가져옵니다.

Word에서 Adobe 문서 생성 Tagger 추가 기능의 ![스크린샷](assets/legal_2.png)

안내할 비디오가 표시됩니다. **시작하기**&#x200B;를 클릭하여 보거나 바로 태그 지정 필드로 이동할 수 있습니다. **시작하기**&#x200B;를 클릭하면 업로드 양식이 나타납니다. **JSON 파일 업로드**&#x200B;를 클릭하고 방금 만든 JSON 파일을 선택합니다. 가져오기가 완료되면 **태그 생성**&#x200B;을 클릭하여 태그를 생성합니다.

태그를 가져오고 생성한 후 이러한 태그를 문서에 추가할 수 있습니다. 태그를 추가하려면 태그가 표시될 정확한 위치에 커서를 놓습니다. 그런 다음 문서 생성 API에서 태그를 선택하고 **텍스트 삽입**&#x200B;을 클릭합니다. 아래의 화면 캡처에는 이러한 절차가 설명되어 있습니다.

![문서에 태그를 추가하는 스크린샷](assets/legal_3.png)

가져온 JSON 데이터 모델을 사용하여 만든 기본 태그 외에, 이미지, 조건부 로직, 계산, 반복 요소 및 조건부 구문과 같은 추가 옵션에 고급 기능을 사용할 수도 있습니다. [문서 생성 태그] 패널에서 **고급**&#x200B;을 클릭하여 이러한 기능에 액세스할 수 있습니다. 아래의 화면 캡처에서 확인할 수 있습니다.

Adobe 문서 생성 Tagger의 ![고급 탭의 스크린샷](assets/legal_4.png)

이러한 고급 기능은 기본 태그와 다르지 않습니다. 조건부 논리를 포함하려면 문서에서 채울 부분을 선택합니다. 그런 다음 태그의 삽입을 결정하는 규칙을 구성합니다.

추가 설명을 위해 계약서에 조건부 섹션만 포함할 수 있습니다. 콘텐츠 형식 선택 필드에서 **섹션을 선택합니다.** 레코드 선택 필드에서 조건부 섹션이 표시되는지 여부를 결정하는 옵션을 선택합니다. 원하는 조건 연산자를 선택하고 값 필드에서 테스트할 값을 설정합니다. 그런 다음 **조건 삽입을 클릭합니다.** 아래 화면 캡처가 이 이 프로세스를 설명합니다.

![조건부 콘텐츠 삽입 스크린샷](assets/legal_5.png)

계산의 경우 산술 또는 합계를 선택한 다음 관련 첫 번째 레코드, 연산자 및 사용 가능한 템플릿 태그를 기반으로 사용할 두 번째 레코드를 포함합니다. 그런 다음 **계산 삽입**&#x200B;을 클릭합니다.

또한, 법률상 계약은 종종 관련 당사자의 서명을 필요로 한다. &quot;수치 계산&quot; 섹션 바로 아래에 있는 Adobe Sign 텍스트 태그를 사용하여 전자 서명을 삽입할 수 있습니다. 전자 서명을 포함하려면 수신자 수를 지정하고 **서명자**&#x200B;를 선택한 다음 드롭다운 목록에서 필드 유형을 지정해야 합니다. 완료되면 **Adobe Sign 텍스트 태그 삽입**&#x200B;을 클릭하여 프로세스를 마무리합니다.

데이터 무결성을 보장하려면 법적 문서를 보호된 형식으로 저장하십시오. [!DNL Acrobat Services] API를 사용하면 문서를 PDF 형식으로 빠르게 변환할 수 있습니다. 간단한 express Node.js 애플리케이션을 빌드하고 문서 생성 API를 통합 한 다음 이 간단한 애플리케이션을 사용하여 태그가 있는 문서를 Word에서 PDF 형식으로 변환할 수 있습니다.

## 프로젝트 설정

먼저 Node.js 애플리케이션에 대한 폴더 구조를 설정합니다. 이 예제에서는 이 간단한 응용 프로그램 AdobeLegalContractAPI를 호출합니다. 소스 코드 [여기](https://github.com/agavitalis/adobe_legal_contracts.git)를 검색할 수 있습니다.

### 디렉토리 구조

AdobeLegalContractAPI라는 폴더를 만들고 선택한 편집기에서 엽니다. 아래 폴더 구조를 사용하여 ```npm init``` 명령으로 기본 Node.js 애플리케이션을 만듭니다.

```
###Directory Structure
AdobeLegalContractAPI
-----config
----------default.json
-----controllers
----------createPDFController.js
----------previewController.js
-----models
----------document.js
-----routes
----------web.js
-----services
-----------upload.js
-----uploads
-----views
-----index.js
```

위의 내용은 응용 프로그램에 대한 간단한 Node.js 응용 프로그램 구조입니다. 이제 필요한 npm 패키지 설치를 진행합니다.

### 패키지 설치

아래 코드 조각에 표시된 대로 npm install 명령을 사용하여 필요한 패키지를 설치합니다.

```
npm install express body-parser morgan multer hbs path config mongoose
```

패키지를 설치한 후 package.json 파일의 내용이 아래 코드 조각과 같은지 확인하십시오.

```
###package.json
{
"name": "adobelegalcontractapi",
"version": "1.0.0",
"description": "",
"main": "index.js",
"directories": {
"test": "test"
},
"dependencies": {
"body-parser": "^1.19.0",
"config": "^3.3.6",
"express": "^4.17.1",
"hbs": "^4.1.1",
"mongoose": "^5.12.1",
"morgan": "^1.10.0",
"multer": "^1.4.2",
"path": "^0.12.7"
},
"devDependencies": {},
"scripts": {
"start": "node index.js"
},
"repository": {
"type": "git",
"url": "https://github.com/agavitalis/adobe_legal_contracts.git"
},
"author": "Ogbonna Vitalis",
"license": "ISC",
"bugs": {
"url": "https://github.com/agavitalis/adobe_legal_contracts/issues"
},
"homepage": "https://github.com/agavitalis/adobe_legal_contracts#readme"
}
```

이러한 코드 조각에서는 해당 뷰에 대한 핸들 템플릿 엔진을 포함하여 응용 프로그램 종속성을 설치했습니다.

이 자습서의 주요 초점은 [[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/)를 사용하여 문서를 PDF으로 변환하는 것입니다. 따라서 이 Node.js 애플리케이션을 빌드하는 방법에 대한 단계별 프로세스는 없습니다. 그러나 [GitHub](https://github.com/agavitalis/adobe_legal_contracts.git)에서 전체 작업 Node.js 응용 프로그램 코드를 검색할 수 있습니다.

## Node.js 애플리케이션에 [!DNL Adobe Acrobat Services] API 통합

[!DNL Adobe Acrobat Services] API는 문서를 원활하게 조작할 수 있도록 설계된 클라우드 기반의 신뢰할 수 있는 서비스입니다. 세 가지 API를 제공합니다.

* Adobe PDF Services API

* Adobe PDF 포함 API

* Adobe 문서 생성 API

[!DNL Acrobat Services] API를 사용하려면 자격 증명이 필요합니다(PDF Embed API 자격 증명과 다름). 유효한 자격 증명이 없는 경우 [등록](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK)하고 아래 화면 캡처에 나와 있는 대로 워크플로를 완료하십시오. [6개월 무료 체험판 및 선불 결제](https://developer.adobe.com/document-services/pricing/main)를 이용하세요. 문서 트랜잭션당 단 $0.05입니다.

![새 자격 증명을 만드는 스크린샷](assets/legal_6.png)

등록 프로세스가 완료되면 코드 샘플이 자동으로 PC에 다운로드되어 시작에 도움을 줍니다. 이 코드 샘플을 추출하고 따를 수 있습니다. 추출된 코드 샘플에서 pdftools-api-credentials.json 및 private.key 파일을 Node.js 프로젝트의 루트 디렉터리로 복사해야 합니다. [!DNL Acrobat Services] API 끝점에 액세스하려면 자격 증명이 필요합니다. 개인 설정된 자격 증명으로 SDK 샘플을 다운로드할 수도 있으므로 샘플 코드의 키를 업데이트할 필요가 없습니다.

이제 애플리케이션의 루트 디렉터리에서 터미널을 사용하여 ```npm install \--save @adobe/documentservices-pdftools-node-sdk``` 명령을 실행하여 Adobe PDF Services 노드 SDK를 설치합니다. 설치가 완료되면 [!DNL Acrobat Services] API를 사용하여 응용 프로그램에서 문서를 조작할 수 있습니다.

## PDF 문서 만들기

[!DNL Acrobat Services] API는 Microsoft Office 문서(Word, Excel, PowerPoint) 및 기타 [지원되는 파일 형식](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf)&#x200B;(예: .txt, .rtf, .bmp, .jpeg, gif, .tiff, .png)에서 PDF 만들기를 지원합니다. Acrobat 서비스 API를 사용하여 법적 약정을 다른 파일 형식에서 PDF으로 쉽게 변환할 수 있습니다.

Adobe 문서 생성 API를 사용하면 Word 파일 또는 PDF으로 변환할 수 있습니다. 예를 들어, Word 템플릿을 사용하여 계약서를 생성할 수 있습니다. 여기에는 편집된 텍스트를 표시하는 redlining도 포함됩니다. 그런 다음 암호를 PDF으로 변환하고 PDF 서비스 API를 사용하여 암호로 문서를 보호하고 서명을 위해 전송하는 등의 작업을 수행합니다.

사용 가능한 지원되는 파일 형식에서 PDF 문서 만들기를 구현하려면 [!DNL Acrobat Services]을(를) 사용하여 변형할 문서를 업로드하는 양식이 있습니다.

설계된 업로드 양식은 아래 화면 캡처에 표시되며 [GitHub](https://github.com/agavitalis/adobe_legal_contracts.git)에서 HTML 및 CSS 파일에 액세스할 수 있습니다.

![양식 업로드의 스크린샷](assets/legal_7.png)

이제 다음 코드 조각을 controllers /createPDFController.js 파일에 추가합니다. 이 코드는 업로드된 문서를 검색하고 이를 PDF으로 변환합니다. [!DNL Acrobat Services]은(는) 원래 업로드한 파일과 변형된 파일을 다른 폴더에 저장합니다.

```
###controllers/createPDFController.js
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
const Document = require('../models/document');
/*
* GET / route to show the createPDF form.
*/
function createPDF(req, res) {
//catch any response on the url
let response = req.query.response
res.render('index', { response })
}
/*
* POST /createPDF to create a new PDF File.
*/
function createPDFPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then(async(result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`
let newFilePath = require('path').resolve('./') + `\\output\\${newFileName}`
await result.saveAsFile(`views/output/${newFileName}`)
//Creates a new document
let newDocument = new Document({
documentName: newFileName,
url: newFilePath
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
}
else {
res.redirect('/?response=PDF Successfully created')
}
});
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

위의 코드 조각에는 이전에 설치한 문서 모델과 [!DNL Acrobat Services] 노드 SDK가 필요합니다. 두 가지 기능이 있습니다.

* createPDF에 문서 업로드 양식이 표시됩니다.

* createPDFPost는 업로드된 문서를 PDF으로 변환합니다.

이 기능은 변환된 PDF 문서를 보기/출력 디렉토리에 저장하며, 여기서 PC에 다운로드할 수 있습니다.

자유 PDF 임베드 API를 사용하여 변형된 PDF 파일을 미리 볼 수도 있습니다. PDF 포함 API를 사용하면 Adobe 자격 증명 [여기](https://www.adobe.com/go/dcsdks_credentials)&#x200B;(사용자의 [!DNL Acrobat Services] 자격 증명과 다름)을 생성하고 API에 액세스할 수 있는 허용된 도메인을 등록할 수 있습니다. 프로세스를 따르고 응용 프로그램에 대한 PDF Embed API 자격 증명을 생성합니다. 데모 [여기](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)를 확인할 수도 있습니다. 데모를 사용하면 코드를 쉽게 생성하여 빠르게 시작할 수 있습니다.

응용 프로그램으로 돌아가서 응용 프로그램의 보기 폴더에 list.hbs 및 preview.hbs 파일을 만들고 아래의 코드 조각을 list.hbs 및 preview.hbs 파일에 각각 붙여넣습니다.

```
###views/list.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>Adobe Legal Contract</title>
<!-- Meta tags -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,
initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<!-- //Meta tags -->
<link
href=".min.css" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/
bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
<link rel="stylesheet" href="css/style.css" type="text/css"
media="all" /><!-- Style-CSS -->
<link href="css/font-awesome.css" rel="stylesheet" /><!--
font-awesome-icons -->
</head>
<body>
<section>
<div class="form-36-mian section-gap">
<div class="wrapper">
<div class="container">
<div class="row">
{{#each documents}}
<div class="col-md-4 mb-2">
<div class="card" style="width:
18rem;">
<img class="card-img-top"
src="./images/pdf.png"
alt="Card image cap">
<div class="card-body">
<h5
class="card-title">{{documentName}}</h5>
<a
href="/downloadPDF/{{_id}}" class="btn btn-primary"><i class="fa
fa-download" aria-hidden="true"></i> Download</a>
<a
href="/previewPDF/{{_id}}" class="btn btn-info"><i class="fa fa-eye"
aria-hidden="true"></i> Preview</a>
</div>
</div>
</div>
{{/each}}
</div>
</div>
<!-- copyright -->
<div class="copy-right">
<p>(c) 2021 Vitalis</p>
</div>
<!-- //copyright -->
</div>
</div>
</section>
</body>
</html>
###views/preview.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>[!DNL Adobe Acrobat Services] PDF Embed API</title>
<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta id="viewport" name="viewport" content="width=device-width,
initial-scale=1" />
</head>
<body style="margin: 0px">
<input type="hidden" id="pdfDocumentName"
value={{document.documentName}} />
<input type="hidden" id="pdfDocumentUrl" value={{document.url}} />
<div id="adobe-dc-view"></div>
<script
src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
let pdfDocumentName =
document.getElementById("pdfDocumentName").value;
let pdfDocumentUrl =
document.getElementById("pdfDocumentUrl").value;
document.addEventListener("adobe_dc_view_sdk.ready", function
() {
var adobeDCView = new AdobeDC.View({ clientId:
"XXXXXXXXXXXXXXXX", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url:
`http://localhost:5000/output/${pdfDocumentName}` } },
metaData: { fileName: pdfDocumentName }
}, {});
});
</script>
</body>
</html>
```

또한 controller/previewController.js 파일을 만들고 아래에 있는 코드 조각을 붙여넣습니다.

```
const Document = require('../models/document');
/*
* GET /listFiles route to show PDF file lists.
*/
async function listFiles(req, res) {
let documents = await Document.find({});
res.render('lists', { documents })
}
/*
* GET /previewPDF route to show PDF file in AdobeEmbedAPI.
*/
async function previewPDF(req, res) {
//catch any response on the url
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.render('preview', { document })
}
/*
* GET /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(document.url);
}
//export all the functions
module.exports = {listFiles, previewPDF, downloadPDF };
```

위의 컨트롤러 파일에는 listFiles, previewPDF 및 downloadPDF 의 세 가지 기능이 있습니다. listFiles 함수는 [!DNL Acrobat Services] API를 사용하여 지금까지 생성된 모든 PDF 파일을 나열합니다. previewPDF 기능을 사용하면 PDF Embed API를 사용하여 PDF 파일을 미리 볼 수 있으며 downloadPDF 기능을 사용하면 생성된 PDF 파일을 PC에 다운로드할 수 있습니다. 아래 화면 캡처에서는 PDF Embed API를 사용한 PDF 미리보기 샘플을 보여 줍니다.

PDF 미리 보기의 ![스크린샷](assets/legal_8.png)

## 요약

이 실습용 튜토리얼에서는 문서 생성 Tagger Microsoft Word 추가 기능을 사용하여 문서에 태그를 지정했습니다. 그런 다음 [!DNL Acrobat Services] API를 Node.js 애플리케이션에 통합하고
PDF이 지정된 문서를 다운로드 가능한 PDF 형식으로 변환했지만, 사용자가 직접 태그를 지정하는 법적 계약서를 만들 수도 있었습니다. 마지막으로, Adobe PDF Embed API 를 사용하여 확인 및 서명을 위해 생성된 PDF을 미리 보았습니다.

완성된 응용 프로그램을 사용하면 동적 필드로 [법률 계약 템플릿](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts)에 태그를 지정하고, 이를 PDF으로 변환하고, 미리 보고, [!DNL Acrobat Services] API를 사용하여 서명하는 것이 훨씬 쉬워집니다. 팀은 고유한 계약을 만드는 데 시간을 들이지 않고 자동으로 각 고객에게 올바른 계약을 보내고 비즈니스를 성장시키는 데 더 많은 시간을 투자할 수 있습니다.

조직은 완벽하고 사용하기 쉽도록 [!DNL Adobe Acrobat Services]개의 API를 사용합니다. 무엇보다도 [6개월 무료 체험 후 선불 결제](https://developer.adobe.com/document-services/pricing/main)를 즐길 수 있습니다. 사용한 만큼만 지불하면 됩니다. 또한 PDF Embed API는 항상 무료입니다.

문서 흐름을 개선하여 생산성을 높일 준비가 되셨습니까? 지금 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html).
