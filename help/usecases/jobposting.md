---
title: 채용 공고
description: 구직자와 고용주를 위한 원활하고 일관된 웹 경험을 개발하는 방법에 대해 알아봅니다
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8092.jpg
feature: Use Cases
jira: KT-8092
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 1%

---

# 채용 공고

![사례 메인 배너 사용](assets/UseCaseJobHero.jpg)

여러 사용자가 있는 웹 사이트를 운영할 때는 모든 사용자에게 원활한 경험을 제공할 수 있는 경험을 디자인하는 것이 중요합니다.

다음과 같은 시나리오를 생각해 보십시오. 고용주가 [채용 공고 업로드](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html). 구직자의 경우 게시물과 관련된 모든 문서를 일관된 형식으로 쉽게 볼 수 있어 편리하다. 그러나 고용주는 어떤 파일 형식이든 정보를 첨부하는 것이 편리합니다. 두 유형의 PDF 모두에게 편의를 제공하기 위해 업로드된 모든 문서를 자동으로 사용자로 변환하여 게시물에 인라인 포함할 수 있습니다.

## 학습 내용

이 실습 자습서에서는 다음을 사용하는 Node.js 예제를 살펴봅니다 [!DNL Adobe Acrobat Services] 및 [Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) Job 공고 사이트에 이러한 기능을 추가하려면 이렇게 하면 고용주와 구직자 모두에게 더 쉽게 사용할 수 있고 매력적인 웹 사이트가 만들어집니다. 여기 있습니다 [완료](https://github.com/contentlab-io/adobe_job_posting) [프로젝트 코드](https://github.com/contentlab-io/adobe_job_posting)를 참조하십시오.

먼저 간단한 Express 기반 Node.js 웹 응용 프로그램을 설정합니다. [Express](https://expressjs.com/) 은 라우팅 및 템플릿과 같은 기능을 제공하는 미니멀리스트 웹 응용 프로그램 프레임워크입니다. 응용 프로그램의 코드는 [GitHub](https://github.com/contentlab-io/adobe_job_posting). 또한 [PostgreSQL 데이터베이스](https://www.postgresql.org/) PDF을 저장하도록 설정합니다.

## 관련 [!DNL Acrobat Services] API

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## Adobe API 자격 증명 만들기

먼저 [자격 증명 만들기](https://www.adobe.com/go/dcsdks_credentials) Adobe PDF Embed API(무료 사용) 및 Adobe PDF Services API(6개월 무료 사용) [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (문서 트랜잭션당 $0.05). PDF 서비스 API에 대한 자격 증명을 만들 때 &quot;개인화된 코드 샘플 만들기&quot; 옵션을 선택합니다. ZIP 파일을 저장하고 pdftools-api-credentials.json 및 private.key를 Node.js Express 프로젝트의 루트 디렉토리에 추출합니다.

또한 자유롭게 사용할 수 있는 임베드 API에 대한 API 키가 필요합니다. 보낸 사람 [프로젝트](https://console.adobe.io/projects)만든 프로젝트로 이동합니다. 그런 다음 **프로젝트에 추가** 및 선택 **API**. 마지막으로 **PDF 포함 API**.

PDF 포함 API의 도메인을 지정합니다. API 키는 공개여야 합니다(브라우저에서 실행하는 코드에서 찾기). 도메인을 지정하여 다른 도메인의 다른 사용자가 API 키를 사용할 수 없도록 합니다.

&quot;localhost&quot;는 도메인으로 사용할 수 없습니다. &quot;testing.local&quot;과 같은 도메인을 지정하고 컴퓨터의 호스트 파일을 편집하여 해당 도메인을 사용자 컴퓨터인 127.0.0.1로 리디렉션하십시오. 그런 다음 응용 프로그램을 localhost:3000에서 테스트하는 대신 testing.local:3000에서 테스트할 수 있습니다. 완료되면 프로젝트 페이지에서 PDF 포함 API에 대한 API 키를 찾습니다.

## 업로드 양식 및 처리기 추가

작동하는 Express 응용 프로그램 및 API 자격 증명을 사용하는 경우 사용자가 문서를 웹 사이트에 업로드할 수 있도록 하는 양식도 필요합니다. 이 용도로 index.jade 템플릿을 편집합니다.

업로드된 Job 공고의 이름과 추가 정보가 포함된 문서에 대한 입력 필드를 생성합니다.

템플릿의 내용 블록 안에 다음 양식을 추가합니다.

```
extends layout

block content
  h1= title

  form(action="/upload", enctype="multipart/form-data", method="POST")
    label Job posting name:&nbsp;
    input(type="text", name="name", required="required")
    br
    br
    label Describing document:&nbsp;
    input(type="file", name="attachment", required="required")
    br
    br
    input(type="submit", value="Submit job posting")
```

그런 다음 POST 요청에 대한 핸들러를 /upload 동작에 추가합니다. 그런 다음 routes/index.js 파일에 /upload 경로를 추가합니다. 이 경로에 대해 새 파일을 만들 수 있지만 app.js 파일을 업데이트하여 새 파일을 반영해야 합니다. 이 경로 처리기 내에서 지정된 이름과 업로드된 파일에 액세스할 수 있습니다.

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

이 함수는 비동기적이므로 함수에서 await 키워드를 사용할 수 있으므로 API 호출을 수행하는 메서드를 호출할 때 편리합니다.

![채용 공고 웹 사이트의 스크린샷](assets/jobs_1.png)

## PDF 서비스 API 사용

PDF 서비스 API를 사용하기 전에 경로 파일의 맨 위에 다음 가져오기를 추가해야 합니다.

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

가져오기에서 바로 API 자격 증명을 로드하고 [실행 내용](https://www.javascripttutorial.net/javascript-execution-context/). 실행 컨텍스트는 다른 작업에 다시 사용할 수 있으므로 한 번만 수행하는 것이 좋습니다.

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

이제 `router.post` 차단합니다. 먼저 문서를 PDF으로 변환합니다.

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

대부분의 작업은 동일한 4단계를 수행합니다. 먼저 해당 클래스의 createNew 메서드를 사용하여 작업 유형을 초기화합니다. 그런 다음 작업에 대한 입력인 FileRef를 만듭니다. 작업의 결과도 FileRef이므로 이후 작업에서는 이 단계를 건너뛸 수 있습니다. 이 초기 작업의 경우 업로드된 파일의 바이트에서 FileRef를 만듭니다. 세 번째, 작업에 입력을 할당해야 합니다. 마지막으로, 작업은 실행 컨텍스트가 실행 메서드의 파라미터로서 실행된다. 이 메서드는 결과를 기다릴 수 있도록 Promise를 반환합니다.

이 코드는 반환된 PDF을 파일에 저장하고 브라우저에 간단한 &quot;성공&quot; 응답을 보냅니다. 파일 이름의 &quot;날짜&quot; 부분은 고유한 파일 이름을 보장합니다. 대상 파일이 있으면 saveAsFile에서 오류를 반환합니다.

## 이미지를 텍스트로 변환 및 PDF 압축

이제 광학 문자 인식(OCR)을 사용하여 이미지를 텍스트로 변환한 다음 결과를 압축합니다. OCR 및 CompressPDF 작업을 사용하여 PDF 만들기와 비슷하게 이 작업을 수행합니다. 경로 파일에 다음을 추가합니다. `router.post`:

```
  const name = req.body.name;
  const fileContents = req.files.attachment.data;

  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);
  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  const ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
  ocrOperation.setInput(result);
  result = await ocrOperation.execute(executionContext);

  const compressPdfOperation = PDFToolsSdk.CompressPDF.Operation.createNew();
  compressPdfOperation.setInput(result);
  result = await compressPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

이 작업은 한 번만 수행하면 됩니다. 결과는 FileRef이므로 코드가 setInput에 전달할 수 있습니다.

하드 디스크에 파일을 저장하고 지나치게 단순화된 HTTP 응답을 반환하는 것보다 더 좋은 방법이 있습니다. 대신 PDF을 데이터베이스에 저장하고 Adobe의 무료 PDF 포함 API를 사용하여 PDF을 포함하는 웹 페이지를 표시합니다. 이렇게 하면 구직자들이 회사 로고와 기타 디자인 요소를 완비하여 찾고 볼 수 있도록 고용주의 채용 공고 또는 브로셔가 웹사이트에 표시됩니다.

## 데이터베이스에 PDF 저장

PDF을 PostgreSQL 데이터베이스에 저장합니다. Node.js의 Postgres에 연결할 node-postgres 패키지를 가져옵니다. 특정 시점에서 PDF의 내용을 버퍼에 저장해야 하며 FileRef는 스트림에서만 작동하므로 스트림 버퍼 패키지를 설치하십시오. 따라서 콘텐츠를 버퍼에 쓰려면 스트림 버퍼 패키지를 사용하십시오.

```
npm install pg stream-buffers
```

이제 작업 게시를 위한 데이터베이스 테이블을 생성합니다. 고유한 식별자의 열, 이름의 열 및 연결된 PDF의 열이 필요합니다. Postgres 명령줄 인터페이스(CLI)에서 데이터베이스 테이블을 생성할 수 있습니다.

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

Node.js 파일로 돌아갑니다. 파일 위쪽에 일부 가져오기 추가:

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

데이터베이스 테이블에 PDF을 저장하려면 업로드 기능을 수정합니다. 마지막 두 줄(saveAsFile 및 send)을 다음 코드 조각으로 바꿉니다.

```
  const pgClient = new Client();
  pgClient.connect();

  const id = Math.random().toString(36).substr(2, 6); // not securely random at all,
  but serves the purpose for this demo

  const writableStream = new streamBuffers.WritableStreamBuffer();
  writableStream.on("finish", async () => {    
    await pgClient.query("INSERT INTO job_postings VALUES ($1, $2, $3)", [
      id,
      name,
      writableStream.getContents()
    ]);
    res.redirect(`/job/${id}`);
  })
  result.writeToStream(writableStream);
```

내용을 쓰려면 WritableStreamBuffer를 만듭니다. finish 이벤트를 사용하면 SQL 쿼리를 실행할 시간입니다. node-postgres 패키지는 자동으로 Buffer 매개 변수를 BYTEA 형식으로 변환합니다. 쿼리에서 사용자를 /job/로 리디렉션합니다.{id}나중에 만든 끝점입니다.

PDF 포함 API의 경우 PDF 내용만 반환하는 끝점이 필요합니다.

```
  router.get('/pdf/:id', async function (req, res, next) {
    const id = req.params.id;
 
    const pgClient = new Client();
    pgClient.connect();

  const pgResult = await pgClient.query("SELECT attachment FROM job_postings WHERE id
  = $1", [id]);
  const buffer = pgResult.rows[0].attachment;
  res.type('pdf');
    return res.send(buffer);
  });
```

## PDF 포함

이제 /job/{id} endpoint - 요청된 작업 게시 이름과 포함된 PDF을 포함하는 템플릿을 렌더링합니다.

```
router.get('/job/:id', async function(req, res, next) {
    const id = req.params.id;

    const pgClient = new Client();
    pgClient.connect();

    const pgResult = await pgClient.query("SELECT name FROM job_postings WHERE id =
  $1", [id]);
    const name = pgResult.rows[0].name;

    res.render('job', { pdf_url: `/pdf/${id}`, name });
  });
```

views/ 디렉토리에서 다음 내용이 포함된 job.jade 파일을 만듭니다.

```
  extends layout

  block content
    h1= name
    div(id='adobe-dc-view')
    script(src='https://documentcloud.adobe.com/view-sdk/main.js')
    script.
      window.embedUrl = "!{pdf_url}";
    script(src='/javascripts/embed-pdf.js')
```

첫 번째 스크립트는 Adobe의 View SDK이며, 이를 통해 PDF을 쉽게 포함할 수 있습니다. 두 번째 스크립트는 window.embedUrl의 값을 Express 경로 처리기에서 제공하는 PDF의 URL로 설정하는 인라인 원라이너입니다. 다음과 같이 세 번째 스크립트를 직접 만듭니다.

```
  document.addEventListener("adobe_dc_view_sdk.ready", function () {
    var adobeDCView = new AdobeDC.View({ clientId: "YOUR API KEY HERE", divId:
   "adobe-dc-view" });
    adobeDCView.previewFile({
      content: { location: { url: '//' + window.location.host + window.embedUrl }
         },
      metaData: { fileName: "Job posting" }
    });
  });
```

이제 문서를 업로드하고, /job/id 페이지로 리디렉션되고, 포함된 PDF을 보는 전체 프로세스를 테스트할 수 있습니다. 사용자는 동일한 단계를 거쳐 채용 공고 또는 기타 문서를 웹 사이트에 추가합니다.

![업로드된 PDF 문서를 테스트하는 스크린샷](assets/jobs_2.png)

인라인 포함 기능이 제대로 작동하는지 확인하려면 [라이브 데모](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf).

## 다음 단계

이 실습 자습서에서는 Node.js를 [!DNL Acrobat Services] 업로드된 파일을 [채용 공고](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html) PDF에 다양한 형식으로 결과 PDF은 웹 페이지에 포함되었습니다. 이제 웹 사이트에 동일한 기능을 추가할 수 있으므로 고용주가 구직자가 쉽게 찾을 수 있도록 직무 설명, 브로셔 등을 업로드할 수 있습니다. 이러한 역량은 모든 사람이 꿈의 직업을 찾는 데 필요한 정보를 얻는 데 도움이 됩니다.

[!DNL Acrobat Services] 웹 사이트 또는 앱에 주요 문서 처리 기능을 추가하는 데 도움이 됩니다. 이러한 API의 기능을 자세히 알아보려면 다음 빠른 시작 설명서를 참조하십시오.

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

웹 사이트에 사용자 친화적인 문서 처리 기능을 추가하려면 [무료 체험판 등록](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). Adobe PDF Embed API는 항상 무료로 사용할 수 있으며 Adobe PDF Services API는 6개월 동안 무료입니다. 단, 문서 트랜잭션당 \$0.05이므로 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 비즈니스 규모에 맞게
