---
title: 문서 생성 API Tutorials
description: 문서 생성 API 튜토리얼 개요
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: 0f62890207722245f33be41ad4c77ba68bf7a0fc
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# 문서 생성 API 튜토리얼

문서 생성 API는 Word 템플릿 및 JSON 데이터에서 PDF 및 Word 문서를 만듭니다.

>[!NOTE]
>
>문서 생성 API는 PDF 서비스 API에 포함되어 있습니다.

## 문서 생성 중

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen
  {target = _self}
  {title = Automate document generation}
  {description = Learn how to automatically generate documents at scale}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium}
  {cta = Watch}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Automate document generation">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" title="문서 생성 자동화" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium" alt="문서 생성 자동화"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" title="문서 생성 자동화">문서 생성 자동화</a>
                    </p>
                    <p class="is-size-6">문서를 자동으로 대규모로 생성하는 방법 알아보기</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 템플릿 만들기

문서 생성 API는 최종 문서 생성을 위해 입력 데이터와 함께 문서 템플릿(템플릿 태그 포함)을 허용합니다. 데이터 입력에 해당하는 실제 값을 바탕으로 문서 템플릿의 모든 템플릿 태그를 동적 콘텐츠로 대체하여 최종 문서를 생성한다.

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Overview of the Adobe Document Generation Tagger">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" title="Adobe 문서 생성 Tagger 개요" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_17b19864efffdb1f8c54017812c7de662e17bf163.png?width=400&format=webply&optimize=medium" alt="Adobe 문서 생성 Tagger 개요"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" title="Adobe 문서 생성 Tagger 개요">Adobe 문서 생성 Tagger 개요</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 API와 함께 사용하도록 설계된 Adobe 문서 생성 Tagger에 대한 개요를 살펴보십시오</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding text tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" title="텍스트 태그 추가" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_113bb0b6c3dfa1329810d3afbba3498af82c6c875.png?width=400&format=webply&optimize=medium" alt="텍스트 태그 추가"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" title="텍스트 태그 추가">텍스트 태그 추가</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 텍스트 태그를 추가하는 방법을 알아봅니다.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding image tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" title="이미지 태그 추가" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1095ed3adad9c9360bb3184dccc41a72a3da94edc.png?width=400&format=webply&optimize=medium" alt="이미지 태그 추가"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" title="이미지 태그 추가">이미지 태그 추가</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 이미지 태그를 추가하여 이미지를 문서에 동적으로 푸시하는 방법을 알아봅니다</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding tables and list tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" title="표 및 목록 태그 추가" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1c2cc8e4bf3a85a977104a3d94073c37b93dcfdf9.png?width=400&format=webply&optimize=medium" alt="표 및 목록 태그 추가"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" title="표 및 목록 태그 추가">테이블 및 목록 태그 추가</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 테이블 및 목록 태그를 추가하고 데이터를 기반으로 테이블 또는 목록 행을 동적으로 추가하는 방법을 알아봅니다.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting numerical calculation tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" title="숫자 계산 태그 설정" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1a64d90689430bd8a1f7a272a66f006f0808ab6cf.png?width=400&format=webply&optimize=medium" alt="숫자 계산 태그 설정"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" title="숫자 계산 태그 설정">숫자 계산 태그 설정</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 태그거를 사용하여 Microsoft Word 템플릿에서 수치 계산 태그를 설정하여 데이터 값의 집계 또는 산술을 계산하는 방법을 알아봅니다</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting conditional content">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" title="조건부 콘텐츠 설정" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_145cebc42cffee358ed1beffcd5015ecb595fc82a.png?width=400&format=webply&optimize=medium" alt="조건부 콘텐츠 설정"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" title="조건부 콘텐츠 설정">조건부 콘텐츠 설정</a>
                    </p>
                    <p class="is-size-6">Adobe 문서 생성 태그거를 사용하여 Microsoft Word 템플릿에서 섹션을 설정하여 데이터를 기반으로 문서의 섹션을 동적으로 포함하거나 제외하는 방법을 살펴보세요</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">시청</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
