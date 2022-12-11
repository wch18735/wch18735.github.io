---
title: "[Front-end] Color-flipper"
excerpt: "클릭을 통해 랜덤한 화면 색을 표시하는 웹 프로젝트"
date: 2021-05-06
layout: single
classes: wide
category:
    - frontend-basic
tag:
    - html
    - css
    - javascript
    - Codding Addict
    - freeCodeCamp
toc: true
toc_label: "Unique Title"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Color-flipper project

Backend Roadmap에서 볼 수 있듯 백엔드 개발자가 되기 위해서는 Frontend에 관한 기본적인 지식이 있어야 합니다. 그래서 이번 프로젝트는 간단한 HTML, CSS, Javascript 를 활용해 **CLICK ME** 버튼을 누를 때마다 각각의 R, G, B 값에 임의의 양의 정수를 할당하고 해당 색을 배경에 나타내는 기능을 구현했습니다. 참고한 강의는 Reference에 기재했습니다.
  
![color-flipper](/_img/2021-05-06/color-flipper.gif){: .align-center}
  
참고한 영상에서는 고정된 색을 배치하고 랜덤한 index를 지정했으나 저는 조금 더 변화를 주어 rgb 모든 값에 임의성을 부여해봤습니다. 먼저 html 코드부터 보겠습니다.

### html code
``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Color Flipper || Simple</title>

    <!-- styles -->
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <nav>
      <div class="nav-center">
        <h4>color flipper</h4>
        <ul class="nav-links">
          <li>
            <a href="index.html">simple</a>
          </li>
          <li>
            <a href="hex.html">hex</a>
          </li>
        </ul>
      </div>
    </nav>

    <main>
      <div class="container">
        <h2>background color : <span class="color">
          #f1f5f8
        </span></h2>
        <button class="btn btn-hero" id="btn">click me</button>
      </div>
    </main>

    <!-- javascript -->
    <script src="app.js"></script>
  </body>
</html>
```  

가장 위에 **\<!DOCTYPE html>** 을 명시하여 해당 문서가 html 문서임을 나타냅니다. 이때 언어는 english로 설정되어있습니다. **\<head>\</head>** 에는 현재 html 문서 전체적으로 적용될 설정 값들이 나타납니다. 인코딩은 **UTF-8**을 따르고 아래는 어떤 비율로 표시할 것인지를 명시합니다.  
  
### CSS code
body의 nav 태그는 navigation을 나타내며 현재 웹을 기준으로 서로 다른 페이지가 어떻게 구성되어있는가를 간접적으로 보여줍니다. 그 안은 **h4** 태그와 **ul** 태그로 구성되고 **ul**태그는 다시 두 개의 링크로 나눠집니다. 이들의 배치와 화면에서의 그래픽적 요소는 css 파일에서 처리하게 됩니다.  
  
마찬가지로 아래 main 태그에서도 각각의 요소들만을 정의하고 이들의 배치나 글자색, 테두리와 같은 그래픽적인 요소는 모두 css 파일에서 처리하게 됩니다. 이때, 각각의 요소와 성질을 매칭시키기 위해 사용하는 것이 바로 **div**, **class**, **id**, **data-id** 등과 같은 **selector(셀렉터)**입니다.  

|Selector|Example|Description|
|--------|--------|--------|
|tag|div|select all tag with \<div>|
|.class|.container|select all element with class="container"|
|#id|#btn|select all element with id="btn"|
|\*|\*|select all element|


``` css
/*
=============== 
Fonts
===============
*/
@import url("https://fonts.googleapis.com/css?family=Open+Sans|Roboto:400,700&display=swap");

/*
=============== 
Variables
===============
*/

:root {
  /* dark shades of primary color*/
  --clr-primary-1: hsl(205, 86%, 17%);
  --clr-primary-2: hsl(205, 77%, 27%);
  --clr-primary-3: hsl(205, 72%, 37%);
  --clr-primary-4: hsl(205, 63%, 48%);
  /* primary/main color */
  --clr-primary-5: hsl(205, 78%, 60%);
  /* lighter shades of primary color */
  --clr-primary-6: hsl(205, 89%, 70%);
  --clr-primary-7: hsl(205, 90%, 76%);
  --clr-primary-8: hsl(205, 86%, 81%);
  --clr-primary-9: hsl(205, 90%, 88%);
  --clr-primary-10: hsl(205, 100%, 96%);
  /* darkest grey - used for headings */
  --clr-grey-1: hsl(209, 61%, 16%);
  --clr-grey-2: hsl(211, 39%, 23%);
  --clr-grey-3: hsl(209, 34%, 30%);
  --clr-grey-4: hsl(209, 28%, 39%);
  /* grey used for paragraphs */
  --clr-grey-5: hsl(210, 22%, 49%);
  --clr-grey-6: hsl(209, 23%, 60%);
  --clr-grey-7: hsl(211, 27%, 70%);
  --clr-grey-8: hsl(210, 31%, 80%);
  --clr-grey-9: hsl(212, 33%, 89%);
  --clr-grey-10: hsl(210, 36%, 96%);
  --clr-white: #fff;
  --clr-red-dark: hsl(360, 67%, 44%);
  --clr-red-light: hsl(360, 71%, 66%);
  --clr-green-dark: hsl(125, 67%, 44%);
  --clr-green-light: hsl(125, 71%, 66%);
  --clr-black: #222;
  --ff-primary: "Roboto", sans-serif;
  --ff-secondary: "Open Sans", sans-serif;
  --transition: all 0.3s linear;
  --spacing: 0.1rem;
  --radius: 0.25rem;
  --light-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  --dark-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  --max-width: 1170px;
  --fixed-width: 620px;
}
/*
=============== 
Global Styles
===============
*/

*,
::after,
::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: var(--ff-secondary);
  background: var(--clr-grey-10);
  color: var(--clr-grey-1);
  line-height: 1.5;
  font-size: 0.875rem;
}
ul {
  list-style-type: none;
}
a {
  text-decoration: none;
}
h1,
h2,
h3,
h4 {
  letter-spacing: var(--spacing);
  text-transform: capitalize;
  line-height: 1.25;
  margin-bottom: 0.75rem;
  font-family: var(--ff-primary);
}
h1 {
  font-size: 3rem;
}
h2 {
  font-size: 2rem;
}
h3 {
  font-size: 1.25rem;
}
h4 {
  font-size: 0.875rem;
}
p {
  margin-bottom: 1.25rem;
  color: var(--clr-grey-5);
}
@media screen and (min-width: 800px) {
  h1 {
    font-size: 4rem;
  }
  h2 {
    font-size: 2.5rem;
  }
  h3 {
    font-size: 1.75rem;
  }
  h4 {
    font-size: 1rem;
  }
  body {
    font-size: 1rem;
  }
  h1,
  h2,
  h3,
  h4 {
    line-height: 1;
  }
}
/*  global classes */

/* section */
.section {
  padding: 5rem 0;
}

.section-center {
  width: 90vw;
  margin: 0 auto;
  max-width: 1170px;
}
@media screen and (min-width: 992px) {
  .section-center {
    width: 95vw;
  }
}
main {
  min-height: 100vh;
  display: grid;
  place-items: center;
}

/*
=============== 
Nav
===============
*/
nav {
  background: var(--clr-white);
  height: 3rem;
  display: grid;
  align-items: center;
  box-shadow: var(--dark-shadow);
}
.nav-center {
  width: 90vw;
  max-width: var(--fixed-width);
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.nav-center h4 {
  margin-bottom: 0;
  color: var(--clr-primary-5);
}
.nav-links {
  display: flex;
}
nav a {
  text-transform: capitalize;
  font-weight: 700;
  font-size: 1rem;
  color: var(--clr-primary-1);
  letter-spacing: var(--spacing);
  margin-right: 1rem;
}
nav a:hover {
  color: var(--clr-primary-5);
}
/*
=============== 
Container
===============
*/
main {
  min-height: calc(100vh - 3rem);
  display: grid;
  place-items: center;
}
.container {
  text-align: center;
}
.container h2 {
  background: var(--clr-black);
  color: var(--clr-white);
  padding: 1rem;
  border-radius: var(--radius);
  margin-bottom: 2.5rem;
}
.color {
  color: var(--clr-primary-5);
}
.btn-hero {
  font-family: var(--ff-primary);
  text-transform: uppercase;
  background: transparent;
  color: var(--clr-black);
  letter-spacing: var(--spacing);
  display: inline-block;
  font-weight: 700;
  transition: var(--transition);
  border: 2px solid var(--clr-black);
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border-radius: var(--radius);
  font-size: 1rem;
  padding: 0.75rem 1.25rem;
}
.btn-hero:hover {
  color: var(--clr-white);
  background: var(--clr-black);
}
```
  

css는 다음에 더욱 자세히 다룰 기회가 있으면 좋겠습니다. 어떤 영역이나 마찬가지겠지만 저한테는 특히 css가 어렵게 느껴집니다. :root에서 설정한 변수들 중 하나인 --dark-shadow를 통해 nav 가 한 줄로 차지하는 아래 부분에 음영을 주어 마치 떠있는 것 처럼 나타냈다는 것만 확인하겠습니다. 어떻게 저런 생각을... 버튼에 마우스를 올려놓을 때 발생하는 이벤트로 버튼 크기가 일정하게 증가하며 음영이 짙어지면 버튼이 올라오는 것 처럼 만들 수 있다고 합니다. 대단해요...  
  
### JavaScript code
마지막으로 자바스크립트 코드입니다. HTML이 화면에 어떠한 것들을 넣을 것인지 결정하고 CSS를 통해 이들의 모양, 형태, 배치 등을 결정했다면 실제 구동 중 발생하는 이벤트를 처리하는 것을 Javascript가 담당하게 됩니다.

``` javascript
const colors = ["green", "red", "rgba(133,122,200)", "#f15025"];

const btn = document.getElementById("btn");
const color = document.querySelector(".color");

btn.addEventListener("click", ()=>{
    // get random number [0, 3] 
    const randomNumber = getRaandomNumber();
    
    console.log(getRandomRGB());

    // document.body.style.backgroundColor = colors[randomNumber];
    // color.textContent = colors[randomNumber];
    const rgbColor = getRandomRGB();
    document.body.style.backgroundColor = rgbColor;
    color.textContent = rgbColor;
});

function getRaandomNumber(){
    // return [0, 1)
    return Math.floor(Math.random() * colors.length);
}

function getRandomRGB(){
    const R = Math.floor(Math.random() * 255);
    const G = Math.floor(Math.random() * 255);
    const B = Math.floor(Math.random() * 255);
    return `rgba(${R}, ${G}, ${B})`
}
```

HTML 문서가 web browser에 로드되면 이것이 **document object**가 됩니다. **document object**는 HTML document의 루트 노드이며 JavaScript file에서 이 document를 사용합니다. 오브젝트 트리는 아래 그림과 같이 나타냅니다.  
![document object tree](/_img/2021-05-06/document_object_tree.jpg){: .align-center}  
  
**document.getElementById()** 와 **document.querySelector()**를 이용해 html의 요소를 지정할 수 있습니다. 그리고 각 요소에 CSS에 미리 추가해 둔 class를 추가하여 성질을 변화시킬 수 있으며 직접적으로 **color.textContent**에 요소를 삽입하거나 **document.body.style.backgroundColor**를 통해 html body의 배경색에 직접 관여할 수 있습니다. 그리고 이러한 동작들이 발생하는 이벤트를 **addEventListener(event, callback function)** 으로 등록할 수 있습니다.

## Reference
- [Build 15 JavaScript Projects - Vanilla JavaScript Course (freeCodeCamp)](https://www.youtube.com/watch?v=3PHXvlpOkf4)
- [w3school](https://www.w3schools.com/js/js_htmldom.asp)