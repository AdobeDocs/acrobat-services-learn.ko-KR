---
title: 검토 및 승인
description: 팀 간 공동 작업을 위한 문서 검토 및 승인 워크플로우를 구축하는 방법에 대해 알아봅니다
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-8094.jpg
kt: 8094
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# 검토 및 승인

![사례 메인 배너 사용](assets/UseCaseReviewsHero.jpg)

코로나19의 팬데믹 상황 속에서 많은 기업이 원격으로 팀과 협업하는 것이 중요해졌고, [디지털 문서 공유 및 검토](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) 팀 및 부서 간 리소스에 대한 일련의 문제를 제시합니다.

이러한 문제에는 다양한 파일 형식으로 문서를 공유하고, 내용을 효과적으로 검토하고 주석을 달고, 가장 최근의 편집 내용과 동기화하는 문제가 포함됩니다. [!DNL Adobe Acrobat Services] API는 응용 프로그램 개발자가 사용자를 위해 이러한 문제를 해결할 수 있도록 설계되었습니다.

## 학습 내용

이 실습 자습서에서는 Node.js 및 Express 웹 응용 프로그램에서 문서 검토 및 승인 작업 과정을 만드는 방법을 보여 줍니다. 이 튜토리얼을 따라 하려면 Node.js에 대한 경험이 필요합니다.

응용 프로그램에는 다음과 같은 기능이 있습니다.

* 다양한 파일 유형을 PDF으로 변환

* 파일 업로드 활성화

* 사용자에게 주석 및 주석 추가 기능 제공

* 해당 주석과 함께 PDF 표시

* 사용자 프로필을 사용하여 주석 작성자를 식별합니다.

* 사용자가 다운로드할 수 있는 최종 PDF에 파일 결합

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 포함 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [프로젝트 코드](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Adobe API 자격 증명 만들기

코드를 시작하기 전에 [자격 증명 만들기](https://www.adobe.com/go/dcsdks_credentials) Adobe PDF Embed API 및 Adobe PDF Services API의 경우 PDF 포함 API는 무료로 사용할 수 있습니다. PDF 서비스 API는 6개월 동안 무료로 사용할 수 있으므로 [사용한 만큼 지불 플랜](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (문서 트랜잭션당 $0.05).

PDF 서비스 API에 대한 자격 증명을 만들 때 **개인화된 코드 샘플 만들기** 옵션으로 Node.js를 선택합니다. ZIP 파일을 저장하고 pdftools-api-credentials.json 및 private.key를 Node.js Express 프로젝트의 루트 디렉토리에 추출합니다.

## 프로젝트 및 종속성 설정

&quot;public&quot;이라는 폴더의 정적 파일을 제공하도록 Node.js 및 Express 프로젝트를 설정합니다. 환경 설정에 따라 프로젝트 방식을 설정할 수 있습니다. 를 사용하면 [Express 앱 생성기](https://expressjs.com/en/starter/generator.html). 또는 간단한 작업을 원할 경우 다음을 수행할 수 있습니다 [처음부터](https://expressjs.com/en/starter/hello-world.html) 코드를 단일 JavaScript 파일로 유지합니다. 위에 연결된 샘플 프로젝트에서는 단일 파일 방식을 사용하고 모든 코드를 index.js에 저장합니다.

복사 `pdftools-api-credentials.json` 및 `private.key` 파일의 이름을 지정합니다. 또한 자격 증명 파일이 실수로 리포지토리로 전송되지 않도록 .gitignore 파일에 자격 증명 파일을 추가합니다.

다음, 실행 `npm install @adobe/documentservices-pdftools-node-sdk` PDF 서비스용 Node.js SDK를 설치합니다. 나머지 종속성을 다음과 같이 가져온 후 이 모듈을 가져오고 코드(샘플 프로젝트의 index.js) 내에 API 자격 증명 개체를 만듭니다.

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

시작 코드는 다음과 같습니다.

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

이제 작업할 준비가 되었습니다 [!DNL Acrobat Services] API

## 파일을 PDF으로 변환

문서 워크플로우의 첫 번째 부분에서 최종 사용자는 공유할 문서를 업로드해야 합니다. 이를 사용하려면 업로드 기능을 추가하고 다양한 문서 파일 형식을 PDF에 통합하여 검토 프로세스를 위해 준비합니다.

먼저 문서를 [PDF 서비스 API에 대한 예제 코드](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html). 이 예제에서는 OCR(Optical Character Recognition), 암호 보호 및 제거, 압축 등 기타 중요한 기능에 대한 코드 단편을 보여 줍니다.

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

이제 이 기능을 사용하여 업로드된 문서에서 PDF을 만들 수 있습니다.

## 파일 업로드 처리

다음으로, 서버는 문서들을 수신하고 처리하기 위해 웹 서버 상의 파일 업로드 엔드포인트가 필요하다.

먼저 업로드 폴더 내에 폴더를 만들고 이름을 &quot;임시&quot;로 지정합니다. 업로드된 파일과 변환된 PDF 파일을 여기에 저장합니다. 다음, 실행 `npm install express-fileupload` express-FileUpload 모듈을 설치하고 코드에서 미들웨어를 Express에 추가하려면

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

이제 `/upload `업로드된 파일을 동일한 파일 이름을 사용하여 초안 폴더에 저장합니다. 그런 다음 이전에 작성한 함수를 호출하여 동일한 문서의 PDF 파일이 아직 PDF 형식이 아닌 경우 이를 만듭니다. 업로드된 원본 문서의 이름을 기반으로 새 PDF 파일의 파일 이름을 생성할 수 있습니다.

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## 업로드 페이지 만들기

이제 웹 응용 프로그램에서 파일을 업로드하려면 `index.html` 업로드 폴더 내의 웹 페이지입니다. 페이지에서 파일을 /upload 끝점으로 보내는 파일 업로드 양식을 추가합니다.

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![웹 페이지의 파일 업로드 기능 스크린샷](assets/reviews_1.png)

이제 문서를 Node.js 서버에 업로드할 수 있습니다. 서버는 uploads/draft 폴더 내에 파일을 저장하고 그 안에 PDF 형식 버전을 만듭니다.

이제 업로드된 문서를 포함할 준비가 되었으므로 PDF 포함 API를 사용하여 사용자가 문서에 주석 및 주석을 쉽게 추가할 수 있습니다.

## PDF 파일 열거

일반적인 문서 작업 과정에는 여러 문서가 포함될 수 있으므로 문서 목록을 표시하고 각 문서를 앱의 새 문서 검토 페이지에 연결해야 합니다.

먼저 서버 코드 내에 uploads/draft 폴더에 저장된 모든 PDF 파일의 목록을 가져와서 반환하는 /files 끝점을 추가합니다.

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

추가 `/download/:file` 웹 페이지에 포함할 수 있도록 업로드된 PDF 파일에 대한 액세스를 제공하는 경로입니다.

>[!NOTE]
>
>프로덕션 응용 프로그램에서는 인증 및 권한 부여를 추가하여 요청이 유효한 사용자로부터 오고 사용자가 문서에 액세스할 수 있도록 해야 합니다.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

index.html 페이지를 로드 시 채워지는 파일 목록 요소로 업데이트합니다. 각 항목은 draft.html 웹 페이지에 링크할 수 있으며 사용자는 쿼리 문자열 매개 변수를 사용하여 파일 이름을 페이지에 전달합니다.

>[!NOTE]
>
>jQuery를 사용하여 각 항목을 추가할 수 있으므로 웹 페이지에 jQuery 라이브러리를 로드하거나 다른 방법을 사용하여 요소를 추가해야 합니다.

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![검토할 파일 선택 스크린샷](assets/reviews_2.png)

## PDF 포함

웹 응용 프로그램 내에 PDF 파일을 포함하고 표시할 준비가 되었습니다.

&quot;draft.html&quot;이라는 웹 페이지를 만들고 포함된 PDF에 대한 div 요소를 페이지에 추가합니다.

```
  <div id="adobe-dc-view"></div>
```

포함 [!DNL Acrobat Services] 라이브러리:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

사용자 정의 스크립트 태그 내에서 페이지에 포함할 파일을 알 수 있도록 쿼리 문자열 매개 변수의 파일 이름을 파싱합니다.

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

지정된 PDF 파일을 div 요소 내의 포함된 보기로 로드하는 adobe_dc_view_sdk.ready 이벤트에 대한 문서 이벤트 리스너를 추가합니다. PDF 포함 API 자격 증명에서 클라이언트 ID를 사용합니다. 주석 및 주석을 활성화하려면 FULL_WINDOW 모드에서 보기를 포함하고 showAnnotationsTools 옵션을 true로 설정합니다.

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## 사용자 프로필 만들기

이 보기에는 기본적으로 주석 및 주석이 &quot;게스트&quot;로 표시됩니다. 페이지 코드의 PDF 프로필 콜백을 페이지 보기에 등록하여 주석 및 주석에 대한 현재 검토자의 이름을 설정할 수 있습니다. 다음은 예제 프로필입니다. 사용자 인증을 포함하는 전체 응용 프로그램에서, 로그인한 사용자 세션의 프로파일 정보는 이러한 방식으로 검토 작업 과정에서 문서의 각 주석(comenter)을 식별할 수 있다.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

프로필은 이 웹 페이지를 사용하여 업로드된 문서를 보고 주석을 달 때 사용자를 특정 사용자로 식별합니다.

## 문서 피드백 저장

사용자가 문서에 주석을 추가한 후 **저장.** 기본적으로 **저장** 업데이트된 PDF 파일을 다운로드합니다. 서버의 현재 PDF 파일을 업데이트하려면 이 동작을 변경합니다.

추가 `/save` uploads/draft 폴더의 PDF 파일을 덮어쓰는 서버 코드에 대한 끝점입니다.

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

/save 끝점에 내용을 업로드하는 SAVE_API에 대한 PDF 보기 콜백을 등록합니다. autoSaveFrequency 값을 변경하여 응용 프로그램이 타이머에 변경 내용을 자동으로 저장하고 원하는 경우 완료 시 현재 포함된 파일에 추가 메타데이터를 포함하도록 할 수 있습니다.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

이제 초안 문서의 주석 및 주석이 서버에 저장됩니다. 또한 [콜백 방법에 대해 더 알아보기](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) 워크플로우에 맞는 디자인 예를 들어, [상태 콜백](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) 여러 사람이 동일한 문서를 동시에 검토하고 주석을 달려는 경우 파일 충돌을 처리할 수 있습니다.

마지막 단계에서는 PDF 서비스 API를 사용하여 편집된 모든 문서를 하나의 PDF 파일로 결합합니다.

## PDF 파일 결합

PDF 조합 코드는 PDF 생성 코드와 비슷하지만 CombineFiles 작업을 사용하고 각 파일을 입력으로 추가합니다.

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## 최종 PDF 다운로드

/finalize라는 끝점을 추가하여 함수를 호출하여 PDF 파일 내부의 `uploads/drafts` 폴더에 `Final.pdf` 파일을 다운로드합니다.

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

마지막으로 이 /finalize 끝점에 기본 index.html 웹 페이지의 링크를 추가합니다. 이 링크를 통해 사용자는 문서 작업 과정의 결과를 다운로드할 수 있습니다.

```
<a href="/finalize">Download final PDF</a>
```

![최종 문서 다운로드 스크린샷](assets/reviews_3.png)

## 다음 단계

이 실습 튜토리얼에서는 [!DNL Acrobat Services] API는 [문서 공유 및 검토 작업 과정](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) 웹 응용 프로그램에 추가합니다. 이 애플리케이션을 통해 원격 작업자는 파일을 공유하고 팀원과 협업할 수 있으며, 특히 재택근무 중인 직원과 계약자에게 도움이 됩니다.

이러한 기술을 사용하여 앱에서 공동 작업을 활성화하거나 탐색할 수 있습니다 [PDF 서비스 노드 SDK 샘플](https://github.com/adobe/pdftools-node-sdk-samples) 및 [PDF 포함 API 샘플](https://github.com/adobe/pdf-embed-api-samples) gitHub에서 Adobe의 API를 사용하는 방법에 대한 영감을 얻으십시오.

자체 앱에서 문서 공유 및 검토를 활성화할 준비가 되셨습니까? 등록 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 개발자 계정입니다. Adobe PDF 임베드를 무료로 이용하고 다른 API의 6개월 무료 체험판을 사용해 보십시오. 체험 기간 이후에는 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 비즈니스 규모에 맞게 문서 트랜잭션당 \$0.05에 이용하세요.
