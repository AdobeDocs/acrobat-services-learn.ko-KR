---
title: Adobe PDF Services API 및 Java 시작하기
description: 개발자는 사용 가능한 모든 웹 서비스에 액세스할 수 있도록 제공되는 샘플 파일을 바로 실행할 수 있으므로 시작할 수 있습니다
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6676
thumbnail: KT-6676.jpg
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Adobe PDF Services API 및 Java 시작하기

![PDF 영웅 이미지 만들기](assets/GettingStartedJava_hero.jpg)

개발자는 사용 가능한 모든 웹 서비스에 액세스할 수 있도록 제공되는 샘플 파일을 바로 실행할 수 있으므로 시작할 수 있습니다. 이 자습서는 PDF 서비스 Java SDK를 사용하여 샘플 실행을 시작하는 모든 단계를 안내합니다.

## 1단계: 자격 증명 얻기 및 샘플 파일 다운로드

첫 번째 단계는 사용 잠금을 해제할 자격 증명(API 키)을 얻는 것입니다. [여기에서 무료 체험판을 등록](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)하고 &#39;시작하기&#39;를 클릭하여 새 자격 증명을 만드세요.

![단계 1](assets/GettingStartedJava_step1.png)

무료 체험판을 등록하려면 &#39;개인 계정&#39;을 선택하는 것이 중요합니다.

![개인](assets/GettingStartedJava_personal.png)

다음 단계에서는 PDF 서비스 API 서비스를 선택한 다음 자격 증명에 대한 이름 및 설명을 추가합니다.

&#39;개인화된 코드 샘플 만들기&#39; 확인란이 있습니다. 이 옵션을 선택하면 새 자격 증명이 샘플 파일에 자동으로 추가되어 프로젝트에 자격 증명을 추가하는 수동 단계가 저장됩니다.

그런 다음 Java 관련 샘플을 받을 언어로 Java를 선택한 다음 &#39;자격 증명 생성&#39; 단추를 누릅니다.

![자격 증명](assets/GettingStartedJava_credentials.png)

다운로드하기 위해 로컬 파일 시스템에 저장할 수 있는 PDFToolsSDK-JavaSamples.zip 이라는 .zip 파일을 받게 됩니다.

## 2단계: Java 환경 설정

1. 아직 설치하지 않았다면 [Java 8 이상](https://www.oracle.com/java/technologies/javase-downloads.html)을(를) 설치하십시오.
1. `javac -version`을(를) 실행하여 설치를 확인하십시오.
1. JDK bin 폴더가 PATH 변수에 포함되어 있는지 확인합니다(방법은 OS에 따라 다름).
1. 아직 설치하지 않았다면 기본 도구를 사용하여 [Maven](https://maven.apache.org/install.html)을 설치하십시오.

개인화된 샘플은 실행 준비 샘플 코드, 포함된 자격 증명 json 파일 및 미리 구성된 연결에서 종속성에 이르기까지 모든 것을 제공합니다.

1. [샘플 프로젝트](https://github.com/adobe/pdftools-java-sdk-samples)를 다운로드합니다.
1. Maven: mvn clean install을 사용하여 샘플 프로젝트를 빌드합니다.
1. 명령줄 또는 선호하는 IDE에서 샘플 코드를 테스트합니다.

## 마지막 생각

PDF 서비스 API는 공통 워크플로우를 자동화하고 처리 부담을 클라우드로 전환하여 수동 프로세스를 제거하는 데 도움이 됩니다. 모든 브라우저가 PDF을 다르게 취급하는 환경에서 PDF 서비스 API와 함께 Adobe PDF Embed API를 사용하면 플랫폼이나 디바이스에 관계없이 **매번**&#x200B;올바르게 실행 및 표시되는 효율적이고 안정적이며 예측 가능한 프로세스를 만들 수 있습니다.

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼에서 확인하십시오

* PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* PDF 서비스 API 질문에 대한 [FAQ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197)

* 라이선스 및 가격에 대한 질문은 [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform)

* 관련 기사

  [새로운 PDF 서비스 API는 문서 작업 과정에 더 많은 기능을 제공합니다](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7월 릴리스 [!DNL Adobe Acrobat Services]: PDF 포함 및 PDF 서비스](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
