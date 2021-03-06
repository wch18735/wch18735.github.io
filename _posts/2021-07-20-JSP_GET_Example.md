---
title: "[JSP] GET 요청 예제"
excerpt: "GET Request by user input"
date: 2021-07-20
layout: single
classes: wide
category:
 - jsp
tag:
 - java
 - get request
 - get
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 사용자 입력을 통한 GET 요청

이전 포스트에서는 `<a href="hello?cnt=2">클릭</a>`을 통해 GET 요청하는 예시를 다뤘습니다. 이번에는 직접 사용자 입력을 스트링 쿼리로 변환하는 방법을 알아보겠습니다. 아래와 같은 HTML 문서를 작성하고, **index.html** 에 원하는 이름 파일을 생성합니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<form action="plus">
			<div>
				<label>"안녕하세요"를 몇 번 듣겠습니까?</label>
			</div>
			<div>
				<input type="text" name="cnt" />
				<input type="submit" value="출력" />
			</div>
		</form>
	</div>
</body>
</html>
```

여기서 **form 태그**는 사용자 입력으로부터 **HTML**을 만들기 위해 사용됩니다. 이는 내부의 **input 태그**로부터 받은 사용자 입력으로 스트링 쿼리를 작성합니다.

![hello ask](/_img/2021-07-20/hello_ask.jpg){: .align-center}

입력 칸에 4를 입력하면 해당 문자열 `4`는 `cnt=4`로 변환되어 `plus?cnt=4`로 합쳐져 서버에 요청됩니다. `<input type="submit" value="출력">` 은 클릭이 될 때, 무조건 서버로 요청을 보내게 됩니다.

이러한 방식으로 사용자가 직접 입력한 문자열 역시 출력할 수 있습니다. 간단하게 이름과 나이를 입력하면 종합된 정보를 출력하는 페이지를 제작해보는 것을 예제로 추천합니다.