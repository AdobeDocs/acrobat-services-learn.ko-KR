---
title: 직원 온보딩 현대화
description: ' [!DNL Adobe Acrobat Services] API를 사용하여 직원 온보딩을 현대화하는 방법을 알아봅니다.'
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10203
thumbnail: KT-10203.jpg
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 0%

---

# 직원 온보딩 현대화

![사례 영웅 배너 사용](assets/usecaseemployeeonboardinghero.jpg)

대규모 조직에서는 직원 온보딩이 대규모일 수 있고, 처리 속도가 느릴 수 있습니다. 일반적으로 새 직원이 제시하고 서명해야 하는 비대화형 자료와 함께 사용자 정의된 문서가 혼합되어 있습니다. 이러한 맞춤형 및 비대화형 재료의 혼합은 여러 단계를 필요로 하며, 이를 통해 프로세스에 관련된 사람들로부터 귀중한 시간을 절약할 수 있습니다. [!DNL Adobe Acrobat Services] 및 Acrobat Sign은 이 접근 방식을 현대화하고 자동화하여 HR 개인에게 보다 중요한 작업을 맡길 수 있도록 합니다. 어떻게 이것이 이루어졌는지 살펴보도록 하자.

## [!DNL Adobe Acrobat Services]이란 무엇입니까?

[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage)은(는) PDF이 아닌 문서 작업과 관련된 API 집합입니다. 일반적으로 이 서비스 집합은 다음과 같은 세 가지 주요 범주로 분류됩니다.

* 먼저 [PDF 서비스](https://developer.adobe.com/document-services/apis/pdf-services/) 도구 집합입니다. PDF 및 기타 문서 작업을 위한 &quot;유틸리티&quot; 방법입니다. 서비스에는 PDF으로 또는 PDF에서 변환, OCR 및 최적화 수행, 페이지 병합 및 분할 등의 작업이 포함됩니다. 문서 처리 기능의 도구 상자입니다.
* [PDF 추출 API](https://developer.adobe.com/document-services/apis/pdf-extract/)는 강력한 AI/ML 기술을 사용하여 PDF을 분석하고 내용에 대한 엄청난 양의 세부 사항을 반환합니다. 여기에는 텍스트, 스타일 및 위치 정보가 포함되며 CSV/XLS 형식의 표 형식 데이터를 반환하고 이미지를 검색할 수도 있습니다.
* 마지막으로 [문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation/)를 사용하면 개발자가 Microsoft Word를 &quot;템플릿&quot;으로 사용하고, 모든 소스에서 얻은 데이터와 혼합하고, 동적 개인 맞춤화된 문서(PDF 및 Word)를 생성할 수 있습니다.

개발자는 [등록](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html)하고 무료 체험판으로 이러한 서비스를 모두 사용할 수 있습니다. [!DNL Acrobat Services] 플랫폼은 REST 기반 API를 사용하지만 노드, Java, .NET 및 Python에 대한 SDK도 지원합니다(현재에만 추출).

API는 아니지만 개발자는 웹 페이지에서 문서를 일관되고 유연하게 볼 수 있는 [PDF 포함 API](https://developer.adobe.com/document-services/apis/pdf-embed/)를 무료로 사용할 수도 있습니다.

## Acrobat Sign 소개

[Acrobat Sign](https://www.adobe.com/acrobat/business/sign.html)은(는) 전자 서명 서비스의 세계 선두주자입니다. 여러 개의 서명을 포함하여 다양한 작업 과정을 사용하여 서명을 받을 문서를 보낼 수 있습니다. Acrobat Sign은 서명 및 추가 정보가 필요한 워크플로우도 지원합니다. 이러한 모든 기능은 유연한 저작 시스템을 갖춘 강력한 대시보드에서 지원됩니다.

[!DNL Acrobat Services]과(와) 마찬가지로 Acrobat Sign에는 개발자가 대시보드와 사용하기 쉬운 REST 기반 API를 통해 서명 프로세스를 테스트할 수 있는 [무료 체험판](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial)이 있습니다.

## 온보딩 시나리오

Adobe의 서비스가 어떻게 도움이 될 수 있는지를 보여주는 현실적인 시나리오를 생각해 보자. 신입사원이 입사하면 자신의 역할에 맞는 맞춤형 정보가 필요하다. 또한 전사적 소재도 필요합니다. 마지막으로, 그들은 문서에 서명함으로써 기업 정책을 수용해야 한다. 이를 구체적인 단계로 나누어보겠습니다.

* 우선 신입사원을 이름으로 맞이하는 맞춤형 자기소개서가 필요하다. 서신에는 직원의 이름, 역할, 급여 및 위치에 대한 정보가 포함되어야 합니다.
* 맞춤형 서신은 기본적인 전사적 정보(다양한 HR 정책, 혜택 등을 고려함)가 포함된 PDF과 결합해야 합니다.
* 직원의 서명과 날짜를 요구하는 최종 문서가 포함되어야 합니다.
* 위의 모든 내용은 서명을 위해 직원에게 전송된 하나의 문서로 표시되어야 합니다.

이 작업을 수행하는 방법에 대한 세부 사항을 살펴보겠습니다.

## 동적 문서 생성

Adobe의 [문서 생성](https://developer.adobe.com/document-services/apis/doc-generation/) API를 사용하면 개발자가 PDF 및 Word 문서를 생성하기 위한 기반으로 Microsoft Word와 간단한 템플릿 언어를 사용하여 동적 문서를 만들 수 있습니다. 다음은 이러한 작동 방식에 대한 예입니다.

값이 하드 코딩된 Word 문서로 시작하겠습니다. 그래픽, 표 등 문서에 원하는 스타일을 지정할 수 있습니다. 여기 첫 번째 문서가 있습니다.

![초기 문서의 스크린샷](assets/onboarding_1.png)

문서 생성은 데이터로 대체되는 Word 문서에 &quot;토큰&quot;을 추가하여 작동합니다. 이러한 토큰은 수동으로 입력할 수 있지만 [Microsoft Word 추가 기능](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/)이 있으므로 이 작업을 보다 쉽게 수행할 수 있습니다. 문서를 열면 작성자가 문서에서 사용할 수 있는 태그나 데이터 세트를 정의할 수 있는 도구가 제공됩니다.

![문서 Tagger의 스크린샷](assets/onboarding_2.png)

로컬 파일에서 JSON 정보를 업로드하거나 JSON 텍스트로 복사하거나 초기 데이터로 계속 진행하도록 선택할 수 있습니다. 이렇게 하면 특정 요구 사항에 따라 임시방편으로 태그를 정의할 수 있습니다. 이 예제에서는 이름, 역할, 급여 및 위치에 대한 태그만 필요합니다. 이 작업은 **태그 만들기** 단추를 사용하여 수행됩니다.

![태그 정의 스크린샷](assets/onboarding_3.png)

첫 번째 태그를 정의한 후 필요한 만큼 계속 정의할 수 있습니다.

![정의된 태그의 스크린샷](assets/onboarding_4.png)

태그를 정의한 상태에서 문서에서 텍스트를 선택하고 해당하는 경우 태그로 대체합니다. 이 예제에서는 이름, 역할 및 급여에 대한 태그가 추가됩니다.

![태그 스크린샷](assets/onboarding_5.png)

문서 생성은 단순 태그뿐만 아니라 논리적 표현식도 지원합니다. 이 문서의 두 번째 단락에는 루이지애나 사람들에게만 적용되는 텍스트가 있습니다. Document Tagger의 고급 탭으로 이동하여 조건을 정의하여 조건부 표현식을 추가할 수 있습니다. 다음은 간단한 같음 조건을 정의하는 방법이지만 숫자 비교와 다른 비교 유형도 지원된다는 점에 유의하십시오.

![조건 스크린샷](assets/onboarding_6.png)

그런 다음 단락에 다음과 같이 삽입 및 감싸기를 적용할 수 있습니다.

![문서의 조건 스크린샷](assets/onboarding_7.png)

작동 방식을 테스트하려면 **문서 생성**&#x200B;을 선택하십시오. 최초 로그인 시 Adobe ID으로 로그인해야 합니다. 로그인 후 수동으로 편집할 수 있는 기본 JSON이 표시됩니다.

![데이터 스크린샷](assets/onboarding_8.png)

이후 보거나 다운로드할 수 있는 PDF이 생성됩니다.

![생성된 PDF 스크린샷](assets/onboarding_9.png)

Document Tagger를 사용하면 빠르게 디자인하고 테스트할 수 있지만, 완료 후 프로덕션 중에 SDK 중 하나를 사용하여 이 프로세스를 자동화할 수 있습니다. 실제 코드는 특정 요구 사항에 따라 다르지만 이 코드가 Node.js에서 어떻게 표시되는지에 대한 예시는 다음과 같습니다.

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

간단히 말해, 코드는 자격 증명을 설정하고 작업 개체를 만들고 입력 및 옵션을 설정한 다음 작업을 호출합니다. 마지막으로 PDF으로 결과를 저장합니다. 결과는 Word로도 출력할 수 있습니다.

문서 생성은 완전히 역동적인 표와 이미지를 갖는 기능을 포함하여 훨씬 더 복잡한 사용 사례를 지원합니다. 자세한 내용은 [설명서](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)를 참조하십시오.

## PDF 작업 수행

[PDF 서비스 API](https://developer.adobe.com/document-services/apis/pdf-services/)는 PDF 작업을 위한 다양한 &quot;유틸리티&quot; 작업을 제공합니다. 이러한 작업에는 다음이 포함됩니다.

* Office 문서에서 PDF 만들기
* Office 문서로 PDF 내보내기
* PDF 결합 및 분할
* PDF에 OCR 적용
* PDF에 대한 보호 설정, 제거 및 수정
* 페이지 삭제, 삽입, 재정렬 및 회전
* 압축 또는 선형화를 통한 PDF 최적화
* PDF 속성을 가져오는 중

이 시나리오에서는 문서 생성 호출의 결과를 표준 PDF에 병합해야 합니다. 이 작업은 SDK를 사용하여 매우 간단합니다. 다음은 Node.js 의 예입니다.

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

이 코드는 두 PDF을 가져와 병합한 다음 결과를 새 PDF에 저장합니다. 간단하고 간단합니다! 수행할 수 있는 작업의 예는 [문서](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/)를 참조하세요.

## 서명 프로세스

온보딩 프로세스의 최종 중단에서 직원은 자신이 내부에 정의된 모든 정책을 읽고 동의한다는 내용의 계약에 서명해야 합니다. [Acrobat Sign](https://www.adobe.com/acrobat/business/sign.html)은(는) [API](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html)를 통해 자동화된 워크플로와 통합을 포함하여 다양한 워크플로 및 통합을 지원합니다. 대략적으로 말하면, 시나리오의 마지막 부분은 다음과 같이 완성될 수 있다:

먼저 서명이 필요한 양식이 포함된 문서를 디자인합니다. 이 작업에는 Adobe Sign 사용자 대시보드에서 설계된 시각적 개체를 포함하여 다양한 방법이 있습니다. 또 다른 옵션은 문서 생성 Word 추가 기능을 사용하여 태그를 삽입하는 것입니다. 이 예에서는 서명과 날짜를 요청합니다.

![서명 태그가 있는 문서의 스크린샷](assets/onboarding_10.png)

이 문서는 PDF으로 저장할 수 있으며 위에서 설명한 방법을 사용하여 모든 문서와 함께 조인할 수 있습니다. 이 프로세스는 개인화된 인사말, 표준 기업 문서, 서명에 적합한 최종 페이지가 포함된 하나의 조화로운 패키지를 만듭니다.

템플릿을 Acrobat Sign 대시보드에 업로드한 다음 새 계약에 사용할 수 있습니다. REST API를 사용하면 이 문서를 잠재 직원에게 전송하여 서명을 요청할 수 있습니다.

![서명된 문서의 스크린샷](assets/onboarding_11.png)

## 직접 경험해 보기

이 문서에 설명된 모든 것은 지금 테스트될 수 있습니다. [!DNL Adobe Acrobat Services] API [무료 체험판](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html)에서는 현재 6개월 동안 1,000개의 무료 요청을 제공합니다. Acrobat Sign [무료 체험판](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial)을 사용하면 워터마크가 있는 계약을 테스트할 목적으로 보낼 수 있습니다.

궁금한 점이 있으십니까? [지원 포럼](https://community.adobe.com/t5/acrobat-services-api/ct-p/ct-Document-Cloud-SDK)은 Adobe 개발자와 지원 담당자가 매일 모니터링합니다. 일반적으로 더 많은 영감을 얻으려면 다음 [종이 클립](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) 에피소드를 꼭 잡으세요. 뉴스와 데모, 고객과의 상담이 있는 정기적인 라이브 미팅이 있다.
