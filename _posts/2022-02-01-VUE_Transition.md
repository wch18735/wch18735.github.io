---
title: "[Vue.js] Vue.js Transition"
excerpt: "vue.js transition"
date: 2022-02-01
category:
    - vue.js
tag:
    - vue.js
    - vue transition
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Transition

Transition 을 통해 애니메이션 효과를 삽입할 수 있다. 특정 태그가 나타날 때가 Enter, 사라질 때 Leave 라 말한다. 아래 `v` 자리에 `<transition name=?>` 에 기재한 name 속성을 *CSS Class* 에 정의하면 사용할 수 있다.

예를 들어 `<transition name='tr1>` 태그 내부에 나타나는 중의 효과를 주고 싶다면 연결된 CSS 파일에 `.tr1-enter-active` 클래스를 정의해주기만 하면 자동으로 적용한다.

<img src='/_img/2022-02-01/transition.png' style='margin: auto auto;'/>

`@keyframes` 먹인 CSS 역시 정상적으로 동작한다. 끝으로 트랜지션 각 단계에 사용자가 정의한 메서드를 삽입할 수 있으며 어노테이션을 이용해 간단히 등록할 수 있다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://unpkg.com/vue"></script>

<style>
	/* tr1 에 공통으로 적용할 효과 */
	.tr1-enter-active, .tr1-leave-active{
		transition : all .5s;
	}
	
	/* 나타나기 시작할 때, 사라짐 완료됐을 때 : 안 보이는 때*/
	.tr1-enter, .tr1-leave-to {
		opacity : 0;
	}
	
	/* 나타남이 끝났을 때, 사라짐 시작할 때 : 보이는 때*/
	.tr1-enter-to, .tr1-leave {
		opacity : 1;
	}
	
	/* bounce 이름의 transition 이 실행되는 중간에 
	keyframes 단위로 나뉜 bounce-in 효과 
	단계별 적용 */
	.bounce-enter-active {
		animation: bounce-in .5s;
	}
	
	.bounce-leave-active { 
		animation: bounce-in .5s reverse;
	}
	
	@keyframes bounce-in {
		0% {
			transform : scale(0);
		}
		50% {
			transform : scale(1.5);
		}
		100% {
			transform : scale(1);
		}
	}
	
</style>

<script>
	window.onload = function(){
		var vm1 = new Vue({
			el : '#test1',
			data: {
				show1 : true,
				show2 : true,
				show3 : true,
				status : ''
			},
			methods : {
				setShow1 : function(){
					if(this.show1 == true){
						this.show1 = false
					} else {
						this.show1 = true
					}
				},
				setShow2 : function(){
					if(this.show2 == true){
						this.show2 = false
					} else {
						this.show2 = true
					}
				},
				setShow3 : function(){
					if(this.show3 == true){
						this.show3 = false
					} else {
						this.show3 = true
					}
				},
				beforeEnter : function(){
					this.status = 'before-enter'
				},
				enter : function(){
					this.status = 'entering'
				},
				afterEnter : function(){
					this.status = 'after-enter'
				},
				enterCanceled : function(){
					this.status = 'canceled enter'
				},
				beforeLeave : function(){
					this.status = 'before-leave'
				},
				leave : function(){
					this.status = 'leaving'
				},
				afterLeave : function(){
					this.status = 'after-leave'
				},
				leaveCanceled : function(){
					this.status = 'canceled leave'
				}
			}
		})
	}
</script>
</head>
<body>

	<div id='test1'>
		<transition name='tr1'>
		<h3 v-if='show1'>문자열</h3>
		</transition>
		<button type='button' @click='setShow1'>show1</button>
		<hr/>
		
		<transition name='bounce'>
			<h2 v-if='show2'>
				문자열! 문자열! 문자열! 문자열! 문자열! 문자열!
			</h2>
		</transition>
		<button type='button' @click='setShow2'>버튼</button>
		<hr/>
		
		<transition name='tr1'
			@before-enter='beforeEnter'
			@enter='enter'
			@after-enter='afterEnter'
			@enter-canceled='enterCanceled'
			@before-leave='beforeLeave'
			@leave='leave'
			@after-leave='afterLeave'
			@leave-canceled='leaveCanceled'>
			<h3 v-if='show3'>문자열</h3>
		</transition>
		<button type='button' @click='setShow3'>버튼</button>
		<h3>status : {{status}}</h3>		
		
	</div>

</body>
</html>
```

