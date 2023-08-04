---
title: 디지털 문서 게시
description: Adobe PDF Embed API를 사용하여 웹 페이지 내에 포함된 PDF 문서를 표시하는 방법을 알아봅니다.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8090
thumbnail: KT-8090.jpg
exl-id: 3aa9aa40-a23c-409c-bc0b-31645fa01b40
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1903'
ht-degree: 0%

---

# 디지털 문서 퍼블리싱

![사례 메인 배너 사용](assets/UseCaseDigitalHero.jpg)

전자 문서는 어디에나 있습니다. 실제로, [수조 개의 PDF](https://itextpdf.com/en/blog/technical-notes/do-you-know-how-many-pdf-documents-exist-world) 전 세계적으로, 그리고 그 숫자는 매일 오른다. 웹 페이지에 PDF 뷰어를 포함하면 사용자가 HTML 및 CSS를 다시 디자인하거나 웹 사이트에 대한 액세스를 방해하지 않고도 문서를 볼 수 있습니다.

인기 있는 시나리오를 살펴보겠습니다. 한 회사가 게시물을 [웹 사이트의 백서](https://www.adobe.io/apis/documentcloud/dcsdk/digital-content-publishing.html)
앱 및 서비스에 대한 컨텍스트를 제공합니다. 이 웹 사이트의 마케터는 사용자들이 자신의 PDF 기반 콘텐츠와 어떻게 상호 작용하고 이를 웹 페이지 및 브랜드와 통합하는 방법을 더 잘 이해하고자 합니다. 그들은 백서를 다음과 같이 게시하기로 결정했다 [게이티드 콘텐츠](https://whatis.techtarget.com/definition/gated-content-ungated-content#:~:text=Gated%20content%20is%20online%20materials,about%20their%20jobs%20and%20organizations.)다운로드 가능한 사용자를 제어합니다.

## 학습 내용

이 실습 튜토리얼에서는 [Adobe PDF 임베드 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)무료로 간편하게 사용할 수 있습니다. 이러한 예제에서는 일부 JavaScript, Node.js, Express.js, HTML 및 CSS를 사용합니다. 프로젝트 코드 전체를 [GitHub](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1).

## 관련 API 및 리소스

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [프로젝트 코드](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&amp;sa=D&amp;source=editors&amp;ust=1617129543031000&amp;usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1)

## 노드 웹 앱 만들기

먼저 멋진 템플릿을 사용하고 다운로드할 수 있는 여러 PDF을 제공하는 Node.js 및 Express를 사용하여 사이트를 만들어 보겠습니다.

먼저 [node.js 다운로드 및 설치](https://nodejs.org/en/download/).

최소 웹 응용 프로그램 구조로 Node.js 프로젝트를 쉽게 만들려면 응용 프로그램 생성기 도구를 설치합니다 `` `express-generator` ``.

```
npm install express-generator -g
```

그런 다음 보기 엔진으로 선택하여 pdf-app이라는 새 Express 앱을 만듭니다.

```
express pdf-app --view=ejs
```

이제 \\pdf-app 디렉터리로 이동하고 모든 프로젝트 종속성을 설치합니다.

```
cd pdf-app
npm install
```

그런 다음 로컬 웹 서버를 시작하고 응용 프로그램을 실행합니다.

```
npm start
```

마지막으로 <http://localhost:3000>.

![기본 웹 사이트의 스크린샷](assets/ddp_1.png)

기본 웹 사이트가 있습니다.

## 백서 데이터 렌더링

웹 사이트에 백서를 게시하기 위해 웹 사이트에 백서 데이터를 정의하고 준비하여 이들 문서를 표시한다. 먼저 프로젝트 루트에 새 \\data 폴더를 만듭니다. 사용 가능한 백서에 대한 정보는 라는 새 파일에서 가져옵니다 [data.json](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/data/data.json)데이터 폴더에 저장됩니다.

웹 앱을 멋지고 세련된 모양으로 만들려면 [Bootstrap](https://getbootstrap.com/) 및 [Font Awesome](https://fontawesome.com/) 프런트 엔드 라이브러리

```
npm install bootstrap
npm install font-awesome
```

app.js 파일을 열고 이러한 디렉토리를 정적 파일의 소스로 포함하여 기존 파일 뒤에 배치합니다 `` `express.static` `` 있습니다.

```
app.use(express.static(path.join(__dirname, '/node_modules/bootstrap/dist')));
app.use(express.static(path.join(__dirname, '/node_modules/font-awesome')));
```

PDF 문서를 포함하려면 프로젝트의 \\public 폴더 아래에 \\pdf라는 폴더를 만듭니다. PDF과 축소판을 직접 만드는 대신 여기에서 복사할 수 있습니다 [GitHub 리포지토리 폴더](https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app/public) \\pdf 및 \\image 폴더에 저장합니다.

이제 \\public\\pdfs 폴더에 PDF 문서가 포함됩니다.

![PDF 파일 아이콘 스크린샷](assets/ddp_2.png)

\\public\\images 폴더에는 각 PDF 문서의 축소판이 포함되어야 합니다.

![PDF 축소판 스크린샷](assets/ddp_3.png)

이제 홈 페이지를 라우팅하기 위한 논리를 포함하는 \\routs\\index.js 파일을 엽니다. data.json 파일의 백서 데이터를 사용하려면 파일 시스템에 액세스하고 상호 작용하는 Node.js 모듈을 로드해야 합니다. 그런 다음 `fs` 다음과 같이 \\routs\\index.js 파일의 첫 번째 행에 상수가 있습니다.

```
const fs = require('fs');
```

그런 다음 data.json 파일을 읽고 파싱하여 papers 변수에 저장합니다.

```
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
```

이제 행을 수정하여 색인 보기에 대한 렌더링 방법을 호출하고 종이 컬렉션을 색인 보기에 대한 모델로 전달합니다.

```
res.render('index', { title: 'Embedding PDF', papers: papers });
```

홈 페이지에서 백서 컬렉션을 렌더링하려면 \\views\\index.ejs 파일을 열고 기존 코드를 프로젝트의 코드로 바꿉니다 [색인 파일](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/views/index.ejs).

이제 npm start 및 open 을 다시 실행합니다. <http://localhost:3000> 사용 가능한 백서 모음을 봅니다.

![백서용 축소판 스크린샷](assets/ddp_4.png)

다음 섹션에서는 웹 사이트 개선 및 사용에 대해 설명합니다 [PDF 포함 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 를 눌러 PDF 문서를 웹 페이지에 표시합니다. PDF 임베드 API는 무료로 사용할 수 있습니다. API 자격 증명만 얻으면 됩니다.

## PDF 임베드 API 자격 증명 가져오기

무료 PDF 임베드 API 자격 증명을 얻으려면 [시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 페이지를 엽니다.

클릭 **새 자격 증명 만들기** 그리고 **시작하기:**

![새 자격 증명을 만드는 방법 스크린샷](assets/ddp_5.png)

이때 무료 계정이 없는 경우 등록하라는 메시지가 표시됩니다.

선택 **PDF 포함 API**&#x200B;자격 증명 이름과 응용 프로그램 도메인을 입력합니다. 사용 **localhost** 도메인(웹 앱을 로컬로 테스트했기 때문).

![PDF 포함 API에 대한 새 자격 증명 만들기 스크린샷](assets/ddp_6.png)

오른쪽 상단의 **자격 증명 만들기** 버튼을 클릭하여 PDF 자격 증명에 액세스하고 클라이언트 ID(API 키)를 가져옵니다.

![새 자격 증명 복사 방법 스크린샷](assets/ddp_7.png)

Node.js 프로젝트에서 응용 프로그램의 루트 폴더에 .ENV라는 파일을 만들고 이전 단계에서 가져온 API KEY 자격 증명의 값으로 PDF 포함 클라이언트 ID에 대한 환경 변수를 선언합니다.

```
PDF_EMBED_CLIENT_ID=**********************************************
```

나중에 이 클라이언트 ID를 사용하여 PDF 포함 API에 액세스합니다. Node.js 코드를 사용하여 이 환경 변수에 액세스하려면 dotenv 패키지를 설치하십시오.

```
npm install dotenv
```

이제 app.js 파일을 열고 Node.js가 dotenv 모듈을 로드할 수 있도록 파일 상단에 다음 행을 추가합니다.

```
require('dotenv').config();
```

## 웹 앱에 PDF 표시

이제 PDF 포함 API를 사용하여 사이트에 PDF을 표시합니다. 라이브 열기 [PDF 임베드 API 데모](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf).

![라이브 PDF 임베드 API 데모 스크린샷](assets/ddp_8.png)

왼쪽 패널에서 웹 사이트 요구 사항에 가장 적합한 포함 모드를 선택할 수 있습니다.

* **전체 창**: PDF은 모든 웹 페이지 공간을 다룹니다.

* **크기 컨테이너**: PDF은 웹 페이지 내에서 한 번에 한 페이지씩 제한된 크기의 div로 표시됩니다

* **인라인**: 전체 PDF이 웹 페이지 내의 div에 표시됩니다.

* **라이트박스**: PDF이 웹 페이지 상단에 레이어로 표시됩니다.

백서는 인라인 포함 모드를 사용하고 나중에 코드 생성기를 사용하여 응용 프로그램에 PDF을 포함하는 것이 좋습니다.

## 인라인 포함 모드 페이지 만들기

PDF 뷰어를 웹 페이지에 포함하고 모든 페이지를 동시에 표시하려면 인라인 포함 모드를 사용하여 새 페이지를 만듭니다.

EJS 뷰 엔진을 사용하여 \\views\\in-line.ejs 파일에 새 뷰를 작성합니다.

```
<! html DOCTYPE >
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/stylesheets/style.css' />
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

고객 중심의 마케팅

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

그런 다음 \\views\\index.ejs를 수정하여 인라인 보기를 여는 단추를 만듭니다.

```
<div class="card-body">
<h5 class="card-title">
<span>
<%= paper.title %>
</span>
</h5>
<p>
<a class="btn btn-sm btn btn-danger" href="/in-line/<%=
paper.id %>">
<span type="button"></span>
<span class="fa fa-file-pdf-o"></span>&nbsp;View Document</button>
</a>
</p>
</div>
```

app.js 파일을 열고 indexRouter 선언 뒤에 새 라우터를 선언합니다.

```
var indexRouter = require('./routes/index');
var inLineRouter = require('./routes/in-line');
```

그런 다음 app.use(&#39;/&#39;, indexRouter); 뒤에 이 코드를 추가합니다. 인라인 포함 모드 보기를 라우터와 연결하려면 다음을 수행하십시오.

```
app.use('/', indexRouter);
app.use('/in-line', inLineRouter);
```

이제 \\routs 아래에 새 in-line.js 파일을 만들어 새 라우터 논리를 만듭니다. 웹 응용 프로그램 백엔드를 활성화하는 노드 모듈인 Express를 포함합니다.

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
```

그런 다음 특정 백서 ID에 대한 GET 요청을 처리하는 끝점을 만들고 in-line.ejs 보기를 렌더링합니다.

```
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
res.render('in-line', { title: paper.title, paper: paper });
});
module.exports = router;
```

다시 한 번 [라이브 데모](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) PDF 포함 API 코드를 자동으로 생성합니다. 클릭 **인라인** 왼쪽 패널에서:

![라이브 PDF 임베드 API 데모 스크린샷](assets/ddp_8.png)

클릭 **코드 생성** 크기 컨테이너 PDF 뷰어를 표시하는 데 필요한 HTML 코드를 확인합니다.

![코드 미리 보기의 스크린샷](assets/ddp_9.png)

클릭 **코드 복사** 코드를 in-line.ejs 파일에 붙여넣습니다.

```
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function(){
var adobeDCView = new AdobeDC.View({clientId: "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
adobeDCView.previewFile({
content:{location: {url: "https://documentcloud.adobe.com/view-sdk-demo/PDFs/Bodea Brochure.pdf"}},
metaData:{fileName: "Bodea Brochure.pdf"}
}, {embedMode: "IN_LINE"});
});
</script>
</div>
```

그러나 문서 매개 변수는 여전히 하드코딩되어 있습니다. 백서 모델 데이터에 따라 페이지를 렌더링하기 위해 EJS 대괄호 구문(\&lt;%= someValue %\>)으로 바꾸어 보겠습니다.

```
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE"
});
});
</script>
```

이제 npm start 명령을 사용하여 애플리케이션을 실행하고 웹 사이트를 <http://localhost:3000>.

![PDF 백서 축소판 스크린샷](assets/ddp_10.png)

마지막으로 흰색 용지 하나를 선택하고 **문서 보기** 인라인 포함 PDF으로 새 페이지를 열려면

![PDF 백서 스크린샷 ](assets/ddp_11.png)

이제 PDF 다운로드 및 인쇄 PDF 옵션이 어떻게 나타나는지 확인합니다.

![다운로드 및 인쇄 옵션 스크린샷](assets/ddp_12.png)

백엔드에서 이 플래그를 제어하려고 합니다. 나중에 사용자 ID를 기반으로 인증 제어를 구현하고 비즈니스 규칙에 따라 액세스를 제한할 수 있습니다. 복잡성은 여기에 필요하지 않으므로 인증된 속성과 사용 권한 속성을 모델 개체에 포함하도록 \\routs\\in-line.js만 수정해 보겠습니다.

```
let authenticated = false;
res.render('in-line', {
title: paper.title,
paper: paper,
authenticated: authenticated,
permissions: {
showDownloadPDF: true,
showPrintPDF: true,
showFullScreen: true
}
});
```

그런 다음 웹 페이지에서 백엔드의 플래그 값을 렌더링할 수 있도록 \\views\\in-line.ejs를 수정합니다.

```
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
Now, open the in-line.js route file and modify it to disallow the printing, downloading, and full-screen controls.
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
```

그런 다음 응용 프로그램을 다시 실행하여 이 변경 사항이 PDF 뷰어에 어떻게 반영되는지 확인합니다.

![PDF 파일의 스크린샷](assets/ddp_13.png)

## 제어된 내용 만들기

최종 사용자 시나리오에 따르면, 이 회사의 웹 사이트 마케터는 사용자가 PDF 기반 콘텐츠와 상호 작용하는 방식을 더 잘 이해하고 해당 콘텐츠를 나머지 웹 페이지 및 브랜드와 통합하고자 합니다.

당사는 PDF 포함에 중점을 두고 있으므로 사용자 인증 기능을 생성하지 않습니다. 대신 일부 사용자 정보를 수용하는 웹 양식을 사용하여 간단한 가짜 페이월을 구현하고 사용자가 양식을 제출하면 PDF 문서를 표시할 수 있습니다.

보기 모델에 사용자 정보를 제공하려면 \\routs\\in-line.js 파일을 아래 내용으로 바꿉니다.

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
let authenticated = false;
let user = {};
if (req.body.firstName) {
user = {
firstName: req.body.firstName,
lastName: req.body.lastName,
jobTitle: req.body.jobTitle,
email: req.body.email,
};
authenticated = true;
}
res.render('in-line', {
title: paper.title,
paper: paper,
user: user,
authenticated: authenticated,
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
});
});
module.exports = router;
```

그런 다음 \\views\\in-line.ejs 내용을 아래 코드로 바꿉니다. 인증된 사용자인지 여부에 따라 PDF 데이터 양식 또는 사용자 뷰어를 표시합니다.

```
<!DOCTYPE html>
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<% if (authenticated) { %>
<header class="bg-dark text-white">
<div class="text-right mr-4">Hello, <%= user.firstName %> <%= user.lastName%></div>
</header>
<% } %>
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

고객 중심의 마케팅

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<% if (!authenticated) { %>
<div class="row">
<form method="POST" class="center-panel text offset-md-3 col-md-6 border">
<fieldset class="offset-md-1">
<legend>Submit your info to<br/>access the whitepaper</legend>
<p><input name="firstName" placeholder="first name"/></p>
<p><input name="lastName" placeholder="last name"/></p>
<p><input name="jobTitle" placeholder="job title"/></p>
<p><input name="email" placeholder="email"/></p>
<p><button type="submit" class="btn btn-sm btn btn-primary">Submit</button></p>
</fieldset>
</form>
</div>
<% } %>
<% if (authenticated) { %>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
});
});
</script>
<% } %>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

![게이팅 컨텐츠 스크린샷](assets/ddp_14.png)

이제 사이트 방문자는 정보를 제출한 후에만 PDF에 액세스할 수 있습니다.

![포함된 뷰어의 PDF 내용 스크린샷](assets/ddp_15.png)

## 이벤트 활성화

PDF 뷰어 이벤트를 애플리케이션과 손쉽게 통합하여 마케터를 위한 분석 데이터를 수집하는 방법을 살펴보십시오. PDF EmbedAPI를 사용하여 뷰어를 확장하려면 adobeDCView 변수를 선언한 후 previewFile 메서드를 호출하기 전에 다음 코드 행을 추가합니다.

```
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.registerCallback(
AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
function(event) {
console.log(event);
},
{ enablePDFAnalytics: true }
);
```

이제 응용 프로그램을 다시 실행하고 웹 브라우저의 개발자 도구를 열어 이벤트 데이터를 확인합니다.

![코드 스크린샷](assets/ddp_16.png)

이 데이터를 [Adobe Analytics](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=view) 또는 다른 분석 툴에 적용할 수 있습니다.

## 다음 단계

[!DNL Acrobat Services] 개발자는 API를 사용하여 PDF 중심의 워크플로우를 통해 디지털 출판 문제를 손쉽게 해결할 수 있습니다. 예제 노드 웹 앱을 만들어 백서 컬렉션을 표시하는 방법을 살펴보았습니다. 그런 다음 [무료 API 자격 증명](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 및 작성된 백서에 대한 제한된 액세스 권한 [포함 모드](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf).

이 워크플로우를 통합하면 [가설 마케터](https://www.adobe.io/apis/documentcloud/dcsdk/digital-content-publishing.html) 백서를 다운로드하는 대신 리드 연락처 정보를 수집하고 PDF과 상호 작용하는 사람에 대한 통계를 확인합니다. 이러한 기능을 웹 사이트에 통합하여 사용자 참여를 유도하고 모니터링할 수 있습니다.

angular 또는 React 개발자인 경우 [추가 샘플](https://github.com/adobe/pdf-embed-api-samples) PDF 임베드 API를 React 및 Angular 프로젝트와 통합하는 방법을 설명합니다.

Adobe을 통해 혁신적인 솔루션을 통해 엔드 투 엔드 고객 경험을 구축할 수 있습니다. 체크아웃 [Adobe PDF 임베드 API](https://www.adobe.io/apis/documentcloud/viesdk) 무료로 이용 그 밖에 가능한 작업을 살펴보려면 [pay-as-you-gopr](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)[착빙](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html).

[시작하기](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 에 [!DNL Adobe Acrobat Services] API를 통해
