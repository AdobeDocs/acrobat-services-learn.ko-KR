---
title: API Tutorials [!DNL Adobe Acrobat Services]개
description: ' [!DNL Adobe Acrobat Services]에 대한 개요 페이지'
feature: Acrobat Sign API, PDF Services API, PDF Embed API, Document Generation API, PDF Electronic Seal API, PDF Extract API, PDF Accessibility Auto-Tag API
role: Developer
level: Beginner, Intermediate, Experienced
jira: KT-7463
type: Tutorial
thumbnail: KT-7463.jpg
exl-id: c73feb77-4057-42fd-831c-a5004c7637c1
source-git-commit: 2e23ffef7e4438a9287c909d2fdb13968195c31f
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 2%

---

# API 튜토리얼 [!DNL Adobe Acrobat Services]개

[!DNL Adobe Acrobat Services]에는 6개의 기본 API가 있습니다.

* [!DNL Adobe PDF Services API]
* [!DNL Adobe PDF Embed API]
* [!DNL Adobe Document Generation API]
* [!DNL Adobe PDF Electronic Seal API]
* [!DNL Adobe PDF Extract API]
* [!DNL Adobe PDF Accessibility Auto-Tag API]

후자의 두 API와 해당 SDK는 유료 서비스의 일부로 [!DNL Adobe PDF Services API]에 번들로 제공됩니다. [!DNL PDF Embed API]은(는) 무료 제공입니다. 이들 API는 일련의 최신 클라우드 기반 웹 서비스를 통해 문서 콘텐츠의 생성, 조작 및 변형을 자동화합니다. 간편한 브랜드 환경을 제공하여 문서 사용자와의 상호 작용을 제어하고, PDF 워크플로우를 간소화하고, 사용 및 유지를 촉진할 수 있도록 합니다. 이 자습서는 [!DNL Adobe Acrobat Services] API를 사용하여 간단하고, 빠르고, 브랜드 성격을 가진 경험을 최신으로 제공하는 데 도움이 됩니다.

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices
  {target = _self}
  {title = PDF Services API}
  {description = PDF APIs with SDKs for node.js, .Net, and Java to create, convert, OCR PDFs, and more}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/docgen/overview-docgen
  {target = _self}
  {title = Document Generation API}
  {description = Generate PDF and Word documents from Word templates and JSON data}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility
  {target = _self}
  {title = Adobe PDF Accessibility Auto-Tag API tutorials}
  {description = This AI-powered API automatically tags documents making it easy to scale PDF accessibiity}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract
  {target = _self}
  {title = PDF Extract API}
  {description = Unlock the structure and content elements of any PDF with a web service powered by Adobe Sensi's machine learning}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal
  {target = _self}
  {title = PDF Electronic Seal API}
  {description = Learn how to apply a tamper-evident electronic seal to PDFs at scale}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed
  {target = _self}
  {title = PDF Embed API}
  {description = Free Javascript API to embed high-fidelity PDFs, enable collaboration, and see analytics}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign
  {target = _self}
  {title = Acrobat Sign API}
  {description = Integrate e-signatures into your platform or application}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/usecases/overview-usecases
  {target = _self}
  {title = Adobe Acrobat Services API use cases}
  {description = A wide variety of Acrobat Services API use cases}
  {image = https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Services API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" title="PDF 서비스 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium" alt="PDF 서비스 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" title="PDF 서비스 API">PDF 서비스 API</a>
                    </p>
                    <p class="is-size-6">node.js, .Net 및 Java용 SDK로 API를 PDF 하여 OCR PDF 등을 생성, 변환 및 관리합니다.</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Document Generation API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" title="문서 생성 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium" alt="문서 생성 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" title="문서 생성 API">문서 생성 API</a>
                    </p>
                    <p class="is-size-6">Word 템플릿 및 JSON 데이터에서 PDF 및 Word 문서 생성</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe PDF Accessibility Auto-Tag API tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" title="Adobe PDF 접근성 자동 태그 API 튜토리얼" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="Adobe PDF 접근성 자동 태그 API 튜토리얼"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" title="Adobe PDF 접근성 자동 태그 API 튜토리얼">Adobe PDF 접근성 자동 태그 API 자습서</a>
                    </p>
                    <p class="is-size-6">이 AI 기반 API는 문서에 자동으로 태그를 지정하므로 쉽게 PDF 접근성을 조정할 수 있습니다</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Extract API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" title="PDF 추출 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="PDF 추출 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" title="PDF 추출 API">PDF 추출 API</a>
                    </p>
                    <p class="is-size-6">Adobe Sensi의 머신 러닝에서 제공하는 웹 서비스를 통해 PDF의 구조와 콘텐츠 요소를 잠금 해제하세요.</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Electronic Seal API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" title="PDF 전자 봉인 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium" alt="PDF 전자 봉인 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" title="PDF 전자 봉인 API">전자 봉인 API PDF</a>
                    </p>
                    <p class="is-size-6">PDF에게 변조 방지 전자 봉인을 규모에 맞게 적용하는 방법 알아보기</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Embed API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" title="PDF 포함 API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium" alt="PDF 포함 API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" title="PDF 포함 API">PDF Embed API</a>
                    </p>
                    <p class="is-size-6">고품질 PDF을 포함하고, 공동 작업을 활성화하고, 분석을 볼 수 있는 무료 Javascript API</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Acrobat Sign API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" title="Acrobat Sign API" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium" alt="Acrobat Sign API"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" title="Acrobat Sign API">Acrobat Sign API</a>
                    </p>
                    <p class="is-size-6">전자 서명을 플랫폼 또는 애플리케이션에 통합</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe Acrobat Services API use cases">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" title="Adobe Acrobat Services API 사용 사례" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium" alt="Adobe Acrobat Services API 사용 사례"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" title="Adobe Acrobat Services API 사용 사례">Adobe Acrobat 서비스 API 사용 사례</a>
                    </p>
                    <p class="is-size-6">다양한 Acrobat Services API 사용 사례</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">튜토리얼 찾아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
