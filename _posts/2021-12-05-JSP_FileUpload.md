---
title: "[JSP] 파일 업로드 예제와 다운로드 (form 태그와 multipart/form-data)"
excerpt: "jsp file upload, download"
date: 2021-12-05
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - file upload
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

# 파일 업로드

이번 포스트에서는 `form 태그`, `multipart/form-data` 내용을 확인하고 `간단한 예제 코드`를 통해 파일 업로드 전체 과정을 배워본다.

## form 태그

form 태그는 입력 양식 전체를 감싸는 태그로 입력된 정보를 한 번에 서버로 전송한다. 

form 태그는 아래와 같은 속성을 가진다.

- **name :** form 을 식별하기 위한 이름 지정
- **action :** 전송된 form 을 수신할 서버 쪽 스크립트 파일 지정
- **accept-charset :** form 전송에 사용할 문자 인코딩 지정
- **target :** action 에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에 열도록 지정
- **method :** form 을 서버에 전송할 HTTP 메소드
- **autocomplete :** 자동 완성 지원
- **enctype :** form 이 서버로 전송될 때, 해당 데이터가 인코딩되는 방법 명시
    - **application/x-www-form-urlencoded :** 모든 문자들을 서버로 보내기 전 인코딩
    - **text/plain :** 공백 문자는 `+ 기호`로 변환하고, 나머지 문자는 모두 인코딩되지 않음
    - **multipart/form-data :** 모든 문자를 인코딩하지 않음

## multipart/form-data

위에서 말한 form 태그가 submit 되면 다음과 같은 과정을 거쳐 생성된 요청이 서버에 전송된다.

- form 태그 내부 submit 이 발생
- 요청을 위한 HTTP Request 생성
    - HTTP Request 는 Body 에 클라이언트가 전송하는 데이터 삽입
    - Body 에 들어가는 데이터 타입을 HTTP Header (Content-type) 에 명시
- 생성된 HTTP Request 서버에 전송

이렇게 전송된 요청에 대해 서버는 Content-type 을 확인하여 해독하게 된다. 이 때, 보통의 경우 Body 내부는 한 가지 방법으로 인코딩 된다. `text/plain`, `text/xml`, `image/jpeg` 등이 그 예시다. 

그러나 파일 전송에는 위 요소들 중 2가지 이상이 복합적으로 사용된다. 그렇기에 이를 구분하기 위해서 multipart/form-data 을 사용해 각각이 서로 다른 인코딩을 가진다는 것을 명시해주는 것이다.

실제로도 웹 브라우저 클라이언트에서 파일 업로드를 구현할 때, 보통 `<form>` 태그를 통해 파일을 등록하고 전송한다. 이때, F12를 눌러 개발자 도구를 살펴보면 웹 브라우저가 보내는 HTTP 메시지의 Content-type 속성이 multipart/form-data 로 설정된 것을 볼 수 있다.

## 파일 업로드 예제

먼저 *html* 이나 *jsp* 파일을 아래와 같이 만든다.

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="/utils/fileUpload" method="post" enctype="multipart/form-data">
        file: <input type="file" name="fileName"> <input type="submit" value="file upload">
    </form>
</body>
</html>
```

이 때, `action="/utils/fileUpload"` 는 `@WebSevlet("/utils/fileUpload")` 어노테이션이 달린 어딘가에 있는 자바 소스를 동작시킨다. 이 자바 파일을 아래와 같이 작성한다.

```java
package com.jsp_example.web;

/* import ...*/

@MultipartConfig(
	// location="/tmp",
	fileSizeThreshold=1024*1024,
	maxFileSize=1024*1024*10,
	maxRequestSize=1024*1024*10*10
)
@WebServlet("/utils/fileUpload")
public class FileUpload extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("Get Request!!");
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("Post Request!!");
		
		// 실제 서블릿이 동작하는 서버 경로 (Not 개발 서버)
		String realPath = req.getServletContext().getRealPath("/upload");
		
		// form 태그 중 name="fileName" 인 요청 파트
		Part filePart = req.getPart("fileName");
		
		// 요청된 파트의 전송된 파일 이름
		String fileName = filePart.getSubmittedFileName();
		
		// 입력 스트림
		InputStream fis = filePart.getInputStream();
		
		// 실제 업로드 될 경로 + 파일명
		String filePath = realPath + File.separator + fileName;
		
		// 파일 출력 스트림 (저장)
		FileOutputStream fos = new FileOutputStream(filePath);
		
		// 1024byte 씩 버퍼에 담아 읽어오는 과정
		// write(buffer, offset, length); 를 통해 읽어온 만큼만 쓰는 방법
		byte[] buf = new byte[1024];
		int size = 0;
		while((size=fis.read(buf)) != -1)
			fos.write(buf, 0, size);
		fos.close();
		fis.close();
		
		PrintWriter out = resp.getWriter();
		out.write("file uploaded to " + realPath +" successfully!");
	}
}
```

이렇게 작성하고 서버를 재시작 한 후 파일을 업로드하면 파일이 정상적으로 업로드되는 것을 확인할 수 있다. `@MultipartConfig` 어노테이션을 이용해 저장 위치와 최대 전송 용량을 지정하는 모습을 볼 수 있다.

코드 내부 주석을 참고하면 어떤 과정을 거쳐 파일을 읽어오고, 정해진 위치로 업로드하는지 자세히 알 수 있다. 궁금한 점이 생길 것 같다. 위 코드는 단일 파일만 업로드하는 예제다. 다중 파일은 어떻게 전송할 수 있을까.

간단하게 생각하면 두 가지 방법이 가능하다고 느낄 것 같다. 서로 다른 `name` 을 지정해 각각 받아오는 방법과 같은 `name` 을 지정해 배열로 받아오는 방법. 본문에서는 두 번째 예제를 살펴본다. `request.getParts()` 를 이용해 배열을 반환받는 예제다.

```java
package com.jsp_example.web;

/* import ... */

@MultipartConfig(
	// location="/tmp",
	fileSizeThreshold=1024*1024,
	maxFileSize=1024*1024*10,
	maxRequestSize=1024*1024*10*10
)
@WebServlet("/utils/fileUpload")
public class FileUpload extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("Get Request!!");
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("Post Request!!");
		
		String realPath = req.getServletContext().getRealPath("/upload");

		// request 객체 내부 getParts() 메소드 사용
		Collection<Part> fileParts = req.getParts();
		
		for(Part filePart : fileParts) {		
			// 요청의 여러 파트(Part)들 중 name 이 filename 으로 설정된 것
			if(filePart.getName().equals("fileName")) continue;
			if(filePart.getSize() == 0) continue;

			String fileName = filePart.getSubmittedFileName();
			InputStream fis = filePart.getInputStream();
			String filePath = realPath + File.separator + fileName;
			FileOutputStream fos = new FileOutputStream(filePath);
			
			byte[] buf = new byte[1024];
			int size = 0;
			while((size=fis.read(buf)) != -1)
				fos.write(buf, 0, size);
			fos.close();
			fis.close();
		}
		
		PrintWriter out = resp.getWriter();
		out.write("file uploaded to " + realPath +" successfully!");
	}
}
```

위에서 얻은 경로와 파일명을 이용해 업로드 수행과 동시에 연결된 데이터베이스에 경로를 저장하고, 추후에 이를 가져와 사용할 수 있게 만들 수 있다. 이 기능까지 구현하면 간단한 게시판을 *JSP* 와 *Java* 코드로 개발해 볼 수 있다.

개발하다보면 다양한 정책들을 세워야 함을 느끼게 된다. 저장할 때, `게시글 ID` 등을 함께 삽입해 중복을 방지하는 방법 등이 예시가 될 수 있다. 이는 개발 경험을 쌓아나가며 하나씩 자기만의 방법을 터득해나가면 된다.

## download

다운로드는 간단하게 `<a download href="filepath"/>` 과 같이 태그 안에 `download` 키워드를 삽입해 다운로드를 활성화 시킬 수 있다. 옛날에는 다운로드 요청에 대해 동작하는 코드를 서버에서 개발했다고 한다.
