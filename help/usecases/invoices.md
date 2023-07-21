---
title: 송장 처리
description: 고객 송장을 자동으로 생성, 암호로 보호 및 전달하는 방법 알아보기
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8145.jpg
jira: KT-8145
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 1%

---

# 송장 처리

![사례 메인 배너 사용](assets/UseCaseInvoicesHero.jpg)

사업이 호황일 때는 좋지만, 모든 인보이스를 준비할 때는 생산성이 떨어진다. 인보이스를 수동으로 생성하는 데에는 시간이 많이 소요되며, 오류를 범할 위험이 있고, 이로 인해 비용이 손실되거나 고객에게 잘못된 금액이 발생할 수 있습니다.

예를 들어 Danielle이 [회계부](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html) [의료용품 기업의](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). 이달 말이라 여러 시스템에서 정보를 가져와 정확도를 재확인하고 청구서 서식을 지정합니다. 이 모든 작업을 마친 후, 그녀는 마지막으로 문서를 PDF으로 변환하여(누구나 특정 소프트웨어를 구매하지 않고도 문서를 볼 수 있도록) 각 고객에게 개인화된 청구서를 보낼 준비가 되었습니다.

월간 인보이스가 완료되더라도 Daniele은 이러한 인보이스를 벗어날 수 없습니다. 일부 고객은 청구 주기가 비월이므로 항상 누군가에게 송장을 발행합니다. 때때로 고객은 송장을 편집하여 과소지급하기도 합니다. 그런 다음 Daniele은 이 인보이스 불일치를 해결하는 데 시간을 할애합니다. 이 속도라면 그녀는 모든 일을 따라가기 위해 조수를 고용해야 한다!

Danielle이 필요로 하는 것은 월말에 일괄적으로 송장을 작성하고 다른 시간에는 임시 송장을 신속하고 정확하게 생성하는 방법입니다. 이러한 인보이스를 편집에서 보호할 수 있다면 불일치 항목 문제를 해결할 필요가 없습니다.

## 학습 내용

이 실습 튜토리얼에서는 Adobe 문서 생성 API를 사용하여 자동으로 송장을 생성하고 PDF을 암호로 보호하며 각 고객에게 송장을 전달하는 방법을 살펴봅니다. Node.js, JavaScript, Express.js, HTML 및 CSS에 대한 약간의 지식만 있으면 됩니다.

이 프로젝트의 전체 코드는 [gitHub에서 사용 가능](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation). 템플릿 및 원시 데이터 폴더를 사용하여 공개 디렉터리를 설정해야 합니다. 프로덕션 중에 외부 API에서 데이터를 가져와야 합니다. 템플릿 리소스가 포함된 이 보관된 버전의 응용 프로그램을 탐색할 수도 있습니다.

## 관련 API 및 리소스

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe 문서 생성 API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [프로젝트 코드](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## 데이터 준비

이 자습서에서는 데이터 웨어하우스에서 데이터를 가져오는 방법을 살펴보지 않습니다. 고객 주문이 데이터베이스, 외부 API 또는 사용자 정의 소프트웨어에 있을 수 있습니다. Adobe 문서 생성 API에서는 CRM(고객 관계 관리) 또는 eCommerce 플랫폼의 정보와 같은 송장 발행 데이터가 포함된 JSON 문서를 예상합니다. 이 자습서에서는 데이터가 이미 JSON 형식인 것으로 가정합니다.

간단하게 인보이스 발행에 다음 JSON 구조를 사용하십시오.

```
{ 
    "customerName": "John Doe", 
    "customerEmail": "john-doe@example.com", 
    "order": [ 
        { 
            "productId": 26, 
            "productTitle": "Bandages", 
            "price": 15.82 
        }, 
        { 
            "productId": 54, 
            "productTitle": "Masks", 
            "price": 25 
        }, 
        { 
            "productId": 76, 
            "productTitle": "Gloves", 
            "price": 7.59 
        } 
    ] 
} 
```

JSON 문서에는 고객 세부 정보와 주문 정보가 포함되어 있습니다. 이 구조화된 문서를 사용하여 송장을 작성하고 요소를 PDF 형식으로 표시합니다.

## 송장 템플리트 작성

Adobe 문서 생성 API에서는 Microsoft Word 기반 템플릿과 JSON 문서를 사용하여 동적 PDF 또는 Word 문서를 만들어야 합니다. 인보이스 작성 응용 프로그램에 사용할 Microsoft Word 템플릿을 만들고 [무료 문서 생성 Tagger 추가 기능](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) 템플릿 태그를 생성합니다. 추가 기능을 설치하고 Microsoft Word에서 탭을 엽니다.

![문서 생성 Tagger 추가 기능의 스크린샷](assets/invoices_1.png)

위와 같이 JSON 콘텐츠를 추가 기능에 붙여넣은 경우 태그 생성을 클릭합니다. 이제 이 플러그인은 개체의 형식을 표시합니다. 기본 템플릿은 고객의 이름과 이메일을 사용할 수 있지만 주문 정보가 표시되지 않습니다. 주문 정보는 이 자습서의 뒷부분에서 설명합니다.

![문서 생성 태거 작성자 템플릿 스크린샷](assets/invoices_2.png)

Microsoft Word 문서에서 인보이스 템플릿을 작성합니다. 동적 데이터를 삽입해야 하는 위치에 커서를 놓은 다음 Adobe 추가 기능 창에서 태그를 선택합니다. 클릭 **텍스트 삽입** Adobe 문서 생성 Tagger 추가 기능을 사용하여 태그를 생성하고 삽입할 수 있습니다. 개인화를 위해 고객의 이름과 이메일을 삽입해 보겠습니다.

이제 새로운 각 인보이스에 따라 변경되는 데이터로 이동합니다. 다음을 선택합니다. **고급** 탭합니다. 고객이 주문한 제품을 기반으로 동적 표를 생성하는 데 사용할 수 있는 옵션을 보려면 **테이블 및 목록** .

선택 **주문** 을 클릭합니다. 두 번째 드롭다운에서 이 표의 열을 선택합니다. 이 튜토리얼에서는 표를 렌더링할 개체의 세 열을 모두 선택합니다.

![문서 생성 태거 고급 탭의 스크린샷](assets/invoices_3.png)

문서 생성 API는 배열 내의 요소를 집계하는 등의 복잡한 작업도 수행할 수 있습니다. 에 **고급** 탭, 선택 **숫자 계산**&#x200B;및 **집계** 탭에서 계산을 적용할 필드를 선택합니다.

![문서 생성 태거 숫자 계산 스크린샷](assets/invoices_4.png)

오른쪽 상단의 **계산 삽입** 단추를 클릭하여 문서 내에서 필요한 위치에 이 태그를 삽입합니다. 이제 Microsoft Word 파일에 다음 텍스트가 나타납니다.

![Microsoft Word 문서의 태그 스크린샷](assets/invoices_5.png)

이 송장 샘플에는 고객 정보, 주문 제품 및 총 만기가 포함되어 있습니다.

## Adobe 문서 생성 API를 사용하여 송장 생성

Adobe PDF Services Node.js SDK(software development kit)를 사용하여 Microsoft Word 및 JSON 문서를 결합합니다. 문서 생성 API를 사용하여 인보이스를 생성하는 Node.js 애플리케이션을 빌드합니다.

PDF 서비스 API에는 문서 생성 서비스가 포함되어 있으므로 두 서비스에 동일한 자격 증명을 사용할 수 있습니다. 즐거운 시간 [6개월 무료 체험판](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)문서 트랜잭션당 $0.05만 지불하면 됩니다.

다음은 PDF 병합에 사용할 코드입니다.

```
async function compileDocFile(json, inputFile, outputPdf) { 
    try { 
        // configurations 
        const credentials =  adobe.Credentials 
            .serviceAccountCredentialsBuilder() 
            .fromFile("./src/pdftools-api-credentials.json") 
            .build(); 

        // Capture the credential from app and show create the context 
        const executionContext = adobe.ExecutionContext.create(credentials); 
  
        // create the operation 
        const documentMerge = adobe.DocumentMerge, 
            documentMergeOptions = documentMerge.options, 
            options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);

        const operation = documentMerge.Operation.createNew(options); 
  
        // Pass the content as input (stream) 
        const input = adobe.FileRef.createFromLocalFile(inputFile); 
        operation.setInput(input); 
  
        // Async create the PDF 
        let result = await operation.execute(executionContext); 
        await result.saveAsFile(outputPdf); 
    } catch (err) { 
        console.log('Exception encountered while executing operation', err); 
    } 
} 
```

이 코드는 입력 JSON 문서 및 입력 템플릿 파일에서 정보를 가져옵니다. 그런 다음 파일을 단일 PDF 보고서로 결합하는 문서 병합 작업을 만듭니다. 마지막으로 API 자격 증명으로 작업을 실행합니다. 아직 없으시면, [자격 증명 만들기](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) (문서 생성 및 PDF 서비스 API에서는 동일한 자격 증명을 사용합니다.)

Express 라우터 내에서 이 코드를 사용하여 문서 요청을 처리합니다.

```
// Create one report and send it back
try {
    console.log(\`[INFO] generating the report...\`);
    const fileContent = fs.readFileSync(\`./public/documents/raw/\${vendor}\`,
    'utf-8');
    const parsedObject = JSON.parse(fileContent);

    await pdf.compileDocFile(parsedObject,
    \`./public/documents/template/Adobe-Invoice-Sample.docx\`,
    \`./public/documents/processed/output.pdf\`);

    await pdf.applyPassword("p@55w0rd", './public/documents/processed/output.pdf',
    './public/documents/processed/output-secured.pdf');

    console.log(\`[INFO] sending the report...\`);
    res.status(200).render("preview", { page: 'invoice', filename: 'output.pdf' });
} catch(error) {
    console.log(\`[ERROR] \${JSON.stringify(error)}\`);
    res.status(500).render("crash", { error: error });
}
```

이 코드가 실행되면 제공된 데이터를 기반으로 동적으로 생성된 인보이스가 포함된 PDF 문서를 제공합니다. 위에서 제공된 샘플 JSON 데이터의 경우 이 코드의 출력은 다음과 같습니다.

![동적으로 생성된 PDF 송장의 스크린샷](assets/invoices_6.png)

이 송장에는 JSON 문서의 동적 데이터가 포함됩니다.

## 송장 비밀번호 보호

회계사 Danielle은 고객이 송장을 변경하는 것을 걱정하므로, 편집을 제한하기 위해 암호를 적용하세요. [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) 문서에 암호를 자동으로 적용할 수 있습니다. 여기서는 Adobe PDF Services SDK를 사용하여 문서를 암호로 보호합니다. 코드는 다음과 같습니다.

```
async function applyPassword(password, inputFile, outputFile) {
    try {
        // Initial setup, create credentials instance.
        const credentials = adobe.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("./src/pdftools-api-credentials.json")
        .build();

        // Create an ExecutionContext using credentials
        const executionContext = adobe.ExecutionContext.create(credentials);
        // Create new permissions instance and add the required permissions
        const protectPDF = adobe.ProtectPDF,
        protectPDFOptions = protectPDF.options;
        // Build ProtectPDF options by setting an Owner/Permissions Password, Permissions,
        // Encryption Algorithm (used for encrypting the PDF file) and specifying the type of content to encrypt.
        const options = new protectPDFOptions.PasswordProtectOptions.Builder()
        .setOwnerPassword(password)
        .setEncryptionAlgorithm(protectPDFOptions.EncryptionAlgorithm.AES_256)
        .build();

        // Create a new operation instance.
        const protectPDFOperation = protectPDF.Operation.createNew(options);

        // Set operation input from a source file.
        const input = adobe.FileRef.createFromLocalFile(inputFile);
        protectPDFOperation.setInput(input);

        // Execute the operation and Save the result to the specified location.
        let result = await protectPDFOperation.execute(executionContext);

        result.saveAsFile(outputFile);
    } catch (err) {
        console.log('Exception encountered while executing operation', err);
    }
}
```

이 코드를 사용하면 문서를 암호로 보호하고 시스템에 새 송장을 업로드합니다. 이 코드의 사용 방법 또는 사용법에 대한 자세한 내용은 [코드 샘플](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation).

인보이스를 완료하면 고객에게 자동으로 이메일을 보낼 수 있습니다. 고객에게 자동으로 이메일을 보내는 방법에는 몇 가지가 있습니다. 가장 빠른 방법은 다음과 같은 도우미 라이브러리와 함께 타사 이메일 API를 사용하는 것입니다 [sendgrid-nodejs](https://github.com/sendgrid/sendgrid-nodejs). 또는 SMTP 서버에 대한 액세스 권한이 있는 경우 [노딜러](https://www.npmjs.com/package/nodemailer) SMTP를 통해 전자 메일을 보냅니다.

## 다음 단계

이 실습 튜토리얼에서는 Daniele이 계정을 사용하는 데 유용한 간단한 앱을 만들었습니다 [인보이스 발행](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). PDF 서비스 API 및 문서 생성 SDK를 사용하여 JSON 문서의 고객 주문 정보로 Microsoft Word 템플릿을 작성하고 PDF 송장을 작성했습니다. 그런 다음 암호 보호 서비스를 사용하여 각 문서를 암호로 보호하여 [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html).

Daniele은 자동으로 송장을 생성할 수 있고 고객이 송장을 편집하는 것에 대해 걱정할 필요가 없으므로 모든 수동 작업을 지원하기 위해 보조 직원을 고용할 필요가 없습니다. 그녀는 추가 시간을 사용하여 AP 파일에서 비용 절감을 찾을 수 있습니다.

지금까지 사용 방법을 살펴보았습니다. 이제 다른 Adobe 툴을 사용하여 이 간단한 앱을 확장하여 웹 사이트에 인보이스를 포함할 수 있습니다. 예를 들어 고객이 언제든지 인보이스나 잔액을 조회할 수 있습니다. [Adobe PDF 임베드 API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 무료로 사용할 수 있습니다. 인사 또는 영업 부서로 이동하여 계약을 자동화하고 전자 서명을 수집할 수 있습니다.

모든 가능성을 살펴보고 간단한 응용 프로그램을 만들기 시작하려면 무료로 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 지금 시작하세요. 6개월 무료 체험판을 사용해 보십시오 [사용한 만큼 지불](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)
비즈니스 규모에 따라 문서 트랜잭션당 0.05 USD
