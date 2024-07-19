---
title: 검토 및 승인
description: 팀 간 공동 작업을 위한 문서 검토 및 승인 워크플로우를 빌드하는 방법에 대해 알아봅니다.
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8094
thumbnail: KT-8094.jpg
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1540'
ht-degree: 0%

---

# 검토 및 승인

![사례 영웅 배너 사용](assets/UseCaseReviewsHero.jpg)

코로나19 팬데믹 기간 동안 많은 기업에서 원격 교차 팀 협력이 필요해졌습니다. [디지털 문서 공유 및 검토](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html)는 팀과 교차 기능 리소스에 대해 일련의 과제를 제시합니다.

이러한 문제에는 다양한 파일 형식의 문서를 공유하고, 콘텐츠를 효과적으로 검토하고 댓글을 달며, 최신 편집 내용과 동기화하는 문제 등이 포함됩니다. [!DNL Adobe Acrobat Services] API는 응용 프로그램 개발자가 사용자를 위해 이러한 문제를 해결할 수 있도록 설계되었습니다.

## 학습 내용

이 실습 튜토리얼에서는 Node.js 및 Express 웹 애플리케이션에서 문서 검토 및 승인 워크플로우를 빌드하는 방법을 보여 줍니다. 이 튜토리얼을 따라 하려면 Node.js 와 관련된 경험이 필요합니다.

응용 프로그램에는 다음과 같은 기능이 있습니다.

* 다른 파일 형식을 PDF으로 변환

* 파일 업로드 활성화

* 사용자에게 주석 및 주석을 추가할 수 있는 기능 제공

* 해당 주석과 함께 PDF 표시

* 사용자 프로필에서 주석 작성자 식별 활성화

* 사용자가 다운로드할 수 있는 최종 PDF에 파일 결합

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [프로젝트 코드](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Adobe API 자격 증명 생성 중

코드를 시작하기 전에 Adobe PDF Embed API 및 Adobe PDF Services API에 대해 [자격 증명을 만들어야](https://www.adobe.com/go/dcsdks_credentials) 합니다. PDF Embed API는 무료로 사용할 수 있습니다. PDF 서비스 API는 6개월 동안 무료로 사용할 수 있으며 문서 트랜잭션당 단 \$0.05의 [종량제 플랜](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)으로 전환할 수 있습니다.

PDF 서비스 API에 대한 자격 증명을 만들 때 **개인화된 코드 샘플 만들기** 옵션을 선택하고 해당 언어에 대해 Node.js를 선택합니다. ZIP 파일을 저장하고 pdftools-api-credentials.json 및 private.key를 Node.js Express 프로젝트의 루트 디렉터리로 추출합니다.

## 프로젝트 및 종속성 설정

Node.js 및 Express 프로젝트가 &quot;public&quot;이라는 폴더에서 정적 파일을 제공하도록 설정합니다. 환경 설정에 따라 프로젝트 방식을 설정할 수 있습니다. [Express 앱 생성기](https://expressjs.com/en/starter/generator.html)를 사용하여 빠르게 시작하고 실행할 수 있습니다. 또는 간단하게 하려면 [처음부터 시작](https://expressjs.com/en/starter/hello-world.html)하고 코드를 하나의 JavaScript 파일에 보관하세요. 위에서 연결된 샘플 프로젝트에서는 단일 파일 접근 방식을 사용하고 모든 코드를 index.js로 유지합니다.

개인 맞춤화된 코드 샘플에서 `pdftools-api-credentials.json` 및 `private.key` 파일을 프로젝트의 루트 디렉터리로 복사합니다. 또한 .gitignore 파일이 있는 경우 해당 파일을 .gitignore 파일에 추가하면 자격 증명 파일이 실수로 저장소로 전송되지 않습니다.

그런 다음 `npm install @adobe/documentservices-pdftools-node-sdk`을(를) 실행하여 PDF 서비스용 Node.js SDK를 설치합니다. 다음과 같이 나머지 종속성을 가져온 후 이 모듈을 가져와 코드(샘플 프로젝트의 index.js) 내에 API 자격 증명 개체를 만듭니다.

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

시작 코드는 다음과 같아야 합니다.

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

이제 [!DNL Acrobat Services] API로 작업할 준비가 되었습니다.

## 파일을 PDF으로 변환

문서 워크플로우의 첫 번째 부분에서는 최종 사용자가 공유할 문서를 업로드해야 합니다. 이 기능을 활성화하려면 업로드 기능을 추가하고 다양한 문서 파일 형식을 PDF으로 통합하여 검토 프로세스를 준비할 수 있습니다.

먼저 [PDF 서비스 API용 예제 코드 조각](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html)을 기반으로 문서를 PDF으로 변환하는 함수를 만듭니다. 이 예는 또한 광학 문자 인식(OCR), 암호 보호 및 제거, 압축 등 다른 중요한 기능에 대한 스니펫을 보여 줍니다.

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

이제 이 기능을 사용하여 업로드된 문서에서 PDF을 생성할 수 있습니다.

## 파일 업로드 처리

다음으로, 서버는 문서를 수신하고 처리하기 위해 웹 서버에 파일 업로드 끝점이 필요합니다.

먼저, 업로드 폴더 내에 폴더를 만들고 이름을 &quot;임시 보관함&quot;으로 지정합니다. 업로드된 파일과 변환된 PDF 파일은 여기에 저장됩니다. 다음으로 `npm install express-fileupload`을(를) 실행하여 Express-FileUpload 모듈을 설치하고 코드에서 미들웨어를 Express에 추가합니다.

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

이제 `/upload `끝점을 추가하고 업로드된 파일을 동일한 파일 이름을 사용하여 임시 보관함 폴더에 저장합니다. 그런 다음 이전에 작성한 함수를 호출하여 동일한 문서의 PDF 파일이 PDF 형식이 아닌 경우 해당 파일을 만듭니다. 업로드된 원본 문서의 이름을 기반으로 새 PDF 파일의 파일 이름을 생성할 수 있습니다.

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

이제 웹 애플리케이션에서 파일을 업로드하려면 업로드 폴더 내에 `index.html` 웹 페이지를 만드십시오. 페이지에서 /upload 엔드포인트로 파일을 보내는 파일 업로드 양식을 추가합니다.

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![웹 페이지에 파일 업로드 기능 스크린샷](assets/reviews_1.png)

이제 문서를 Node.js 서버에 업로드할 수 있습니다. 서버는 파일을 업로드/초안 폴더에 저장하고 이와 함께 PDF 형식 버전을 만듭니다.

이제 업로드된 문서를 포함할 준비가 되었으므로, PDF 포함 API 를 사용하여 사용자가 문서에 댓글과 주석을 간편하게 추가할 수 있습니다.

## PDF 파일 열거 중

일반적인 문서 작업 과정에는 여러 문서가 포함될 수 있으므로 문서 목록을 표시하고 각 문서를 앱의 새 문서 검토 페이지에 연결해야 합니다.

먼저 서버 코드 내에 uploads/drawinds 폴더에 저장된 모든 PDF 파일의 목록을 가져오고 반환하는 /files 엔드포인트를 추가합니다.

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

웹 페이지에 포함할 업로드된 PDF 파일에 대한 액세스를 제공하는 `/download/:file` 경로를 추가합니다.

>[!NOTE]
>
>프로덕션 응용 프로그램에서는 요청이 유효한 사용자로부터 나오고 사용자가 문서에 액세스할 수 있도록 하려면 인증 및 권한 부여를 추가해야 합니다.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

로드 시 채워지는 파일 목록 요소를 사용하여 index.html 페이지를 업데이트합니다. 각 항목은 draft.html 웹 페이지에 링크할 수 있으며 쿼리 문자열 매개 변수를 사용하여 파일 이름을 페이지에 전달합니다.

>[!NOTE]
>
>jQuery를 사용하여 각 항목을 추가할 수 있으므로 웹 페이지에 jQuery 라이브러리를 로드하거나 다른 메서드를 사용하여 요소를 추가해야 합니다.

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

## PDF 임베드

웹 애플리케이션 내에 PDF 파일을 임베드하고 표시할 준비가 되었습니다.

&quot;draft.html&quot;이라는 웹 페이지를 만들고 포함된 PDF의 페이지에 div 요소를 추가합니다.

```
  <div id="adobe-dc-view"></div>
```

[!DNL Acrobat Services] 라이브러리 포함:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

사용자 정의 스크립트 태그 내에서 쿼리 문자열 매개 변수에서 파일 이름을 구문 분석하면 페이지에 포함할 파일을 알 수 있습니다.

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

지정된 PDF 파일을 div 요소 내의 포함된 뷰에 로드하는 adobe_dc_view_sdk.ready 이벤트에 대한 문서 이벤트 리스너를 추가합니다. PDF Embed API 자격 증명에서 클라이언트 ID를 사용합니다. 주석과 주석을 활성화하려면 FULL_WINDOW 모드에서 보기를 포함하고 showAnnotationsTools 옵션을 true로 설정합니다.

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

기본적으로 댓글과 주석은 이 보기에서 &quot;게스트&quot;로 표시됩니다. PDF 코드의 사용자 프로필 콜백을 [사용자 보기]에 등록하여 주석 및 주석에 대한 현재 검토자의 이름을 설정할 수 있습니다. 다음은 예제 프로필입니다. 사용자 인증을 포함하는 본격적인 응용 프로그램에서는, 로그인한 사용자 세션의 프로파일 정보를 이러한 방식으로 설정하여 검토 워크플로우에서 문서의 각 댓글을 식별할 수 있다.

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

## 문서 피드백 저장 중

사용자가 문서에 주석을 추가한 후 **저장을 클릭합니다.** 기본적으로 **저장**&#x200B;을 클릭하면 업데이트된 PDF 파일이 다운로드됩니다. 서버에 있는 현재 PDF 파일을 업데이트하려면 이 작업을 변경합니다.

업로드/임시 보관함 폴더에 있는 PDF 파일을 덮어쓰는 `/save` 끝점을 서버 코드에 추가합니다.

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

콘텐츠를 /save 엔드포인트에 업로드하는 SAVE_API에 대한 PDF 보기 콜백을 등록합니다. autoSaveFrequency 값을 변경하여 응용 프로그램이 타이머의 변경 내용을 자동으로 저장하고 완료 시 현재 포함된 파일에 추가 메타데이터를 포함할 수 있도록 할 수 있습니다.

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

이제 초안 문서의 주석과 주석이 서버에 저장됩니다. [콜백](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows)이 작업 과정에 적용되는 방식에 대해 자세히 알아볼 수 있습니다. 예를 들어 [상태 콜백](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback)은 여러 사람이 같은 문서를 동시에 검토하고 주석을 달고자 하는 경우 파일 충돌을 처리하는 데 도움이 됩니다.

마지막 단계에서는 PDF 서비스 API를 사용하여 편집된 모든 문서를 하나의 PDF 파일로 결합합니다.

## PDF 파일 결합 중

PDF 조합 코드는 PDF 생성 코드와 유사하지만 CombineFiles 작업을 사용하고 각 파일을 입력으로 추가합니다.

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

`uploads/drafts` 폴더 내의 모든 PDF 파일을 `Final.pdf` 파일로 결합하는 함수를 호출하는 /finalize라는 끝점을 추가한 다음 다운로드합니다.

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

마지막으로 index.html 웹 페이지의 링크를 이 /finalize 끝점에 추가합니다. 이 링크를 통해 사용자는 문서 워크플로우의 결과를 다운로드할 수 있습니다.

```
<a href="/finalize">Download final PDF</a>
```

![최종 문서 다운로드 스크린샷](assets/reviews_3.png)

## 다음 단계

이 실습용 튜토리얼에서는 [!DNL Acrobat Services] API가 [문서 공유 및 검토 워크플로](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html)를 웹 응용 프로그램에 통합하는 방법을 보여 주었습니다. 이 애플리케이션을 사용하면 원격 작업자들이 파일을 공유하고 팀원들과 협업할 수 있어 재택근무 중인 직원과 계약자들에게 특히 도움이 된다.

이러한 기술을 사용하여 앱에서 공동 작업을 활성화하거나 GitHub에서 [PDF 서비스 노드 SDK 샘플](https://github.com/adobe/pdftools-node-sdk-samples) 및 [PDF 포함 API 샘플](https://github.com/adobe/pdf-embed-api-samples)을 탐색하여 Adobe의 API를 사용하는 다른 방법에 대한 영감을 얻을 수 있습니다.

내 앱에서 문서 공유 및 검토를 활성화할 준비가 되셨습니까? [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 개발자 계정을 등록합니다. Adobe PDF Embed에 무료로 액세스하고, 다른 API에 대한 6개월 무료 체험판을 사용해 보십시오. 체험판 사용 후 비즈니스가 성장함에 따라 문서 트랜잭션당 \$0.05만 지불하면 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)할 수 있습니다.
