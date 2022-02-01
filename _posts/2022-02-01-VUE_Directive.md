---
title: "[Vue.js] Vue.js 사용자 정의 지시자"
excerpt: "vue.js directive"
date: 2022-02-01
category:
    - vue.js
tag:
    - vue.js
    - vue directive
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 사용자 정의 지시자

`v-` 로 시작하는 지시자는 vue.js 에서 정의해 제공하는 것도 있지만 개발자가 직접 만들어 사용하는 것도 가능하다. 사용자 정의 지시자를 만드는 방법은 Vue 객체 내외부에서 모두 가능하다. 외부에서 만들면 동일한 지시자를 여러 Vue 객체에서 사용할 수 있다.

아래는 `focus` 라는 이름으로 지시자를 등록한 것이다. 해당 지시자를 포함하고 있는 태그가 매개변수 `el`로 전달되어 함수 내부에서 원하는 처리를 수행할 수 있다.

```js
    Vue.directive('focus', {
		inserted : function(el){
			el.focus()
		}
	})
```

지시자 역시 생명 주기를 가진다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	/* Vue 객체 외부 지시자 선언 */
	Vue.directive('focus', {
		inserted : function(el){
			el.focus()
		}
	})
	
	/* Vue 객체 내부 지시자 선언 */
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			data : {
				str : '',
				data1 : 'data1 value'
			},
			methods : {
				setStr : function(){
					this.str = '변경된 문자열'
				}
			},
			directives : {
				focus2 : {
					inserted : function(el){
					el.focus();
					}
				},
				test : {
					// 지시자가 태그에 세팅될 때 딱 한 번 호출
					bind : function(el){
						console.log('bind')
					},
					// 지시자가 세팅된 HTML 태그가 객체로 만들어져 사용 가능한 형태가 되었을 때 호출되는 함수
					inserted : function(el){
						console.log('inserted')
					},
					// 지시자가 세팅된 HTML 태그 내부가 변경되기 전
					update : function(el){
						console.log('update')
					},
					// 지시자가 세팅된 HTML 태그 내부가 변경된 후
					componentUpdated : function(el){
						console.log('componentUpdated')
					},
					// 지시자가 세틴된 HTML 태그에서 지시자가 제거될 때 (페이지 이동)
					unbinded : function(el){
						console.log('unbind')
					}
				},
				test2 : {
					bind : function(el, binding, vnode){
						console.log('el : ' + el)
						console.log('binding.name : ' + binding.name)
						console.log('binding.value : ' + binding.value)
						console.log('binding.arg : ' + binding.arg)
						console.log('binding.modifiers.a1 : ' + binding.modifiers.a1)
						console.log('binding.modifiers.a2 : ' + binding.modifiers.a2)
						console.log('binding.modifiers.a3 : ' + binding.modifiers.a3)
						console.log('vnode.tag : ' + vnode.tag)
					}
				}
			}
		})
	}
</script>
</head>
<body>

	<div id='test1'>
		<input type='text' v-focus/>
		<input type='text' v-focus2/>
		<h3 v-test>{{str}}</h3>
		<button type='button' @click='setStr'>str 변경</button>
		
		<h3 v-test2='data1'>{{str}}</h3>
		<h3 v-test2:arg1.a1.a2='data1'>{{str}}</h3>
	</div>
	
</body>
</html>
```