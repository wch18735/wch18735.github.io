---
title: "[Vue.js] Vue.js 라우팅"
excerpt: "vue.js elementary basic routing concept"
date: 2022-02-01
category:
    - vue.js
tag:
    - vue.js
    - vue routing
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 라우팅

일반적인 웹 어플리케이션은 링크를 클릭하면 해당 화면이나 서비스를 서버에 요청한다. Vue.js 에서는 링크를 클릭하면 화면 전체를 구성하는 대신 웹 페이지 일부분을 변경하는 작업을 수행할 수 있다. 이 때, VueJS 라우팅 기능 구현을 위해 vue router 라이브러리를 사용한다. 

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
	var homeComponent = {
			template : '<h3>Home</h3>'
	}
	
	var page1Component = {
			/* route 객체 안 prams 내부 data1 에 데이터 받음 */
			template : '<h3>Page1 : {{$route.params.data1}} </h3>'
	}
	
	var page2Component = {
			template : '<h3>Page2</h3>'
	}
	
	var router = new VueRouter({
		routes : [
			{
				path : '/',
				component : homeComponent
			},
			{
				/* :data 받아옴 */
				path : '/page1/:data1',
				component : page1Component
			},
			{
				path : '/page2',
				component : page2Component
			}
		]
	})
	
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			router : router
		})
	}
</script>

</head>
<body>

	<div id='test1'>
		<router-link to='/'>Home</router-link>
		<router-link to='/page1/test1'>Page1 - test1</router-link>
		<router-link to='/page1/test2'>Page1 - test2</router-link>
		<router-link to='/page2'>Page2</router-link>
		
		<!-- 링크를 누르면 보여줄 화면이 표시되는 부분 -->
		<router-view></router-view>
	</div>

</body>
</html>
```