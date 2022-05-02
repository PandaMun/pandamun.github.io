---
layout: post
title: HttpServletRequest,HttpServletResponse
subtitle: HttpServletRequest,HttpServletResponse 대해 알아보자
category: web
---


## HTTPServletRequest

### HttpServletRequest 역할

HTTP 요청 메시지를 개발자가 직접 파싱해서 사용하기 불편하기 때문에 서블릿이 HTTP 요청 메시지를 편리하게 사용할수 있도록  파싱하여 결과를 ‘HttpServletRequest’ 객체에 담아서 제공합니다.

**HTTP 요청 메시지**

```
  POST /save HTTP/1.1
  Host: localhost:8080
  Content-Type: application/x-www-form-urlencoded

  username=kim&age=20
```

- START LINE
    - HTTP 메소드
    - URL
    - 쿼리 스트링
    - 스키마, 프로토콜
- 헤더
    - 헤더 조회
- 바디
    - form 파라미터 형식 조회
    - message body 데이터 직접 조회

    HttpServletRequest 객체는 추가로 여러가지 부가기능도 함께 제공한다.

    **임시 저장소 기능**

- 해당 HTTP 요청이 시작부터 끝날때까지 유지되는 임시 저장소 기능
    - 저장 : request.setAttribute(name, value)
    - 조회 : request.getAttribute(name)

    **세션 관리 기능**

- request.getSession(create : true)

    > **중요**                                                                                                                             HttpServletRequest, HttpServletResponse를 사용할 때 가장 중요한 점은 이 객체들이 HTTP 요청 메시지, HTTP 응답 메시지를 편리하게 사용하도록 도와주는 객체라는 점이다. 따라서 이 기능에 대해서 깊이있는 이해를 하려면 **HTTP 스펙이 제공하는 요청, 응답 메시지 자체를 이해**해야 한다.
    >

**HTTP 요청 데이터**

- **GET - 쿼리 파라미터**
    - /url**?username=hello&age=20** 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달
    예) 검색, 필터, 페이징등에서 많이 사용하는 방식
    - 서버에서는 HttpServletRequest 가 제공하는 다음 메서드를 통해 쿼리 파라미터를 편리하게 조회할 수
    있다.
    - **쿼리 파라미터 조회 메서드**

        ```java
        String username = request.getParameter("username"); //단일 파라미터 조회
        Enumeration<String> parameterNames = request.getParameterNames(); //  파라미터 이름들 모두 조회
        Map<String, String[]> parameterMap = request.getParameterMap(); //파라미터를 Map 으로 조회
        String[] usernames = request.getParameterValues("username"); //복수 파라미터 조회
        ```


- **POST - HTML Form**
    - content-type: application/x-www-form-urlencoded
    메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20
    예) 회원 가입, 상품 주문, HTML Form 사용

application/x-www-form-urlencoded 형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같다.
따라서 **쿼리 파라미터 조회 메서드를 그대로 사용**하면 된다.
클라이언트(웹 브라우저) 입장에서는 두 방식에 차이가 있지만, 서버 입장에서는 둘의 형식이 동일하므로,
request.getParameter() 로 편리하게 구분없이 조회할 수 있다.

**참고**

> content-type은 HTTP 메시지 바디의 데이터 형식을 지정한다.
**GET URL 쿼리 파라미터 형식**으로 클라이언트에서 서버로 데이터를 전달할 때는 HTTP 메시지 바디를 사용하지 않기 때문에 content-type이 없다.
**POST HTML Form 형식**으로 데이터를 전달하면 HTTP 메시지 바디에 해당 데이터를 포함해서 보내기때문에 바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정해야 한다. 이렇게 폼으로 데이터를
전송하는 형식을 application/x-www-form-urlencoded 라 한다.
>
- **API 메시지 바디 - 단순 텍스트**
    - **HTTP message body**에 데이터를 직접 담아서 요청
        - HTTP API에서 주로 사용, JSON, XML, TEXT
        - 데이터 형식은 주로 JSON 사용
        - POST, PUT, PATCH

    ```java
    ServletInputStream inputStream = request.getInputStream();
    String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
    ```

    [Spring framework] StreamUtils 인터페이스

    1. `static byte[] copyToByteArray(InputStream in)`
        - InputStream을 byte 배열로 반환한다.
    2. `static String copyToString(InputStream in, Charset charset)`
        - InputStream을 문자열로 반환한다.
    3. `static InputStream emptyInput()`
        - 빈 InputStream을 반환한다.

- **API 메시지 바디 - JSON**
    - **JSON 형식 전송**
        - POST : http://localhost:8080/request-body-json
        - content-type: **application/json**
        - messagebody: {"username": "hello", "age": 20}
        - 결과: messageBody = {"username": "hello", "age": 20}
    - **JSON 형식 파싱 추가**
        - JSON 결과를 파싱해서 사용할수 있는 자바 객체로 변환 하라면 Jackson, Gson 같은 JSON 변환 라이브러리를 추가하여 사용해야합니다.

    - JSON 형식으로 파싱할수 있도록 객체를 생성합니다.

    ```java
    @Getter
    @Setter
    public class User{

    	private String username;
    	private int age;
    }
    ```

    - **Jackson 라이브러리  ***
        - Jackson은 Java용 JSON 라이브러리 입니다.
        - JSON → Java Object

        ```java
        message = {"name" : "panda" , "age" : 26}
        // jackson objectmapper 객체 생성
        ObjectMapper objectMapper = new ObjectMapper();
        //JSON -> Java Object
        User user= objectMapper.readValue(message, User.class);
        //객체 출력
        System.out.println("username : " + user.getUsername());
        System.out.println("age : " + user.getAge());

        ```

## HTTPServletResponse

### HttpServletResponse 역할

**HTTP 응답 메시지 생성**

- HTTP 응답코드 지정
- 헤더 생성
- 바디 생성

편의 기능 제공

- Content-Type, 쿠키, Redirect

```java
//[message body]
PrintWriter writer = response.getWriter();
writer.println("ok");
//[Content 편의 메서드]
response.setContentType("text/plain");
response.setCharacterEncoding("utf-8");
//response.setContentLength(2); //(생략시 자동 생성)
//[Cookie 편의 메서드]
Cookie cookie = new Cookie("myCookie", "good");
cookie.setMaxAge(600); //600초
response.addCookie(cookie);
//[redirect 편의 메서드]
response.sendRedirect("/basic/hello-form.html");
```

**HTTP 응답 데이터 - 단순 텍스트, HTML**

- 단순 텍스트 응답
    - response.getWriter().println(”ok);
- HTML 응답
- HTTP-API - MessageBody JSON 응답

**HTTP 응답 데이터 - API JSON**

HTTP 응답으로 JSON을 반환할 때는 content-type을 application/json 로 지정해야 한다.

Jackson 라이브러리가 제공하는 objectMapper.writeValueAsString() 를 사용하면 객체를 JSON 문자로 변경할 수 있다.
