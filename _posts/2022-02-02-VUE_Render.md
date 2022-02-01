---
title: "[Vue.js] Vue.js Render"
excerpt: "vue.js render"
date: 2022-02-02
category:
    - vue.js
tag:
    - vue.js
    - vue mixin
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 랜더 함수

> 이제는 html, css 없이 javascript 만 가지고 웹을 만들 수 있다

이런 말을 많이 들어봤을 것 같다. Render 함수를 사용하면 VueJS를 통해 HTML을 랜더링 할 때 개발자가 더욱 자유롭게 Component 를 만들 수 있도록 지원한다. 완전 필요없는 것은 아니지만 굉장히 편리하게 태그에 적용될 속성들을 관리할 수 있다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>
<script>
	Vue.component('com1', {
		template : '<h1>Hello World</h1>'
	})
	
	Vue.component('com2', {
		template : '<h1><slot></slot></h1>'
	})
	
	Vue.component('com3', {
		/* component 내부 template or render 를 찾음 */
		render : function(createElement){
			/* render 에 등록된 함수를 사용해 랜더링 */
			/* createElement 는 "함수"로 랜더링에 활용된다 */
			/* this.$slots.default[0].text 값으로 태그 내부에 적힌 문자열을 가져올 수 있다 */
			var msg = 'Hello World ' + this.$slots.default[0].text
			var h1_tag = createElement('h1', {
				/* 이곳에 태그에 적용할 속성들을 기재할 수 있다 */
				attrs : {
					id : 'a1'
				},
				style : {
					color : '#0000FF',
					backgroundColor : '#420003'
				}
			}, msg)
			return h1_tag
		}
	})
	
	Vue.component('com4', {
		render : function(createElement){
			var msg = this.$slots.default[0].text
			var tagName = this.tagname
			var idValue = this.id
			var colorValue = this.color
			var bgcolorValue = this.bgcolor
			
			var h1_tag = createElement(tagName, {
				attrs: {
					id : idValue
				},
				style : {
					color : colorValue,
					backgroundColor : bgcolorValue
				}
			}, msg)
			return h1_tag
		},
		/* 태그 내부에서 개별 속성 값을 지정할 때 사용할 것들을 미리 지정 */
		props : {
			tagname : {
				attributes : String,
				required : true
			},
			id: {
				attributes : String,
				required : true
			},
			color : {
				attributes: String
			},
			bgcolor : {
				attributes: String
			}
		}
	})
	
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1'
		})
	}
</script>
</head>
<body>

	<div id='test1'>
		<!-- 컴포넌트 내부 값들은 무시됨 -->
		<com1>Apple</com1>
		<com1>Google</com1>
		<com1>Microsoft</com1>
		
		<hr/>
		<!-- template 에 <slot></slot> 삽입하면 태그 내부 String 인식 -->
		<com2>Apple</com2>
		<com2>Google</com2>
		<com2>Microsoft</com2>
		
		<hr/>
		<!-- render 함수 사용해 랜더링 -->
		<com3>Apple</com3>
		
		<hr/>
		<!-- 태그 내부에서 공통 속성 제외한 컴포넌트 개별 속성 값 지정 -->
        <!-- 문자열일 때 '' 을 "" 로 감싸주는 것 주의 -->
		<com4 :tagname="'h1'" :id="'a1'" :color="'red'" :bgcolor="'blue'" >Gazua</com4>
	</div>

</body>
</html>
```