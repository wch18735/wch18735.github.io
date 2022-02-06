---
title: "[Vue.js] Vue.js Single File Component"
excerpt: "vue.js single file component"
date: 2022-02-02
category:
    - vue.js
tag:
    - vue.js
    - vue ajax
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Single File Component

확장자가 vue 인 파일이다. Vue 컴포넌트의 템플릿 속성에 사용할 HTML 데이터를 가지고 있다. Component 를 만들기 위한 HTML 코드를 문자열로 세팅하는 것이 아니라 외부에 파일로 만들고 이를 이용해 Component 를 만들 수 있다. 외부 HTML 파일을 로드하는 방법은 많으며 여기는 HttpVueLoader 라이브러리를 사용한다.

1. 외부에 Vue 파일 생성 (.vue 파일)
2. 해당 .vue 파일 내부에 `<a href="?">` 경로를 `<router-link to='?'>` 형태로 변환
3. router.js 에서 `httpVueLoader(경로)` 사용해 컴포넌트 반환

참고로 Eclipse 에서 `.vue` 파일은 text 형태로 링크 강조가 일어나지 않는다. 원활한 편집을 위해 Vue 파일 하이라이트 기능을 위한 플러그인을 Eclipse Marketplace 에서 설치하는 것을 추천한다.


 