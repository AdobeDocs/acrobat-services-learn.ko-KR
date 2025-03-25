---
title: 직원 제안서 관리
description: 서명을 위해 신규 직원에게 전달할 수 있는 오퍼 레터를 생성하는 방법에 대해 알아봅니다.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8096
thumbnail: KT-8096.jpg
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1714'
ht-degree: 0%

---

# 직원 제안서 관리

![사례 영웅 배너 사용](assets/UseCaseOfferHero.jpg)

직원 오퍼 편지는 직원이 귀사에서 처음으로 경험한 내용 중 하나입니다. 따라서 구인 문자가 온브랜드인지 확인해야 하지만 매번 워드 프로세서에서 문자를 처음부터 작성할 필요는 없습니다. [!DNL Adobe Acrobat Services] API는 [신입 사원에게 제안서 작성 및 전달](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)의 주요 부분을 처리하는 빠르고 쉽고 효과적인 방법을 제공합니다.

## 학습 내용

이 실습형 튜토리얼에서는 사용자가 직원 세부 정보를 채울 수 있는 웹 양식을 표시하는 Node Express 프로젝트 설정을 살펴봅니다. 이러한 세부 정보는 웹을 통해 [!DNL Acrobat Services]을(를) 사용하여 Adobe Sign API를 사용하여 서명을 위해 고객에게 전달할 수 있는 PDF으로 오퍼 레터를 생성합니다.

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe 문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [문서 생성 Tagger Word 추가 기능](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)

* [프로젝트 샘플](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)

## 시작하기

[Node.js](https://nodejs.org/)은(는) 프로그래밍 플랫폼입니다. Express 웹 서버와 같은 방대한 라이브러리 세트가 함께 제공됩니다. [Node.js](https://nodejs.org/en/download/)을(를) 다운로드하고 단계에 따라 이 멋진 오픈 소스 개발 환경을 설치하십시오.

Node.js에서 Adobe 문서 생성 API를 사용하려면 [문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation) 사이트로 이동하여 계정에 액세스하거나 새 계정을 등록하십시오. 귀하의 계정은 문서 트랜잭션당 단 $0.05에 대해 6개월 동안 [무료 다음 사용한 만큼 지불](https://developer.adobe.com/document-services/pricing/main)되므로 안심하고 사용해 보고 회사가 성장하고 있을 때만 결제할 수 있습니다.

[Adobe Developer Console](https://developer.adobe.com/console/)에 로그인한 후 **[!UICONTROL 새 프로젝트 만들기]**&#x200B;를 클릭합니다. 기본적으로 프로젝트의 이름은 &quot;Project 1&quot;로 지정됩니다. **[!UICONTROL 프로젝트 편집]** 단추를 클릭하고 이름을 &quot;제공 편지 생성기&quot;로 변경합니다. 화면 중앙에는 **[!UICONTROL 새 프로젝트 시작하기]** 섹션이 있습니다. 프로젝트에서 보안을 활성화하려면 다음 단계를 수행하십시오.

**API 추가**&#x200B;를 클릭합니다. 선택할 수 있는 여러 API가 표시됩니다. **[!UICONTROL 제품별 필터링]** 섹션에서 **[!UICONTROL Document Cloud]**&#x200B;를 선택한 다음 **[!UICONTROL 다음]**&#x200B;을 클릭합니다.

이제 자격 증명을 생성하여 API에 액세스합니다. 자격 증명은 보안 통신을 위한 개방형 표준인 JSON 웹 토큰([JWT](https://jwt.io/)) 형식입니다. JWT에 익숙하고 이미 키를 생성한 경우에는 여기에 공개 키를 업로드할 수 있습니다. 또는 Adobe이 키를 생성하도록 하려면 **옵션 1**&#x200B;을 선택하여 계속 진행하십시오.

![자격 증명 생성 스크린샷](assets/offer_1.png)

**[!UICONTROL 키 쌍 생성]** 단추를 클릭합니다. 다운로드할 config.zip 파일이 있습니다. 아카이브 파일의 압축을 풉니다. 여기에는 certificate_pub.crt와 private.key라는 두 개의 파일이 포함됩니다. 후자의 경우 개인 자격 증명이 포함되어 있으며 사용자가 통제할 수 없는 경우 허위 문서를 생성하는 데 사용될 수 있으므로 보안을 유지해야 합니다.

**[!UICONTROL 다음]**&#x200B;을 클릭합니다. 아니요. PDF 생성 API에 액세스할 수 있도록 합니다. **[!UICONTROL 제품 프로필 선택]** 화면에서 **[!UICONTROL Enterprise PDF 서비스 개발자]**&#x200B;를 선택하고 **[!UICONTROL 구성된 API 저장]** 단추를 클릭합니다. 이제 API를 사용할 준비가 되었습니다.

## 프로젝트 설정

코드를 실행할 노드 프로젝트를 설정합니다. 이 예제에서는 [Visual Studio 코드](https://code.visualstudio.com/)&#x200B;(VS 코드)을(를) 편집기로 사용합니다. &quot;letter-generator&quot;라는 폴더를 만들고 VS 코드에서 엽니다. **[!UICONTROL 파일]** 메뉴에서 **[!UICONTROL 터미널]** \> **[!UICONTROL 새 터미널]**&#x200B;을 선택하여 이 폴더에서 셸을 엽니다. 다음을 입력하여 노드가 설치되어 있고 경로에 있는지 확인합니다.

```
node -v
```

설치한 노드의 버전이 표시됩니다.

이제 개발 환경을 설치했으므로 프로젝트를 계속 진행할 수 있습니다.

먼저 npm(노드 패키지 관리자)을 사용하여 프로젝트를 초기화합니다. 다음을 입력합니다.

```
npm init
```

노드 프로젝트에 대한 몇 가지 질문이 있습니다. 이러한 질문은 대부분 건너뛸 수 있지만 프로젝트 이름이 &quot;letter-generator&quot;이고 진입점이 **index.js**&#x200B;인지 확인하십시오. 프로젝트 초기화를 완료하려면 **예**&#x200B;를 선택하십시오.

이제 package.json 파일이 있습니다. 노드는 이 파일을 사용하여 프로젝트를 구성합니다. index.js를 만들기 전에 다음을 사용하여 Adobe 라이브러리를 추가해야 합니다
명령:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

프로젝트에 node_modules 라는 새 폴더가 추가되어야 합니다. 이 폴더에서는 모든 라이브러리(노드의 종속성)가 다운로드됩니다. package.json 파일도 Adobe PDF Services에 대한 참조로 업데이트됩니다.

이제 간단한 웹 프레임워크로 Express를 설치하려고 합니다. 다음 명령을 입력하십시오

```
npm install express –save
```

이전과 마찬가지로 package.json의 종속성 섹션이 그에 따라 업데이트됩니다.

## 제안서 템플리트 생성

이제 프로젝트 루트에서 &quot;app.js&quot;라는 파일을 만듭니다. 다음 시작 코드를 삽입해 보겠습니다.

```
const express = require('express');
const bodyParser = require('body-parser');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk')
const path = require('path');
const app = express();
const port = 8000;
app.use(bodyParser.urlencoded({ extended: true }));
app.get('/', (req, res) => {
res.sendFile(path.join(__dirname + '/index.html'));
});
app.post('/', (req, res) => {
console.log('Got body:', req.body);
res.sendStatus(200);
});
app.listen(port, () => {
console.log(`Candidate offer letter app listening on port ${port}!`)
});
```

get 경로가 **index.html** 파일을 반환합니다. 그러면 해당 이름과 다음의 간단한 양식을 사용하여 HTML 파일을 만들어 보겠습니다. CSS 스타일 및 기타 디자인 요소는 나중에 필요에 따라 추가할 수 있습니다. 이 양식은 환영 편지를 생성하기 위한 후보자의 기본 세부 사항을 채택합니다.

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Offer Letter Generator</title>
</head>
<body>
<h1>Enter Candidate Details</h1>
<form action="" method="post">
<div>
<label for="firstname">First name: </label>
<input type="text" name="firstname" id="firstname" required>
</div>
<div>
<label for="lastname">Last name: </label>
<input type="text" name="lastname" id="lastname" required>
</div>
<div>
<label for="salary">Salary ($): </label>
<input type="number" name="salary" id="salary" required>
</div>
<div>
<label for="startdate">Start Date: </label>
<input type="date" name="startdate" id="startdate" required>
</div>
<div>
<input type="submit" value="Generate Letter">
</div>
</form>
</body>
</html>
```

다음 명령으로 웹 서버를 실행합니다.

```
node app.js
```

&quot;포트 8000에서 수신 대기 중인 후보 오퍼 레터 앱&quot;이라는 메시지가 표시됩니다. <http://localhost:8000/>에 대한 브라우저를 열면 양식은 다음과 같이 표시됩니다.

![웹 양식의 스크린샷](assets/offer_2.png)

양식이 자체에 게시됩니다. 데이터를 입력하고 **Generate Letter,**&#x200B;을(를) 클릭하면 콘솔에 다음 정보가 표시됩니다.

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

이 콘솔 로깅을 [!DNL Acrobat Services]에 대한 웹 서비스 호출로 대체합니다. 먼저 정보의 JSON 기반 모델을 만들어야 합니다. 이 모델의 형식은 다음과 같습니다.

```
{
    "offer_letter": {
    "firstname": "John",
    "lastname": "Doe",
    "salary": "887888",
    "startdate": "2021-04-01"
    }
}
```

원하는 경우 이 모델을 더 정교하게 만들 수 있지만, 이 튜토리얼에서는 이 간단한 예제를 사용합니다. 이 문서의 범위를 벗어나므로 이 양식에 대한 유효성 검사가 없습니다. 양식 본문을 위에서 설명한 데이터 모델로 변환하려면 app.post 처리기 메서드를 다음 코드로 변경합니다.

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

첫 번째 줄은 JSON 데이터를 원하는 형식으로 배치합니다. 이제 이 데이터를 generateLetter 함수로 전달합니다. 서버를 중지하고 app.js 끝에 다음 코드를 붙여넣습니다. 이 코드는 Word 문서를 템플릿으로 사용하고 JSON 문서의 정보로 자리 표시자를 채웁니다.

```
// Letter generation function
function generateLetter(jsonDataForMerge) {
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(
'resources/OfferLetter-Template.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/OfferLetter.pdf'))
.catch(err => {
if(err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log(
'Exception encountered while executing operation', err);
} else {
console.log(
'Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

거기에는 짐을 풀 수 있는 많은 코드가 있다. 먼저 `documentMergeOperation`을(를) 주요 부분으로 하겠습니다. 이 섹션에서는 JSON 데이터를 가져와 Word 문서 템플릿과 병합합니다. Adobe 사이트의 [예제](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)를 참조로 사용할 수 있지만, 간단한 예제를 만들어 보겠습니다. Word를 열고 새 빈 문서를 만듭니다. 원하는 만큼 사용자 정의할 수 있지만 적어도 다음과 같은 기능이 있습니다.

친애하는 X 님,

일년에 X달러의 직책을 제안하게 되어 기쁩니다. 시작 날짜는 X입니다.

시작

프로젝트 루트의 &quot;resources&quot; 폴더에 문서를 &quot;OfferLetter-Template.docx&quot;로 저장합니다. 문서의 Xs 3개를 확인합니다. 해당 Xs는 JSON 정보를 위한 임시 자리 표시자입니다. 이러한 자리 표시자를 대체할 특수 구문을 사용할 수도 있지만 Adobe에서는 이 작업을 단순화하는 Word 추가 기능을 제공합니다. 추가 기능을 설치하려면 Adobe [Document Generation Tagger Word 추가 기능](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin) 사이트로 이동하세요.

OfferLetter-Template에서 새 **문서 생성** 단추를 클릭합니다. 측면 패널이 열립니다. **시작하기**&#x200B;를 클릭합니다. 샘플 JSON 데이터에 붙여넣을 텍스트 영역이 제공됩니다. JSON의 &quot;offer-data&quot; 스니펫을 위에서 텍스트 영역으로 복사합니다. 다음과 같이 표시됩니다.

![문자 및 코드 스크린샷](assets/offer_3.png)

**태그 생성** 단추를 클릭합니다. 문서의 해당 지점에 삽입할 태그의 드롭다운 메뉴가 표시됩니다. 문서의 첫 번째 X를 강조 표시하고 **[!UICONTROL 이름]**&#x200B;을 선택합니다. **[!UICONTROL 텍스트 삽입]**&#x200B;을 클릭하면 &quot;친애하는 X 님&quot;이 &quot;친애하는 ```{{`offer_letter`.firstname}}``` 님&quot;으로 변경됩니다. 이 태그는 `documentMergeOperation`에 대한 올바른 형식입니다. 계속해서 나머지 세 개의 태그를 해당 Xs에 추가합니다. OfferLetter-template.docx 를 저장하는 것을 잊지 마십시오. 다음과 같이 표시됩니다.

안녕하세요, ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```

연간 ```{{`offer_letter`.salary}}```달러의 직책을 제안하게 되어 기쁩니다. 시작 날짜는 ```{{`offer_letter`.startdate}}```입니다.

시작

이제 Word 템플릿에는 JSON 형식과 일치하는 마크업이 있습니다. 예를 들어 Word 문서의 시작 부분에 있는 ```{{`offer_letter`.`firstname`}}```은(는) JSON 데이터의 &quot;firstname&quot; 섹션에 있는 값으로 대체됩니다.

`generateLetter` 함수로 돌아갑니다. REST 호출을 보호하려면 프로젝트 루트에서 pdftools-api-credentials.json이라는 이름의 새 파일을 만듭니다. 다음 JSON 데이터에 붙여넣고 [개발자 콘솔](https://developer.adobe.com/console/)의 서비스 계정(JWT) 섹션에서 세부 정보로 조정합니다.

```
{
"client_credentials": {
"client_id": "<YOUR_CLIENT_ID>",
"client_secret": "<YOUR_CLIENT_SECRET>"
},
"service_account_credentials": {
"organization_id": "<YOUR_ORGANIZATION_ID>",
"account_id": "<YOUR_TECHNICAL_ACCOUNT_ID>",
"private_key_file": "<PRIVATE_KEY_FILE_PATH>"
}
}
```

* 클라이언트 ID, 클라이언트 암호 및 조직 ID는 콘솔의 **[!UICONTROL 자격 증명 세부 정보]** 섹션에서 직접 복사할 수 있습니다.

* 계정 ID는 **기술 계정 ID**&#x200B;입니다.

* 이전에 생성한 private.key 파일을 프로젝트로 복사하고 해당 이름을 파일의 private_key_file 섹션에 입력합니다.
pdftools-api-credentials.json 파일. 원하는 경우 여기에 개인 키 파일의 경로를 삽입할 수 있습니다. 한 번 사용하지 않을 경우 잘못 사용할 수 있으므로 안전하게 보관해야 합니다.

JSON 데이터가 채워진 PDF을 생성하려면 **[!UICONTROL 후보자 세부 정보 입력]** 웹 양식으로 돌아가서 일부 데이터를 게시하십시오. Adobe에서 문서를 다운로드해야 하므로 시간이 조금 걸리지만 [출력]이라는 새 폴더에 OfferLetter.pdf라는 이름의 파일이 있어야 합니다.

## 다음 단계

바로 그거야! 이건 시작에 불과해 Word 추가 기능의 [문서 생성] 탭에서 [고급] 섹션을 살펴보는 경우 모든 자리 표시자 마커가 연결된 JSON 데이터에서 가져온 것은 아닙니다. 서명 태그를 추가할 수도 있습니다. 이 태그를 사용하면 결과 문서를 가져와서 [Adobe Sign](https://www.adobe.com/ca/sign.html)에 업로드하여 새 직원에게 전달하고 서명할 수 있습니다. Adobe Sign API 시작하기 를 참조하여 방법을 알아보십시오. 이 프로세스는 JWT 토큰으로 보호된 REST 호출을 사용하기 때문에 비슷합니다.

위에서 제공된 단일 문서 예제는 조직에서 여러 위치에 걸쳐 직원의 [계절적 채용을 증가](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)해야 하는 경우 응용 프로그램의 기반으로 사용할 수 있습니다. 입증된 바와 같이, 주요 흐름은 온라인 애플리케이션을 통해 후보자들로부터 데이터를 가져오는 것이다. 이 데이터는 오퍼 레터의 필드를 채우고 전자 서명을 위해 보내는 데 사용됩니다.

[!DNL Adobe Acrobat Services]은(는) 6개월 동안 무료로 사용할 수 있으며, 문서 트랜잭션당 단 $0.05의 [종량제](https://developer.adobe.com/document-services/pricing/main)를 사용할 수 있으므로 이를 통해 비즈니스가 성장함에 따라 구인 공고 워크플로를 확장할 수 있습니다. [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
나만의 템플릿을 만들고 [개발자 계정에 등록](https://developer.adobe.com/)하세요.
