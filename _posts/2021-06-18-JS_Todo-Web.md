---
title: "[Front-end] TODO LIST 보여주는 웹 페이지"
excerpt: "Local storage를 이용한 사용자별 간단한 일정 관리기"
date: 2021-06-18
layout: single
classes: wide
category:
    - javascript
tag:
    - html
    - css
    - javascript
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# TODO LIST를 저장하는 웹 페이지

이번 프로젝트는 로컬 스토리지를 이용하여 사용자가 웹을 새로고침 하여도 이전 정보가 유지될 수 있도록 하는 방법을 알아봅니다.

프로젝트를 진행하며 배운 내용들을 아래에 간략하게 정리하겠습니다. 끝에는 웹이 동작하는 간단한 화면을 첨부하였습니다.

## input.value

먼저 입력부로부터 텍스트를 받아오는 작업입니다. **html** 문서에서 `grocery`를 **id**로 가지는 **input element**에서 다음과 같이 텍스트를 받아올 수 있습니다.

```js
const grocery = document.getElementById("grocery");
const value = grocery.value;
```

**html** 문서의 **input element**는 이때 placeholder를 다음과 같이 지정할 수 있습니다. 보통 입력을 아무것도 하지 않았을 때 나타나는 글자입니다.

```html
<input type="text" id="grocery" placeholder="What you gonna do?" />
```

이러한 placeholder 역시 value 값으로 받아와지기 때문에 입력 값의 존재 유무와 더불어 사용자가 실제로 입력을 했는지 여부를 체크해야 합니다.

## = vs == vs ===

이때, 비교 연산자로 사용되는 `=`이 보통의 비교 연산자와 다른 것을 확인할 수 있습니다. 아래와 같이 표로 정리했습니다.

|**=**|**==**|**===**|
|:---|:---|:---|
|변수를 할당하는 데 사용됩니다|두 변수를 비교하지만 데이터 타입을 무시합니다|비교하는 두 변수의 데이터 타입까지 비교합니다|
|**할당 연산자(assignment operator)** 라 불립니다|**비교 연산자(comparison operator)** 라 불립니다|**비교 연산자(comparison operator)** 라 불립니다|

## function displayAlert(text, action)

이 함수는 빈 값이 입력되었을 때, 아무것도 입력되지 않았을 때 알람을 표시하는 함수입니다. **alert**을 클래스로 가지는 객체에 입력으로 받은 **text**를 입력합니다. 

여기서 `backticks (~ 밑 문자)`과 `${ }`를 이용해 입력으로 받은 action을 원하는 위치에 입력합니다. 끝으로 **setTimeout()** 을 이용해 1초가 지난 뒤 알람을 지워줍니다.

```js
const alert = document.querySelector(".alert");

// display alert
function displayAlert(text, action) {
  alert.textContent = text;
  alert.classList.add(`alert-${action}`);
  // remove alert
  setTimeout(function () {
    alert.textContent = "";
    alert.classList.remove(`alert-${action}`);
  }, 1000);
}
```

## document.createAttribute()

해당 함수는 새로운 attribute node를 반환합니다. **Attribute**의 개념은 아래 그림에서 직관적으로 확인할 수 있습니다.

![attributes](/_img/2021-06-18/html_attribute.jpg){: .align-cetner}

이렇게 만들어진 **attribute**는 **setAttributeNode()** 메소드를 이용해 엘리먼트에 저장될 수 있습니다.

```js
const element = document.createElement("article");
let attr = document.createAttribute("data-id");
attr.value = id;
element.setAttributeNode(attr);
```

비슷한 예로 **document.createElement()** 메소드가 존재합니다. 이는 위에서 보이는 Element를 생성하는 메소드로 **appendChild()** 등의 메소드로 추가될 수 있습니다.

## Local Storage

**HTML Web storage**는 클라이언트(사용자 브라우저)에 데이터를 저장하는 두 가지 방식을 제공합니다.

- `window.localStorage` - expiration date 없이 데이터를 저장합니다. 탭을 닫아도 데이터가 저장됩니다.
- `window.sessionStorage` - 현재 세션을 위해서만 저장합니다. 브라우저 탭이 종료되면 데이터가 사라집니다.

이를 사용하는 세 가지 **Saving**, **Reading**, **Removing** 간단한 예시를 확인해보겠습니다. 먼저 **localStorage**를 선언하여 활용합니다.

```js
myStorage = windows.localStorage;

// saving item
myStorage.setItem('myCat', 'Tom');

// reading item
const cat = myStorage.getItem('myCat');

// remove item
myStorage.removeItem('myCat');

// remove all item
myStorage.clear();
```

여기서 `localStorage`의 중요한 특징 중 하나는 이들을 모두 **String**으로 저장한다는 것입니다. 저장하려는 개체가 Dictionary 형태여도 이를 **String**으로 저장한다는 것입니다. 그렇다면 이를 어떻게 변환할 수 있을까요?

간단한 정수는 String 형태로 바꿀 수 있지만 List 타입이나 Dictionary 타입은 이 방법이 복잡합니다. 다행히 Javascript에서는 이를 수행해주는 모듈이 있습니다. **JSON 모듈**입니다.

- `JSON.stringify()` - Data to String
- `JSON.parse()` - String to Data

List를 저장하는 예시를 확인해보며 그 예시를 먼저 익혀보겠습니다.

```js
// add list by using JSON.stringify()
localStorage['mylist'] = JSON.stringify(['string1', 'string2']);

// load list by using JSON.parse()
var mylist = JSON.parse(localStorage['mylist']);

// add more element to list
mylist.push('newstring');

// add list by using JSON.stringify()
localStorage['mylist'] = JSON.stringify(mylist);
```

딕셔너리 타입도 똑같은 방법으로 저장하고, 로드할 수 있습니다. 현재 프로젝트에 활용된 코드를 확인해보겠습니다. `addToLocalStorage()`, `getLocalStorage()`, `removeFromLocalStorage(id)`, `editLocalStorage(id, value)` 네 가지 입니다.

```js
function addToLocalStorage(id, value) {
  const grocery = { id, value };
  let items = getLocalStorage();
  items.push(grocery);
  localStorage.setItem("list", JSON.stringify(items));
}

function getLocalStorage() {
  return localStorage.getItem("list")
    ? JSON.parse(localStorage.getItem("list"))
    : [];
}

function removeFromLocalStorage(id) {
  let items = getLocalStorage();

  items = items.filter(function (item) {
    if (item.id !== id) {
      return item;
    }
  });

  localStorage.setItem("list", JSON.stringify(items));
}
function editLocalStorage(id, value) {
  let items = getLocalStorage();

  items = items.map(function (item) {
    if (item.id === id) {
      item.value = value;
    }
    return item;
  });
  localStorage.setItem("list", JSON.stringify(items));
}
```

마지막으로 처음 페이지에 접속할 때, 저장되었던 storage 정보들을 모두 가져와 세팅하는 코드입니다. 지금까지 웹 애플리케이션 토이 프로젝트를 진행하며 배웠던 모든 개념이 다 사용되었습니다.

```js
// containing element list
const list = document.querySelector(".grocery-list");

// ****** setup items **********
function setupItems() {
  let items = getLocalStorage();

  if (items.length > 0) {
    items.forEach(function (item) {
      createListItem(item.id, item.value);
    });
    container.classList.add("show-container");
  }
}

function createListItem(id, value) {
  const element = document.createElement("article");
  let attr = document.createAttribute("data-id");
  attr.value = id;
  element.setAttributeNode(attr);
  element.classList.add("grocery-item");
  element.innerHTML = `<p class="title">${value}</p>
            <div class="btn-container">
              <!-- edit btn -->
              <button type="button" class="edit-btn">
                <i class="fas fa-edit"></i>
              </button>
              <!-- delete btn -->
              <button type="button" class="delete-btn">
                <i class="fas fa-trash"></i>
              </button>
            </div>
          `;
  // add event listeners to both buttons;
  const deleteBtn = element.querySelector(".delete-btn");
  deleteBtn.addEventListener("click", deleteItem);
  const editBtn = element.querySelector(".edit-btn");
  editBtn.addEventListener("click", editItem);

  // append child
  list.appendChild(element);
}
```