---
title: Adobe PDF 서비스 API를 사용하여 OCR PDF 파일
description: OCR(Optical Character Recognition)을 사용하면 스캔한 PDF의 잠금을 해제하여 텍스트를 추출하고 검색 가능한 파일을 만들 수 있습니다
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6677.jpg
kt: 6677
keywords: 영웅
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 2%

---

# Adobe PDF 서비스 API를 사용하여 OCR PDF 파일

![PDF 메인 이미지 만들기](assets/OCR_hero.jpg)

OCR(Optical Character Recognition)을 사용하면 스캔한 PDF의 잠금을 해제하여 텍스트를 추출하고 검색 가능한 파일을 만들 수 있습니다. 강력한 클라우드 기반의 API를 사용하면 OCR을 모든 문서 워크플로우에 통합하여 텍스트 보관, 복사, 검색 가능한 문서 색인 작성 등의 작업을 수행할 수 있습니다. 스캔한 PDF 저장소에서 검색 가능한 아카이브를 생성하여 중요한 정보의 잠금을 해제하고 빠른 검색으로 시간을 절약할 수 있습니다. 또한 업로드된 스캔에서 PDF에 OCR을 적용하여 온보딩 워크플로우에서 사용할 수 있도록 편집할 수 있습니다.

개발자는 OCR용으로 제공되는 샘플 파일을 바로 실행할 수 있으므로 몇 분 안에 시작할 수 있습니다.

이 자습서에서는 Node.js, Java 및 .Net 언어용 샘플 파일을 사용하여 첫 번째 PDF 서비스 API OCR 작업을 실행하는 방법에 대한 기본 사항을 다룹니다.

## 1단계: 자격 증명 만들기 및 환경 설정

아래 시작하기 자습서를 사용하여 API 자격 증명을 만들고, 샘플 파일을 다운로드하고, 환경을 설정합니다.

[PDF 서비스 API 및 Java 시작하기](gettingstartedjava.md)
[PDF 서비스 API 및 .Net 시작하기](gettingstartednet.md)
[PDF 서비스 API 및 Node.js 시작하기](createpdffromhtml.md)

## 샘플 파일에 제공된 OCR 예제 실행

OCR 작업에서는 기본적으로 영어 로케일을 허용하지만 독일어, 프랑스어, 덴마크어 및 [기타 언어](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language). 기본값은 en-us 로케일입니다.

특정 로캘을 포함하여 OCR 작업으로 옵션을 전달하면 메서드에서도 두 가지 옵션이 있는 &#39;type&#39; 매개 변수를 받습니다.

* SEARCHABLE_IMAGE: 보이지 않는 텍스트 레이어를 배치하기 전에 정리 프로세스 중에(예: 기울이기 해제) 원본 이미지를 수정합니다. 이 유형은 원치 않는 아티팩트를 제거하고 일부 시나리오에서 더 읽기 쉬운 문서를 만들 수 있습니다.

* SEARCHABLE_IMAGE_EXACT: 텍스트를 검색하고 선택할 수 있습니다. 이 옵션은 원본 이미지를 유지하고 보이지 않는 텍스트 레이어를 그 위에 배치합니다. 원본 이미지에 대한 충실도를 최대화해야 하는 경우에 권장됩니다.

**Java**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>.

1. 다음 명령을 실행합니다.

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

PDF이 src/main/resources 디렉터리에 생성됩니다.

**.Net**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 디렉토리를 다시 OcrPDF 디렉토리로 변경합니다.

1. 다음 명령을 실행합니다.

   `dotnet run OcrPDF.csproj`

PDF이 동일한 디렉터리에 생성됩니다.

**Node.js**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 다음 명령을 실행합니다.

   `node src/ocr/ocr-pdf.js`

PDF은 출력에 지정된 위치에 생성됩니다. 이 위치는 기본적으로 출력 디렉토리입니다.

## 최종 생각

샘플 파일을 사용하는 간단한 단계를 통해 빌드할 수 있는 작업 예제가 있어야 합니다. 이 튜토리얼에서 사용한 OCR 예제 외에도 앞에서 설명한 지원되는 유형 및 로케일 옵션을 사용하여 OCR을 수행하는 또 다른 예제가 있습니다.

여기에서 샘플에 있는 입력 및 출력 파일을 간단히 교체하여 자신의 PDF을 사용하여 자신의 사용 사례에 대한 개념 증명을 완료할 수 있습니다.

![개념 증명](assets/OCR_poc.png)

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼

* PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) PDF 서비스 API 질문에 대한

* [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform) 라이센스 및 가격 관련 질문
