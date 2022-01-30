---
title: "[Vue.js] Vue.js computed"
excerpt: "vue.js computed"
date: 2022-01-30
category:
    - vue.js
tag:
    - vue.js
    - vue component
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# computed

HTML을 랜더링 할 때 필요한 값을 생성하는 함수의 일종으로 Vue 객체 내 methods 속성에서 함수를 정의하는 것 대신 사용한다.

동작하는 모습을 보면 두 개념이 비슷하게 느껴질 수 있다. 그럼 어떤 차이가 있을까?

- methods : 반환하는 값이 계속 같아도 함수가 계속 호출된다.
- computed : 반환하는 값이 변하지 않았다면 메모리에 저장된 이전에 반환한 값을 기억해 두었다가 그 값을 그대로 사용한다.

이를 확인해보려면 methods 와 computed 내부에 console.log() 를 찍어볼 수 있다. computed 는 반환하는 값이 바뀌지 않으면 더 이상 호출되지 않는 것을 볼 수 있다.

즉, 빠르다. 그러나 한 가지 문제가 있다. `Math.random()` 함수를 사용해보자. 사용할 때마다 계속 변경된 값이 나타나야 하는데 같은 값이 3번 나타난다. Vue 객체 내부의 데이터 값이 변경되는 경우에만 다시 호출되고 외부 라이브러리를 호출하는 작업은 단 한 번만 진행된다.

따라서 중간에 바뀌는 값이 아니라면 메모리에 캐싱하는 computed 를 사용해 성능을 향상시켜보자.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			data : {
				a1 : 100,
				a2 : 200
			},
			methods : {
				test_method : function(){
					console.log('test method')
					return this.a1 + this.a2
				},
				setValue : function(){
					this.a1 = 1000
					this.a2 = 2000
				},
				getRandomMethod : function(){
					return Math.random()
				}
			},
			computed : {
				test_computed : function(){
					console.log('test computed')
					return this.a1 + this.a2
				},
				getRandomComputed : function(){
					return Math.random()
				}
			}
		})
	}
</script>
</head>
<body>

	<div id='test1'>
		<h3>a1 : {{a1}}</h3>
		<h3>a2 : {{a2}}</h3>
		<h3>test method : {{test_method()}}</h3>
		<h3>test method : {{test_method()}}</h3>
		<h3>test method : {{test_method()}}</h3>
		
		<h3>test computed : {{test_computed}}</h3>
		<h3>test computed : {{test_computed}}</h3>
		<h3>test computed : {{test_computed}}</h3>
		
		<button type='button' v-on:click='setValue'>값 변경</button>
		
		<h3>get random method : {{getRandomMethod()}}</h3>
		<h3>get random method : {{getRandomMethod()}}</h3>
		<h3>get random method : {{getRandomMethod()}}</h3>
		
		<h3>get random computed : {{getRandomComputed}}</h3>
		<h3>get random computed : {{getRandomComputed}}</h3>
		<h3>get random computed : {{getRandomComputed}}</h3>
	</div>

</body>
</html>
```

## computed 와 watch

watch는 변수 하나를 감시하는 요소이고 computed는 여러 변수 변화를 감지할 수 있다. 따라서 watch로 처리할 코드 다수를 computed 하나로 통합할 수 있다.

watch로는 result 하나를 도출할 때, 여러 코드에 동일한 내용을 작성해야 하는 경우가 있다. computed는 사용하는 모든 변수들을 감지하고 있다가 해당 변수의 변화가 감지되면 이를 새롭게 반환한다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			data : {
				data1 : 'aaa',
				data2 : 'bbb',
				result1 : ''
			},
			watch : {
				data1 : function(){
					this.result1 = 'data1 : ' + this.data1 + ', data2 : ' + this.data2
				},
				data2 : function(){
					this.result1 = 'data1 : ' + this.data1 + ', data2 : ' + this.data2
				}
			},
			computed : {
				result2 : function(){
					return 'data1 : ' + this.data1 + ', data2 : ' + this.data2
				}
			}
		})
	}
</script>
</head>
<body>
	<div id='test1'>
		<h3>data1 : {{data1}}</h3>
		<h3>data2 : {{data2}}</h3>
		<input type='text' v-model='data1'/><br/>
		<input type='text' v-model='data2'/><br/>
		<h3>data1, data2 변수를 watch 로 감시하는 예시 -> {{result1}}</h3>
		<div>computed 로 두 변수를 한 번에 감시하는 예시 -> {{result2}}</div>
	</div>

</body>
</html>
```

## computed 의 set, get

computed 에서는 computed 내부 설정된 변수에 get 을 이용해 값을 입력하는 것이 가능하다. 반대로 computed 에 설정된 변수 값이 변경될 때 set 을 이용해 이를 다른 변수에 입력하는 작업이 가능하다.

- get : computed로 지정된 값을 사용할 때 호출
- set : computed에 값을 설정할 때 호출

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			data : {
				data1 : 'aaa',
				data2 : 'bbb'
			},
			computed : {
				data3 : {
					get : function(){
						return this.data1 + ', ' + this.data2
					},
					set : function(newVal){
						var list = newVal.split(',')
						
						if(list.length >= 2){
							this.data1 = list[0]
							this.data2 = list[1]
						}
					}
				}
			}
		})
	}
</script>
</head>
<body>
	<div id='test1'>
		<input type='text' v-model='data1'/><br/>
		<input type='text' v-model='data2'/><br/>
		<input type='text' v-model='data3'/><br/>
	</div>

</body>
</html>
```