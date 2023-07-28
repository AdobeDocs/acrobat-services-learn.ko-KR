---
title: Node.js의 계약 작업 과정
description: "[!DNL Adobe Acrobat Services] API는 PDF 기능을 웹 애플리케이션에 쉽게 통합합니다."
type: Tutorial
role: Developer
level: Beginner
feature: Use Cases
thumbnail: KT-7473.jpg
jira: KT-7473
keywords: 추천
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 1%

---

# Node.js의 계약 작업 과정

![사례 메인 배너 사용](assets/UseCaseAgreementHero.jpg)

많은 비즈니스 애플리케이션 및 프로세스에는 제안서 및 계약서와 같은 문서가 필요합니다. PDF 문서를 사용하면 파일을 보다 안전하게 수정할 수 있습니다. 또한 디지털 서명 지원도 제공하므로 고객은 문서를 빠르고 손쉽게 작성할 수 있습니다. [!DNL Adobe Acrobat Services] API는 PDF 기능을 웹 애플리케이션에 쉽게 통합합니다.

## 학습 내용

이 실습 튜토리얼에서는 Node.js 애플리케이션에 PDF 서비스를 추가하여 계약 프로세스를 디지털화하는 방법을 살펴봅니다.

## 관련 API 및 리소스

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [프로젝트 코드](https://github.com/adobe/pdftools-node-sdk-samples)

## 설정 [!DNL Adobe Acrobat Services]

시작하려면 사용할 자격 증명을 설정하십시오. [!DNL Adobe Acrobat Services]. 계정을 등록하고 [Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) 더 큰 응용 프로그램에 기능을 통합하기 전에 자격 증명이 작동하는지 확인합니다.

먼저 Adobe 개발자 계정을 가져옵니다. 그런 다음 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) 페이지에서 *시작하기* 옵션을 선택합니다. 6개월 동안 사용할 수 있는 1,000개의 문서 트랜잭션을 제공하는 무료 평가판에 등록할 수 있습니다.

![새 자격 증명 만들기 이미지](assets/AWNjs_1.png)

다음 새 자격 증명 만들기 페이지에서 PDF 포함 API와 PDF 서비스 API 간에 결정하라는 메시지가 표시됩니다.

선택 *PDF 서비스 API*.

응용 프로그램의 이름을 입력하고 레이블이 지정된 상자를 선택합니다 *개인화된 코드 샘플 만들기*. 이 상자를 선택하면 코드 샘플에 자격 증명이 자동으로 포함됩니다. 이 확인란을 선택하지 않은 상태로 두면 자격 증명을 응용 프로그램에 수동으로 추가해야 합니다.

선택 *Node.js* 를 클릭하고 *자격 증명 만들기*.

잠시 후 .zip 파일이 자격 증명을 포함한 샘플 프로젝트로 다운로드되기 시작합니다. 의 Node.js 패키지 [!DNL Acrobat Services] 샘플 프로젝트 코드의 일부로 이미 포함되어 있습니다.

![PDF 서비스 API 인증서 선택 이미지](assets/AWNjs_2.png)

## 샘플 프로젝트 수동 구성

새 자격 증명 만들기 페이지에서 샘플 프로젝트를 다운로드하지 않도록 선택한 경우 프로젝트를 수동으로 설정할 수도 있습니다.

자격 증명 포함 안 함 [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). 이 버전의 코드를 사용하는 경우 사용하기 전에 pdftools-api-credentials.json 파일에 자격 증명을 추가해야 합니다.

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

응용 프로그램의 경우 개인 키 파일 및 자격 증명 파일을 응용 프로그램 소스에 복사해야 합니다.

에 대한 Node.js 패키지를 설치해야 합니다. [!DNL Acrobat Services]. 패키지를 설치하려면 다음 명령을 사용하십시오.

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## 로깅 설정

여기서 샘플은 응용 프로그램 프레임워크에 Express를 사용합니다. 또한 응용 프로그램 로깅에 log4js를 사용합니다. log4js를 사용하면 콘솔에 대한 로깅을 쉽게 지시하거나 파일에 대한 로깅을 수행할 수 있습니다.

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

위의 코드에서는에서 로그에 기록된 데이터를 파일에 씁니다./logs/applicationlog.txt을 참조하십시오. 대신 콘솔에 쓰도록 하려면 log4js.configure에 대한 호출을 주석 처리할 수 있습니다.

## Word 파일을 PDF으로 변환

계약 및 제안서는 Microsoft Word와 같은 생산성 응용 프로그램에 작성하는 경우가 많습니다. 이 형식의 문서를 수락하고 문서를 PDF으로 변환하려면 응용 프로그램에 기능을 추가할 수 있습니다. 이제 Express 애플리케이션에서 문서를 업로드하고 저장한 다음 파일 시스템에 저장하는 방법을 살펴보겠습니다.

응용 프로그램의 HTML에서 파일 요소와 업로드 시작 단추를 추가합니다.

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

업로드된 파일을 수락할 폴더를 선택합니다. 응용 프로그램에 이 폴더의 경로가 필요합니다. \_\_dirname과 연결된 상대 경로를 사용하여 절대 경로를 찾습니다.

```
const uploadFolder = path.join(__dirname, "../uploads");
```

파일이 post를 통해 전송되므로 서버 측의 post 메시지에 응답해야 합니다.

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

이 함수가 실행되면 파일이 응용 프로그램 업로드 폴더에 저장되고 추가 처리에 사용할 수 있습니다.

다음으로 파일을 기본 포맷에서 PDF 포맷으로 변환합니다. 이전에 다운로드한 샘플 코드에는 `create-pdf-from-docx.js` 문서를 PDF으로 변환하는 데 사용합니다. 다음 함수, `convertDocumentToPDF`에서 업로드된 문서를 가져와서 다른 폴더의 PDF으로 변환합니다.

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

다음과 같은 일반 패턴이 코드에 표시될 수 있습니다.

코드에서는 자격 증명 개체와 실행 컨텍스트를 빌드하고 일부 작업을 초기화한 다음 실행 컨텍스트로 작업을 실행합니다. 샘플 코드 전체에서 이 패턴을 볼 수 있습니다.

이 함수가 호출되도록 업로드 함수에 몇 가지를 추가하면, 이제 사용자가 업로드하는 Word 문서가 자동으로 PDF으로 변환됩니다.

다음 코드에서는 변환된 PDF의 대상 경로를 빌드하고 변환을 시작합니다.

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

## 다른 파일 유형을 PDF으로 변환

문서 툴킷은 다른 형식을 정적 PDF 같은 다른 일반적인 문서 형식인 HTML으로 변환합니다. 툴킷은 동일한 .zip 파일에 있는 문서에서 참조하는 모든 리소스(CSS 파일, 이미지 및 기타 파일)를 포함하는 .zip 파일로 패키징된 HTML 문서를 수락합니다. HTML 문서 자체의 이름은 index.html이어야 하며 .zip 파일의 루트에 배치되어야 합니다.

HTML이 포함된 .zip 파일을 변환하려면 다음 코드를 사용하십시오.

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

함수 `setCustomOptions` 페이지 크기와 같은 다른 PDF 설정을 지정합니다. 여기에서 이 함수는 페이지 크기를 11.5 x 11인치로 설정합니다.

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

몇 가지 용어가 포함된 HTML 문서를 열면 브라우저 내에서 다음과 같은 메시지가 표시됩니다.

![컴퓨터 용어 이미지](assets/AWNjs_3.png)

이 문서의 소스는 CSS 파일과 HTML 파일로 구성되어 있습니다.

![CSS 및 HTML 파일 이미지](assets/AWNjs_4.png)

HTML 파일을 처리한 후 동일한 텍스트가 PDF 형식으로 표시됩니다.

![컴퓨터 약관의 PDF 파일](assets/AWNjs_5.png)

## 페이지 추가

PDF 파일을 사용하는 또 다른 일반적인 작업에는 용어 목록과 같은 표준 텍스트가 있을 수 있는 페이지가 끝에 추가됩니다. 문서 툴킷은 여러 PDF 문서를 단일 문서로 결합할 수 있습니다. 문서 경로 목록이 있는 경우 `sourceFileList`), 결합 작업을 위해 각 파일의 파일 참조를 객체에 추가할 수 있습니다.

결합 작업이 실행되면 소스 내용이 포함된 단일 파일이 제공됩니다. 사용할 수 있습니다. `saveAsFile` 파일을 저장소에 보관합니다.

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

PDF 파일에 대해 여러 작업을 수행했지만 결국 사용자가 문서를 확인해야 합니다. Adobe의 PDF 포함 API를 사용하여 웹 페이지에 문서를 포함할 수 있습니다.

PDF이 표시된 페이지에서 `<div />` 요소를 추가합니다. 이 아이디는 곧 사용하실 수 있습니다. 웹 페이지에 `<script />` Adobe JavaScript 라이브러리에 대한 참조:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

필요한 코드의 마지막 비트는 Adobe PDF Embed API JavaScript가 로드되면 문서를 표시하는 함수입니다. adobe_dc_view\_sdk.ready 이벤트를 통해 스크립트가 로드된다는 알림을 받으면 새 AdobeDC.View 객체를 만듭니다. 이 개체에는 클라이언트 ID와 이전에 만든 요소의 ID가 필요합니다. 다음에서 클라이언트 ID를 찾습니다. [Adobe Developer 콘솔](https://console.adobe.io/). 이전에 자격 증명을 생성할 때 만든 응용 프로그램의 설정을 보면 클라이언트 ID가 표시됩니다.

![API 클라이언트 키 이미지](assets/AWNjs_6.png)

## 기타 PDF 옵션

추가 [Adobe PDF Embed API 데모](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) PDF 문서를 포함하기 위한 다른 다양한 옵션을 미리 볼 수 있습니다.

![포함 PDF 옵션 이미지 ](assets/AWNjs_7.png)

다양한 옵션을 켜거나 끄고 렌더링 방법을 즉시 확인할 수 있습니다. 원하는 조합을 찾으면 *\&lt;/\> 코드 생성* 단추를 눌러 이러한 옵션을 사용하여 실제 HTML 코드를 생성합니다.

![코드 미리 보기 이미지](assets/AWNjs_8.png)

## 디지털 서명 및 보안 추가

문서가 준비되면 Adobe Sign을 사용하여 승인을 위해 디지털 서명에 추가할 수 있습니다. 이 기능은 지금까지 사용한 기능과 약간 다르게 작동합니다. 디지털 서명의 경우 사용자 인증에 OAuth를 사용하도록 응용 프로그램을 구성해야 합니다.

응용 프로그램을 설정하는 첫 번째 단계는 [응용 프로그램 등록](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) Adobe Sign에 OAuth를 사용합니다. 로그인하고 나면 *계정*&#x200B;를 연 다음 *Adobe Sign API* 섹션을 클릭하고 *API 응용 프로그램* 을 눌러 등록된 응용 프로그램 목록을 엽니다.

![응용 프로그램 등록 첫 단계 이미지](assets/AWNjs_9.png)

새 응용 프로그램 항목을 만들려면 오른쪽 상단에 있는 더하기 아이콘을 클릭합니다.

![화면 오른쪽 상단에 있는 더하기 아이콘 이미지](assets/AWNjs_10.png)

열린 창에서 응용 프로그램 이름과 표시 이름을 입력합니다. 선택 *고객* 도메인에 대해 를 클릭한 다음 *저장*.

![응용 프로그램 이름 및 표시 이름을 입력할 위치 이미지](assets/AWNjs_11.png)

응용 프로그램을 만든 후 목록에서 선택하고 *응용 프로그램에 대한 OAuth 구성*. 옵션을 선택합니다. 리디렉션 URL에 응용 프로그램의 URL을 입력합니다. 여기에 여러 URL을 입력할 수 있습니다. 테스트하려는 응용 프로그램의 경우 값은 다음과 같습니다.

```
http://localhost:3000/signed-in 
```

토큰을 획득하기 위해 OAuth를 사용하는 프로세스는 표준입니다. 응용 프로그램은 사용자에게 로그인을 위한 URL을 안내합니다. 사용자가 성공적으로 로그인하면 페이지의 쿼리 매개 변수에 추가 정보가 포함된 응용 프로그램으로 다시 리디렉션됩니다.

로그인 URL의 경우 응용 프로그램이 클라이언트 ID, 리디렉션 URL 및 필요한 범위 목록을 전달해야 합니다.

URL의 패턴은 다음과 같습니다.

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

사용자에게 Adobe Sign ID에 로그인하라는 메시지가 표시됩니다. 로그인 후 사용자에게 응용 프로그램에 권한을 제공할지 여부를 묻는 메시지가 표시됩니다.

![액세스 확인 화면 이미지](assets/AWNjs_12.png)

사용자가 *액세스 허용* 리디렉션 URL에서 code라는 쿼리 매개 변수가 인증 코드를 전달합니다.

https://YourServer.com/?code=**\&lt;authorization_code>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

이 코드를 클라이언트 ID 및 클라이언트 시크릿과 함께 Adobe Sign 서버에 게시하면 서비스에 액세스하기 위한 액세스 토큰이 제공됩니다. 매개 변수에 값 저장 `api_access_point` 및 `web_access_point`. 이 값은 추가 요청에 사용됩니다.

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

문서에 서명이 필요한 경우 먼저 문서를 업로드해야 합니다. 응용 프로그램에서 문서를 `api_access_point` oauth 토큰을 요청하는 동안 받은 값입니다. 끝점은 `/api/rest/v6/transientDocuments`. 요청 데이터는 다음과 같습니다.

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

요청에서 `transientID` 값입니다. 문서가 업로드되었지만 아직 전송되지 않았습니다. 문서를 보내려면 `transientID` 문서를 보내달라고 요청합니다.

서명할 문서에 대한 정보가 포함된 JSON 개체를 만들어 시작합니다. 다음에서 변수는 `transientDocumentId` 위의 코드에서 가져온 ID를 포함하며 `agreementDescription` 이 파일에는 서명이 필요한 계약에 대해 설명하는 텍스트가 포함되어 있습니다. 문서에 서명할 인물이 `participantSetsInfo` 전자 메일 주소 및 역할별.

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

이 웹 요청을 보내면 서명 요청이 빌드되고 계약 요청에 대한 ID를 포함하는 JSON 개체가 반환됩니다.

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

서명자가 서명을 잊어버리고 다른 알림 전자 메일이 필요한 경우 이전에 받은 ID를 사용하여 알림을 다시 보냅니다. 유일한 차이점은 당사자의 참가자 ID도 추가해야 한다는 것입니다. GET 요청을 보내 참가자 ID를 받을 수 있습니다. `/agreements/{agreementID}/members`.

알림 메시지 전송을 요청하려면 먼저 요청을 설명하는 JSON 개체를 빌드합니다. 최소 개체에는 참가자 ID 목록과 미리 알림 상태(&quot;ACTIVE&quot;, &quot;COMPLETE&quot; 또는 &quot;CANCELLED&quot;)가 필요합니다.

요청에는 사용자에게 표시할 &quot;메모&quot; 값과 같은 추가 정보가 선택적으로 포함될 수 있습니다. 또는 미리 알림을 보낼 때까지 기다리는 지연(시간)(초) `firstReminderDelay`) 및 미리 알림 빈도(&quot;빈도&quot; 필드에 있음)를 지정합니다. 이 빈도에는 DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED 또는 WEEKLY_UNTIL_SIGNED 등의 값을 사용할 수 있습니다.

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

알림 요청을 보낼 때는 이게 전부입니다.

![웹 양식 이미지](assets/AWNjs_13.png)

## 웹 양식 만들기

Adobe Sign API를 사용하여 웹 양식을 만들 수도 있습니다. 웹 양식 을 사용하면 웹 페이지에 양식을 포함하거나 양식에 직접 연결할 수 있습니다. 웹 양식이 생성되면 Adobe Sign 콘솔의 웹 양식 간에도 표시됩니다. 증분 작성을 위한 초안 상태, 웹 양식 필드를 편집하기 위한 작성 상태 및 활성 상태를 사용하여 웹 양식을 즉시 호스팅할 수 있습니다.

![Adobe Sign 관리 화면의 웹 양식 이미지](assets/AWNjs_14.png)

웹 양식을 만들려면 양식을 사용합니다 `transientDocumentId`. 양식의 제목과 초기화 상태를 결정합니다.

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

이제 문서를 포함하거나 연결할 수 있습니다.

## 다음 단계

빠른 시작과 제공된 코드에서 볼 수 있듯이, Node와 함께 [!DNL Adobe Acrobat Services] API Adobe의 API는 기존 클라이언트 애플리케이션에 원활하게 통합됩니다.

호출에 필요한 범위를 검색하거나 호출이 어떻게 구성되는지 확인하려면 [Rest API 설명서](https://secure.na4.adobesign.com/public/docs/restapi/v6). 추가 [빠른 시작](https://github.com/adobe/pdftools-node-sdk-samples) 또한 다른 기능과 파일 형식을 보여 줍니다. [!DNL Adobe Acrobat Services] APIs 프로세스

응용 프로그램에 다양한 PDF 기능을 추가하여 사용자가 문서를 빠르고 쉽게 보고 서명할 수 있도록 할 수 있습니다. 시작하려면 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) 미래
