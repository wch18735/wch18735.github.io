---
title: "[Front-end] Counter"
excerpt: "클릭 이벤트로 값을 더하고 빼는 기능을 구현한 웹"
date: 2021-05-07
layout: single
classes: wide
category:
    - front-end
tag:
    - html
    - css
    - javascript
    - Codding Addict
    - freeCodeCamp
    - span
    - button
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Counter project

이번 프로젝트는 버튼에 이벤트를 등록하고, 매 이벤트마다 동작하는 함수를 정의해 화면에 나타내는 간단한 웹 제작입니다. 동작은 아래에서 보시는 것 처럼 **INCREASE**, **DECREASE**, **RESET** 버튼으로 가운데 숫자를 조작합니다.  
  
![counter](/_img/2021-05-07/counter.gif){: .align-center}

## HTML code

HTML 코드는 아래와 같습니다. **\<span\>** 태그와 **\<button\>** 태그를 모두 포함하는 **\<div\>** 태그가 **\<main\>** 안에 위치합니다. 모든 버튼들은 `button-container` attribute를 가진 **\<div\>** 태그에 포함됩니다.

``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Counter</title>

    <!-- styles -->
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <main>
      <div class="container">
        <h1>
          Counter
        </h1>
        <span id="value">0</span>
        <div class="button-container">
          <button class="btn decrease">decrease</button>
          <button class="btn reset">reset</button>
          <button class="btn increase">increase</button>
        </div>
      </div>
    </main>

    <!-- javascript -->
    <script src="app.js"></script>
  </body>
</html>
```

### span

**\<span\>** 태그는 컨텐츠를 담는 **inline container**입니다. 이때, inline container란 **\<block\>** 형식과 같이 한 행 모두를 차지하는 것이 아니라, 화면의 일부만을 사용하는 것을 말합니다. 보통 요소 그룹에 동일한 속성을 부여할 때 사용합니다. **\<span\>** 태그의 사용 예시는 아래와 같습니다.  

``` html
<p>Add the <span class="ingredient">basil</span>, <span class="ingredient">pine nuts</span> and <span class="ingredient">garlic</span> to a blender and blend into a paste.</p>

<p>Gradually add the <span class="ingredient">olive oil</span> while running the blender slowly.</p>
```

``` css
span.ingredient {
    color: #f00;
}
```

이처럼 각 텍스트 부분에 동일한 속성을 부여하기 위해 많이 사용되는 것이 **\<span\>** 태그입니다. 예시에서는 ingredient 클래스를 통해 텍스트의 색상을 조정합니다.

### button

<button name="button">Press me</button>

**\<button\>** 태그는 클릭 가능한 버튼을 생성합니다. 특정 입력을 완료하고 제출할 때, 접근할 때와 같이 여러 경우에 활용될 수 있습니다. 버튼 위의 텍스트는 버튼 태그 사이 문자가 나타나며, 외형은 CSS file을 이용해 사용자 임의로 바꿀 수 있습니다. 다양한 **Attributes**가 있기 때문에 [이곳](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)을 참조해 한 번 읽어보면 좋을 것 같습니다.  
  
\<button\> 요소 안에는 \<input\> 태그보다 더 쉽게 여러 스타일을 첨가할 수 있습니다. 예를들어 **\<i\> (아이콘)**, **\<br\> (공백 계행)**, **\<img\> (사진)**, **\:: before**, **\:: after** 등을 사용할 수 있습니다.  
  
만약 버튼이 제출용이 아니라면 **type="button"**으로 지정하여 어떠한 동작도 하지 않음을 명시하는 것이 권장됩니다. 만약 그렇지 않다면 버튼이 수행하는 전송 시도에 의해 현재 document가 처리하는 상태에 이상이 생길 수 있다고 합니다.  
  
보통 버튼에는 **:hover**, **:active**, **:focus**, **:target** 등의 pseudo class가 사용됩니다. 각각은 마우스가 올라갔을 때, 클릭됐을 때, 클릭된 후 클릭된 상태를 고정, 클릭된 타겟을 표현하는 기능을 가지고 있습니다. 아래는 그 예시인 **버튼**들과 **target**들을 간단하게 표현한 것입니다.

<style>
.hover:hover{
    color:red
}
.active:active {
  color: red;
}
.focus:focus {
  color: red;
}
.target:target {
  color: red;
}
</style>

<button class='hover'>Hover</button>
<button class='active'>Active</button>
<button class='focus'>Focus</button>
<a href='#target1' id='target1' class='target'>Target 1</a>
<a href='#target2' id='target2' class='target'>Target 2</a>

## CSS code
CSS file에서 알아볼 것은 **\::before** 과 **\::after**라는 pseudo element 입니다. 간단하게 이해한대로만 설명하자면 태그 앞에 붙은 **::before** 나 **::after**는 해당 태그 내부의 가장 안쪽과 가장 끝에 붙어 **content: "string"** 에 있는 string과 style을 표현하는 것을 뜻합니다. 이들은 기본적으로 **inline-element** 성질을 가지고 있으나 이는 수정 가능합니다. [이곳](https://www.youtube.com/watch?v=zGiirUiWslI)에 정말 굉장한 예와 설명이 있으니 꼭 들어보시길 바랍니다.  
  
두 번째로는 **\@media** 쿼리가 있습니다. 아직까지는 반응형 웹을 만들 때 사용되는 것으로 **min-width: N px**와 **max-width: N px** 등으로 웹의 크기에 따른 표현 형식을 지정할 수 있다는 것까지만 알고 있습니다.

끝으로 **\<main\>** 태그가 가운데에 위치할 수 있도록 돕는 것이 바로 **display: grid**와 **place-items: center** 입니다. 해당 그리드는 1개의 셀만을 가지고 있습니다. 한 셀 안에는 9개의 구역이 생길 수 있는데, **place-items: row-place, col-place**를 통해 각각 좌, 중, 우를 결정해 나타낼 수 있습니다.  
  
현재 **center** 한 가지는 **center center** 해석되어 정 중앙에 **container**를 위치시킵니다. [Momentum 프로젝트](https://github.com/wch18735/Momentum) 프로젝트에서 가운데 위치시키는 부분이 어려웠던 게 기억나네요.

## JavaScript code

``` javascript
let count = 0;
let result = document.getElementById("value");

let decrease_btn = document.querySelector(".decrease");
let reset_btn = document.querySelector(".reset");
let increase_btn = document.querySelector(".increase");

decrease_btn.addEventListener("click", ()=>{
    count -= 1;
    result.textContent = count;
});

increase_btn.addEventListener("click", ()=>{
    count += 1;
    result.textContent = count;
});

reset_btn.addEventListener("click", ()=>{
    count = 0;
    result.textContent = count;
});
```

Javascript 코드는 간단합니다. **.decrease**, **.reset**, **.increase** 클래스를 가진 버튼 요소들을 가져와 **addEventListener**를 통해 이벤트를 등록하고, 콜백 함수를 지정해 전역 변수인 count 값을 변화시키는 것. 마지막으로 결과를 **.textContent** 에 삽입하여 수정해 화면에 표시하는 것입니다.



## Reference
- [MDN Web Docs](https://developer.mozilla.org/en-US/)
- [Before and After pseudo elements explained - part one: how they work](https://www.youtube.com/watch?v=zGiirUiWslI)