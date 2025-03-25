---
title: Acrobat Sign API 시작하기
description: 애플리케이션에 Acrobat Sign API를 포함하여 서명 및 기타 정보를 수집하는 방법에 대해 알아봅니다
feature: Acrobat Sign API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8089
thumbnail: KT-8089.jpg
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1906'
ht-degree: 0%

---

# Adobe Sign API 시작하기

[Acrobat Sign API](https://developer.adobe.com/adobesign-api/)는 서명된 계약을 관리하는 방법을 개선하는 좋은 방법입니다. 개발자는 시스템을 Sign API와 쉽게 통합할 수 있으므로 문서를 업로드하고, 서명을 위해 전송하고, 미리 알림을 전송하고, 전자 서명을 수집하는 안정적이고 간편한 방법을 제공합니다.

## 학습 내용

이 실습용 튜토리얼에서는 개발자가 Sign API를 사용하여 [!DNL Adobe Acrobat Services]&#x200B;(으)로 만든 응용 프로그램과 작업 과정을 향상시키는 방법에 대해 설명합니다. [!DNL Acrobat Services]에는 [Adobe PDF Services API](https://developer.adobe.com/document-services/apis/pdf-services), [Adobe PDF Embed API](https://developer.adobe.com/document-services/apis/pdf-embed/)&#x200B;(무료) 및 [Adobe 문서 생성 API](https://developer.adobe.com/document-services/apis/doc-generation)가 포함됩니다.

보다 구체적인 설명을 위해 애플리케이션에 Acrobat Sign API를 포함시켜 보험 양식에 대한 직원 정보와 같은 서명 및 기타 정보를 수집하는 방법을 살펴보세요. 간소화된 HTTP 요청 및 응답이 있는 일반 단계가 사용됩니다. 이러한 요청은 자주 사용하는 언어로 구현할 수 있습니다. [[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/)의 조합을 사용하여 PDF을 만들고, 이를 Sign API에 [임시](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md) 문서로 업로드하고, 계약 또는 [위젯](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md) 작업 과정을 사용하여 최종 사용자 서명을 요청할 수 있습니다.

## PDF 문서 만들기

Microsoft Word 템플릿을 만들고 PDF으로 저장하는 것으로 시작합니다. 또는 문서 생성 API를 사용하여 파이프라인을 자동화하여 Word에서 만든 템플릿을 업로드한 다음 PDF 문서를 생성할 수 있습니다. 문서 생성 API는 [!DNL Acrobat Services]의 일부입니다. [6개월 동안 무료인 다음 문서 트랜잭션당 단 $0.05(종량 과금제)](https://developer.adobe.com/document-services/pricing/main)에 지불하십시오.

이 예에서 템플릿은 몇 개의 서명자 필드가 있는 간단한 문서에 불과합니다. 일단 필드의 이름을 지정한 다음 나중에 이 튜토리얼에서 실제 필드를 삽입합니다.

![일부 필드가 있는 보험 양식의 스크린샷](assets/GSASAPI_1.png)

## 유효한 API 액세스 포인트 검색

Sign API로 작업하기 전에 [무료 개발자 계정](https://acrobat.adobe.com/ca/en/sign/developer-form.html)을 만들어 API에 액세스하고, 문서 교환 및 실행을 테스트하고, 전자 메일 기능을 테스트합니다.

Adobe은 Acrobat Sign API를 &quot;shards&quot;라는 많은 배포 단위로 전 세계에 배포합니다. 각 샤드는 NA1, NA2, NA3, EU1, JP1, AU1, IN1 및 기타 여러 고객의 계정을 제공합니다. 분할 이름은 지리적 위치에 해당합니다. 이러한 샤드는 API 끝점의 기본 URI(액세스 포인트)를 구성합니다.

Sign API에 액세스하려면 먼저 사용자의 계정에 대한 올바른 액세스 지점을 확인해야 합니다. 위치에 따라 api.na1.adobesign.com, api.na4.adobesign.com, api.eu1.adobesign.com 등이 될 수 있습니다.

```
  GET /api/rest/v6/baseUris HTTP/1.1
  Host: https://api.adobesign.com
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body (example):

  {
    "apiAccessPoint": "https://api.na4.adobesign.com/", 
    "webAccessPoint": "https://secure.na4.adobesign.com/" 
  }
```

위의 예에서 은 값을 액세스 포인트로 사용하는 응답입니다.

>[!IMPORTANT]
>
>이 경우 Sign API에 대한 후속 요청은 해당 액세스 포인트를 사용해야 합니다. 지역에 맞지 않는 액세스 포인트를 사용하면 오류가 발생합니다.

## 임시 문서 업로드

Adobe Sign을 사용하면 서명이나 데이터 수집을 위해 문서를 준비하는 다양한 플로우를 만들 수 있습니다. 응용 프로그램의 흐름과 관계없이 문서를 먼저 업로드해야 하며, 이 문서는 7일 동안만 사용할 수 있습니다. 그런 다음 후속 API 호출은 이 임시 문서를 참조해야 합니다.

POST 요청을 사용하여 `/transientDocuments` 끝점에 문서를 업로드합니다. multipart 요청은 파일 이름, 파일 스트림 및 문서 파일의 MIME(미디어) 유형으로 구성됩니다. 끝점 응답에 문서를 식별하는 ID가 포함되어 있습니다.

또한 애플리케이션은 서명 프로세스가 완료되면 앱에 알리고 Acrobat Sign에서 ping을 수행하기 위한 콜백 URL을 지정할 수 있습니다.


```
  POST /api/rest/v6/transientDocuments HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: multipart/form-data
  File-Name: "Insurance Form.pdf"
  File: "[path]\Insurance Form.pdf"
  Accept: application/json

  Response Body (example):

  {
     "transientDocumentId": "3AAA...BRZuM"
  }
```

## 웹 양식 만들기

웹 양식 (이전에 서명 위젯이라고 함)은 액세스 권한이 있는 모든 사용자가 서명할 수 있는 호스팅된 문서입니다. 웹 양식의 예로는 많은 사람들이 온라인으로 액세스하고 서명하는 등록 시트, 포기 및 기타 문서가 있습니다.

Sign API를 사용하여 새 웹 양식을 만들려면 먼저 임시 문서를 업로드해야 합니다. `/widgets` 끝점에 대한 POST 요청에서 반환된 `transientDocumentId` 을(를) 사용합니다.

이 예에서 웹 양식은 `ACTIVE`이지만 다음 세 가지 상태 중 하나로 만들 수 있습니다.

* 초안 — 웹 양식을 점진적으로 빌드합니다.

* 작성 — 웹 양식에서 양식 필드를 추가하거나 편집합니다.

* 활성 - 웹 양식을 즉시 호스팅합니다.

폼의 참가자에 대한 정보도 정의해야 합니다. `memberInfos` 속성에는 전자 메일과 같은 참가자에 대한 데이터가 포함되어 있습니다. 현재 이 집합은 두 개 이상의 구성원을 지원하지 않습니다. 그러나 웹 양식 서명자의 전자 메일은 웹 양식을 만들 때 알 수 없으므로 다음 예와 같이 빈 상태로 두어야 합니다. `role` 속성은 `memberInfos`의 구성원이 가정하는 역할(예: SIGNER 및 APPROVER)을 정의합니다.

```
  POST /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: application/json

  Request Body:

  {
    "fileInfos": [
      {
      "transientDocumentId": "YOUR-TRANSIENT-DOCUMENT-ID"
      }
     ],
    "name": "Insurance Form",
      "widgetParticipantSetInfo": {
          "memberInfos": [{
              "email": ""
          }],
      "role": "SIGNER"
      },
      "state": "ACTIVE"
  }

  Response Body (example):

  {
     "id": "CBJ...PXoK2o"
  }
```

`DRAFT` 또는 `AUTHORING`(으)로 웹 양식을 만든 다음 양식이 응용 프로그램 파이프라인을 통과할 때 해당 상태를 변경할 수 있습니다. 웹 양식 상태를 변경하려면 [PUT /widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState) 끝점을 참조하십시오.

## 웹 양식 호스팅 URL 읽기

다음 단계는 웹 양식을 호스팅하는 URL을 검색하는 것입니다. /widgets 끝점은 서명 및 기타 양식 데이터를 수집하기 위해 사용자에게 전달하는 웹 양식의 호스팅된 URL을 포함하여 웹 양식 데이터 목록을 검색합니다.

이 끝점은 목록을 반환하므로 웹 양식을 호스팅하는 URL을 가져오기 전에 `userWidgetList`에서 해당 ID로 특정 양식을 찾을 수 있습니다.

```
  GET /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body:

  {
    "userWidgetList": [
      {
        "id": "CBJCHB...FGf",
        "name": "Insurance Form",
        "groupId": "CBJCHB...W86",
        "javascript": "<script type='text/javascript' ...
        "modifiedDate": "2021-03-13T15:52:41Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...Rag*",
        "hidden": false
      },
      {
        "id": "CBJCHB...I8_",
        "name": "Insurance Form",
        "groupId": "CBJCHBCAABAAyhgaehdJ9GTzvNRchxQEGH_H1ya0xW86",
        "javascript": "<script type='text/javascript' language='JavaScript'
        src='https://sec
        "modifiedDate": "2021-03-13T02:47:32Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...AAB",
        "hidden": false
      },
      {
        "id": "CBJCHB...Wmc",
```

## 웹 양식 관리

이 양식은 사용자가 작성할 PDF 문서입니다. 그러나 사용자가 입력해야 하는 필드와 문서 내의 위치를 양식 편집기에 알려야 합니다.

![일부 필드가 있는 보험 양식의 스크린샷](assets/GSASAPI_1.png)

위 문서에는 아직 필드가 표시되지 않습니다. 이 필드들은 해당 크기와 위치뿐만 아니라 서명자의 정보를 수집하는 필드를 정의하는 동안 추가됩니다.

이제 &quot;계약&quot; 페이지의 [웹 양식](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform) 탭으로 이동하여 만든 양식을 찾습니다.

Acrobat Sign 관리 탭의 ![스크린샷](assets/GSASAPI_2.png)

![웹 양식이 선택된 Acrobat Sign 관리 탭의 스크린샷](assets/GSASAPI_3.png)

**편집**&#x200B;을 클릭하여 문서 편집 페이지를 엽니다. 사용 가능한 미리 정의된 필드는 오른쪽 패널에 있습니다.

![Acrobat Sign 양식 작성 환경의 스크린샷](assets/GSASAPI_4.png)

편집기를 사용하여 텍스트 및 서명 필드를 끌어서 놓을 수 있습니다. 필요한 필드를 모두 추가한 후 크기를 조정하고 정렬하여 양식을 다듬을 수 있습니다. 마지막으로 **저장**&#x200B;을 클릭하여 양식을 만듭니다.

양식 필드가 추가된 Acrobat Sign 양식 작성 환경의 ![스크린샷](assets/GSASAPI_5.png)

## 서명을 위해 웹 양식 보내기

웹 양식을 완료한 후 사용자가 양식을 작성하고 서명할 수 있도록 제출해야 합니다. 양식을 저장하면 URL과 포함된 코드를 보고 복사할 수 있습니다.

**웹 양식 URL 복사**: 이 URL을 사용하여 검토 및 서명을 위해 사용자를 이 계약의 호스팅된 버전으로 보냅니다. 예를 들면 다음과 같습니다.

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**웹 양식 포함 코드 복사**: 이 코드를 복사하여 HTML에 붙여넣어 웹 사이트에 계약을 추가합니다.

예를 들면 다음과 같습니다.

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![최종 웹 양식의 스크린샷](assets/GSASAPI_6.png)

사용자가 양식의 호스팅된 버전에 액세스하면 지정된 대로 필드가 배치된 상태로 처음 업로드된 임시 문서를 검토합니다.

![최종 웹 양식의 스크린샷](assets/GSASAPI_7.png)

그러면 사용자가 필드를 채우고 양식에 서명합니다.

![서명 필드를 선택하는 사용자의 스크린샷](assets/GSASAPI_8.png)

그런 다음 사용자는 이전에 저장한 서명이나 새 서명을 사용하여 문서에 서명합니다.

![서명 환경의 스크린샷](assets/GSASAPI_9.png)

![서명 스크린샷](assets/GSASAPI_10.png)

사용자가 **적용**&#x200B;을 클릭하면 Adobe이 전자 메일을 열고 서명을 확인하도록 지시합니다. 서명은 확인 메시지가 도착할 때까지 보류 상태로 유지됩니다.

![한 단계만 더 진행된 스크린샷](assets/GSASAPI_11.png)

이 인증은 다단계 인증을 추가하고 서명 프로세스 보안을 강화합니다.

![확인 메시지 스크린샷](assets/GSASAPI_12.png)

![완료 메시지 스크린샷](assets/GSASAPI_13.png)

## 완료된 웹 양식 읽기

이제 사용자가 작성한 양식 데이터를 가져올 차례입니다. `/widgets/{widgetId}/formData` 끝점은 사용자가 양식에 서명할 때 대화형 양식으로 입력한 데이터를 검색합니다.

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

결과 CSV 파일 스트림에는 양식 데이터가 포함됩니다.

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## 계약 만들기

웹 폼에 대한 대안으로 계약을 만들 수 있습니다. 다음 섹션에서는 Sign API를 사용하여 계약을 관리하는 몇 가지 간단한 단계에 대해 설명합니다.

서명 또는 승인을 위해 지정된 수신자에게 문서를 보내면 계약이 만들어집니다. API를 사용하여 계약 상태 및 완료 상태를 추적할 수 있습니다.

[임시 문서](https://helpx.adobe.com/sign/kb/how-to-send-an-agreement-through-REST-API.html), [라이브러리 문서](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md) 또는 URL을 사용하여 계약을 만들 수 있습니다. 이 예에서 계약은 이전에 만든 웹 양식과 마찬가지로 `transientDocumentId`을(를) 기반으로 합니다.

```
POST /api/rest/v6/agreements HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
Request Body:
{
    "fileInfos": [
      {
      "transientDocumentId": "{transientDocumentId}"
      }
     ],
    "name": "{agreementName}",
    "participantSetsInfo": [
      {
      "memberInfos": [
          {
          "email": "{signerEmail}"
          }
        ],
        "order": 1,
        "role": "SIGNER"
      }
    ],
    "signatureType": "ESIGN",
    "state": "IN_PROCESS"
  }
```

이 예에서 계약은 IN_PROCESS로 생성되지만 다음 세 가지 상태 중 하나로 생성할 수 있습니다.

* 초안 - 계약을 보내기 전에 점진적으로 빌드합니다.

* 작성 — 계약서에 양식 필드를 추가하거나 편집합니다.

* IN_PROCESS - 계약을 즉시 보냅니다.

계약 상태를 변경하려면 `PUT /agreements/{agreementId}/state` 끝점을 사용하여 아래에서 허용되는 상태 전환 중 하나를 수행하십시오.

* 작성으로 초안

* IN_PROCESS 작성

* 처리 중(_P) - 취소됨

위의 `participantSetsInfo` 속성은 계약에 참여할 것으로 예상되는 사람과 수행(서명, 승인, 확인 등) 작업을 위한 전자 메일을 제공합니다. 위의 예에서 참가자는 서명자만 존재합니다. 자필 서명은 문서당 4개로 제한됩니다.

웹 양식과 달리 계약을 만들면 Adobe이 서명을 위해 자동으로 보냅니다. 끝점이 계약의 고유 식별자를 반환합니다.


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## 계약 멤버에 대한 정보 검색

계약을 만든 후에는 `/agreements/{agreementId}/members` 끝점을 사용하여 계약 멤버에 대한 정보를 검색할 수 있습니다. 예를 들어 참가자가 계약에 서명했는지 여부를 확인할 수 있습니다.

```
GET /api/rest/v6/agreements/{agreementId}/members HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: application/json
```

결과 JSON 응답 본문에는 참여자에 대한 정보가 포함됩니다.

```
  Response Body:

  {
     "participantSets":[
        {
           "memberInfos":[
              {
                 "id":"CBJ...xvM",
                 "email":"participant@email.com",
                 "self":false,
                 "securityOption":{
                    "authenticationMethod":"NONE"
                 },
                 "name":"John Doe",
                 "status":"ACTIVE",
                 "createdDate":"2021-03-16T03:48:39Z",
                 "userId":"CBJ...vPv"
              }
           ],
           "id":"CBJ...81x",
           "role":"SIGNER",
           "status":"WAITING_FOR_MY_SIGNATURE",
           "order":1
        }
     ],
```

## 계약 미리 알림 보내기

업무 규칙에 따라, 기한을 정하면 특정 날짜 이후에 참가자가 계약에 서명할 수 없습니다. 계약에 만료일이 있는 경우 참가자에게 해당 날짜가 다가오면 알림을 보낼 수 있습니다.

마지막 섹션의 `/agreements/{agreementId}/members` 끝점을 호출한 후 받은 계약 멤버의 정보를 기반으로 아직 계약에 서명하지 않은 모든 참가자에게 전자 메일 알림 메시지를 보낼 수 있습니다.

`/agreements/{agreementId}/reminders` 끝점에 대한 POST 요청이 `agreementId` 매개 변수로 식별된 계약의 지정된 참가자에 대한 미리 알림을 만듭니다.

```
POST /agreements/{agreementId}/reminders HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
  Request Body:

  {
    "recipientParticipantIds": [{agreementMemberIdList}],
    "agreementId": "{agreementId}",
    "note": "This is a reminder that you haven't signed the agreement yet.",
    "status": "ACTIVE"
  }

  Response Body:

  {
     id (string, optional): An identifier of the reminder resource created on the
     server. If provided in POST or PUT, it will be ignored
  }
```

알림 메시지를 게시하면 사용자는 계약의 세부 정보와 계약 링크가 포함된 이메일을 수신하게 됩니다.

![알림 메시지 스크린샷](assets/GSASAPI_14.png)

## 완료된 계약 읽기

웹 폼처럼 수신자가 서명한 계약에 대한 세부 사항을 읽을 수 있습니다. `/agreements/{agreementId}/formData` 끝점은 사용자가 웹 양식에 서명할 때 입력한 데이터를 검색합니다.

```
GET /api/rest/v6/agreements/{agreementId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
Response Body:
"completed","email","role","first","last","title","company","agreementId"
"2021-03-16 18:11:45","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","CBJCHBCAABAA5Z84zy69q_Ilpuy5DzUAahVfcNZillDt"
```

## 다음 단계

Acrobat Sign API를 사용하면 문서, 웹 양식 및 계약을 관리할 수 있습니다. 웹 양식 및 계약을 사용하여 만든 간단하면서도 완전한 워크플로우는 개발자가 모든 언어를 사용하여 웹 양식을 구현할 수 있는 일반적인 방법으로 수행됩니다.

Sign API 작동 방식에 대한 개요는 [API 사용 개발자 안내서](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/api_usage.md)에서 예를 찾을 수 있습니다. 이 설명서에는 문서 전체에서 수행한 많은 단계와 기타 관련 항목에 대한 간단한 도움말이 포함되어 있습니다.

Acrobat Sign API는 [단일 및 다중 사용자 전자 서명 플랜](https://acrobat.adobe.com/kr/ko/sign/pricing/plans.html)의 여러 계층을 통해 사용할 수 있으므로 요구 사항에 가장 적합한 가격 모델을 선택할 수 있습니다. Sign API를 앱에 통합하는 것이 얼마나 쉬운지 알게 되었으므로 푸시 기반 프로그래밍 모델인 [Acrobat Sign Webhooks](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md)와 같은 다른 기능에 관심을 가질 수 있습니다. 앱에서 Acrobat Sign 이벤트를 자주 확인하도록 요구하는 대신 Webhook을 사용하면 이벤트가 발생할 때마다 Sign API가 POST 콜백 요청을 실행하는 HTTP URL을 등록할 수 있습니다. Webhook은 실시간 및 인스턴트 업데이트를 통해 응용 프로그램을 지원하여 강력한 프로그래밍을 활성화합니다.

6개월 무료 Adobe PDF Services API 체험판이 종료되는 시점과 무료 Adobe PDF Embed API에 대해서는 [선불 종량제 가격](https://developer.adobe.com/document-services/pricing/main)을 확인하십시오.

자동 문서 생성 및 문서 서명과 같은 흥미로운 기능을 앱에 추가하려면 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)을(를) 시작하십시오.
