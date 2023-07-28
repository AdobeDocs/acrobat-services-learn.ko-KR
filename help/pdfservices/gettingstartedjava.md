---
title: Adobe PDF 서비스 API 및 Java 시작하기
description: 개발자는 사용 가능한 모든 웹 서비스에 액세스하기 위해 제공되는 샘플 파일을 바로 실행하여 단 몇 분 내에 시작할 수 있습니다
type: Tutorial
role: Developer
level: Beginner
feature: PDF Services API
thumbnail: KT-6676.jpg
kt: 6676
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Adobe PDF 서비스 API 및 Java 시작하기

![PDF 메인 이미지 만들기](assets/GettingStartedJava_hero.jpg)

개발자는 사용 가능한 모든 웹 서비스에 액세스하기 위해 제공되는 샘플 파일을 바로 실행할 수 있으므로 몇 분 안에 시작할 수 있습니다. 이 자습서에서는 PDF 서비스 Java SDK를 사용하여 샘플 실행을 시작하는 모든 단계를 안내합니다.

## 1단계: 자격 증명 획득 및 샘플 파일 다운로드

첫 번째 단계는 사용 잠금을 해제할 자격 증명(API 키)을 얻는 것입니다. [여기에서 무료 평가판 등록](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 새 자격 증명을 만들려면 &#39;시작하기&#39;를 클릭하십시오.

![1단계](assets/GettingStartedJava_step1.png)

무료 평가판에 등록하려면 &#39;개인 계정&#39;을 선택하는 것이 중요합니다.

![개인](assets/GettingStartedJava_personal.png)

다음 단계에서 PDF 서비스 API 서비스를 선택한 다음 자격 증명에 대한 이름과 설명을 추가합니다.

&#39;개인화된 코드 샘플 만들기&#39; 확인란이 있습니다. 새 자격 증명을 프로젝트에 추가하는 수동 단계를 저장하는 샘플 파일에 자동으로 추가하려면 이 옵션을 선택합니다.

다음으로 Java를 언어로 선택하여 Java 관련 샘플을 받은 다음 &#39;자격 증명 만들기&#39; 단추를 클릭합니다.

![자격 증명](assets/GettingStartedJava_credentials.png)

PDFToolsSDK-JavaSamples.zip이라는 .zip 파일을 다운로드하여 로컬 파일 시스템에 저장할 수 있습니다.

## 2단계: Java 환경 설정

1. 설치 [Java 8 이상](https://www.oracle.com/java/technologies/javase-downloads.html) 아직 안하셨다면
1. 실행 `javac -version` 를 클릭하십시오.
1. JDK bin 폴더가 PATH 변수에 포함되어 있는지 확인합니다(메서드는 OS에 따라 다름).
1. 설치 [메이븐](https://maven.apache.org/install.html) 선호하는 도구를 사용합니다.

개인화된 샘플은 바로 실행 가능한 샘플 코드, 포함된 자격 증명 json 파일, 사전 구성된 연결에서 종속성에 이르는 모든 것을 제공합니다.

1. 다운로드 [샘플 프로젝트](https://github.com/adobe/pdftools-java-sdk-samples).
1. Maven과 함께 샘플 프로젝트 만들기: mvn 새로 설치
1. 명령줄 또는 원하는 IDE에서 샘플 코드를 테스트합니다.

## 최종 생각

PDF 서비스 API를 사용하면 일반적인 워크플로우를 자동화하고 처리 부담을 클라우드로 전환하여 수동 프로세스를 제거할 수 있습니다. 모든 브라우저가 PDF을 다르게 취급하는 환경에서 PDF 서비스 API와 함께 Adobe PDF Embed API를 활용하면 올바로 실행 및 표시되는 간소화되고 안정적이며 예측 가능한 프로세스를 만들 수 있습니다 **매번** 플랫폼 또는 디바이스에 관계없이 가능합니다.

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼

* PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) PDF 서비스 API 질문에 대한

* [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform) 라이센스 및 가격 관련 질문

* 관련 기사

  [새로운 PDF 서비스 API는 문서 워크플로우를 위한 더욱 다양한 기능을 제공합니다](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7월 릴리스 [!DNL Adobe Acrobat Services]: PDF 포함 및 PDF 서비스](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
