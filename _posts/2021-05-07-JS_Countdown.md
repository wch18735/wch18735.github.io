---
title: "[Front-end] Countdown"
excerpt: "남은 기한을 1초 단위로 연산해 보여주는 웹"
date: 2021-05-07
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
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Countdown project

이번 프로젝트는 간단한 HTML, CSS, Javascript를 활용해 정해진 기한까지 남은 일, 시, 분, 초를 보여주는 기능을 구현했습니다. 참고한 강의는 [Javascript 15 projects](https://www.youtube.com/watch?v=3PHXvlpOkf4)입니다. 자바스크립트를 다루는 데 초점이 맞춰져있지만 유용한 HTML과 CSS 요소도 한꺼번에 포스팅하여, 나중에 css와 html 기초를 공부할 때 다시 한 번 확인하려고 합니다. 아래는 웹의 동작을 보여줍니다. 첫 번째는 일반적으로 구동하는 모습을, 두 번째는 웹이 만기됐을 때를 보여줍니다.

![normal](/_img/2021-05-07/counter_normal.gif){: .align-center}
  
![expired](/_img/2021-05-07/counter_expired.gif){: .align-center}
  

## HTML code
``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Countdown</title>

    <!-- styles -->
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <section class="section-center">
      <!-- image -->
      <article class="gift-img">
        <img src="./gift.jpeg" alt="gift picture">
      </article>

      <!-- info -->
      <article class="gift-info">
        <h3>old iphone giveaway</h3>
        <h4 class="giveaway">
          giveaway ends on Sunday, 24 April 2020, 8:00am
        </h4>
        <p>
          Lorem ipsum dolor sit, amet consectetur adipisicing elit. Ipsum placeat laborum, tenetur accusamus voluptatem, expedita dolore rem fugiat nobis, voluptatibus maiores iure adipisci porro modi officiis molestiae corrupti consequuntur est!
        </p>
        <div class="deadline">
          <div class="deadline-format">
            <div>
              <h4 class="days">34</h4>
              <span>days</span>
            </div>
          </div>

          <div class="deadline-format">
            <div>
              <h4 class="hours">17</h4>
              <span>hours</span>
            </div>
          </div>

          <div class="deadline-format">
            <div>
              <h4 class="mins">58</h4>
              <span>mins</span>
            </div>
          </div>

          <div class="deadline-format">
            <div>
              <h4 class="secs">13</h4>
              <span>secs</span>
            </div>
          </div>
        </div>
      </article>
    </section>

    <!-- javascript -->
    <script src="app.js"></script>
  </body>
</html>
```

이번 프로젝트를 진행하며 한 가지 알아챈 점은 HTML 문서 안에 **.css**와 **.js**를 추가하는 방법이 다르다는 것입니다. **css** 파일은 link 태그에 href로 경로를 지정하고 그 관계를 rel로서 표현합니다. 그러나 **js** 파일은 script 태그에 src로 경로를 지정하여 사용했습니다.  

``` html
<link href="css_file_path" rel="stylesheet">
<script src="js_file_path">
```

다음은 **article** 태그입니다. 처음 HTML 문서를 작성하시는 분들은 조금 생소할 수 있을 것 같습니다. 저 역시 줄곧 **div** 태그만 사용했기 때문에 조금 어색했습니다. 대표적인 block element(한 줄을 모두 차지하는 요소)인 **div**와 **section**, **article**을 비교하겠습니다.  

### div
**div** 태그는 컨텐츠를 나누는 가장 일반적인 방법입니다. CSS 파일을 통해 레이아웃이나 스타일을 얻기 전까지는 어떤 변형도 가지지 않으며 HTML 문서에서 가장 빈번하게 사용됩니다. 그러나 nav나 article과 같은 태그들과 함께 사용되지 않아야 합니다.

### article
**article** 태그는 스스로를 포함하는 구성을 보일 때 사용됩니다. 이들은 페이지에서 독립적으로 분포할 수 있으며, 같은 형태가 여러 번 재사용 될 수 있습니다. [출처](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article#Example_A)에서 사용시 주의할 점을 확인할 수 있습니다.
- 각 \<article\>은 식별되어야하며 일반적으로 \<h1\> ~ \<h6\> 태그를 자식 요소로 가져야 합니다.
- \<article\>요소가 다른 태그에 의해 감싸질 때, 감싸진 \<article\>은 바깥의 \<article\>과 관련되어야 합니다. 
- \<article\>의 저자 정보는 \<address\> 태그를 통해 전달될 수 있으며 이는 \<article\> 바깥에 위치해야 합니다.
- \<article\>의 시간 정보는 \<time\> 태그의 **datetime** attribute를 통해 나타낼 수 있습니다.

예시는 아래와 같이 나타낼 수 있습니다. 메인이 되는 \<article\> 내부에 두 \<section\>: \<section class="main_review"\>와 \<section class="user_review"\>가 나타납니다. 그리고 유저 리뷰 안에 여러 리뷰들이 다시 \<article\> 태그로 감싸져 나타나게 됩니다.

``` html
<article class="film_review">
  <h2>Jurassic Park</h2>
  <section class="main_review">
    <h3>Review</h3>
    <p>Dinos were great!</p>
  </section>
  <section class="user_reviews">
    <h3>User reviews</h3>
    <article class="user_review">
      <h4>Too scary!</h4>
      <p>Way too scary for me.</p>
      <footer>
        <p>
          Posted on
          <time datetime="2015-05-16 19:00">May 16</time>
          by Lisa.
        </p>
      </footer>
    </article>
    <article class="user_review">
      <h4>Love the dinos!</h4>
      <p>I agree, dinos are my favorite.</p>
      <footer>
        <p>
          Posted on
          <time datetime="2015-05-17 19:00">May 17</time>
          by Tom.
        </p>
      </footer>
    </article>
  </section>
  <footer>
    <p>
      Posted on
      <time datetime="2015-05-15 19:00">May 15</time>
      by Staff.
    </p>
  </footer>
</article>
```

### section
\<section\> 태그는 위의 예에서 알 수 있습니다. \<section\> 태그는 어떠한 부분이 의미적으로 더 이상 다른 특별함을 가질 수 없는 **구역**으로 나눠졌을 때, 이를 구분하기 위해 사용됩니다. \<section\> 태그는 특별한 경우를 제외하고는 headline 태그(h1 ~ h6)를 가집니다. 이러한 \<section\>은 아래와 같은 상황을 고려해야 합니다.

- 컨텐츠가 독립적이고 원자성을 지녀 혼자서 구성될 수 있을 때, 이러한 것들의 묶음을 \<section\>으로 감쌀 수 있습니다.
- 컨텐츠가 주요한 컨텐츠는 아니지만 중심 내용을 보충하거나 연관있을 때는 \<section\> 대신 \<asice\> 태그를 사용합니다.
- 컨텐츠가 중심 내용이라면 \<main\> 태그를 사용합니다.
- 오직 스타일링만을 위해 어떠한 컨텐츠를 감싸야 한다면 이는 \<div\>를 사용합니다. \<section\>은 오직 논리적으로 구분할 수 있는 기준이 존재할 때만 사용합니다.

## JavaScript code

이번 프로젝트에서는 **querySelectorAll()**을 통해 document에 존재하는 모든 element를 가져옵니다. 그리고 **forEach(function(item, index){ ... })** 을 통해 python의 **enumerate**와 비슷한 동작을 수행할 수 있습니다. 그러나 저는 이 방법을 조금 수정해 **if ~ else** 구문으로 처리하여 유연성을 더했습니다.  
  
이번 **Counter project**를 통해 배운 Skill은 다음과 같습니다.
- **new Date()**: 현재 날짜 또는 지정된 날짜를 얻어올 수 있습니다.
- **.getDate() vs .getDay()**: 각각 날짜와 요일을 얻는 함수로 헷갈릴 수 있으니 주의해야합니다. 
- **backtick**을 활용한 인자의 문자열화입니다.
``` javascript
giveaway.textContent = `giveaway ends on ${day}, ${days} ${month} ${year}, ${hours}:${mins}${ampm}`;
```
- **.textContent**를 이용한 내용 수정이 가능한 점입니다.
- **innerHTML**을 이용한 내용 수정이 가능합니다. 내부의 모든 태그들이 대체됩니다.
- **setInterval(function, ms time)** 으로 주기적인 함수 실행이 가능해집니다. 이때 전달되는 객체를 이용해 주기적인 반복을 멈출 수 있습니다.
- **clearInterval(setInterval object)** 으로 주기적으로 반복되는 함수를 멈출 수 있습니다.

``` javascript
const months = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];
const weekdays = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
];

const giveaway = document.querySelector('.giveaway');
const deadline = document.querySelector('.deadline');
const items = document.querySelectorAll('.deadline-format h4')

// If you want to keep this application always working
// let tmpDate = new Date();
// let tmpYear = tmpDate.getFullYear();
// let tmpMonth = tmpDate.getMonth();
// let tmpDay = tmpDate.getDate();
// const futureDate = new Date(tmpYear, tmpMonth, tmpDay + 10, 11, 30, 0);

// months are zero index base
let futureDate = new Date(2021, 4, 10, 13, 30, 0);

const year = futureDate.getFullYear();
const month = months[futureDate.getMonth()];
const day = weekdays[futureDate.getDay()];
const days = futureDate.getDate();
const mins = futureDate.getMinutes();
let hours = futureDate.getHours();
let ampm = 'am'

if(hours == 0){
  hours = 12;
} else if (hours == 12)
{
  ampm = 'pm';
} else if (hours > 12)
{
  hours -= 12;
  ampm = 'pm';
}

giveaway.textContent = `giveaway ends on ${day}, ${days} ${month} ${year}, ${hours}:${mins}${ampm}`;

// future time in ms
const futureTime = futureDate.getTime();
console.log(futureTime);

function formatting(item){
  if(item < 10){
    return (item = `0${item}`);
  } return item;
}

function getRemainingTime(){
  const today = new Date().getTime();
  let total_diff = futureTime - today;

  // 1s = 1000ms
  // 1min = 60s
  // 1hr = 60min
  // 1day = 24hr

  if(total_diff < 0){
    clearInterval(countdown);
    deadline.innerHTML = `<h4 class="expired">sorry, this giveaway has expired</h4>`;
  }

  const oneDay = 24*60*60*1000; // milliseconds in one day 
  const oneHour = 60*60*1000;   // milliseconds in one hour
  const oneMin = 60*1000;       // milliseconds in one minute
  const oneSec = 1000;          // milliseconds in one second

  let days = Math.floor(total_diff / oneDay);
  total_diff %= oneDay;
  let hours = Math.floor(total_diff / oneHour);
  total_diff %= oneHour;
  let mins = Math.floor(total_diff / oneMin);
  total_diff %= oneMin;
  let secs = Math.floor(total_diff / oneSec);

  items.forEach(function(elem){
    if(elem.classList.contains('days')){
      elem.textContent = formatting(days);
    } else if(elem.classList.contains('hours')){
      elem.textContent = formatting(hours);
    } else if(elem.classList.contains('mins')){
      elem.textContent = formatting(mins);
    } else if(elem.classList.contains('secs')){
      elem.textContent = formatting(secs);
    }
  });
};

// to access in getRemainning function, make it global
let countdown = setInterval(getRemainingTime, 1000);
```
