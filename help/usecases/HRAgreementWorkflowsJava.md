---
title: Java의 HR 문서 워크플로우
description: "[!DNL Adobe Acrobat Services] API는 PDF 기능을 HR 웹 애플리케이션에 쉽게 통합합니다."
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-7474.jpg
kt: 7474
exl-id: add4cc5c-06e3-4ceb-930b-e8c9eda5ca1f
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1899'
ht-degree: 2%

---

# Java의 HR 문서 워크플로우

![사례 메인 배너 사용](assets/UseCaseHRHero.jpg)

많은 기업에서 재택근무 직원의 업무 협약 등 신규 채용과 관련된 문서를 요구하고 있습니다. 일반적으로 기업은 이러한 문서를 관리하거나 저장하기 어려운 형식으로 물리적으로 관리했습니다. 전자 문서로 전환할 때 PDF 파일은 다른 파일 유형에 비해 보안 수준이 높고 수정 가능성이 낮기 때문에 이상적인 선택입니다. 또한 디지털 서명도 지원합니다.

## 학습 내용

이 실습 튜토리얼에서는 간단한 Java Spring MVC 응용 프로그램에서 로그오프와 함께 작업 공간 계약을 PDF에 저장하는 웹 기반 HR 양식을 구현하는 방법을 알아봅니다.

## 관련 API 및 리소스

* [PDF 서비스 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [프로젝트 코드](https://github.com/dawidborycki/adobe-sign)

## API 자격 증명 생성 중

먼저 Adobe PDF 서비스 API 무료 체험판을 등록하십시오. 다음으로 이동 [Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) [웹 사이트](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) 을 클릭하고 *시작하기* 단추 아래쪽 *새 자격 증명 만들기*. 무료 평가판에서는 6개월 동안 사용할 수 있는 1,000개의 문서 트랜잭션을 제공합니다. 다음 페이지(아래 참조)에서 서비스(PDF 서비스 API)를 선택하고 자격 증명 이름(예: HRDocumentWFCredentials)을 설정한 다음 설명을 입력합니다.

언어(이 예제의 경우 Java)를 선택하고 *개인화된 코드 샘플 만들기*. 마지막 단계에서는 API 내에서 앱을 인증하기 위한 개인 키와 함께 사용하는 미리 채워진 pdftools-api-credentials.json 파일이 코드 샘플에 이미 포함되어 있는지 확인합니다.

마지막으로 *자격 증명 만들기* 단추를 클릭합니다. 이렇게 하면 자격 증명이 생성되고 샘플이 자동으로 다운로드를 시작합니다.

![새 자격 증명 스크린샷 만들기](assets/HRWJ_1.png)

자격 증명이 작동하는지 확인하려면 다운로드한 샘플을 여십시오. 여기서 IntelliJ IDEA를 사용하고 있습니다. 소스 코드를 열면 통합 개발 환경(IDE)에서 빌드 엔진을 요구합니다. 이 샘플에는 Maven이 사용되지만 환경 설정에 따라 Gradle로 작업할 수도 있습니다.

다음으로 `mvn clean install` jar 파일을 만드는 것을 목표로 삼았습니다.

마지막으로 아래와 같이 CombinePDF 샘플을 실행합니다. 코드는 출력 폴더 내에 PDF을 생성합니다.

![CombinePDF 샘플 스크린샷을 실행하는 메뉴](assets/HRWJ_2.png)

## Spring MVC 응용 프로그램 만들기

자격 증명이 제공되면 응용 프로그램을 생성합니다. 이 예제에서는 Spring Initializr를 사용합니다.

먼저 Java 8 언어 및 Jar 패키징을 사용하도록 프로젝트 설정을 구성합니다(아래 스크린샷 참조).

![봄 초기화 스크린샷](assets/HRWJ_3.png)

둘째, Spring Web(웹) 및 Thymeleaf(템플릿 엔진) 추가:

![Spring Web 및 Thymeleaf를 추가하는 스크린샷](assets/HRWJ_4.png)

프로젝트를 만든 후 pom.xml 파일로 이동하여 pdftools-sdk 및 log4j-slf4j-impl로 종속성 섹션을 보완합니다.

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
```

그런 다음 샘플 코드와 함께 다운로드한 두 개의 파일로 프로젝트의 루트 폴더를 보완합니다.

* pdftools-api-credentials.json

* private.key

## 웹 양식 렌더링

웹 양식을 렌더링하려면 개인 데이터 양식을 렌더링하고 양식 게시를 처리하는 컨트롤러를 사용하여 응용 프로그램을 수정합니다. 따라서 먼저 PersonForm 모델 클래스를 사용하여 응용 프로그램을 수정합니다.

```
package com.hr.docsigning;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class PersonForm {
    @NotNull
    @Size(min=2, max=30)
    private String firstName;

    @NotNull
    @Size(min=2, max=30)
    private String lastName;

    public String getFirstName() {
            return this.firstName;
    }


    public void setFirstName(String firstName) {
            this.firstName = firstName;
    }

    public String getLastName() {
           return this.lastName;
    }

    public void setLastName(String lastName) {
            this.lastName = lastName;
    }

    public String GetFullName() {
           return this.firstName + " " + this.lastName;
    }
}
```

이 클래스에는 다음 두 개의 속성이 포함되어 있습니다. `firstName` 및 `lastName`. 또한 이 간단한 유효성 검사를 사용하여 두 자에서 30자 사이인지 확인합니다.

모델 클래스가 지정된 경우 컨트롤러를 만들 수 있습니다(컴패니언 코드의 PersonController.java 참조).

```
package com.hr.docsigning;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import javax.validation.Valid;


@Controller
public class PersonController {
    @GetMapping("/")
    public String showForm(PersonForm personForm) {
        return "form";
    }
}
```

컨트롤러에는 하나의 방법만 있습니다. showForm을 입력합니다. resources/templates/form.html에 있는 HTML 템플릿을 사용하여 양식을 렌더링하는 작업은 담당합니다.

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<body>
<div class="w3-container">
    <h1>HR Department</h1>
</div>
 
<form class="w3-panel w3-card-4" action="#" th:action="@{/}"
        th:object="${personForm}" method="post">
    <h2>Personal data</h2>
    <table>
        <tr>
            <td>First Name:</td>
            <td><input type="text" class="w3-input"
                placeholder="First name" th:field="*{firstName}" /></td>
            <td class="w3-text-red" th:if="${#fields.hasErrors('firstName')}"
                th:errors="*{firstName}"></td>
        </tr>
        <tr>
            <td>Last Name:</td>
            <td><input type="text" class="w3-input"
                placeholder="Last name" th:field="*{lastName}" /></td>
            <td class="w3-text-red" th:if="${#fields.hasErrors('lastName')}"
                th:errors="*{lastName}"></td>
        </tr>
        <tr>
            <td><button class="w3-button w3-black" type="submit">Submit</button></td>
        </tr>
    </table>
</form>
</body>
</html>
```

동적 콘텐츠를 렌더링하기 위해, Thymeleaf 템플릿 렌더링 엔진이 채용된다. 따라서 응용 프로그램을 실행한 후 다음 항목이 표시됩니다.

![렌더링된 콘텐츠의 스크린샷](assets/HRWJ_5.png)

## 동적 내용으로 PDF 생성하기

이제 개인 데이터 양식을 렌더링한 후 선택한 필드를 동적으로 채워서 가상 계약이 포함된 PDF 문서를 생성합니다. 특히 개인 데이터를 미리 생성된 계약에 채워야 합니다.

여기서는 간단히 하기 위해 머리글, 하위 머리글 및 문자열 상수 읽기만 있습니다. &quot;이 계약은 다음에 대해 준비되었습니다.&lt;full name=&quot;&quot; of=&quot;&quot; the=&quot;&quot; person=&quot;&quot;>&quot;.

이 목표를 달성하려면 먼저 Adobe [동적 HTML에서 PDF 만들기](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-dynamic-html) 예. 샘플 코드를 분석하면 동적 HTML 필드 채우기 프로세스가 다음과 같이 작동함을 알 수 있습니다.

먼저 정적 및 동적 내용이 있는 HTML 페이지를 준비해야 합니다. 동적 부분은 JavaScript를 사용하여 업데이트됩니다. 즉, PDF 서비스 API가 JSON 개체를 HTML에 삽입하게 됩니다.

그런 다음 HTML 문서가 로드될 때 호출되는 JavaScript 함수를 사용하여 JSON 속성을 가져옵니다. 이 JavaScript 함수는 선택한 DOM 요소를 업데이트합니다. 다음은 span 요소를 채우고 개인의 데이터를 유지하는 예제입니다(컴패니언 코드의 src\\main\\resources\\contract\\index.html 참조).

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<body onload="updateFullName()">
    <script src="./json.js"></script>
    <script type="text/javascript">
        function updateFullName()
        {
            var document = window.document;
            document.getElementById("personFullName").innerHTML = String(
                window.json.personFullName);
        }
    </script>
 
    <div class="w3-container ">
        <h1>HR Department</h1>
 
        <h2>Contract details</h2>
 
        <p>This contract was prepared for:
            <strong><span id="personFullName"></span></strong>
        </p>
    </div>
</body>
</html>
```

그런 다음 모든 종속 JavaScript 및 CSS 파일과 함께 HTML을 압축해야 합니다. PDF 서비스 API는 HTML 파일을 허용하지 않습니다. 대신 zip 파일이 입력으로 필요합니다. 이 경우 압축된 파일은 src\\main\\resources\\contract\\index.zip에 저장합니다.

그 후에, 당신은 `PersonController` POST 요청을 처리하는 다른 방법 사용:

```
@PostMapping("/")
public String checkPersonInfo(@Valid PersonForm personForm,
    BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        return "form";
    }
 
    CreateContract(personForm);
 
    return "contract-actions";
}
```

상기 방법은 제공된 개인 데이터를 사용하여 PDF 계약을 생성하고 계약-조치 뷰를 렌더링한다. 후자는 생성된 PDF에 대한 링크 및 PDF 서명을 위한 링크를 제공합니다.

이제 어떻게 `CreateContract` 메서드가 작동합니다(전체 목록은 아래에 나와 있음). 메서드는 다음 두 필드에 의존합니다.

* `LOGGER`에서, 모든 예외에 대한 디버그 정보를 보려면

* `contractFilePath`, 생성된 PDF에 대한 파일 경로 포함

추가 `CreateContract` 메서드가 자격 증명을 설정하고 HTML에서 PDF을 만듭니다. 계약에 개인의 데이터를 전달하고 채우려면 `setCustomOptionsAndPersonData` 도우미. 이 메서드는 양식에서 개인의 데이터를 검색한 다음 위에서 설명한 JSON 개체를 통해 생성된 PDF으로 보냅니다.

또한 `setCustomOptionsAndPersonData` 머리글과 바닥글을 사용하지 않도록 설정하여 PDF 모양을 제어하는 방법을 보여 줍니다. 이 절차를 완료하면 PDF 파일을 output/contract.pdf에 저장하고 이전에 생성된 파일을 삭제합니다.

```
private static final Logger LOGGER = LoggerFactory.getLogger(PersonController.class);
private String contractFilePath = "output/contract.pdf"; 
private void CreateContract(PersonForm personForm) {
    try {
        // Initial setup, create credentials instance.
        Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
                .fromFile("pdftools-api-credentials.json")
                .build();

        //Create an ExecutionContext using credentials 
       //and create a new operation instance.
        ExecutionContext executionContext = ExecutionContext.create(credentials);
        CreatePDFOperation htmlToPDFOperation = CreatePDFOperation.createNew();

        // Set operation input from a source file.
        FileRef source = FileRef.createFromLocalFile(
           "src/main/resources/contract/index.zip");
       htmlToPDFOperation.setInput(source);

        // Provide any custom configuration options for the operation
        // You pass person data here to dynamically fill out the HTML
        setCustomOptionsAndPersonData(htmlToPDFOperation, personForm);

        // Execute the operation.
        FileRef result = htmlToPDFOperation.execute(executionContext);

        // Save the result to the specified location. Delete previous file if exists
        File file = new File(contractFilePath);
        Files.deleteIfExists(file.toPath());

        result.saveAs(file.getPath());

    } catch (ServiceApiException | IOException | 
             SdkException | ServiceUsageException ex) {
        LOGGER.error("Exception encountered while executing operation", ex);
    }
}
 
private static void setCustomOptionsAndPersonData(
    CreatePDFOperation htmlToPDFOperation, PersonForm personForm) {
    //Set the dataToMerge field that needs to be populated 
    //in the HTML before its conversion
    JSONObject dataToMerge = new JSONObject();
    dataToMerge.put("personFullName", personForm.GetFullName());
 
    // Set the desired HTML-to-PDF conversion options.
    CreatePDFOptions htmlToPdfOptions = CreatePDFOptions.htmlOptionsBuilder()
        .includeHeaderFooter(false)
        .withDataToMerge(dataToMerge)
        .build();
    htmlToPDFOperation.setOptions(htmlToPdfOptions);
}
```

계약을 생성할 때 동적 개인 특정 데이터를 고정 계약 조건과 병합할 수도 있습니다. 이렇게 하려면 [정적 HTML에서 PDF 만들기](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-dynamic-html) 예. 또는 [두 PDF 병합](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-static-html).

## 다운로드할 PDF 파일 표시

이제 사용자가 다운로드할 수 있도록 생성된 PDF에 대한 링크를 제공할 수 있습니다. 이렇게 하려면 먼저 contract-actions.html 파일을 만듭니다(컴패니언 코드의 resources/templates contract-actions.html 참조).

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<div class="w3-container ">
    <h1>HR Department</h1>
 
    <h2>Contract file</h2>
 
    <p>Click <a href="/pdf">here</a> to download your contract</p>
</div>
</body>
</html>
```

그런 다음 `downloadContract` 메서드 `PersonController` 클래스:

```
@RequestMapping("/pdf")
public void downloadContract(HttpServletResponse response)
{
    Path file = Paths.get(contractFilePath);
 
    response.setContentType("application/pdf");
    response.addHeader(
        "Content-Disposition", "attachment; filename=contract.pdf");

    try
    {
        Files.copy(file, response.getOutputStream());
        response.getOutputStream().flush();
    }
    catch (IOException ex) 
    {
        ex.printStackTrace();
    }
}
```

앱을 실행하면 다음과 같은 흐름이 표시됩니다. 첫 번째 화면에는 개인 데이터 양식이 표시됩니다. 테스트하려면 2~30자 사이의 값으로 채웁니다.

![데이터 값 스크린샷](assets/HRWJ_6.png)

아이콘을 클릭한 후 *제출* HTML(resources/contract/index.html)에 따라 양식이 검증되고 PDF이 생성됩니다. PDF을 다운로드할 수 있는 다른 보기(계약 세부 정보)가 표시됩니다.

![PDF 다운로드가 가능한 스크린샷](assets/HRWJ_7.png)

PDF은 웹 브라우저에서 렌더링한 후 다음과 같이 표시됩니다. 즉, 입력한 개인 데이터는 PDF에 전파됩니다.

![개인 데이터가 렌더링된 PDF 스크린샷](assets/HRWJ_8.png)

## 서명 및 보안 활성화

계약이 준비되면, Adobe Sign은 승인을 나타내는 디지털 서명을 추가할 수 있습니다. Adobe Sign 인증은 OAuth와 약간 다르게 작동합니다. 이제 응용 프로그램을 Adobe Sign과 통합하는 방법을 살펴보겠습니다. 이렇게 하려면 응용 프로그램의 액세스 토큰을 준비해야 합니다. 그런 다음 Adobe Sign Java SDK를 사용하여 클라이언트 코드를 작성합니다.

인증 토큰을 받으려면 다음 단계를 수행해야 합니다.

먼저 [개발자 계정](https://acrobat.adobe.com/kr/ko/sign/developer-form.html).

CLIENT 응용 프로그램을 [Adobe Sign 포털](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md).

설명된 대로 응용 프로그램에 대한 OAuth를 구성합니다. [여기](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/configure_oauth.md) 및 [여기](https://secure.eu1.adobesign.com/public/static/oauthDoc.jsp). 클라이언트 식별자와 클라이언트 암호를 기록합니다. 그러면 `https://www.google.com` 리디렉션 URI 및 다음 범위로:

* user_login: 자아

* agreement_read: 계정

* agreement_write: 계정

* agreement_send: 계정

\ 대신 클라이언트 ID를 사용하여 다음과 같이 URL을 준비하십시오.&lt;client_id>:

```
https://secure.eu1.adobesign.com/public/oauth?redirect_uri=https://www.google.com
&response_type=code
&client_id=<CLIENT_ID>
&scope=user_login:self+agreement_read:account+agreement_write:account+agreement_send:account
```

웹 브라우저에 위의 URL을 입력합니다. google.com으로 리디렉션되며 코드는 주소 표시줄에 code=\&lt;your_code>예:

```
https://www.google.com/?code=<YOUR_CODE>&api_access_point=https://api.eu1.adobesign.com/&web_access_point=https://secure.eu1.adobesign.com%2F
```

\&lt;your_code> 및 api_access_point를 추가합니다.

액세스 토큰을 제공하는 HTTP POST 요청을 보내려면 클라이언트 ID \&lt;your_code>및 api_access_point 값. 사용할 수 있습니다. [Postman](https://helpx.adobe.com/sign/kb/how-to-create-access-token-using-postman-adobe-sign.html) 또는 cURL:

```
curl --location --request POST "https://**api.eu1.adobesign.com**/oauth/token"
\\

\--data-urlencode "client_secret=**\<CLIENT_SECRET\>**" \\

\--data-urlencode "client_id=**\<CLIENT_ID\>**" \\

\--data-urlencode "code=**\<YOUR_CODE\>**" \\

\--data-urlencode "redirect_uri=**https://www.google.com**" \\

\--data-urlencode "grant_type=authorization_code"
```

샘플 응답은 다음과 같습니다.

```
{
    "access_token":"3AAABLblqZhByhLuqlb-…",
    "refresh_token":"3AAABLblqZhC_nJCT7n…",
    "token_type":"Bearer",
    "expires_in":3600
}
```

access_token을 기록합니다. 클라이언트 코드를 인증하는 데 필요합니다.

## Adobe Sign Java SDK 사용

액세스 토큰이 있으면 REST API 호출을 Adobe Sign으로 보낼 수 있습니다. 이 프로세스를 간소화하려면 Adobe Sign Java SDK를 사용하십시오. 소스 코드는 [Adobe GitHub 리포지토리](https://github.com/adobe-sign/AdobeSignJavaSdk).

이 패키지를 응용 프로그램과 통합하려면 코드를 복제해야 합니다. 그런 다음 Maven 패키지(mvn 패키지)를 만들고 다음 파일을 프로젝트에 설치합니다(adobe-sign-sdk 폴더의 보조 코드에서 찾을 수 있음).

* target/swagger-java-client-1.0.0.jar

* target/lib/gson-2.8.1.jar

* target/lib/gson-fire-1.8.0.jar

* target/lib/hamcrest-core-1.3.jar

* target/lib/junit-4.12.jar

* target/lib/logging-interceptor-2.7.5.jar

* target/lib/okhttp-2.7.5.jar

* target/lib/okio-1.6.0.jar

* target/lib/swagger-annotations-1.5.15.jar

IntelliJ IDEA에서는 *프로젝트 구조* (파일/프로젝트 구조)

## 서명을 위해 PDF 보내기

이제 서명을 위해 계약을 보낼 준비가 되었습니다. 이렇게 하려면 먼저 보내기 요청에 대한 다른 하이퍼링크를 사용하여 contract-details.html을 추가합니다.

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<div class="w3-container ">
    <h1>HR Department</h1>
 
    <h2>Contract file</h2>
 
    <p>Click <a href="/pdf"> here</a> to download your contract</p>
 
    
</div>
</body>
</html>
```

다른 컨트롤러를 추가하시면 `AdobeSignController`를 구현합니다. `sendContractMethod` 자세한 내용은 컴패니언 코드를 참조하십시오. 이 메서드는 다음과 같이 작동합니다.

첫째, `ApiClient` API 끝점을 가져옵니다.

```
ApiClient apiClient = new ApiClient();

//Default baseUrl to make GET /baseUris API call.
String baseUrl = "https://api.echosign.com/";
String endpointUrl = "/api/rest/v6";
apiClient.setBasePath(baseUrl + endpointUrl);

// Provide an OAuth Access Token as "Bearer access token" in authorization
String authorization = "Bearer ";

// Get the baseUris for the user and set it in apiClient.
BaseUrisApi baseUrisApi = new BaseUrisApi(apiClient);
BaseUriInfo baseUriInfo = baseUrisApi.getBaseUris(authorization);
apiClient.setBasePath(baseUriInfo.getApiAccessPoint() + endpointUrl);
```

그런 다음 contract.pdf 파일을 사용하여 임시 문서를 만듭니다.

```
// Get PDF file
String filePath = "output/";
String fileName = "contract.pdf";
File file = new File(filePath + fileName);
String mimeType = "application/pdf";
 
//Get the id of the transient document.
TransientDocumentsApi transientDocumentsApi =
    new TransientDocumentsApi(apiClient);
TransientDocumentResponse response = transientDocumentsApi.createTransientDocument(authorization,
    file, null, null, fileName, mimeType);
String transientDocumentId = response.getTransientDocumentId();
```

다음으로, 계약을 작성해야 합니다. 이렇게 하려면 contract.pdf 파일을 사용하고 계약 상태를 IN_PROCESS로 설정하여 파일을 즉시 보내십시오. 또한 전자 서명을 선택합니다.

```
// Create AgreementCreationInfo
AgreementCreationInfo agreementCreationInfo = new AgreementCreationInfo();
 
// Add file
FileInfo fileInfo = new FileInfo();
fileInfo.setTransientDocumentId(transientDocumentId);
agreementCreationInfo.addFileInfosItem(fileInfo);
 
// Set state to IN_PROCESS, so the agreement is be sent immediately
agreementCreationInfo.setState(AgreementCreationInfo.StateEnum.IN_PROCESS);
agreementCreationInfo.setName("Contract");
agreementCreationInfo.setSignatureType(AgreementCreationInfo.SignatureTypeEnum.ESIGN);
```

다음으로 계약 수신자를 다음과 같이 추가합니다. 두 명의 수신자를 추가합니다(직원 및 관리자 섹션 참조).

```
// Provide emails of recipients to whom agreement is be sent
// Employee
ParticipantSetInfo participantSetInfo = new ParticipantSetInfo();
ParticipantSetMemberInfo participantSetMemberInfo = new ParticipantSetMemberInfo();
participantSetMemberInfo.setEmail("");
participantSetInfo.addMemberInfosItem(participantSetMemberInfo);
participantSetInfo.setOrder(1);
participantSetInfo.setRole(ParticipantSetInfo.RoleEnum.SIGNER);
agreementCreationInfo.addParticipantSetsInfoItem(participantSetInfo);
 
// Manager
participantSetInfo = new ParticipantSetInfo();
participantSetMemberInfo = new ParticipantSetMemberInfo();
participantSetMemberInfo.setEmail("");
participantSetInfo.addMemberInfosItem(participantSetMemberInfo);
participantSetInfo.setOrder(2);
participantSetInfo.setRole(ParticipantSetInfo.RoleEnum.SIGNER);
agreementCreationInfo.addParticipantSetsInfoItem(participantSetInfo);
```

마지막으로, `createAgreement` Adobe Sign Java SDK의 메서드:

```
// Create agreement using the transient document.
AgreementsApi agreementsApi = new AgreementsApi(apiClient);
AgreementCreationResponse agreementCreationResponse = agreementsApi.createAgreement(
    authorization, agreementCreationInfo, null, null);
 
System.out.println("Agreement sent, ID: " + agreementCreationResponse.getId());
```

이 코드를 실행하면 코드에 지정된 주소로 `<email_address>)` 서명을 요청합니다. 전자 메일에는 수신자가 서명을 수행하도록 Adobe Sign 포털으로 안내하는 하이퍼링크가 포함되어 있습니다. Adobe Sign Developer Portal(아래 그림 참조)에 문서가 표시되고 [getAgreementInfo](https://github.com/adobe-sign/AdobeSignJavaSdk/blob/master/docs/AgreementsApi.md#getAgreementInfo) 있습니다.

마지막으로 다음과 같이 PDF 서비스 API를 사용하여 PDF을 암호로 보호할 수도 있습니다 [예제](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples/protectpdf).

![계약 세부 정보 스크린샷](assets/HRWJ_9.png)

## 다음 단계

보시다시피 빠른 시작을 활용하여 간단한 웹 양식을 구현하여 Adobe PDF 서비스 API를 통해 Java에서 승인된 PDF을 생성할 수 있습니다. Adobe PDF API는 기존 클라이언트 애플리케이션에 원활하게 통합됩니다.

이 예시를 더 자세하게 들자면, 수신자가 원격으로 안전하게 서명할 수 있는 양식을 만들 수 있습니다. 여러 서명이 필요한 경우 워크플로우에서 양식을 여러 사용자에게 자동으로 전달할 수도 있습니다. 직원 온보딩이 향상되어 인사부에서 당신을 사랑할 것입니다.

체크아웃 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) 애플리케이션에 다양한 PDF 기능을 추가할 수 있습니다.
