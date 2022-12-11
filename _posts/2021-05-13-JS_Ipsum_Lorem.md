---
title: "[Front-end] Lorem Ipsum"
excerpt: "map과 innerHTML을 이용해 화면을 업데이트하는 웹"
date: 2021-05-13
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

# Lorem Ipsum project

이번 프로젝트는 **Dummy Paragraphs** 을 출력해주는 프로젝트입니다. 이번에도 역시 생소한 내용이 많았습니다. 그럼에도 불구하고 배울 점이 아주 많은 프로젝트였습니다.  

![lorem ipsum](/_img/2021-05-13/lorem_ipsum.gif){: .align-center}

### JavaScript code

``` javascript
// lorem text: 9 praragraphs
const text = ['p', 'a', 'r', 'a', 'g', 'r', 'a', 'p', 'h'];

const form = document.querySelector('.lorem-form');
const amount = document.getElementById('amount');
const result = document.querySelector('.lorem-text');

// event submit: default action -> send event to server
form.addEventListener('submit', function(e){
  // prevent default action
  e.preventDefault();

  // got it as string, so we need to parse it to integer
  // string is shown as black color in console
  // number is shown as blue color in console
  const value = parseInt(amount.value);
  const random = Math.floor(Math.random() * text.length);

  // empty value
  // negative value
  // more than 9
  if(isNaN(value) || value < 0 || value > 9){
    result.innerHTML = `<p class="result">${text[random]}</p>`;
  }
  else {
    // slice -> array[start:end] in python -> [start, end)
    let tempText = text.slice(0, value);

    // map + return + join
    tempText = tempText.map(function(item){
      return `<p class="result">${item}</p>`
    }).join("");
    result.innerHTML = `<p class="result">${tempText}</p>`;
  }
});
```

사용된 기능들을 하나하나 살펴보겠습니다.

> getElementById()

이 메소드는 document 안에서 특정 id를 가진 첫 번째 요소를 반환합니다. 이를 통해 **amount**라는 id를 가진 요소를 반환하고, 해당 요소가 가진 **value**에 접근할 수 있도록 합니다.

> event.preventDefault()

이 메소드를 이용해 이벤트가 기본적으로 실행하는 동작을 막을 수 있습니다. **input**의 **submit** 이벤트는 서버로 **form**을 보내는 것을 예방합니다. 주의할 점은 아래와 같습니다.

- 모든 이벤트들이 이처럼 취소 가능하지는 않습니다.
- **preventDefault()** 메소드는 이벤트가 DOM으로 전파되는 것을 막지 않습니다. 만약 이를 막으려면 **stopPropagation()** 을 사용해야 합니다.

> array.slice(start, end)

**array.slice**는 **Python**에서 배열을 슬라이싱 하는 것과 동일하게 동작합니다. **start**는 포함이며 **end**는 포함되지 않기 때문에 **[start, end)** 로 나타낼 수 있습니다. 

> forEach | map | filter | reduce | find

이 포스트에서 가장 중요한 내용이라고 생각됩니다. 이전부터 꼭!! 한 번 정리하고 싶었던 내용이고 JavaScript를 처음 접하는 많은 분들도 비슷한 고민을 하셨을 것 같습니다. 똑같이 배열의 요소들을 순회하며 동작하는 것 같은데 어떤 기능들이 있기에 이렇게 나뉘었을까요.  



<br>


<span style="color:#fddx54; font-weight: bold; font-size: 25px;">■ forEach</span>

먼저 **forEach()** 부터 살펴보겠습니다. `.forEach()`는 `for`와 마찬가지로 배열을 순회할 때 사용됩니다. 이름에서 보시는 것처럼 정확하게 `for`와 동일한 기능을 한다고 생각하시면 편할 것 같습니다. **array**에 대하여 두 구문은 완벽히 같은 동작을 수행합니다.

```javascript
var array = [1, 2, 3];

// for loop
for (var i = 0; i < array.length; i++){
    console.log(i);
}

// forEach
array.forEach(function(i){
    console.log(i);
});
```

이 `forEach()`에 대한 더욱 자세한 설명은 [이곳](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)을 참고해주시면 되겠습니다. 지금은 간단하게 `for`과 동일하게 동작하는 메소드라고 생각해주시면 편할 것 같습니다.



<br>

<span style="color:#fddx54; font-weight: bold; font-size: 25px;">■ map</span>

다음은 **map()** 에 관해 알아보도록 하겠습니다. `.map()`은 각 요소를 **array**로 변환할 때 사용됩니다. 예시를 보겠습니다.

``` javascript
tempText = tempText.map(function(item){
      return `<p class="result">${item}</p>`
    }).join("");
```

[이곳](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)을 확인하시면 `.map()` 메소드의 반환 값에 대한 설명으로 `A new array with each element being the result of the callback function.` 이라 쓰여 있는 것을 확인하실 수 있습니다. 즉, callback function 내부에서 **return**하는 것이 원소가 되는 것임을 알 수 있습니다.

주의할 점으로는 다음과 같은 상황에서는 `map`을 사용하면 안 됩니다.

- `return` 받는 변수를 할당하지 않고 사용하는 상황
- `callback function` 에서 아무것도 `return 하지 않는 상황`

위의 예시에서 `tempText.map(callback function)`을 통해 반환된 배열에 `join`을 사용하여 하나의 **string**을 만들어 다시 한 번 **let** 으로 선언된 가변적인 **tempText**에 할당하는 것을 확인할 수 있습니다.


<br>


<span style="color:#fddx54; font-weight: bold; font-size: 25px;">■ filter</span>

다음은 **filter()** 입니다. 이전 **map()** 이 `callback function`에서 모든 값을 반환해야 했다면, `.filter()`는 조건을 통해 값들을 **걸러내고** 유효한 값들만 `return`하여 생성한 배열을 반환합니다. 아래는 그 예시입니다.

```javascript
var arr = [4, 15, 377, 395, 400, 1024, 3000]
var arr2 = arr.filter((v) => (v % 5 === 0))
```


<br>

<span style="color:#fddx54; font-weight: bold; font-size: 25px;">■ reduce</span>

마지막으로 **reduce()** 에 대해 알아보겠습니다. `.reduce()`는 여러 요소를 가진 배열에서 특정한 하나의 값을 도출할 때 사용됩니다. 이때 **single value**는 array가 될 수도 있고 단 하나의 값이 될 수도 있습니다. `.reduce()`는 지금까지 배운 모든 것들을 대체할 수 있다고 생각하시면 편할 것 같습니다.

**reducer** 함수는 아래 네 가지를 순서대로 인자로 받습니다.

1. Accumulator
2. Current Value
3. Current Index
4. Source Array
extra. initialValue

`.reduce()` 메소드 안에서 반환된 값들은 모두 **Accumulator**에 저장되어 최종적으로 반환됩니다. **initialValue**는 **Accumulator**에 쌓이기 전 초기 값으로 어떤 값을 지정해 줄 수 있도록 합니다. 아래 예시에서 `.reduce()`를 이용해 다양한 메소드를 대체하는 것을 확인할 수 있습니다.

```javascript
let arr = [9, 2, 8, 5, 7]
let sum = arr.reduce((accumulator, current_value) => pre + val)
console.log(sum)

// map
var arr = ['foo', 'hello', 'diamond', 'A']
var arr2 = arr.reduce((accumulator, current_value) => {
    pre.push(value.length)
    return pre
}, [])
console.log(arr2)

// filter
var arr = [4, 15, 377, 395, 400, 1024, 3000]
var arr2 = arr.reduce((accumulator, current_value) => {
    if (value % 5 == 0) {
        pre.push(value);
    }
    return pre;
}, []);
console.log(arr2)

// find
var arr = [4, 15, 377, 395, 400, 1024, 3000]
var arr2 = arr.reduce((accumulator, current_value) => {
    if (typeof pre == 'undefined' && value % 5 == 0) {
        pre = value;
    }
    return pre;
}, undefined);
console.log(arr2)
```

추후 이곳에 사용된 **Arror Function**에 관해서도 꼭 포스팅 할 수 있는 기회가 있으면 좋겠습니다.


<br>


<span style="color:#fddx54; font-weight: bold; font-size: 25px;">■ find</span>

**find()** 메소드는 배열의 여러 요소들 중 조건에 맞는 한 원소 한 가지를 반환합니다. 조건에 부합하는 원소가 여러 개라면 가장 먼저 발견되는 요소를 반환합니다.

함께 많이 사용되는 것으로 아래와 같은 메소드들이 있습니다.

- `findIndex()`: 찾아진 값이 처음으로 등장하는 **INDEX**를 반환합니다.
- `indexOf()`: `findIndex()`와 유사하게 특정 값이 처음으로 등장하는 **INDEX**를 반환합니다.
- `Array.prototype.includes()`: 이는 특정한 값이 배열에 존재하는지 여부를 확인해줍니다. 반환값은 `true` 또는 `false` 입니다.
- `Array.prototype.some()`: 이는 특정 조건을 만족하는 값이 하나라도 있는지 여부를 확인해줍니다. 반환값은 `true` 또는 `false` 입니다.

### Reference
- [JavaSript: Why and when to use forEach, map, filter, reduce, and find in JavaScript.](https://medium.com/@JeffLombardJr/understanding-foreach-map-filter-and-find-in-javascript-f91da93b9f2c)
- [ES6의 map, filter, reduce 정리](https://velog.io/@decody/map-%EC%A0%95%EB%A6%AC)
