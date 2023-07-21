---
title: 세일즈 프로세스 가속화
description: 문서 경험과 [!DNL Adobe Acrobat Services]
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10222.jpg
jira: KT-10222
exl-id: 9430748f-9e2a-405f-acac-94b08ad7a5e3
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 0%

---

# 세일즈 프로세스 가속화

![사례 메인 배너 사용](assets/UseCaseAccelerateSalesHero.jpg)

백서부터 계약 및 계약에 이르기까지 구매 여정 전반에서 수많은 문서가 필요합니다. 이 튜토리얼에서는 [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/) 는 이 여정 전반에서 문서 경험을 통합하여 판매를 촉진할 수 있습니다.

## 데이터에서 계약 및 판매 주문 생성

판매 계약서, 계약서 및 기타 문서는 특정 기준에 따라 크게 달라질 수 있습니다. 예를 들어 판매 계약에는 특정 국가 또는 주에 있거나 계약의 일부로 특정 제품을 포함하는 등 고유한 기준에 따라 특정 약관만 포함될 수 있습니다. 이러한 문서를 수동으로 작성하거나 다양한 템플릿 버전을 유지 관리하면 변경 사항을 수동으로 검토하는 데 드는 비용을 크게 늘릴 수 있습니다.

[Adobe 문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation/) crm 또는 다른 데이터 시스템에서 데이터를 가져와 해당 데이터를 기반으로 세일즈 문서를 동적으로 생성할 수 있습니다.

## 자격 증명 가져오기

무료 Adobe PDF 서비스 자격 증명을 등록하여 시작합니다.

1. 탐색 [여기](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) 자격 증명을 등록하려면
1. Adobe ID으로 로그인합니다.
1. 자격 증명 이름(예: Sales Agreemo)을 설정합니다.

   ![자격 증명 이름 설정 스크린샷](assets/accsales_1.png)

1. 샘플 코드를 다운로드할 언어를 선택합니다(예: Node.js).
1. 동의 확인 **[!UICONTROL 개발자 약관]**.
1. 선택 **[!UICONTROL 자격 증명 만들기]**.
샘플 파일, pdfservices-api-credentials.json 및 인증을 위한 private.key가 포함된 ZIP 파일로 파일이 컴퓨터에 다운로드됩니다.

   ![자격 증명 스크린샷](assets/accsales_2.png)

1. 선택 **[!UICONTROL Microsoft Word 추가 기능 다운로드]** 또는 [AppSource](https://appsource.microsoft.com/en-cy/product/office/WA200002654) 설치합니다.

   >[!NOTE]
   >
   >Word 추가 기능을 설치하려면 Microsoft 365 내에서 추가 기능을 설치할 권한이 있어야 합니다. 권한이 없는 경우 Microsoft 365 관리자에게 문의하십시오.

## 데이터

특정 데이터 시스템에서 데이터를 가져오는 경우 해당 데이터를 JSON 데이터로 출력하거나 고유한 스키마를 생성해야 합니다. 이 시나리오는 미리 생성된 다음과 같은 샘플 데이터 세트를 사용합니다.

```
{
    "salesOrder": {
        "comment": "Make sure to call 555-555-1234 when you arrive. The front door is broken."
    },
    "company": {
        "name":"Home Services Co.",
        "address": {
            "city": "Homestead",
            "state": "NY",
            "zip": "14623",
            "streetAddress": "123 Demohome Street"
        }
    },
    "customer": {
        "address": {
            "city": "Seattle",
            "state": "WA",
            "zip": "98052",
            "streetAddress": "20341 Whitworth Institute 405 N. Whitworth"
        },
        "email": "mailto:jane-doe@xyz.edu",
        "jobTitle": "Professor",
        "name": "Jane Doe",
        "telephone": "(425) 123-4567",
        "url": "http://www.janedoe.com"
    },
    "tax": {
        "state":"WA",
        "rate": 0.08
    },
    "referencesOrder": [
        {
            "description": "Carpet Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 359.54
            },
            "orderedItem": {
                "description": "Carpet Cleaning Service"
            }
        },
        {
            "description": "Home Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 299.99
            },
            "orderedItem": {
                "description": "House Cleaning Service"
            }
        }
    ]
}
```

## 문서에 기본 태그 추가

이 시나리오에서는 다운로드할 수 있는 판매 주문 문서를 사용합니다 [여기](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/SalesOrder/Exercise/SalesOrder_Base.docx?raw=true).

![샘플 판매 주문 문서의 스크린샷](assets/accsales_3.png)

1. 열기 *SalesOrder.docx* Microsoft Word의 샘플 문서.
1. 문서 생성 플러그인이 설치되어 있는 경우 **[!UICONTROL 문서 생성]** 있습니다. 리본에 문서 생성이 표시되지 않으면 다음 지침을 따르십시오.
1. 선택 **[!UICONTROL 시작하기]**.
1. 위에 기록된 JSON 샘플 데이터를 *JSON 데이터* 필드에 표시됩니다.

   ![JSON 데이터 복사 스크린샷](assets/accsales_4.png)

문서 생성 태거 패널로 이동하여 문서에 태그를 배치합니다.

1. 바꿀 텍스트를 선택합니다(예: *회사 이름*).
1. 에 *문서 생성 태거* 패널에서 &quot;name&quot;을 검색합니다.
1. 태그 목록에서 회사 아래의 이름을 선택합니다.
1. 선택 **[!UICONTROL 텍스트 삽입]**.

   ![태그 삽입 스크린샷](assets/accsales_5.png)

   이 프로세스에서는 라는 태그를 {{company.name}} 태그가 JSON의 경로 아래에 있기 때문입니다.

   ```
   {
   …
   "company": {
       "name":"Home Services Co.",
       …
   },
   …
   }
   ```

STREET ADDRESS, CITY, STATE, ZIP 등과 같이 문서의 일부 추가 태그에 대해 이러한 작업을 반복합니다.

## 생성된 문서 미리 보기

Microsoft Word에서 바로 샘플 JSON 데이터를 기반으로 생성된 문서를 미리 볼 수 있습니다.

1. 에 *문서 생성 태거* 패널, 선택 **[!UICONTROL 문서 생성]**. 처음으로 Adobe ID으로 로그인하라는 메시지가 표시될 수 있습니다. 선택 **[!UICONTROL 로그인]** 자격 증명으로 로그인하라는 메시지가 표시됩니다.

   ![생성된 문서를 미리 보는 방법 스크린샷](assets/accsales_6.png)

1. 선택 **[!UICONTROL 문서 보기]**.

   ![문서 보기 단추 스크린샷](assets/accsales_7.png)

1. 문서 결과를 미리 볼 수 있는 브라우저 창이 열립니다.

   ![브라우저 창의 문서 스크린샷](assets/accsales_8.png)

원본 샘플 데이터의 데이터로 대체된 태그를 문서에서 확인할 수 있습니다.

![데이터로 바뀐 태그의 스크린샷](assets/accsales_9.png)

## 템플릿에 테이블 추가

다음 시나리오에서는 제품 목록을 문서의 표에 추가합니다.

1. 표를 배치해야 하는 위치에 커서를 삽입합니다.
1. 에 *문서 생성 태거* 패널, 선택 **[!UICONTROL 고급]**.
1. 확장 **[!UICONTROL 테이블 및 목록]**.
1. 에 *테이블 레코드* 필드, 선택 *referencesOrder*&#x200B;모든 제품 항목을 나열하는 배열입니다.
1. 열 레코드 선택 필드에 포함할 내용을 입력합니다 *설명* 및 *totalPaymentDue.price* 필드에 표시됩니다.
1. 선택 **[!UICONTROL 표 삽입]**.

   ![표 삽입 스크린샷](assets/accsales_10.png)

표를 편집하여 Microsoft Word의 다른 표와 마찬가지로 스타일, 크기 및 기타 매개 변수를 조정합니다.

## 숫자 계산 추가

숫자 계산을 사용하면 배열과 같은 데이터 컬렉션을 기반으로 합계 및 기타 계산을 계산할 수 있습니다. 이 시나리오에서는 소계를 계산할 필드를 추가합니다.

1. 다음을 선택합니다. *0.00달러* 를 클릭합니다.
1. 에 *[!UICONTROL 문서 생성 태거]* 패널, 확장 **[!UICONTROL 숫자 계산]**.
1. 아래 *[!UICONTROL 계산 유형 선택]*, 선택 **[!UICONTROL 집계]**.
1. 아래 *[!UICONTROL 문자 선택]*, 선택 **[!UICONTROL 합계]**.
1. 아래 *[!UICONTROL 레코드 선택]*, 선택 **[!UICONTROL ReferencesOrder]**.
1. 아래 *[!UICONTROL 합계를 수행할 항목 선택]**, **[!UICONTROL totalPaymentsDue.price]**.
1. 선택 **[!UICONTROL 계산 삽입]**.

이 프로세스에서는 값의 합계를 제공하는 계산 태그를 삽입합니다. JSONata 계산을 사용하여 고급 계산을 수행할 수 있습니다. 예를 들면 다음과 같습니다.

* 소계: `${{expr($sum(referencesOrder.totalPaymentDue.price))}}`
referencesOrder.totalPaymentDue.price의 합계를 계산합니다.

* 판매세: `${{expr($sum(referencesOrder.totalPaymentDue.price)*0.08)}}`
가격을 계산하고 8%를 곱하여 세금을 계산합니다.

* 총 기한: `${{expr($sum(referencesOrder.totalPaymentDue.price)*1.08)}}`
가격 및 1.08의 배수를 계산하여 소계 + 세금을 계산합니다.

## 조건 용어 추가

조건부 섹션을 사용하면 특정 조건이 충족될 때만 문장 또는 단락을 포함할 수 있습니다. 이 시나리오에서는 특정 상태와 일치하는 섹션만 포함됩니다.

1. 문서에서 *캘리포니아 개인정보취급방침*.
1. 커서를 사용하여 섹션을 선택합니다.

   ![선택 스크린샷](assets/accsales_11.png)

1. 에 *[!UICONTROL 문서 생성 태거]*, 선택 **[!UICONTROL 고급]**.
1. 확장 **[!UICONTROL 조건부 내용]**.
1. 에 *[!UICONTROL 레코드 선택]* 필드, 검색 및 선택 **[!UICONTROL customer.address.state]**.
1. 에 *[!UICONTROL 연산자 선택]* 필드, 선택 **=**.
1. 에 *[!UICONTROL 값 필드]*, 유형 *CA*.
1. 선택 **[!UICONTROL 조건 삽입]**.

캘리포니아 섹션은 customer.address.state = CA인 경우에만 생성된 문서에 나타납니다.

다음으로 WASHINGTON 개인정보취급방침 섹션을 선택하고 위의 단계를 반복하여 값 CA를 WA로 대체합니다.

## 동적 이미지 추가

문서 생성 API를 사용하면 데이터에서 동적으로 이미지를 삽입할 수 있습니다. 이는 다양한 하위 브랜드가 있고 특정 업계에 적합한 로고, 인물 이미지 또는 이미지를 변경하고자 할 때 유용합니다.

이미지는 데이터 또는 base64 내용의 URL로 전달할 수 있습니다. 이 예제에서는 URL을 사용합니다.

1. 이미지를 포함할 위치에 커서를 놓습니다.
1. 에 *[!UICONTROL 문서 생성 태거]* 패널, 선택 **[!UICONTROL 고급]**.
1. 확장 **[!UICONTROL 이미지]**.
1. 에 *[!UICONTROL 태그 선택]* 필드, 선택 **[!UICONTROL 로고]**.
1. 에 *[!UICONTROL 선택적 대체 텍스트]* 필드에 설명을 입력합니다(예: 로고). 이렇게 하면 다음과 같은 이미지 자리 표시자가 삽입됩니다.

   ![자리 표시자 이미지 스크린샷](assets/accsales_12.png)

그러나 이미 레이아웃에 있는 이미지에 동적으로 이미지를 설정하려는 경우 다음을 수행할 수 있습니다.

1. 삽입된 자리 표시자 이미지를 마우스 오른쪽 단추로 클릭합니다.

   ![자리 표시자 이미지 스크린샷](assets/accsales_13.png)

1. 선택 **[!UICONTROL 대체 텍스트 편집]**.
1. 패널에서 다음과 같은 텍스트를 복사합니다.
   `{ "location-path": "logo", "image-props": { "alt-text": "Logo" }}`
1. 문서에서 동적으로 만들 다른 이미지를 선택합니다.

   ![문서의 새 이미지 스크린샷](assets/accsales_14.png)

1. 이미지를 마우스 오른쪽 단추로 클릭하고 **[!UICONTROL 대체 텍스트 편집]**.
1. 값을 패널에 붙여넣습니다.

이 프로세스는 이미지를 데이터의 로고 변수에 있는 이미지로 대체합니다.

## Acrobat Sign용 태그 추가

Adobe Acrobat Sign을 사용하면 문서에서 전자 서명을 캡처할 수 있습니다. Acrobat Sign에서는 웹 인터페이스 내에서 필드를 쉽게 드래그하여 놓을 수 있지만, 텍스트 태그를 사용하여 서명 및 기타 필드 배치를 제어할 수도 있습니다. Adobe 문서 생성 태거를 사용하면 이러한 텍스트 태그 필드를 쉽게 배치할 수 있습니다.

1. 샘플 문서에서 서명이 필요한 위치로 이동합니다.
1. 서명이 필요한 위치에 커서를 삽입합니다.
1. 에 *[!UICONTROL Adobe 문서 생성 태거]* 패널, 선택 **[!UICONTROL Adobe Sign]**.
1. 에 *[!UICONTROL 수신자 수 지정]* 필드에 수신자의 수를 설정합니다(이 예에서는 1입니다).
1. 에 *[!UICONTROL 수신자]* 필드, 선택 **[!UICONTROL Signer-1]**.
1. 에 *[!UICONTROL 필드]* 문자, 선택 **[!UICONTROL 서명]**.
1. 선택 **[!UICONTROL Adobe Sign 텍스트 태그 삽입]**.

태그가 문서에 삽입됩니다.

![문서의 서명 태그 스크린샷](assets/accsales_15.png)

Acrobat Sign에서는 날짜 필드와 같이 배치할 수 있는 다른 여러 유형의 필드를 제공합니다.
1. 에 *필드* 문자, 선택 **[!UICONTROL 날짜]**.
1. 커서를 문서의 날짜 위치 위로 이동합니다.
1. 선택 **[!UICONTROL Adobe Sign 텍스트 태그 삽입]**.

![문서의 date 태그 스크린샷](assets/accsales_16.png)

## 계약서 생성

문서에 태그를 지정했으므로 이제 시작할 준비가 되었습니다. 다음 단원에서는 Node.js용 문서 생성 API 샘플을 사용하여 문서를 생성하는 방법을 안내하지만 모든 언어에서 작동합니다.

자격 증명을 등록할 때 다운로드한 pdfservices-node-sdk-samples-master를 엽니다. 이러한 파일에는 pdfservices-api-credentials.json 및 private.key 파일이 포함되어야 합니다.

1. 터미널을 열어 npm install 을 사용하여 종속성을 설치합니다.
1. 샘플 data.json을 resources 폴더에 복사합니다.
1. Word 템플릿을 리소스 폴더에 복사합니다.
1. samples 폴더의 루트 디렉터리에 generate-salesOrder.js라는 새 파일을 만듭니다.

```
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
const fs = require('fs');
const path = require('path');

var dataFileName = path.join('resources', '<INSERT JSON FILE');
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".pdf");
var inputFileName = path.join('resources', '<INSERT DOCX>');

//Loads credentials from the file that you created.
const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Setup input data for the document merge process
const jsonString = fs.readFileSync(dataFileName),
jsonDataForMerge = JSON.parse(jsonString);

// Create an ExecutionContext using credentials
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance
const documentMerge = PDFServicesSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile(inputFileName);
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile(outputFileName))
.catch(err => {
    if(err instanceof PDFServicesSdk.Error.ServiceApiError
        || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
        console.log('Exception encountered while executing operation', err);
    } else {
        console.log('Exception encountered while executing operation', err);
    }
});
```

1. 바꾸기 `<INSERT JSON FILE>` JSON 파일의 이름을 /resources에 추가합니다.
1. 바꾸기 `<INSERT DOCX>` 를 반환합니다.
1. 실행하려면 터미널을 사용하여 generate-salesOrder.js 노드를 실행합니다.

출력 파일은 문서가 올바르게 생성된 /output 폴더에 있어야 합니다.

## 기타 옵션

문서가 생성되면 다음과 같은 추가 작업을 수행할 수 있습니다.

* 암호로 문서 보호
* 큰 이미지가 있는 경우 PDF 압축
* 문서에서 전자 서명 캡처

사용 가능한 다른 작업에 대해 자세히 알아보려면 샘플 파일의 /src 폴더에 있는 스크립트를 참조하십시오. 다양한 작업에 대한 설명서를 검토하여 자세히 알아볼 수도 있습니다.

## 추가 사용 사례

[!DNL Adobe Acrobat Services] 디지털 문서 워크플로우를 통해 세일즈 주기의 많은 부분을 간소화할 수 있습니다.

* Adobe PDF Embed API를 사용하여 백서 및 기타 콘텐츠를 웹 사이트에 임베드하는 동시에 시청률에서 분석을 측정하고 수집할 수 있습니다
* Acrobat Sign을 사용하여 생성된 계약에 대한 전자 서명 캡처
* Adobe PDF Extract API를 사용하여 PDF 문서에서 계약 데이터 추출

## 추가 학습

더 많은 것을 배우고 싶습니까? 사용할 수 있는 몇 가지 추가 방법을 살펴보십시오 [!DNL Adobe Acrobat Services]:

* 자세한 내용은 [설명서](https://developer.adobe.com/document-services/docs/overview/)
* Adobe Experience League 튜토리얼 더 보기
* /src 폴더의 샘플 스크립트를 사용하여 PDF을 활용하는 방법을 확인합니다.
* 팔로우 [Adobe 기술 블로그](https://medium.com/adobetech/tagged/adobe-document-cloud) 최신 팁 및 요령
* 구독 대상 [종이 클립(월별 라이브 스트림)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) 자동화에 대해 더 알아보기 [!DNL Adobe Acrobat Services]. =======
* 자세한 내용은 [설명서](https://developer.adobe.com/document-services/docs/overview/)
* Adobe Experience League 튜토리얼 더 보기
* /src 폴더의 샘플 스크립트를 사용하여 PDF을 활용하는 방법을 확인합니다.
* 팔로우 [Adobe 기술 블로그](https://medium.com/adobetech/tagged/adobe-document-cloud) 최신 팁 및 요령
* 구독 대상 [종이 클립(월별 라이브 스트림)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) 자동화에 대해 더 알아보기 [!DNL Adobe Acrobat Services]
