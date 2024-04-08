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
source-git-commit: c64f1519438addb4081469afaed811fbf03ac88e
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---


# 문서 생성 API 튜토리얼

문서 생성 API는 Word 템플릿 및 JSON 데이터에서 PDF 및 Word 문서를 만듭니다.

>[!NOTE]
>
>문서 생성 API는 PDF 서비스 API에 포함되어 있습니다.

<table style="table-layout:fixed">
<tr>
 <td>
   <a href="automate-doc-gen.md">
      <img alt="문서 생성 자동화" src="assets/automate-doc-gen.png" />
   </a>
    <div>
   <a href="taggeroverview.md"><strong>문서 생성 자동화</strong></a>
    </div>
    <em>문서를 자동으로 대규모로 생성하는 방법 알아보기</em>
    <br>
  </td>
    <td>
    <img alt="스페이서" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
   <td>
    <img alt="스페이서" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
  </td>
   <td>
    <img alt="스페이서" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
</tr>
</table>

## 템플릿 만들기

문서 생성 API는 최종 문서 생성을 위해 입력 데이터와 함께 문서 템플릿(템플릿 태그 포함)을 허용합니다. 데이터 입력에 해당하는 실제 값을 바탕으로 문서 템플릿의 모든 템플릿 태그를 동적 콘텐츠로 대체하여 최종 문서를 생성한다.

<table style="table-layout:fixed">
<tr>
 <td>
   <a href="taggeroverview.md">
      <img alt="Adobe 문서 생성 Tagger 개요" src="assets/Taggeroverview_thumb.png" />
   </a>
    <div>
   <a href="taggeroverview.md"><strong>Adobe 문서 생성 Tagger 개요</strong></a>
    </div>
    <em>Adobe 문서 생성 API와 함께 사용하도록 설계된 Adobe 문서 생성 Tagger에 대한 개요를 살펴보십시오</em>
    <br>
  </td>
  <td>
   <a href="taggeraddtexttags.md">
      <img alt="텍스트 태그 추가" src="assets/Taggertexttags_thumb.png" />
   </a>
    <div>
   <a href="taggeraddtexttags.md"><strong>텍스트 태그 추가</strong></a>
    </div>
    <em>Adobe 문서 생성 API와 함께 사용할 Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 텍스트 태그를 추가하는 방법을 알아봅니다</em>
    <br>
  </td>
  <td>
   <a href="taggeraddimagetags.md">
      <img alt="이미지 태그 추가" src="assets/Taggerimagetags_thumb.png" />
   </a>
    <div>
   <a href="taggeraddimagetags.md"><strong>이미지 태그 추가</strong></a>
    </div>
    <em>Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 이미지 태그를 추가하고 Adobe 문서 생성 API를 사용하여 이미지를 문서에 동적으로 푸시하는 방법을 알아봅니다</em>
    <br>
  </td>
  <td>
   <a href="taggertables.md">
      <img alt="표 및 목록 태그 추가" src="assets/Taggertables_thumb.png" />
   </a>
    <div>
   <a href="taggertables.md"><strong>표 및 목록 태그 추가</strong></a>
    </div>
    <em>Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿에 테이블 및 목록 태그를 추가하고 Adobe 문서 생성 API를 사용하여 데이터를 기반으로 테이블 또는 목록 행을 동적으로 추가하는 방법을 알아봅니다.</em>
    <br>
  </td>
</tr>
<tr>
  <td>
   <a href="taggercalculations.md">
      <img alt="숫자 계산 태그 설정" src="assets/Taggercalculations_thumb.png" />
   </a>
    <div>
   <a href="taggercalculations.md"><strong>숫자 계산 태그 설정</strong></a>
    </div>
    <em>Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿의 숫자 계산 태그를 설정하여 Adobe 문서 생성 API를 사용하여 데이터 값의 집계 또는 산술을 계산하는 방법을 알아봅니다.</em>
    <br>
  </td>
  <td>
   <a href="taggerconditional.md">
      <img alt="조건부 콘텐츠 설정" src="assets/Taggerconditional_thumb.png" />
   </a>
    <div>
   <a href="taggerconditional.md"><strong>조건부 콘텐츠 설정</strong></a>
    </div>
    <em>Adobe 문서 생성 Tagger를 사용하여 Microsoft Word 템플릿의 섹션을 설정하고 Adobe 문서 생성 API를 사용하여 데이터를 기반으로 문서의 섹션을 동적으로 포함하거나 제외하는 방법을 알아봅니다.</em>
    <br>
  </td>
  <td>
    <img alt="스페이서" src="../assets/GrayBanner_Placeholder.png" />
    <div>
    <br>
  </td>
   <td>
    <img alt="스페이서" src="../assets/GrayBanner_Placeholder.png" />
    <div>
    <br>
  </td>
</tr>
</table>
