---
title: "[Spring Boot] Multiple File Uploading"
excerpt: "multiple file uploading"
date: 2022-02-28
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - interceptors
    - transaction
author: 1FeS
comments: true
---

# 다중 파일 업로드

- `@RestController`: 모든 Mapping 에 `@ResponseBody` 적용
- `@ResponseBody`: Return 하는 객체 값을 JSON 형태로 반환
- `@RequestPart`: key, value 로 들어오는 form-data 요청에 대해 key 값에 따라 변수를 매핑하는 어노테이션

```java
@RestController
@RequestMapping("/board")
public class FileUploadController {
	
	@PostMapping("/post")
	public Post upload(@RequestPart Post post, 
					   @RequestPart(required = false) List<MultipartFile> files) 
							   throws IllegalStateException, IOException {
		
		// File Handling
		if(files != null && !files.isEmpty()) {
			for(MultipartFile file : files) {
				System.out.println(file.getOriginalFilename());
				File f = new File("c:\\Temp", file.getOriginalFilename());
				file.transferTo(f);
			}
		}
		
		return post;
	}
}
```

Postman 에서 같은 key 값으로 파일을 여러 개 보내면 List 형태로 전달된다. **Client 에서 전달할 때, 같은 key 값으로 묶어서 보내는지 확인 필요.**

<img src="/_img/2022-02-28/post_multifile.png">