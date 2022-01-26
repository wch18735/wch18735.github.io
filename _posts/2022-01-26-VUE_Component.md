---
title: "[Vue.js] Vue.js 컴포넌트"
excerpt: "vue.js component"
date: 2022-01-26
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

Component.. Vue.js 핵심 요소! 기존에는 HTML 내부에 화면 요소를 개발했다면 Vue.js 에서는 Component 를 따로 개발해 가져다 붙일 수 있다. Like a 레고. 아래는 활용 예제다. 감만 익혀두자.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	Vue.component('component1', {
		template : '<div><h3>첫 번째 h3 태그</h3><h3>컴포넌트 템플릿 안에는 모든 태그들을 감싸는 하나의 태그만 올 수 있다</h3></div>'
	})
	
	Vue.component('component2', {
		template : '<h3>세 번째 h3 태그</h3>'
	})
	
	Vue.component('data_component', {
		template : '<div><h3>데이터를 사용하는 템플릿 내부는 항상 </h3><h3>{{a1}} {{a2}} ({{test_method1()}})</h3></div>',
		data : function(){
			return_obj = {
				a1 : '함수를 호출해 반환하는 객체를',
				a2 : '반환 받아 사용하게 된다'
			}
			
			return return_obj
		},
		methods : {
			test_method1 : function(){
				return '컴포넌트 내부에 정의된 메소드 호출'
			}
		}
	})
	
	window.onload = function(){
		var test1 = new Vue({
			el : '#test1'
		})
		
		var test2 = new Vue({
			el : '#test2'
		})
		
		var test3 = new Vue({
			el : '#test3',
			components : {
				'component3' : {
					template : '<h3> id 가 test3 인 Vue 객체에서만 사용 가능한 컴포넌트</h3>'
				},
				
				'component4' : {
					template : '<h3> 한 Vue 객체 안에 여러 개 컴포넌트 정의 가능 </h3>'
				}
			}
		})
		
		var test4 = new Vue({
			el : '#test4'
		})
		
		var test5 = new Vue({
			el : '#test5',
			components : {
				'component5' : {
					template : '<h3>v-bind:is 사용해</h3>'
				},
				'component6' : {
					template : '<h3>데이터에 담아 컴포넌트를 사용 외부 컴포넌트 사용</h3>'
				}
			},
			data : {
				view1 : 'component5',
				view2 : 'component6',
			}
		})
		
		test6_var = 7
		var test6 = new Vue({
			el : '#test6',
			components :{
				'even_component' : {
					template : '<h3>데이터에 따라 동적인 컴포넌트 삽입 (짝수)</h3>'
				},
				'odd_component' : {
					template : '<h3>데이터에 따라 동적인 컴포넌트 삽입 (홀수)</h3>'
				}
			},
			data : function(){
				if(test6_var % 2 == 0) {
					return {
						returned_view : 'even_component'
					}
				} else {
					return {
						returned_view : 'odd_component'
					}
				}
			}
		})
	}
</script>

</head>
<body>

	<div id='test1'>
		<component1></component1>
	</div>
	<hr/>
	<div id='test2'>
		<component1></component1>
		<component2></component2>
	</div>
	<hr/>
	<div id='test3'>
		<component2></component2>
		<component3></component3>
		<component4></component4>
	</div>
	<hr/>
	<div id='test4'>
		<data_component></data_component>
	</div>
	<hr/>
	<div id='test5'>
		<component5 v-bind:is='view1'></component5>
		<component6 v-bind:is='view2'></component6>
	</div>
	<hr/>
	<div id='test6'>
		<component v-bind:is='returned_view'></component>		
	</div>
</body>
</html>
```