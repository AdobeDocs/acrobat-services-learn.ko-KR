---
title: Adobe PDF 서비스 API 및 .Net 시작하기
description: 개발자는 사용 가능한 모든 웹 서비스에 액세스하기 위해 제공되는 샘플 파일을 바로 실행하여 단 몇 분 내에 시작할 수 있습니다
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6675.jpg
jira: KT-6675
keywords: 추천
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Adobe PDF 서비스 API 및 .Net 시작하기

![PDF 메인 이미지 만들기](assets/GettingStartedJava_hero.jpg)

개발자는 사용 가능한 모든 웹 서비스에 액세스하기 위해 제공되는 샘플 파일을 바로 실행할 수 있으므로 몇 분 안에 시작할 수 있습니다. 이 자습서에서는 PDF 서비스 .Net SDK를 사용하여 샘플 실행을 시작하는 모든 단계를 안내합니다.

## 1단계: 자격 증명 획득 및 샘플 파일 다운로드

첫 번째 단계는 사용 잠금을 해제할 자격 증명(API 키)을 얻는 것입니다. [여기에서 무료 평가판 등록](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 새 자격 증명을 만들려면 &#39;시작하기&#39;를 클릭하십시오.

![1단계](assets/GettingStartedJava_step1.png)

무료 평가판에 등록하려면 &#39;개인 계정&#39;을 선택하는 것이 중요합니다.

![개인](assets/GettingStartedJava_personal.png)

다음 단계에서 PDF 서비스 API 서비스를 선택한 다음 자격 증명에 대한 이름과 설명을 추가합니다.

&#39;개인화된 코드 샘플 만들기&#39; 확인란이 있습니다. 새 자격 증명을 프로젝트에 추가하는 수동 단계를 저장하는 샘플 파일에 자동으로 추가하려면 이 옵션을 선택합니다.

다음으로 Node.js를 언어로 선택하여 Node.js 특정 샘플을 받은 다음 &#39;자격 증명 만들기&#39; 단추를 클릭합니다.

![자격 증명](assets/GettingStartedJava_credentials.png)

PDFToolsSDK-.NetSamples.zip이라는 .zip 파일을 다운로드하여 로컬 파일 시스템에 저장할 수 있습니다.

## 2단계: .Net 환경 설정 및 샘플 코드 실행

1. 다운로드 및 설치 [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. 다운로드한 파일 추출 **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** 콘텐츠의 압축을 풉니다.
1. cd를 샘플 루트 디렉토리로 **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. 샘플 루트 디렉터리에서 `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>dotnet build

   이제 샘플 파일을 실행할 준비가 되었습니다!

   다음 마지막 단계에서는 Word에서 PDF 만들기 작업으로 첫 번째 샘플을 실행하는 방법을 보여 줍니다.

1. 샘플 루트 디렉토리에서 CreatePDFFromDocx 폴더로 디렉토리 변경, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. 실행 `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet run CreatePDFFromDocx.csproj

PDF은 출력에 지정된 위치에 생성되며, 기본적으로 동일한 폴더입니다.

## 최종 생각

PDF 서비스 API를 사용하면 일반적인 워크플로우를 자동화하고 처리 부담을 클라우드로 전환하여 수동 프로세스를 제거할 수 있습니다. 모든 브라우저가 PDF을 다르게 취급하는 환경에서 PDF 서비스 API와 함께 Adobe PDF Embed API를 활용하면 올바로 실행 및 표시되는 간소화되고 안정적이며 예측 가능한 프로세스를 만들 수 있습니다 **매번** 플랫폼 또는 디바이스에 관계없이 가능합니다.

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼

* PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) PDF 서비스 API 질문에 대한

* [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform) 라이센스 및 가격 관련 질문

* 관련 기사

  [새로운 PDF 서비스 API는 문서 워크플로우를 위한 더욱 다양한 기능을 제공합니다](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7월 릴리스 [!DNL Adobe Acrobat Services]: PDF 포함 및 PDF 서비스](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
