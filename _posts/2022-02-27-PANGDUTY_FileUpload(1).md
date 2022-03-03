---
title: "[Pangtudy-web] File Upload Issue"
excerpt: "file upload implementation"
date: 2022-02-27
category:
    - pangtudy-web
tag:
    - web development
    - spring boot
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 파일 업로드

- POST 명령 때, 파일 업로드랑 게시글 삽입 호출이 동시에 수행되는 것 같아서 한 번에 받아서 처리하는 방법으로 바꿈.
- `@RequestPart` 적용
```java
@RestController
@RequestMapping("/board")
public class FileUploadController {
	
	@PostMapping("/post")
	@ResponseBody
	public Post upload(@RequestPart Post post, 
					   @RequestPart(required = false) MultipartFile file) 
							   throws IllegalStateException, IOException {
		
		// File Handling...
	}
}
```
- 요청 방식을 form-data 의 CONTENT TYPE 을 application/json 으로 설정 (415 Error)
<img src="/_img/2022-02-27/post.png">

## TO-DO

- 다중 파일 업로드 `MultipartFile` → `MultipartFile[]`
- FileDAO, FileService Layer 로 분할
- 데이터베이스 추가할 때, Hash Issue
    - Hash 함수에 따라 같은 이름은 같은 Hash 값 가질 수 있음 -> 파일 Binary 값으로 Hashing
    - `POSTID ↔ File String` 테이블 등 어떻게 구상할지
- `6.14.15 / npm i /npm run serve`
    - ※ Shell 껐다가 키면 default 값이 바뀔 수 있음
- Static 한 파일은 DB 엮여있으면 동적이라고 생각하면 된다
- Like 눌렀을 때, `1) 누가 좋아요를 눌렀는지 확인 하는 기능`, `2) 내가 좋아요 누른 포스트 종합`
    - JPA로 하면 편한데.. 라는 의견
