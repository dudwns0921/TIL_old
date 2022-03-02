

# Web ServletContext와 ServletConfig 사용법

## ServletContext

ServletContext 클래스는 서블릿 컨테이너 실행 시 각 웹 애플리케이션마다 한 개의 ServletContext 객체를 생성한다.

생성된 ServletContext 객체는 애플리케이션 전체의 공통 자원이나 정보를 미리 바인딩해서 서블릿들이 공유해서 사용하도록 한다.

서블릿 컨테이너가 종료되면 ServletContext 객체 역시 소멸된다. 



## 자원 바인딩 기능

자원 바인딩 기능을 위해 javax.servlet.GenericServlet 클래스의 메서드와 ServletContext에서 제공하는 2가지 메서드를 사용했다.

| 메서드                                  | 기능                                                |
| --------------------------------------- | --------------------------------------------------- |
| getServletContext()                     | ServletContext 객체를 반환한다.                     |
| setAttibute(String name, Object object) | 해당 name으로 객체를 ServletContext에 바인딩합니다. |
| getAtrribute(String name)               | 주어진 name을 이용해 바인딩된 value를 가져옵니다.   |



```java
package test.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/TestController")
public class TestController extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		ServletContext context = getServletContext();
		
		ArrayList data = new ArrayList();
		data.add("정영준");
		data.add(27);
		
		
		context.setAttribute("data", data);
		
		out.print("<html><body>");
		out.print("data 리스트에 이름과 나이 추가 후 바인딩");
		out.print("</body></html>");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		this.doGet(request, response);
	}

}
```

```java
package test.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/TestController2")
public class TestController2 extends HttpServlet {

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
		ServletContext context = getServletContext();
		
        ArrayList data = (ArrayList) context.getAttribute("data");
		String name = (String) data.get(0);
		int age = (Integer) data.get(1);
		
		out.print("<html><body>");
		out.print("제 이름은 "+name+"이고, 제 나이는 "+age+"살입니다.");
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```



## 매개변수 설정 기능

개인적으로는 자원 바인딩 기능과 큰 차이가 없다고 생각한다.

결국 두 기능의 핵심은 데이터를 key와 value 방식으로 저장한 후에 웹 애플리케이션 내에서 불러오는 것이다.



웹 애플리케이션의 환경 설정 파일인 web.xml에 웹 애플리케이션 내에서 공통으로 필요한 데이터를 설정한다.

이 때 `<context-param>`, `<param-name>`, 그리고`<param-value>`태그를 이용한다.

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>TestModel2</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <context-param>
  	<param-name>공통</param-name>
  	<param-value>공통으로 사용하는 것</param-value>
  </context-param>
</web-app>
```

| 메서드                        | 기능                                             |
| ----------------------------- | ------------------------------------------------ |
| getInitParameter(String name) | name에 해당되는 매개변수의 초기화 값을 반환한다. |

```java
package test.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/TestController")
public class TestController extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		
		ServletContext context = this.getServletContext();
        
        // this는 생략가능
		
		String 공통 = context.getInitParameter("공통");
		
		out.print("<html><body>");
		out.print(공통);
		out.print("</body></html>");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		this.doGet(request, response);
	}

}
```



## ServletConfig

ServletConfig 객체는 각 Servlet 객체에 대해 생성된다.

ServletConfig는 javax.servlet 패키지에 인터페이스로 선언되어 있으며, 이를 GenericServlet 클래스가 실제로 구현하고 있다.



해당 서블릿에서만 접근할 수 있으며 공유는 불가능하다.

ServletConfig는 서블릿과 동일하게 생성되고 서블릿이 소멸되면 같이 소멸된다.



## 매개변수 설정 기능

ServletContext와 마찬가지로 매개변수를 설정하고 ServletConfig 객체에서 가져올 수 있다.

하지만 위에서 말했듯이 해당 서블릿에서만 접근할 수 있으며 공유할 수 없다는 차이점이 있다.

이 때는  `<init-param>`, `<param-name>`, 그리고`<param-value>`태그를 이용한다.

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>TestModel2</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <init-param>
  	<param-name>서블릿만</param-name>
  	<param-value>서블릿에서만 사용하는 것</param-value>
  </init-param>
</web-app>
```

위처럼 직접 web.xml에 설정하는 경우는 거의 없고 이클립스에서 서블릿을 생성할 때 매개변수를 등록할 수 있다. 

```java
package test.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/TestController")
public class TestController extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		
		ServletConfig config = this.getServletContext();
		
		String 서블릿만 = config.getInitParameter("서블릿만");
		
		out.print("<html><body>");
		out.print(서블릿만);
		out.print("</body></html>");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		this.doGet(request, response);
	}

}
```



지금까지 ServletContext와 ServletConfig를 살펴봤다.

사실 둘은 코드와 데이터를 분리해서 유지보수성을 향상시키기 위해 사용한다는 점에서 크게 다르지 않다.



둘의 차이점은 scope, 설정된 데이터의 접근 범위이다.

특정 서블릿에서만 사용하고 싶은 데이터가 있다면 ServletConfig를, 웹 애플리케이션 전체에서 사용하고 싶다면 ServeltContext를 사용하면 된다.