---
title: PDF 서비스 API를 사용하여 PDF을 Word, PowerPoint 등으로 내보내기
description: Node.js, Java 및 .Net 언어용 샘플 파일을 사용하여 PDF 서비스 API 내보내기 작업을 실행하는 방법에 대해 알아봅니다
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# PDF 서비스 API를 사용하여 PDF을 Word, PowerPoint 등으로 내보내기

![PDF 영웅 이미지 만들기](assets/ExportPDF_hero.jpg)

Adobe PDF Services API는 API를 사용하여 PDF 파일을 MS Office, 텍스트 및 이미지로 변환합니다. 콘텐츠 편집 및 분석을 위해 기존 PDF을 잠금 해제하는 데 사용되는 많은 일반적인 사용 사례가 있으며 PDF 서비스 API 개발자는 이 기능을 기존 시스템 및 애플리케이션에 쉽게 통합할 수 있습니다. PDF 파일을 MS Word로 변환하여 컨텐츠를 편집하고 승인을 받은 다음 나중에 서명을 받기 위해 보내 사용자 정의 계약 워크플로우를 만들 수 있습니다. 또는 송장 및 재무 계산 또는 데이터 분석을 위해 PDF 콘텐츠를 MS Excel 형식으로 내보냅니다.

내보내기 작업은 다음 PDF 파일 변환을 지원합니다.

* Microsoft Word(DOC, DOCX)로 PDF
* Microsoft PowerPoint(PPTX)에 PDF
* Microsoft Excel(XLSX)에 PDF
* 텍스트로 PDF(RTF)
* 이미지에 PDF (JPEG, PNG)

이 자습서에서는 Node.js, Java 및 .Net 언어용 샘플 파일을 사용하여 첫 번째 PDF 서비스 API 내보내기 작업을 실행하는 기본 사항을 알아봅니다.

## 1단계: 자격 증명을 만들고 환경 설정:

아래의 시작하기 튜토리얼을 사용하여 API 자격 증명을 만들고, 샘플 파일을 다운로드하고, 환경을 설정하십시오.

[PDF 서비스 API 및 Java 시작하기](gettingstartedjava.md)

[PDF 서비스 API 및 .Net 시작하기](gettingstartednet.md)

[PDF 서비스 API 및 Node.js 시작하기](createpdffromhtml.md)

## 2단계: 샘플 파일을 사용하여 pdf 내보내기 작업 실행

**Java**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. 다음 명령을 실행합니다.

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

PDF은 src/main/resources 디렉터리에 만들어집니다.

**.Net**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 디렉터리를 다시 ExportPDFtoDocx 디렉터리로 변경합니다.

1. 다음 명령을 실행합니다.

   `dotnet run ExportPDFToDocx.csproj`

PDF이 동일한 디렉토리에 생성됩니다.

**Node.js**

1. 명령 프롬프트를 엽니다.

1. 디렉터리를 샘플 코드 디렉터리로 변경합니다.

   예: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 다음 명령을 실행합니다.

   `node src/ocr/ocr-pdf.js`

PDF은 출력에 지정된 위치에 생성되며, 기본적으로 pdfServicesSdkResult 디렉터리가 됩니다.

## 마지막 생각

이제 기존 애플리케이션으로 가져와서 개념 증명을 시작할 수 있는 작업 예제가 있어야 합니다. 각 샘플 디렉터리에는 PDF 파일을 이미지 형식으로 내보내는 또 다른 샘플이 있습니다. 위의 동일한 단계를 통해 해당 샘플도 실행할 수 있습니다. 다른 형식으로 변경하려면 원하는 새 형식으로 코드를 업데이트할 수 있습니다.

SupportedTargetFormats.PPTX

그리고 대상 결과:

output/exportPdfOutput.PPTX

다른 형식으로

## 리소스 및 다음 단계

* 추가 도움말 및 지원은 [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 커뮤니티 포럼을 방문하세요.

* PDF 서비스 API [설명서](https://www.adobe.com/go/pdftoolsapi_doc)

* PDF 서비스 API 질문에 대한 [FAQ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197)

* 라이선스 및 가격에 대한 질문은 [문의하기](https://www.adobe.com/go/pdftoolsapi_requestform)
