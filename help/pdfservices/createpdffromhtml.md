---
title: PDF 서비스 API와 Node.js를 통해 몇 분 안에 HTML 또는 MS Office에서 PDF 생성
description: PDF 서비스 API 내에는 PDF을 만들고 조작하거나 PDF에서 MS Office 및 기타 형식으로 내보낼 수 있는 여러 가지 서비스가 있습니다
type: Tutorial
role: Developer
level: Beginner
feature: PDF Services API
thumbnail: KT-6673.jpg
jira: KT-6673
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# PDF 서비스 API와 Node.js를 통해 몇 분 안에 HTML 또는 MS Office에서 PDF 생성

![PDF 메인 이미지 만들기](assets/createpdffromhtml_hero.jpg)

개발자가 복잡한 비즈니스 워크플로우의 요구 사항을 충족하기 위해 여러 강력한 PDF 조작 서비스 중에서 자유롭게 선택하고 선택할 수 있는 새로운 Adobe PDF 서비스 API를 사용하여 문서 워크플로우를 디지털화하는 것이 그 어느 때보다 쉬워졌습니다. 쉽게 사용할 수 있는 클라우드 기반 웹 서비스를 통해 복잡한 아키텍처, 구현 전략 및 기술 향상을 간소화할 수 있습니다.

PDF 서비스 API 내에는 PDF을 만들고 조작하거나 PDF에서 MS Office 및 기타 형식으로 내보낼 수 있는 여러 가지 서비스가 있습니다.

* 정적 또는 동적 HTML, MS Word, PowerPoint, Excel 등에서 PDF 파일 만들기
* MS Word, PowerPoint, Excel 등으로 Export PDF
* PDF 파일에서 텍스트를 인식하고 문서 검색을 활성화하는 OCR
* 문서를 열 때 암호가 있는 Protect PDF
* PDF 페이지 또는 PDF 문서를 단일 PDF으로 결합
* PDF을 압축하여 이메일 또는 온라인을 통해 공유할 수 있는 크기를 줄입니다
* 선형화를 통해 웹에서 빠르게 볼 수 있도록 PDF 최적화
* 서비스 삽입, 교체, 재정렬, 삭제 및 회전으로 PDF 페이지 구성

개발자는 사용 가능한 모든 웹 서비스에 액세스하기 위해 제공되는 샘플 파일을 바로 실행할 수 있으므로 몇 분 안에 시작할 수 있습니다. 시작 방법

## 자격 증명 획득 및 샘플 파일 다운로드

첫 번째 단계는 사용 잠금을 해제할 자격 증명(API 키)을 얻는 것입니다. [여기에서 무료 평가판 등록](https://www.adobe.com/go/dcsdks_credentials) 새 자격 증명을 만들려면 &#39;시작하기&#39;를 클릭하십시오.

![API 키](assets/apikey.png)

무료 평가판에 등록하려면 &#39;개인 계정&#39;을 선택하는 것이 중요합니다.

![개인 계정](assets/personalaccount.png)

다음 단계에서 PDF 서비스 API 서비스를 선택한 다음 자격 증명에 대한 이름과 설명을 추가합니다.

&#39;개인화된 코드 샘플 만들기&#39; 확인란이 있습니다. 이 옵션을 선택하면 수동 단계를 건너뛰고 샘플 파일에 새 자격 증명이 자동으로 추가됩니다.

다음으로 Node.js를 언어로 선택하여 Node.js 특정 샘플을 받은 다음 &#39;자격 증명 만들기&#39; 단추를 클릭합니다.

![자격 증명 만들기](assets/createcredentials.png)

로컬 파일 시스템에 저장할 수 있는 PDFToolsSDK-Node.jsSamples.zip이라는 .zip 파일을 다운로드하여 받을 수 있습니다.

## 코드 샘플에 자격 증명 추가

&#39;개인화된 코드 샘플 만들기&#39; 옵션을 선택한 경우 클라이언트 ID를 코드 샘플 파일에 수동으로 추가할 필요가 없으며 다음 단계를 건너뛰고 아래의 실행 중인 코드 샘플 섹션으로 바로 이동할 수 있습니다.

&#39;개인화된 코드 샘플 생성&#39; 옵션을 선택하지 않은 경우 User.io Console에서 클라이언트 ID(API 키)를 복사해야 합니다.

![코드 샘플](assets/codesample.png)

PDFToolsSDK-Node.jsSamples.zip의 압축을 해제합니다.

adobe-dc-pdf-tools-sdk-node-samples 폴더 아래의 루트 디렉터리로 이동합니다.

텍스트 편집기 또는 IDE를 사용하여 pdftools-api-credentials.json을 엽니다.

코드에서 클라이언트 ID에 대한 필드에 자격 증명을 붙여넣습니다.

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

파일을 저장하고 다음 단계로 진행하여 코드 샘플을 실행합니다.

## 첫 번째 코드 샘플 실행

명령 프롬프트를 사용하여 adobe-dc-pdf-tools-sdk-node-samples 폴더 아래의 루트 디렉터리로 이동합니다.

npm install 을 입력합니다.

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm 설치

이제 샘플 파일을 실행할 준비가 되었습니다!

첫 번째 샘플의 경우 PDF을 만듭니다.

명령 프롬프트에 있는 동안 다음 명령을 사용하여 create PDF 샘플을 실행합니다.

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>노드 src/createpdf/create-pdf-from-docx.js

예제 출력:

![예제 출력](assets/exampleoutput.png)

PDF은 출력에 지정된 위치에 만들어집니다. 이 위치는 기본적으로 pdfServicesSdkResult 디렉터리입니다.

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼

PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) PDF 서비스 API 질문에 대한

* [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform) 라이센스 및 가격 관련 질문

* 관련 기사:
  [새로운 PDF 서비스 API는 문서 워크플로우를 위한 더욱 다양한 기능을 제공합니다](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7월 릴리스 [!DNL Adobe Acrobat Services]: PDF 포함 및 PDF 서비스](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
