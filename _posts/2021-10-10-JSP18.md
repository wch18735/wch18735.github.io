---
title: "[JSP] Redirect"
excerpt: "page transform at the server side"
date: 2021-10-10
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - application
    - session
    - cookie
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 서버에서 페이지 전환해주기 (redirection)

servlet 이 요청을 받은 다음 특정한 HTML을 전달해주지 않으면 웹 브라우저는 백지 상태를 보여줍니다. 이를 막는 방법으로 Servlet에서 전달할 HTML 문서, 출력이 없을 때에는 정해진 HTML 문서를 돌려주는 방식이 **redirection** 입니다.

앞선 예제에서는 계산기의 `+`, `-` 를 누른 뒤 **뒤로 가기**를 눌러야 다시 계산 화면으로 돌아올 수 있었습니다. 그러나 `response.sendRedirect("calc2.html");` 을 추가해주면 다시 뒤로가기를 누르지 않아도 입력만을 전달한 뒤 다시 계산을 이어갈 수 있는 웹을 만들 수 있습니다.

## 예시 코드

```java
@WebServlet("/calc2")
public class Calc2 extends HttpServlet{
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();

		// Session
		HttpSession session = request.getSession();
		
		// Cookie
		Cookie[] cookies = request.getCookies();
		
		String v_ = request.getParameter("v");
		String op = request.getParameter("operator");
		
		int v = 0;
		if(!v_.equals("")) v = Integer.parseInt(v_);
		
		if(op.equals("="))
		{
			int x = 0, y = v, result = 0;
			String operator = "";
			if(cookies != null)
				for(Cookie _tmp: cookies)
					if(_tmp.getName().equals("value"))
						x = Integer.parseInt(_tmp.getValue());
			
			if(cookies != null)
				for(Cookie _tmp: cookies)
					if(_tmp.getName().equals("op"))
						operator = _tmp.getValue();
			
			if(operator.equals("+"))
				result = x + y;
			else
				result = x - y;
			
			response.getWriter().printf("result is %d\n", result);
		}
		else 
		{
			Cookie valueCookie = new Cookie("value", String.valueOf(v));
			Cookie opCookie = new Cookie("op", op);
			valueCookie.setPath("/calc2");
			opCookie.setPath("/calc2");
			response.addCookie(valueCookie);
			response.addCookie(opCookie);
			
            // use redirect
			response.sendRedirect("calc2.html");
		}
	}
}
```