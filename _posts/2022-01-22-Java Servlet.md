---
layout: post
title: Java Servlet
subtitle: 자바 서블릿에 대해 알아보자
category: web
---


### Servlet

Servlet은 server, 운영 체제 및 하드웨어를 다루는 대부분의 작업을 수행하는 일종의 환경 내에서 호스팅되는 응용 프로그램을 의미하는 applet, 두 단어의 합성어로써 자바를 이용하여 웹에서 실행되는 프로그램을 작성하는 기술을 말합니다.

### Servlet의 특징

- 클라이언트가 URL을 입력하면 HTTP Request가 Servlet Container로 전송합니다.
- Servlet 또한 자바 프로그램의 다른 클래스들처럼 JVM에서 동작해야 하므로 클래스 파일이 생성되어야 합니다.
- Java Thread를 이용하여 동작합니다.
- 브라우저를 통해 자바 클래스가 실행되도록 하기 위해서는 javax.servlet.http 패키지에서 제공하는 HttpServlet 클래스를 상속받아 구현해야 합니다.
- MVC 패턴에서 Controller로 이용됩니다.

### Servlet의 동작 과정

![ProcessofServlet.png.png](/img/post/ProcessofServlet.png)

1. 클라이언트가 URL요청을 보내게 되면 Web Server는 HTTP Request를 서블릿 컨테이너(Web Container)에게 전달하게 됩니다.
    - Request를 전달받은 서블릿 컨테이너(Web Container)는 HTTP 요청을 처리하기 위한 HttpServletRequest, HttpServletResponse 객체를 생성하고 Depolyment Descritor(Web.xml)기반으로 여떤 URL과 매핑되어있는지 확인합니다.
2. 서블릿 컨테이너(Web Container)에서 실행된적이나 메모리에 생성된 인스턴스가 있는지 체크합니다.
    - 처음 실행이라면 인스턴스를 생성, init 메소드를 호출하여 초기화한후 스레드를 생성합니다.
    - 이미 실행되적이 있다면 기존 인스턴스에 스레드를 생성합니다.
3. 각 스레드의 service 메소드 호출 및 GET , POST 방식에 따라 doGet() 또는 doPost() 호출되며, HttpServletRequest, HttpServletResponse 객체가 인자로 전달 됩니다.
4. 생성된 동적 웹 페이지 결과물은 HttpServletResponse 객체에 담겨 서블릿 컨테이너(Web Container)에서 HTTP 형태로 바뀌어 웹서버로 전송됩니다. 그후 HttpServletRequest, HttpServletResponse 객체의 메모리 소멸 그리고 스레드가 종료됩니다.

### 서블릿 인스턴스는 하나만 생성되어 웹 애플리케이션이 종료될때까지 사용됩니다.

### Servlet LifeCycle

다른 웹 기술들은 클라이언트들의 요청이 있을때마다 작업을 처음부터 새롭게 하여 제공하지만, 서블릿은 그렇지 않습니다.                                                                                                                                                                     서블릿이 첫번째 요청인 경우에는 서블릿 클래스를 찾아 메모리에 로딩하여 인스턴스를 생성합니다. 이때 생성된 서블릿 인스턴스는 메모리에 계속 남아 있게 되므로 이후부터는 서블릿이 호출되어도  서블릿 인스턴스가 다시 생성되지 않고 이미 메모리에 로딩된 서블릿으로부터 서비스만 받기 때문에 수행 속도가 빠릅니다.

![LifeCycleofServlet.png](/img/post/LifeCycleofServlet.png)

1. 클라이언트로부터 요청을 받게 되면 WAS는 해당 서블릿이 메모리에 있는지 확인합니다.
2. 메모리에 없다면 서블릿 클래스를 메모리에 올리고 init 메소드가 호출됩니다. init 메소드에서는 주로 초기화 작업을 합니다.
3. 그후 에는 클라이언트의 요청이 있을때 doGet 메소드 혹은 doPost 메소드가 실행됩니다.
4. 서블릿이 더 이상 서비스를 하지 않을 경우(서블릿 컨테이너 종료)에 destroy 메소드가 호출됩니다.
