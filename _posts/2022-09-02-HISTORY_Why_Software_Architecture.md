---
title: "[후기] Why Software Architecture 세미나 후기"
excerpt: "center seminar review"
date: 2022-09-02
category:
    - history
tag:
    - blog post
toc: false
author: 1FeS
---

# Why Software Architecture?

요즘 체중 조절을 위해 Take-Out을 가져다 자리에서 먹고있는데, 심심해서 보기 시작한 것이 있다. 바로 Leader's Lecture 다. Leader's Lecture 는 각 팀 리더분들의 기술적 지식이나 도메인 지식을 공유하는 자리로, 매 주 한 개 세미나가 업데이트되는 것 같다. 오늘은 **왜 우리는 Software Architecture 를 배워야 하는지**를 주제로 한 소프트웨어 연구소 세미나를 들었는데, 너무 기억에 남아 관련해서 글을 조금 써보려고 한다.

## Introduction

발표자님께서는 현재 SW 연구소에서 일하고 계신 37년 경력의 개발자셨다. 1부에서는 인류가 불을 발견한 시점부터 폰 노이만 구조를 거쳐 현대에 이르기까지, 소프트웨어 발전사를 간략하게 소개하셨다. 각 장마다 그림 몇 장이 올라가 있었는데, 글자와 숫자로 빽빽하게 채워져있던 여느 발표들보다 편안하게 듣는 사람을 빨아들이는 이야기들이었던 것 같다. 

잠깐 센터장님께서 말씀해주셨던 **좋은 발표**가 떠올랐다. 좋은 발표란 청중들을 보고 하는 발표, 청중들과 눈을 마주치며 하는 발표라고 하셨던 기억이 오버랩됐다.

## with Experts Case

세미나 2부 시작은 직접 경험했던 하나의 사례로 시작되었다. 자세한 제품을 밝힐 수는 없지만, 우리 회사 제품에 탑재되는 Software 를 개발하면서 느꼈던 점이었다. 특이한 것은 개발팀의 구성원이었다. 외부 개발 인력들과 함께 프로젝트를 진행하던 이전과 달리 우리 회사의 Expert 를 비롯한 고급 인력들만으로 팀을 모았다고 한다. 그리고 그 결과는 매우 놀라웠다고 한다. 좋은 Architecture 를 선택하는 습관이 체화된 사람들과 함께 일하니 같은 기간 안에 더 성능이 우수한 SW를 만들어 낼 수 있었고, 리뷰ㆍ보고ㆍ점검회의가 필요없는 개발 문화가 정착되었다고 했다.

이 사례가 보여주는 시사점은 간단했다. **우수한 개발자는 10,000명을 먹여 살린다**는 것은 편견이 아니라는 것. 다음 장에 첨부되어있던 사내 코딩 테스트에서 작성된 코드들을 분석한 자료가 안타깝지만 이 말을 뒷바침하는 증거처럼 보였다. Expert 들이 제출한 코드는 다른 코드의 라인 수보다 4배 정도 짧았고, 성능은 최대 20배까지 높았다고 한다.

## 보통 개발팀의 리더가 버스 기사라면, 우수한 개발팀의 리더는 기장과 같다

보통 개발팀의 리더는 시작부터 끝까지 매 정거장마다 승객들을 챙기며, 운전대에서 한시도 눈을 떼지 못 하는 버스 기사와 같다. Kick-off, WBS 체크, 산출물 확인, 중간QA, 완료QA, 단테와 통테 등 각각의 마일스톤을 지나기 위해 시간에 허덕이는 모습이 일반적인 개발팀의 리더, PM(Project Manager)의 모습이다. 

그치만 우수한 개발팀의 리더는 마치 기장과 같아 이륙과 착륙에 온 신경을 다하고, 비행은 자융항법장치에 맡긴 채 여유있게 미래를 고민할 수 있다고 했다. 실제로 프로젝트 시작과 끝에 초점을 집중하고, 리딩 보다는 모니터링에 집중해 보다 장기적인 시각으로 프로젝트를 수행할 수 있었다고 얘기해주셨다.

## 그래서 결론은?

그래서 결론은 무엇일까. 우수한 개발자가 되어라? 아니었다. 

Algorithm 풀이 능력은 축구로 따지면 개인기에 비유할 수 있다. 똑같이 11명이 뛰는 경기에서 한국이 브라질을 이기지 못 하는 이유는 많은 이유가 있겠지만, 가장 큰 부분을 차지하는게 바로 개인의 기량이다. 중요한 것은 어떻게 그 차이를 극복하느냐, 이다. 그리고 여기서 세미나의 주제가 나왔다. SW Architecture Design 능력은 이런 개인기의 열세를 극복하게 해준다.

잘 설계된 SW Architecture 는 모듈 간 Dependency 를 줄여, 이상적으로는 완벽에 가까운 분업을 지원한다. 말하자면 좋은 Architecture 는 Team Play 를 도와 Divide & Conquer 를 가능케 하는 기반이 되어준다는 것이다. 

## Perfect Architecture?

발표자님께서 생각하는 완벽한 Architecture 는 Uni-directional 하고 Single Interface 성질을 가진 것이라 하셨다. 물론 그런 시스템은 존재하지 않아 일은 항상 어렵다고 한다. 그러면서, 리더로서 본인의 역할은 Architecture 를 설계했을 때, 가장 어렵고 난이도가 높은 곳에 최적의 인물을 배치하는 것이라 말씀하셨다. 이렇게 완벽한 아키텍처와 리더로서 자신의 역할까지 간단하게 발표에 녹이고 마지막 이야기로 넘어가게 된다.

## 37년 개발 경험으로 얻은 신념과 10계명

이후로도 듣는 이로 하여금 따끔해지는 말씀들을 많이 해주셨다. Information-centric Design 사상인 OOP 에 대해서는 <u>객체지향이 추구하는 사상을 모른채 코드를 작성하니 코드를 뜯어보면 이게 OOP인지 POP인지 구분이 안 가더라</u>, Design Pattern 에 대해서는 <u>어렸을 때 배우는 태극 8장 같이 이런걸 체화시켜 놓으면 태권도를 배우는데 조금 더 도움이 될 뿐이지 그걸로 겨루기는 할 수 없다</u> 라는 조금은 새로운 말들을 들려주셨다.

끝으로는, 본인이 후배들에게 들려주고 싶은 10가지 이야기로 세미나를 마무리 지었다. 사실 이 부분이 글을 쓰게 된 가장 큰 이유가 아닐까 싶다. 전적으로 공감하는 것도 있고, 이건 좀 내 생각과 다르다 싶은 부분도 있었다. 다른 엔지니어들과 만났을 때, 아래 짤막하게 적힌 10개 생각을 다시 한 번 읽어보고 얘기 나눠보고 싶다.

0. 나는 훌륭한 엔지니어인가? 주변 모든 것에 호기심이 없으면 직업을 바꾸어라.
1. Software 요구사항은 항상 변한다. 이게 불만이면 프로그래밍에서 손 떼라.
2. 왜 Software Architecture 를 배워야 할까? 개발 과정에서는 철저한 분업을, 릴리즈 후에는 안정적 유지ㆍ보수를 보장한다.
3. 좋은 Architecture 는 첫 째도, 둘 째도 Dependency 최적화다.
4. 모든 Software 는 지금보다 2배 빨라질 수 있다.
5. Code Review? 고지마섬의 원숭이가 왜 고구마를 씻지 않고 먹는지 알 수 없다. Code Review 대신 Architecture Review 를 해라.
6. Test 불가한, 심지어 Test 계획조차 없는 Software 는 작성조차 하지마라. Test 를 소홀히하는 사람? 무시하고 경멸해도 좋다.
7. Software Domain 은 알고보면 거기서 거기다. Software 개발에 달나라 가는 기술은 없으니 쫄지 말고 전부 배워라.
8. 직접 Coding 해보지 않은 사람과는 논쟁하지 말라. 설명해줘도 모른다.
9. 인물 사진은 인물이 80% 이고, 소프트웨어의 주력은 개발자다. 프로그래머임을 자랑스럽게 여겨라.

