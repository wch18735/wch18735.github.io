---
title: "[Front-end] 면접 질문 프린팅 사이트"
excerpt: "Javascript로 Excel 정보를 읽어 표시하는 웹"
date: 2021-06-02
layout: single
classes: wide
category:
    - frontend
tag:
    - css
    - grid
    - javascript
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 면접 질문 랜덤 표시 웹

면접 준비는 정말 힘듭니다. 잠시 기분전환 겸 면접질문을 자동을 표현해주는 웹을 제작하고, 정적 호스팅을 통해 같이 스터디하시는 분들도 접속할 수 있도록 만들었습니다.

이번 짤막한 프로젝트로 배운 건 다음과 같습니다.

1. **CSS** 이용해 **Web contents**를 원하는 장소에 위치시키는 방법
2. **Javascript**로 Excel 파일 읽는 방법
2. **Amazon S3**에 정적 웹 사이트 올리는 방법

어려운 건 하나도 없었습니다. 역시 구글에 검색하면 모든 게 다 나오네요. 영어로 쓰여있는 걸 해석하는게 조금 귀찮았지만 결과물을 보니 매우 만족스럽네요!

## Grid

Grid 템플릿은 화면을 격자 형식으로 나누는 것이라 생각하면 편합니다. 제가 이해한 바로는 배치하고 싶은 **컨텐츠**를 담고있는 **컨테이너**에 **그리드(Grid)** 형식을 부여합니다. 코드로는 다음과 같습니다.

```css
.content-container {
    display: grid;
}
```

그러면 이 컨테이너는 그리드 레이아웃을 가지며 내부 컨텐츠들은 그리드에 맞춰 배치될 수 있습니다. 한 칸에 하나밖에 들어가지 못하는건가? 라는 생각이 들었는데, 그런 경우 다른 컨테이너 하나를 더 만들어서 부착하면 되는 것이었어요. **응용!!**

칸을 어떻게 나눌 것인가는 아래 코드로 나타낼 수 있습니다. 행과 열 개수와 더불어 사이즈 비율을 지정해줄 수 있어 매우 직관적입니다.

```css
.content-container {
    display: grid;
    grid-template-columns: ... ;
    grid-template-rows: ... ;
}
```

이렇게 나뉜 칸은 **grid-column-start / grid-column-end**와 **grid-row-start / grid-row-end**를 이용해 영역을 지정하고 표시해줄 수 있습니다.

지정되지 않은 개체들은 자동으로 (작은 행, 작은 열) $\rarr$ (큰 행, 큰 열) 순서로 배치됩니다. 이때, 해당 아이템은 그 그리드 한 칸 안에서 어떤 위치에 지정될 것인가를 **justify-self**와 **align-self**를 통해 지정할 수 있습니다.

```css
/* 정중앙에 배치 */
.item-a {
    justify-self: center;
    align-self: center;
}
```

## JavaScript로 Excel 파일 읽는 방법

먼저 **HTML** 파일에 **Cloud**에 저장된 소스를 불러오는 코드를 추가합니다.

```html
<script src="https://unpkg.com/read-excel-file@4.x/bundle/read-excel-file.min.js"></script>
```

이 코드가 가끔씩 응답을 하지 않는 경우가 있습니다. **npm (Node Package Manager)** 를 통해 `import` 할 수 있다고 하는데 이는 기능들을 추가하면서 활용해볼 생각입니다.

등록한 이벤트 리스너가 동작을 안 하길래 이리저리 코드를 확인하다가 **HTML**에 스크립트 추가를 해주지 않은 걸 발견했습니다. 꼭 html에서 `type="text/javascript"` 와 함께 스크립트를 추가하기를 습관화 해봐요! ㅎㅎ

아래는 질문의 갯수만큼 랜덤한 숫자를 발생시키는 스크립트 동작입니다. `then`을 이용해 작업을 순차적으로 진행시켰고, 이들이 완료되면 화면에 띄우도록 했습니다.

```js
var input = document.getElementById('input');
var question_list = Array();
var idx_list = Array();

const previous_btn = document.querySelector('.previous-btn');
const next_btn = document.querySelector('.next-btn');
const question = document.querySelector('.questions');

input.addEventListener('change', function(){
    readXlsxFile(input.files[0]).then(function(data){
        data.forEach(function(elements){
            question_list.push(elements[0]);
        })
    }).then(function(){
        var i;
        for(i = 0; i < question_list.length; i++)
        {
            idx_list.push(Math.floor(Math.random() * question_list.length));
        }

        question.innerHTML = "Go!";
    });
});
```

## Amazon S3 정적 웹 호스팅

정말 [이 곳](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/WebsiteHosting.html)에 표기된 방법을 그대로 따라했습니다. 처음엔 폴더 그대로 업로드를 했는데, 그러면 **index.html**을 찾지 못하더군요. 그래서 내부 파일구조를 그대로 복사해 업로드했더니 성공적으로 호스팅 할 수 있었습니다.


## 사이트

[여기](http://myeonjeop.com.s3-website-us-east-1.amazonaws.com/)에서 체험해볼 수 있습니다. 동작은 아래와 같습니다.

![interview web](/_img/2021-06-02/interview_web.gif){: .align-center}