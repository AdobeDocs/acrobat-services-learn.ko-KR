---
title: 사원 제안서 관리
description: 서명을 받기 위해 새 직원에게 전달할 수 있는 제안서를 생성하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8096.jpg
kt: 8096
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 2%

---

# 직원 고용 제안서 관리

![사례 메인 배너 사용](assets/UseCaseOfferHero.jpg)

직원 고용 제안서는 직원들이 조직 내에서 경험하는 첫 번째 사례 중 하나입니다. 그 결과, 오퍼 레터가 브랜드에 적합한지 확인해야 하지만 워드 프로세서에서 매번 처음부터 레터를 작성하지 않아도 됩니다. [!DNL Adobe Acrobat Services] API를 통해 [신규 사원에게 제안서 생성 및 전달](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html).

## 학습 내용

이 실습 튜토리얼에서는 사용자가 직원 세부 정보로 채울 수 있는 웹 양식을 표시하는 Node Express 프로젝트를 설정하는 방법을 살펴봅니다. 이러한 세부 정보는 [!DNL Acrobat Services] Adobe Sign API를 사용하여 고객의 서명을 받기 위해 고객에게 전달할 수 있는 PDF으로 제안서를 웹에서 생성합니다.

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe 문서 생성 API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [문서 생성 태거 Word 추가 기능](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin)

* [프로젝트 샘플](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html)

## 시작하기

[Node.js](https://nodejs.org/) 프로그래밍 플랫폼입니다. Express 웹 서버와 같은 방대한 라이브러리 세트가 제공됩니다. [Node.js 다운로드](https://nodejs.org/en/download/) 이 훌륭한 오픈 소스 개발 환경을 설치하려면 다음 단계를 따르십시오.

Node.js에서 Adobe 문서 생성 API를 사용하려면 [문서 생성 API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) 사이트에 로그인하여 계정에 액세스하거나 새 계정에 가입합니다. 계정: [6개월 무료 후 종량제](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 0.05 USD의 문서 트랜잭션 비용만 지불하면 되므로 별도 수수료 없이 사용해보고 회사 성장에 따라 비용을 지불할 수 있습니다.

에 로그인한 후 [Adobe Developer 콘솔](https://console.adobe.io/), 클릭 **[!UICONTROL 새 프로젝트 만들기]**. 프로젝트의 이름은 기본적으로 &quot;프로젝트 1&quot;입니다. 오른쪽 상단의 **[!UICONTROL 프로젝트 편집]** 단추를 클릭하고 이름을 &quot;Offer Letter Generator&quot;로 변경합니다. 화면 중앙에는 **[!UICONTROL 새 프로젝트 시작하기]** 섹션을 참조하십시오. 프로젝트에서 보안을 사용하려면 다음 단계를 수행하십시오.

클릭 **API 추가**. 선택할 수 있는 다양한 API가 표시됩니다. 에 **[!UICONTROL 제품별 필터링]** 섹션, 선택 **[!UICONTROL Document Cloud]**&#x200B;을 클릭하고 **[!UICONTROL 다음]**.

이제 자격 증명을 생성하여 API에 액세스합니다. 자격 증명은 JSON 웹 토큰([JWT](https://jwt.io/)): 보안 통신을 위한 개방형 표준 JWT에 익숙하고 이미 키를 생성한 경우 여기에서 공개 키를 업로드할 수 있습니다. 또는 **옵션 1** Adobe이 키를 생성하도록 하십시오.

![자격 증명 생성 스크린샷](assets/offer_1.png)

오른쪽 상단의 **[!UICONTROL 키 페어 생성]** 단추를 클릭합니다. 다운로드할 config.zip 파일이 제공됩니다. 아카이브 파일의 압축을 풉니다. 여기에는 다음 두 개의 파일이 포함됩니다. certificate_pub.crt 및 private.key. 후자의 경우 개인 자격 증명이 포함되어 있고 사용자가 통제할 수 없는 경우 잘못된 문서를 생성하는 데 사용될 수 있으므로 후자의 보안을 유지해야 합니다.

**[!UICONTROL 다음]**&#x200B;을 클릭합니다. 아니요. PDF 생성 API에 액세스할 수 있습니다. 에 **[!UICONTROL 제품 프로필 선택]** 화면, 확인 **[!UICONTROL 엔터프라이즈 PDF 서비스 개발자]**&#x200B;을 클릭하고 **[!UICONTROL 구성된 API 저장]** 단추를 클릭합니다. 이제 API를 사용할 준비가 되었습니다.

## 프로젝트 설정

코드를 실행하도록 노드 프로젝트를 설정합니다. 이 예제에서는 [Visual Studio 코드](https://code.visualstudio.com/) (VS 코드)를 편집기로 사용합니다. &quot;letter-generator&quot;라는 폴더를 만들고 VS 코드에서 엽니다. 원본 **[!UICONTROL 파일]** 메뉴, 선택 **[!UICONTROL 터미널]** \> **[!UICONTROL 새 터미널]** 이 폴더에서 셸을 엽니다. 다음을 입력하여 노드가 설치되어 있고 경로에 있는지 확인합니다.

```
node -v
```

설치한 노드의 버전이 표시됩니다.

이제 개발 환경이 설치되었으므로 프로젝트를 만들 수 있습니다.

먼저 npm(Node Package Manager)을 사용하여 프로젝트를 초기화합니다. 다음을 입력합니다.

```
npm init
```

노드 프로젝트에 대한 몇 가지 질문이 있습니다. 이러한 질문은 대부분 생략할 수 있지만 프로젝트 이름이 &quot;letter-generator&quot;이고 진입점이 **index.js**. 선택 **예** 프로젝트 초기화를 완료합니다.

이제 package.json 파일이 만들어집니다. 노드에서는 이 파일을 사용하여 프로젝트를 구성합니다. index.js를 만들기 전에 다음 명령을 사용하여 Adobe 라이브러리를 추가해야 합니다.

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

node_modules라는 새 폴더가 프로젝트에 추가되어야 합니다. 이 폴더는 모든 라이브러리(노드에서 종속성이라고 함)가 다운로드되는 곳입니다. package.json 파일도 Adobe PDF 서비스에 대한 참조로 업데이트됩니다.

이제 간단한 웹 프레임워크로 Express를 설치하려고 합니다. 다음 명령을 입력하십시오

```
npm install express –save
```

이전과 마찬가지로 package.json의 종속성 섹션도 그에 따라 업데이트됩니다.

## 오퍼 레터 템플릿 만들기

이제 프로젝트 루트에서 &quot;app.js&quot;라는 파일을 만듭니다. 그러면 다음과 같은 시작 코드를 입력하겠습니다.

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

get 경로가 **index.html** 있습니다. 이 이름과 다음의 간단한 양식을 사용하여 HTML 파일을 만들어 보겠습니다. 나중에 필요에 따라 CSS 스타일 및 기타 디자인 요소를 추가할 수 있습니다. 이 양식은 환영 편지를 생성하기 위한 후보자의 기본 세부 정보를 사용합니다.

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

다음 명령을 사용하여 웹 서버를 실행합니다.

```
node app.js
```

&quot;Candidate offer letter app listening on port 8000&quot;이라는 메시지가 표시되어야 합니다. 브라우저를 <http://localhost:8000/>양식은 다음과 같아야 합니다.

![웹 양식 스크린샷](assets/offer_2.png)

양식이 자신에게 게시됩니다. 데이터를 입력하고 **문자 생성,** 콘솔에 다음 정보가 표시됩니다.

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

이 콘솔 로깅을 웹 서비스 호출로 바꿉니다. [!DNL Acrobat Services]. 먼저 정보의 JSON 기반 모델을 만들어야 합니다. 이 모델의 형식은 다음과 같습니다.

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

원한다면 이 모델을 더 정교하게 만들 수 있지만 이 튜토리얼에서는 이 간단한 예제를 살펴봅니다. 이 양식은 이 문서의 범위를 벗어나므로 유효성 검사가 없습니다. 양식 본문을 위에서 설명한 데이터 모델로 변환하려면 app.post 처리기 메서드를 다음 코드로 변경하십시오.

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

첫 번째 줄은 JSON 데이터를 원하는 형식으로 저장합니다. 이제 이 데이터를 generateLetter 함수에 전달합니다. 서버를 중지하고 app.js의 끝에 다음 코드를 붙여넣습니다. 이 코드는 Word 문서를 템플릿으로 가져오고 JSON 문서의 정보로 자리 표시자를 채웁니다.

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

거기에는 짐을 풀기 위한 많은 코드가 있습니다. 먼저 주요 부분을 살펴보겠습니다. 이 `documentMergeOperation`. 이 섹션에서는 JSON 데이터를 가져와 Word 문서 템플릿과 병합합니다. 다음을 사용할 수 있습니다. [Adobe 사이트의 예](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) 참고로 하지만, 여러분만의 간단한 예를 만들어 보겠습니다. Word를 열고 새 빈 문서를 만듭니다. 원하는 대로 사용자 정의할 수 있지만 최소한 다음과 같은 옵션을 사용할 수 있습니다.

X님께,

일년에 $X의 직책을 제안하게 되어 기쁩니다. 시작 날짜는 X입니다.

시작

프로젝트 루트의 &quot;resources&quot;라는 폴더에 문서를 &quot;OfferLetter-Template.docx&quot;로 저장합니다. 문서의 Xs 3개가 표시됩니다. 이러한 Xs는 JSON 정보의 임시 자리 표시자입니다. 특수 구문을 사용하여 이러한 자리 표시자를 바꿀 수 있지만 Adobe은 이 작업을 단순화하는 Word 추가 기능을 제공합니다. 추가 기능을 설치하려면 Adobe [문서 생성 태거 Word 추가 기능](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin) 사이트로 이동합니다.

OfferLetter-Template에서 새 **문서 생성** 단추를 클릭합니다. 측면 패널이 열립니다. **시작하기**&#x200B;를 클릭합니다. 샘플 JSON 데이터에 붙여넣을 텍스트 영역이 제공됩니다. JSON의 &quot;offer-data&quot; 스니펫을 위에서 텍스트 영역으로 복사합니다. 다음과 같습니다.

![문자와 코드의 스크린샷](assets/offer_3.png)

오른쪽 상단의 **태그 생성** 단추를 클릭합니다. 문서의 적절한 지점에 삽입할 태그의 드롭다운 메뉴가 표시됩니다. 문서에서 첫 번째 X를 강조 표시하고 **[!UICONTROL 이름]**. 클릭 **[!UICONTROL 텍스트 삽입]** &quot;친애하는 X&quot;는 &quot;친애하는 Dear X&quot;로 ```{{`offer_letter`.firstname}}```,&quot;. 이 태그는 `documentMergeOperation`. 나머지 세 개의 태그를 해당하는 Xs에 추가합니다. OfferLetter-template.docx를 저장하는 것을 잊지 마십시오. 다음과 같이 표시됩니다.

안녕하세요, ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```

우리는 당신에게 $에 대한 자리를 제안하게 되어 기쁩니다 ```{{`offer_letter`.salary}}``` 일 년 시작 날짜는 ```{{`offer_letter`.startdate}}```.

시작

이제 Word 템플릿에는 JSON 형식과 일치하는 마크업이 있습니다. 예: ```{{`offer_letter`.`firstname`}}``` word 문서의 시작 부분은 JSON 데이터의 &quot;firstname&quot; 섹션에 있는 값으로 대체됩니다.

로 돌아가기 `generateLetter` 사용합니다. REST 호출을 보호하려면 프로젝트 루트에서 pdftools-api-credentials.json 이라는 새 파일을 만듭니다. 다음 JSON 데이터에 붙여넣은 다음, 서비스 계정(JWT) 섹션의 세부 정보로 조정합니다. [개발자 콘솔](https://console.adobe.io/).

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

* 클라이언트 ID, 클라이언트 암호 및 조직 ID는 **[!UICONTROL 자격 증명 세부 정보]** 섹션을 참조하십시오.

* 계정 ID는 **기술 계정 ID**.

* 이전에 생성한 private.key 파일을 프로젝트에 복사하고 pdftools-api-credentials.json 파일의 private_key_file 섹션에 해당 이름을 입력합니다. 원하는 경우 여기에 개인 키 파일의 경로를 입력할 수 있습니다. 한 번 실수로 잘못 사용될 수 있으므로 보안을 유지해야 합니다.

JSON 데이터가 채워진 PDF을 생성하려면 **[!UICONTROL 지원자 상세내역 입력]** 웹 양식 및 게시하기 Adobe에서 문서를 다운로드해야 하므로 시간이 조금 걸리지만 출력이라는 새 폴더에 OfferLetter.pdf라는 파일이 있어야 합니다.

## 다음 단계

이상입니다! 이것은 시작에 불과합니다. Word 추가 기능의 [문서 생성] 탭에 있는 [고급] 섹션을 살펴보면 일부 자리 표시자 마커가 연결된 JSON 데이터에서 생성된 것은 아닙니다. 서명 태그를 추가할 수도 있습니다. 이러한 태그를 사용하여 결과 문서를 [Adobe Sign](https://acrobat.adobe.com/ca/en/sign.html) 새 직원에게 전달하거나 서명하는 데 사용합니다. 방법을 알아보려면 Adobe Sign API 시작하기를 참조하십시오. 이 프로세스는 JWT 토큰으로 보안이 설정된 REST 호출을 사용하므로 유사합니다.

위에서 제공된 단일 문서 예제는 조직이 필요로 하는 경우 응용 프로그램의 기초로 사용할 수 있습니다 [계절적 고용 촉진](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html) 여러 지역에 있는 직원의 수 입증된 바와 같이, 주요 흐름은 온라인 애플리케이션을 통해 후보자의 데이터를 취취하는 것이다. 데이터는 제안서의 필드를 채우고 전자 서명을 위해 전송하는 데 사용됩니다.

[!DNL Adobe Acrobat Services] 6개월 동안 무료로 사용할 수 있습니다 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 문서 트랜잭션당 0.05 USD에 불과하므로 비즈니스 규모에 따라 제안서 워크플로우를 확장할 수 있습니다. 끝 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
고유한 템플릿 만들기 [개발자 계정 등록](https://www.adobe.io/).
