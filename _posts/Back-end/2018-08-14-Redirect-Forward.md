---
layout: post
title:  "Redirect & Forward"
categories: Back-end
author : choising
tags: java jsp servlet
---

# 20180814

## 리다이렉트(redirect)

- 서버가 클라이언트로부터 요청을 받은 후, **클라이언트를 특정 URL로 이동**시키는 요청을 보내는 것.
    ![redirect](https://github.com/Oraindrop/oraindrop.github.io/blob/master/assets/_img/redirect.png?raw=true)
    - 서버는 클라이언트에게 응답 시, 상태코드 *302*와 함께 이동할 URL정보를 Location Header에 담아 전송.
    - 클라이언트는 서버로부터 받은 상태코드값이 *302*일 경우, Location Header 값으로 `재요청` 전송.
    - 이 때, 브라우저의 주소창은 전송받은 `URL로 변경`되게 된다.

- 리다이렉트는 http 프로토콜 준수.
- 서블릿이나 jsp에서 redirect를 위해 `HttpServletResponse` 객체가 가지고 있는 `sendRedirect()` 메소드를 사용한다.
```java
	response.sendRedirect("redirect02.jsp");
```

## 포워드(forward)

- WAS의 서블릿이나 JSP가 요청을 받은 후 그 요청을 처리하다가, **추가적인 처리를 같은 웹 어플리케이션 안에 있는 다른 서블릿이나 JSP에게 위임**한다.
    ![forward](https://github.com/Oraindrop/oraindrop.github.io/blob/master/assets/_img/forward.png?raw=true)
    - 클라이언트가 Servlet1에게 요청
    - Servlet1은 요청을 처리한 후, 그 결과를 `HttpServletRequest`에 저장.
    - Servlet1은 결과가 저장된 `HttpServletRequest`와 응답을 위한 `HttpServletResopnse`를 Servlet2에게 전송(forward)
    - Servlet2는 Servlet1로부터 받은 request, response 객체를 사용하여 요청을 처리한 클라이언트에게 응답.
    
- redirect와의 가장 큰 차이점은 `request가 한 번`이고, `url이 변경되지 않는다`는 점이다.
```java 
    request.setAttribute("key", value); // arg1 type String, arg2 type Object
    RequestDispatcher requestDispatcher = request.getRequestDispatcher("/servlet2");
    requestDispatcher.forward(request, response);
```
- request 객체의 getRequestDispatcher의 argument는 `/`로 시작하여야 한다.
    - Contents root 아래 값을 주면 됨.

## Servlet & JSP 연동

- **Servlet은 프로그램 로직이 수행**되기에 JSP보다 상대적으로 유리. 
- **JSP는 결과를 출력**하기에 Servlet보다 상대적으로 유리.
- 즉, `로직 수행은 Servlet`, `결과 출력은 JSP`에서 하는것이 유리.
- 둘의 장단점을 상호보완하여 Servlet에서 프로그램 로직이 수행되고, 그 결과를 JSP에 `포워딩`하는 방법이 사용.
![forward2](https://github.com/Oraindrop/oraindrop.github.io/blob/master/assets/_img/forward2.png?raw=true)
