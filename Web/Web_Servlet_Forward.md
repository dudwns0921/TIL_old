# Web Servlet Forward

웹 애플리케이션을 실행하다 보면 서블릿끼리 또는 서블릿과 JSP를 연동해서 작업해야 하는 경우가 있다.

예를 들어 쇼핑몰의 경우 상품 관리 서블릿과 조회된 상품을 화면에 표시하는 JSP는 각각 따로 존재한다.

따라서 사용자가 상품 조회를 요청하면 상품 관리 서블릿은 데이터베이스에서 상품 정보를 조회한 후

다시 JSP에게 전달하여 상품 정보를 표시한다.



이처럼 하나의 서블릿에서 다른 서블릿이나 JSP와 연동하는 방법을 포워드라고 한다.

포워드 기능이 사용되는 용도는 여러 가지이며 요약하면 다음과 같다.

- 요청에 대한 추가 작업을 다른 서블릿에게 수행하게 한다.
- 요청에 포함된 정보를 다른 서블릿이나 JSP와 공유할 수 있다;.
- 요청에 정보를 포함시켜 다른 서블릿에 전달할 수 있다.
- 모델2 개발 시 서블릿에서 JSP로 데이터를 전달하는 데 사용



포워드 방법에는 다음 네 가지 방법이 있다.

## redirect

- HttpServletResponse 객체의 sendRedirect() 메서드 이용
- 웹 브라우저에 재요청하는 방식
- 형식 : `response.sendRedirect("포워드할 서블릿 또는 JSP")`

## refresh

- HttpServletResponse 객체의 addHeader() 메서드 이용
- 웹 브라우저에 재요청하는 방식
- 형식 : `response.addHeader("Refresh", 경과시간(초);url="요청할 서블릿 또는 JSP")`

## location

- 자바스크립트 location 객체의 href 속성을 이용
- 자바스크립트에 재요청하는 방식
- 형식 : `location.href = "요청할 서블릿 또는 JSP"`

## dispatch

- 일반적으로 포워드 기능을 지칭

- 서블릿이 직접 요청하는 방법

- RequestDispatcher 객체의 forward() 메서드 이용

- 형식 : `RequestDispatcher dis = request.getRequestDispatcher("포워드할 서블릿 또는 JSP")`

  ​		 ` dis.forward(request, response);`



redirect, refresh, location 방법은 서블릿이 웹 브라우저를 거쳐 다른 서블릿이나 JSP에게 요청하는 방식이다.

이와 달리 dispatch 방법은 서블릿에서 웹 브라우저를 거치지 않고 바로 다른 서블릿에게 요청하는 방법이다.

따라서 웹 브라우저 주소창의 URL이 변경되지 않아 클라이언트 측에서는 포워드가 진행되었는지 알 수 없다.



## 데이터 전달

포워드를 사용해 서블릿끼리 혹은 서블릿과 JSP를 연동시키는 이유는 결국 데이터를 주고받기 위해서이다.

데이터를 전달하는 데에는 GET 방식과 바인딩, 총 두 가지 방법이 있다.

둘 다 key값으로 value에 접근하는 방식을 사용한다.

자바의 컬렉션 프레임워크 중 Map을 떠올리면 좀 더 쉽게 이해가 될 것이다.



### GET

GET 방식을 사용할 때는 **쿼리스트링**을 사용한다.

GET 방식으로 요청했을 때 URL 주소 뒤에 데이터를 함께 제공하는 방법으로 `"URL주소?key=value"` 의 형식을 취한다.

전달해야 할 데이터가 여러 개일 때는 &로 쿼리 스트링들을 연결한다.

`"URL주소?key=value&key=value ... "`



### 바인딩

GET 방식은 전달하는 데이터의 양이 적거나 데이터가 보안과 상관없을 때 유용하다.

하지만 대량의 데이터를 전달하거나 사용자 정보와 같이 보안과 직접적으로 관련이 있는 데이터들이라면 바인딩 방식을 활용하는 것이 좋다.



바인딩은 사전적 의미는 "두 개를 하나로 묶는다"는 것이다.

이는 웹 애플리케이션 실행 시 데이터를 서블릿 관련 객체에 저장하는 방법으로,

주로 HttpServletRequest, HttpSession, ServeltContext 객체에서 사용한다.



다음은 서블릿 관련 객체에서 바인딩 관련 기능을 제공하는 메서드들이다.

| 관련 메서드                           | 기능                                           |
| ------------------------------------- | ---------------------------------------------- |
| setAttribute(String name, object obj) | 데이터를 각 객체에 바인딩한다.                 |
| getAttribute(String name)             | 각 객체에 바인딩된 데이터를 name으로 가져온다. |
| removeAttribute(String name)          | 각 객체에 바인딩된 자원을 name으로 제거한다.   |



주의할 점은 HttpServletRequest 객체를 이용하는 바인딩은 웹 브라우저를 재요청하는 포워드 방법으로는 사용할 수 없다.

그 이유는 처음에 웹 브라우저에서 요청할 때 서블릿에 전달되는 request와 이후에 웹 브라우저에 재요청할 때 전달되는 request는 다르기 때문이다.

이처럼 각 객체의 scope, 바인딩된 데이터들에 대한 접근 범위가 다 다르므로 주의해서 사용하자.



### 예시

```java
package login.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import login.service.LoginService;


@WebServlet("/LoginController")
public class LoginController extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doPost(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// get요청이나 post요청이나 다 여기서 처리
		String command = request.getParameter("cmd");
		String url = "/login/result.jsp";
		
		if(command.equals("loginCheck")) {
			String id = request.getParameter("userId");
			String pwd = request.getParameter("userPwd");
			String message1 = null;
			String message2 = null;
			
			boolean flag = new LoginService().loginService(id, pwd);
			
			if(flag) {
				// 유저 정보가 있음
				message1 = "로그인 성공했습니다!";
				message2 = " 돌아오신 걸 환영합니다!";
			} else {
				// 유저 정보가 없음
				message1 = "로그인 실패했습니다.";
				message2 = "을 찾을 수 없습니다. 회원가입 부탁드립니다.";
				
			}
			
			request.setAttribute("message1", message1);
			request.setAttribute("message2", message2);
			request.setAttribute("id", id);
            // 여기서 HttpServletRequest객체의 setAttribute 메서드를 사용해 데이터를 이름과 값으로 바인딩한다.
			
		}
		
		
		RequestDispatcher rd = request.getRequestDispatcher(url);
		rd.forward(request, response);
	}

}
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
	String message1 = (String) request.getAttribute("message1");
	String message2 = (String) request.getAttribute("message2");
	String id = (String) request.getAttribute("id");
	out.print(message1);
    // getAttribute 메서드를 이용해 위에서 설정한 속성 이름으로 바인딩한 값을 가져온다.
    // getAttribute 메서드의 리턴 타입이 object이기 때문에 원하는 데이터 타입에 담으려면 반드시 캐스팅을 해줘야 한다.
%>
<br>
<%
	out.print(id+"님"+message2);
%>
</body>
</html>
```



# :books:참고자료

- https://velog.io/@pear/Query-String-%EC%BF%BC%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A7%81%EC%9D%B4%EB%9E%80
- 이병승, 자바 웹을 다루는 기술, 도서출판 길벗, 2019

