---
title: Microsoft Power Automate 자격 증명 가져오기
description: Adobe PDF 서비스를 사용하거나 사용하기 위해 자격 증명을 얻는 방법에 대해 알아봅니다.
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-10382.jpg
jira: KT-10382
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 3%

---

# Microsoft Power Automate 자격 증명 가져오기

[Microsoft Power Automate](https://powerautomate.microsoft.com/ko-kr/) 시민 개발자와 개발자가 코드를 작성하지 않고도 비즈니스를 향상시킬 수 있는 강력한 자동화 프로세스를 만들 수 있는 강력한 방법을 제공합니다. [Adobe PDF 서비스](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) 커넥터 [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services)를 통해 사용자는 Microsoft Power Automate 내의 Adobe PDF Services API에서 사용할 수 있는 작업을 수행할 수 있습니다.

이 튜토리얼에서는 Adobe PDF Services를 사용하거나 사용하기 위해 자격 증명을 얻는 방법을 살펴봅니다. 평가판 사용자인지 또는 기존 고객인지에 따라 이 튜토리얼에서는 자격 증명을 얻기 위한 적절한 단계를 안내합니다.

## Microsoft Power Automate 사용자는 Adobe PDF Services 커넥터를 어떻게 사용할 수 있습니까?

기존 Microsoft Power Automate 사용자는 [평가판 자격 증명 가져오기](https://www.adobe.com/go/powerautomate_getstarted_kr) Adobe PDF 서비스 위 링크는 Microsoft Power Automate 사용자를 위해 이 프로세스를 지원하는 특별 등록 링크입니다.

![Adobe Developer 사용자 로그인](assets/credentials_1.png)


>[!IMPORTANT]
> 평가판을 위해 로그인하는 경우 Enterprise ID이 아닌 Adobe ID을 사용해야 합니다. 현재 Adobe PDF 서비스 API를 구독하지 않고 Enterprise ID으로 로그인을 시도하면 엔터프라이즈에서 Adobe PDF 서비스 API를 사용할 권한이 없기 때문에 권한 오류가 발생할 수 있습니다. 따라서 무료 개인 Adobe ID을 사용하는 것이 좋습니다.
>

1. 로그인 후 새 자격 증명의 이름을 선택하라는 메시지가 표시됩니다. 다음을 입력합니다. *자격 증명 이름*.
1. 개발자 약관에 동의하려면 확인란을 선택합니다.
1. 선택 **[!UICONTROL 자격 증명 만들기]**.

   ![자격 증명 이름 지정](assets/credentials_2.png)

이러한 자격 증명은 5개의 다른 값을 포함합니다.

* 클라이언트 ID(API 키)
* 클라이언트 암호
* 조직 ID
* 기술 계정 ID
* Base64(인코딩된 개인 키)

![새 자격 증명](assets/credentials_3.png)

이러한 값을 모두 포함하는 JSON 파일도 자동으로 시스템에 다운로드됩니다. 이 파일의 이름은 `pdfservices-api-pa-credentials.json` 다음과 같습니다.

```json
{
 "client_id": "client id value",
 "client_secret": "client secret value",
 "organization_id": "organized id value",
 "account_id": "account id value",
 "base64_encoded_private_key": "base64 version of the private key"
}
```

개인 키의 복사본을 다시 가져올 수 없으므로 이 파일을 안전한 위치에 저장하십시오.

### Microsoft Power Automate에서 연결 추가

자격 증명을 취득했다면 이제 Microsoft Power Automate 흐름에서 사용할 수 있습니다.

1. 사이드바 메뉴에서 **[!UICONTROL 데이터]** 메뉴와 선택 **연결**:

   ![Microsoft Power Automate 사이트의 연결 메뉴](assets/credentials_4.png)

1. 선택 **+ [!UICONTROL 새 연결]**.

1. 다음 화면에 가능한 연결 유형 목록이 표시됩니다. 오른쪽 상단에서 &quot;adobe&quot;를 입력하여 옵션을 필터링합니다.

   ![Adobe 연결 목록](assets/credentials_5.png)

1. 선택 **[!UICONTROL Adobe PDF 서비스(미리 보기)]**.
1. 모달 창에서 이전에 생성한 5개의 값을 모두 입력합니다. 선택 **[!UICONTROL 만들기]** 완료 시.

   ![자격 증명 정보를 입력하는 양식 필드](assets/credentials_6.png)

이제 Microsoft Power Automate에서 Adobe PDF 서비스를 사용할 준비가 되었습니다.

### 자격 증명을 만든 후 자격 증명에 액세스

자격 증명을 이미 만들었는데 다운로드한 자격 증명을 잘못 배치한 경우 [Adobe Developer 콘솔](https://developer.adobe.com/console).

1. 로그인 후 [Adobe Developer 콘솔](https://developer.adobe.com/console)먼저 프로젝트를 찾아 선택합니다.
1. 왼쪽 메뉴에서 *자격 증명*, 선택 **서비스 계정(JWT)**:

   ![기존 자격 증명](assets/credentials_7.png)

1. 여기에 표시된 다섯 가지 값을 확인합니다. *클라이언트 ID*, *클라이언트 암호*, *기술 계정 ID*, *기술 계정 이메일*&#x200B;및 *조직 ID*.

죄송합니다. 이전 개인 키를 다운로드할 수는 없지만 &quot;공개/비공개 키 페어 생성&quot; 버튼을 사용하여 새 키를 생성할 수 있습니다.

## 기존 Adobe PDF 서비스 자격 증명 사용

기존 Adobe PDF 서비스 API 자격 증명이 있는 경우 [!DNL Adobe Acrobat Services] 웹 사이트에서 Microsoft Power Automate와 함께 사용할 수 있습니다. 등록하는 동안 SDK를 다운로드한 경우 기존 자격 증명은 대부분 이름이 지정된 JSON 파일 형식으로 제공됩니다 `pdfservices-api-credentials.json`. 이 JSON 파일에는 연결 자격 증명을 만들 때 필요한 5개의 키가 포함되어 있습니다. JSON 파일의 각 값을 해당 연결 필드에 복사합니다.

개인 키 값은 `private.key`.

위에 설명된 대로 Adobe Developer Console에서 값을 가져올 수도 있습니다.

## 방법 [!DNL Adobe Acrobat Services] 사용자가 Microsoft Power Automate로 작업을 시작하십니까?

Power Automate 작업을 시작하려면 <https://powerautomate.microsoft.com> &quot;무료 시작&quot; 버튼을 사용합니다. Microsoft 계정이 없는 경우에는 계정을 만들어야 합니다. 로그인 후 Power Automate 대시보드가 표시됩니다.

![PA 대시보드, 처음 보기](assets/credentials_8.png)

이 튜토리얼의 시작 부분에서 설명한 대로 새 플로우를 만들고 단계를 추가한 다음 Adobe PDF 서비스를 찾습니다. 작업을 선택하면 프리미엄 계정이 필요하다는 경고가 표시될 수 있습니다.

![프리미엄 계정 경고](assets/credentials_9.png)

위 스크린샷과 같이 회사 계정으로 전환하거나 새 조직 계정을 설정할 수 있습니다. 완료되면 Adobe PDF 서비스 동작을 추가할 수 있습니다.

자세한 내용은 다음을 사용하여 첫 번째 Microsoft Power Automate 흐름 만들기를 참조하십시오. [!DNL Adobe Acrobat Services], 참조 [Microsoft Power Automate에서 간단한 워크플로우 만들기](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/create-workflow-power-automate.html).

## 추가 리소스

자세한 내용은 다음 리소스를 참조하십시오.

* 먼저 Adobe PDF Services Power Automate 문서를 살펴봅니다. <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. 이러한 리소스는 여기에서 배운 내용을 보완합니다.
* 예제가 필요하십니까? 당신은 많은 것을 찾을 수 있습니다 [Power Automate 템플릿](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) PDF 서비스 시연
* 라이브 비디오 콘텐츠 [종이 클립](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF)또한 Power Automate 사용을 보여주는 비디오가 포함되어 있습니다.
* 추가 [Adobe 기술 블로그](https://medium.com/adobetech/tagged/microsoft-power-automate) Power Automate를 사용한 작업에 대한 많은 문서를 제공합니다.
* 마지막으로, 핵심과 [PDF 서비스](https://developer.adobe.com/document-services/docs/overview/) 설명서도 제공됩니다.
