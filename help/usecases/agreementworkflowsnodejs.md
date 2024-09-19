---
title: Node.js 의 계약 워크플로우
description: "[!DNL Adobe Acrobat Services] API는 웹 응용 프로그램에 PDF 기능을 쉽게 통합합니다."
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: 추천 항목
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: f8a31b8f98d99bf1f3787e0f0f19cc9f26e24d8d
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Node.js 의 계약 워크플로우

![사례 영웅 배너 사용](assets/UseCaseAgreementHero.jpg)

많은 비즈니스 애플리케이션과 프로세스에는 제안서 및 계약서와 같은 문서가 필요합니다. PDF 문서를 사용하면 파일을 보다 안전하게 수정할 수 있습니다. 또한 고객이 쉽고 빠르게 문서를 완료할 수 있도록 디지털 서명 지원을 제공합니다. [!DNL Adobe Acrobat Services] API는 웹 응용 프로그램에 PDF 기능을 쉽게 통합합니다.

## 학습 내용

이 실습 튜토리얼에서는 Node.js 애플리케이션에 PDF 서비스를 추가하여 계약 프로세스를 디지털화하는 방법을 살펴보세요.

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [프로젝트 코드](https://github.com/adobe/pdftools-node-sdk-samples)

## [!DNL Adobe Acrobat Services] 설정 중

시작하려면 [!DNL Adobe Acrobat Services]을(를) 사용하도록 자격 증명을 설정하십시오. 더 큰 응용 프로그램에 기능을 통합하기 전에 계정을 등록하고 [Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js)를 사용하여 자격 증명 작업을 확인하십시오.

먼저 Adobe 개발자 계정을 얻습니다. 그런 다음 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) 페이지의 새 자격 증명 만들기에서 *시작하기* 옵션을 선택합니다. 무료 체험판에 등록하면 6개월 동안 사용할 수 있는 1,000개의 문서 트랜잭션을 제공하는 무료 체험판을 사용할 수 있습니다.

![새 자격 증명을 만드는 이미지](assets/AWNjs_1.png)

다음 새 자격 증명 생성 페이지에서 PDF Embed API와 PDF 서비스 API 중에서 결정하라는 메시지가 표시됩니다.

*PDF 서비스 API*&#x200B;를 선택합니다.

응용 프로그램 이름을 입력하고 *개인화된 코드 샘플 만들기*&#x200B;라는 상자를 선택합니다. 이 상자를 선택하면 자격 증명이 코드 샘플에 자동으로 포함됩니다. 이 확인란을 선택하지 않은 상태로 두면 자격 증명을 애플리케이션에 수동으로 추가해야 합니다.

응용 프로그램 유형으로 *Node.js*&#x200B;을(를) 선택하고 *자격 증명 만들기*&#x200B;를 클릭합니다.

잠시 후에 .zip 파일이 자격 증명을 포함한 샘플 프로젝트와 함께 다운로드되기 시작합니다. [!DNL Acrobat Services]에 대한 Node.js 패키지가 이미 샘플 프로젝트 코드의 일부로 포함되어 있습니다.

![PDF 서비스 API 자격 증명 선택 이미지](assets/AWNjs_2.png)

## 수동으로 샘플 프로젝트 구성

새 자격 증명 생성 페이지에서 샘플 프로젝트를 다운로드하지 않도록 선택한 경우 프로젝트를 수동으로 설정할 수도 있습니다.

[GitHub](https://github.com/adobe/pdftools-node-sdk-samples)에서 자격 증명이 포함되지 않은 코드를 다운로드합니다. 이 버전의 코드를 사용하는 경우 사용하기 전에 pdftools-api-credentials.json 파일에 자격 증명을 추가해야 합니다.

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

자체 응용 프로그램의 경우 개인 키 파일과 자격 증명 파일을 응용 프로그램 원본에 복사해야 합니다.

[!DNL Acrobat Services]에 대한 Node.js 패키지를 설치해야 합니다. 패키지를 설치하려면 다음 명령을 사용하십시오.

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## 로깅 설정

이 예제에서는 응용 프로그램 프레임워크에 Express를 사용합니다. 또한 응용 프로그램 로깅에는 log4js를 사용합니다. log4js를 사용하면 콘솔에 대한 로깅 또는 파일 로그아웃을 쉽게 수행할 수 있습니다.

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

위 코드는 로깅된 데이터를 의 파일에 기록합니다./logs/applicationlog.txt. 대신 콘솔에 쓰도록 하려면 log4js.configure에 대한 호출을 주석 처리할 수 있습니다.

## Word 파일을 PDF으로 변환

계약서와 제안서는 Microsoft Word와 같은 생산성 응용 프로그램에 작성되는 경우가 많습니다. 이 형식의 문서를 수락하고 문서를 PDF으로 변환하려면 응용 프로그램에 기능을 추가할 수 있습니다. Express 애플리케이션에서 문서를 업로드하여 저장한 다음 이를 파일 시스템에 저장하는 방법을 살펴보겠습니다.

응용 프로그램의 HTML에서 파일 요소를 추가하고 업로드를 시작하기 위한 단추를 추가합니다.

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

페이지의 JavaScript에서 fetch 함수를 사용하여 파일을 비동기적으로 업로드합니다.

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

업로드된 파일을 수락할 폴더를 선택합니다. 응용 프로그램에는 이 폴더에 대한 경로가 필요합니다. \_\_dirname과 연결된 상대 경로를 사용하여 절대 경로를 찾습니다.

```
const uploadFolder = path.join(__dirname, "../uploads");
```

파일이 post를 통해 전송되므로 서버 측에서 post 메시지에 응답해야 합니다.

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

이 기능이 실행되면 파일은 애플리케이션 업로드 폴더에 저장되며 추가 처리를 위해 사용할 수 있습니다.

그런 다음 파일을 기본 형식에서 PDF 형식으로 변환합니다. 이전에 다운로드한 샘플 코드에는 문서를 PDF으로 변환하기 위한 `create-pdf-from-docx.js`이라는 스크립트가 포함되어 있습니다. 다음 함수 `convertDocumentToPDF`은(는) 업로드된 문서를 가져와 다른 폴더의 PDF으로 변환합니다.

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

코드에 일반적인 패턴이 있을 수 있습니다.

이 코드는 자격 증명 개체와 실행 컨텍스트를 빌드하고 일부 작업을 초기화한 다음 실행 컨텍스트와 함께 작업을 실행합니다. 샘플 코드 전체에서 이 패턴을 볼 수 있습니다.

업로드 기능에 몇 가지를 추가하여 이 기능을 호출하면 사용자가 업로드하는 Word 문서가 자동으로 PDF으로 변환됩니다.

다음 코드는 변환된 PDF의 대상 경로를 빌드하고 변환을 시작합니다.

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## 다른 파일 형식을 PDF으로 변환

문서 툴킷은 다른 형식을 다른 일반적인 문서 유형인 정적 PDF과 같은 HTML으로 변환합니다. 툴킷은 동일한 .zip 파일에서 문서에서 참조하는 모든 리소스(CSS 파일, 이미지 및 기타 파일)를 사용하여 .zip 파일로 패키지된 HTML 문서를 허용합니다. HTML 문서 자체의 이름은 index.html이어야 하며 .zip 파일의 루트에 배치되어야 합니다.

HTML을 포함하는 .zip 파일을 변환하려면 다음 코드를 사용합니다.

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

`setCustomOptions` 함수는 PDF 크기와 같은 다른 페이지 설정을 지정합니다. 여기에서 기능을 통해 페이지 크기를 11.5 x 11인치로 설정할 수 있습니다.

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

용어가 포함된 HTML 문서를 열면 브라우저에서 다음 항목이 표시됩니다.

![컴퓨터 용어 이미지](assets/AWNjs_3.png)

이 문서의 소스는 CSS 파일과 HTML 파일로 구성됩니다.

![CSS 이미지 및 HTML 파일](assets/AWNjs_4.png)

HTML 파일을 처리한 후 다음과 같은 PDF 형식의 동일한 텍스트가 있습니다.

![컴퓨터 약관 PDF 파일](assets/AWNjs_5.png)

## 페이지 추가

PDF 파일에 일반적인 또 다른 작업은 용어 목록과 같은 표준 텍스트가 있을 수 있는 페이지를 끝에 추가하는 것입니다. 문서 툴킷은 여러 PDF 문서를 단일 문서로 결합할 수 있습니다. 문서 경로 목록(여기 `sourceFileList`에 있음)이 있는 경우 결합 작업을 위해 각 파일의 파일 참조를 개체에 추가할 수 있습니다.

결합 작업이 실행되면 소스 내용의 단일 파일이 제공됩니다. 개체에서 `saveAsFile`을(를) 사용하여 파일을 저장소에 유지할 수 있습니다.

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## PDF 문서 표시

PDF 파일에 대해 여러 작업을 수행했지만, 궁극적으로는 사용자가 문서를 보아야 합니다. Adobe의 PDF Embed API를 사용하여 문서를 웹 페이지에 임베드할 수 있습니다.

PDF이 표시되는 페이지에서 문서를 보관할 `<div />` 요소를 추가하고 ID를 지정합니다. 이 ID는 곧 사용할 수 있습니다. 웹 페이지에 Adobe JavaScript 라이브러리에 대한 `<script />` 참조를 포함합니다.

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

필요한 코드의 마지막 비트는 Adobe PDF Embed API JavaScript가 로드되면 문서를 표시하는 함수입니다. adobe_dc_view\_sdk.ready 이벤트를 통해 스크립트가 로드되었다는 알림을 받으면 새 AdobeDC.View 개체를 만듭니다. 이 개체에는 클라이언트 ID와 이전에 만든 요소의 ID가 필요합니다. [Adobe Developer Console](https://console.adobe.io/)에서 클라이언트 ID를 찾습니다. 이전에 자격 증명을 생성할 때 생성한 애플리케이션의 설정을 보면 클라이언트 ID가 표시됩니다.

![API 클라이언트 키 이미지](assets/AWNjs_6.png)

## 기타 PDF 옵션

[Adobe PDF Embed API 데모](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)를 사용하면 PDF 문서 포함에 대한 기타 다양한 옵션을 미리 볼 수 있습니다.

![PDF 옵션 포함 이미지 ](assets/AWNjs_7.png)

다양한 옵션을 켜거나 끌 수 있으며 렌더링 방식을 즉시 확인할 수 있습니다. 원하는 조합을 찾으면 *\&lt;/\> 코드 생성* 버튼을 클릭하여 해당 옵션을 사용하여 실제 HTML 코드를 생성합니다.

![코드 미리 보기 이미지](assets/AWNjs_8.png)

## 디지털 서명 및 보안 추가

문서가 준비되면 Adobe Sign에서 승인을 위해 디지털 서명을 추가할 수 있습니다. 이 기능은 지금까지 사용하던 기능과 약간 다르게 작동합니다. 디지털 서명의 경우 사용자 인증에 OAuth를 사용하도록 애플리케이션을 구성해야 합니다.

응용 프로그램을 설정하는 첫 번째 단계는 [응용 프로그램을 등록](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md)하여 Adobe Sign용 OAuth를 사용하는 것입니다. 로그인하고 나면 *계정*&#x200B;을 클릭하여 응용 프로그램을 만드는 화면으로 이동한 다음 *Adobe Sign API* 섹션을 열고 *API 응용 프로그램*&#x200B;을 클릭하여 등록된 응용 프로그램 목록을 엽니다.

![응용 프로그램 등록의 첫 번째 단계 이미지](assets/AWNjs_9.png)

새 응용 프로그램 항목을 만들려면 오른쪽 상단에 있는 더하기 아이콘을 클릭합니다.

![화면의 오른쪽 위 모서리에 있는 더하기 아이콘 이미지](assets/AWNjs_10.png)

창이 열리면 응용 프로그램 이름과 표시 이름을 입력합니다. 도메인에 대해 *고객*&#x200B;을 선택한 다음 *저장*&#x200B;을 클릭하세요.

![응용 프로그램 이름과 표시 이름을 입력할 위치 이미지](assets/AWNjs_11.png)

응용 프로그램을 만든 후 목록에서 응용 프로그램을 선택하고 *응용 프로그램에 대한 OAuth 구성*&#x200B;을 클릭합니다. 옵션을 선택합니다. 리디렉션 URL에서 응용 프로그램의 URL을 입력합니다. 여기에 URL을 여러 개 입력할 수 있습니다. 테스트 중인 애플리케이션의 경우 값은 다음과 같습니다.

```
http://localhost:3000/signed-in 
```

OAuth를 사용하여 토큰을 가져오는 프로세스는 표준입니다. 응용 프로그램에서 사용자에게 로그인할 URL을 지정합니다. 사용자가 성공적으로 로그인한 후,
페이지의 질의 매개변수에 추가 정보가 있는 응용 프로그램으로 다시 리디렉션됩니다.

로그인 URL의 경우 애플리케이션은 클라이언트 ID, 리디렉션 URL 및 필요한 범위 목록을 전달해야 합니다.

URL의 패턴은 다음과 같습니다.

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

사용자에게 Adobe Sign의 ID에 로그인하라는 메시지가 표시됩니다. 로그인 후 애플리케이션에 권한을 제공할 것인지 묻는 메시지가 표시됩니다.

![액세스 확인 화면 이미지](assets/AWNjs_12.png)

사용자가 리디렉션 URL에서 *액세스 허용*&#x200B;을 클릭하면 코드라는 쿼리 매개 변수가 인증 코드를 전달합니다.

https://YourServer.com/?code=**\&lt;authorization_code\>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

이 코드를 클라이언트 ID 및 클라이언트 암호와 함께 Adobe Sign 서버에 게시하면 서비스에 액세스할 수 있는 액세스 토큰이 제공됩니다. `api_access_point` 및 `web_access_point` 매개 변수에 값을 저장합니다. 이러한 값은 추가 요청에 사용됩니다.

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

문서에 서명이 필요한 경우 먼저 문서를 업로드해야 합니다. 응용 프로그램에서 OAUTH 토큰을 요청하는 동안 받은 `api_access_point` 값에 문서를 업로드할 수 있습니다. 끝점은 `/api/rest/v6/transientDocuments`입니다. 요청 데이터는 다음과 같이 표시됩니다.

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

응용 프로그램 내에서 다음 코드를 사용하여 요청을 빌드합니다.

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

요청에서 `transientID` 값을 반환합니다. 문서가 업로드되었지만 아직 전송되지 않았습니다. 문서를 보내려면 `transientID`을(를) 사용하여 문서 보내기를 요청하십시오.

먼저 서명할 문서에 대한 정보가 포함된 JSON 오브젝트를 빌드합니다. 다음에서 변수 `transientDocumentId`에는 위 코드의 ID가 포함되어 있고 `agreementDescription`에는 서명이 필요한 계약을 설명하는 텍스트가 포함되어 있습니다. 문서에 서명할 사람이 전자 메일 주소와 역할별로 `participantSetsInfo`에 나열됩니다.

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

이 웹 요청을 전송하면 서명 요청이 작성되고 계약 요청에 대한 ID가 있는 JSON 개체가 반환됩니다.

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

서명자가 서명을 잊어 다른 알림 이메일이 필요한 경우 이전에 받은 ID를 사용하여 알림을 다시 보냅니다. 유일한 차이점은 당사자의 참가자 ID도 추가해야 한다는 것입니다. `/agreements/{agreementID}/members`에게 GET 요청을 보내 참가자 ID를 가져올 수 있습니다.

알림 메시지 전송을 요청하려면 먼저 요청을 설명하는 JSON 오브젝트를 빌드합니다. 최소 개체에는 참가자 ID 목록과 미리 알림 상태(&quot;ACTIVE&quot;, &quot;COMPLETE&quot; 또는 &quot;CANCELLED&quot;)가 필요합니다.

요청은 선택적으로 사용자에게 표시될 &quot;메모&quot;에 대한 값과 같은 추가 정보를 가질 수 있습니다. 또는 미리 알림을 보낼 때까지 기다리는 지연 시간(시간)(`firstReminderDelay`)과 미리 알림 빈도(필드 &quot;빈도&quot;)를 지정합니다. 이 빈도는 DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED 또는 WEEKLY_UNTIL_SIGNED 등의 값을 허용합니다.

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

알림 메시지 요청만 보내면 됩니다.

![웹 양식의 이미지](assets/AWNjs_13.png)

## 웹 양식 만들기

Adobe Sign API를 사용하여 웹 양식을 만들 수도 있습니다. 웹 양식 을 사용하면 웹 페이지 내에 양식을 포함하거나 직접 링크할 수 있습니다. 웹 양식이 생성되면 Adobe Sign 콘솔의 웹 양식 간에도 표시됩니다. 증분 작성을 위한 초안 상태, 웹 양식 필드를 편집하기 위한 작성 상태 및 양식을 즉시 호스팅하기 위한 활성 상태의 웹 양식을 만들 수 있습니다.

Adobe Sign 관리 화면의 ![웹 양식 이미지](assets/AWNjs_14.png)

웹 양식을 만들려면 `transientDocumentId` 양식을 사용하십시오. 양식의 제목과 양식 초기화 상태를 결정합니다.

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

이제 문서를 임베드하거나 문서에 연결할 수 있습니다.

## 다음 단계

빠른 시작과 제공된 코드에서 볼 수 있듯이 [!DNL Adobe Acrobat Services] API를 사용하여 노드를 사용하여 PDF 및 디지털 문서 승인 프로세스를 쉽게 구현할 수 있습니다. Adobe의 API는 기존 클라이언트 애플리케이션에 원활하게 통합됩니다.

호출에 필요한 범위를 확인하거나 호출이 빌드되는 방법을 보려면 [Rest API 설명서](https://secure.na4.adobesign.com/public/docs/restapi/v6)에서 샘플 호출을 빌드할 수 있습니다. [빠른 시작](https://github.com/adobe/pdftools-node-sdk-samples)에서는 [!DNL Adobe Acrobat Services] API 프로세스의 다른 기능과 파일 형식도 보여 줍니다.

애플리케이션에 다양한 PDF 기능을 추가하여 사용자들이 자신의 문서를 빠르고 쉽게 보고 서명할 수 있도록 할 수 있습니다. 시작하려면 오늘 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/)을(를) 확인하십시오.
