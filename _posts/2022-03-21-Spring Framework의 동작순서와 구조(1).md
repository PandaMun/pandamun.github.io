---
layout: post
title: Spring Framework의 동작순서와 구조(1)
subtitle: Spring Framework의 동작순서와 구조(1)
category: web
---

Spring 프레임워크를 사용하여 개발하던중 Spring의 동작순서와 구조에 대하여 명확하게 정리하고 싶어서 작성하였습니다.

## Spring Framework란?

자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크로써 앤터프라이즈급의 애플리케이션을 개발하기 위하여 필요한 기능을 제공하는 프레임워크입니다.

### 특징

경량 컨테이너로 자바 객체를 직접관리하여 객체 생성,소멸같은 라이프 싸이클을 관리하여 필요한 객체를 스프링 컨테이너로부터 가져와 사용할수 있습니다.

세부적인 특징으로는 아래와 같습니다.

- [IoC(Inversion of Control)](https://pandamun.github.io//2022-02-21-IoC(Inversion-of-Control)/) 제어의 반전을 지원한다.
    - 객체나 메소드의 생명주기를 컨테이너가 관리해주는것
- [DI(Dependency Injection)](https://pandamun.github.io//2022-02-21-IoC(Inversion-of-Control)/) 의존성 주입을 지원한다.
- AOP(Aspect Oriented Programming) 관점지향 프로그래밍을 지원한다,.
    - 트랜젝션이나 로깅 보안과 같이 핵심적인 로직과는 관계가 없으나 여러 모듈에서 공통적으로 쓰이는 기능을 분리하여 개발하고 실행시에 조합할수 있다.

## Spring Framework의 구조

스프링 프레임워크는 아래 그림처럼 여러개의 모듈로 이루어져 있습니다. 자신이 필요할것만 사용할수 있도록 설계되어있어 편리하게 개발할수 있게 도와줍니다.

![Structure_of_Spring_Framework.png](/img/post/Structure_of_Spring_Framework.png)

 Referece of docs.spring.io

간단하게 몇가지만 살펴보면 아래와 같습니다.

1. Core Container
    - Beans : Bean을 지원하는 모듈로써 의존성 주입을 관리합니다.
    - Core : 모든 모듈의 핵심이 되는 부분으로써 Core의 인터페이스와 클래스를 상속/구현 받아서 사용합니다.
    - Context : 애플리케이션의 Context를 담당하는 부분으로써 컨텍스트를 만드는데 필요한 기능과 빈 스캐너, 자바 코드 설정 기능, 표현식, 스크립트 언어 지원등 주요한 기능을 담고 있습니다.
    - Expression Language : 스프링 표현 언어 기능을 제공합니다.
2. AOP : 관점 지향 프로그래밍을 지원합니다.
3. Data Access/Integration : JDBC, ORM, Transaction등을 통하여 데이터에 쉽게 접근할수 있는 방법을 제공합니다.
4. Web : MVC 구현 기능을 제공하고, 웹 애플리케이션 이용에 편리한 기능등을 제공합니다.
5. Test : 스프링은 Test 모듈을 제공합니다.

## Spring Framework의 동작원리

![Order_of_Spring_Framework.png](/img/post/Order_of_Spring_Framework.png)

- DispatcherServlet : HTTP 요청을 받아 객체 사이의 흐름을 제어합니다.
- HandlerMapping : 클라이언트 요청을 바탕으로 어떤 Handler(Controller)을 실행할지 결정합니다.
- Model : Controller에서 View로 넘겨줄 객체가 저장되는곳
- ViewResolver : View name을 바탕으로 View 객체를 결정합니다.
- View : 뷰(Front)에 화면표시 처리를 의뢰합니다.
- Controller : 클라이언트 요청에 맞는 처리를 실행합니다.
- 뷰(Front) : 클라이언트에게 화면을 표시합니다.

1. DispatcherServlet이 request(요청)을 브라우저로부터  받습니다.
2. DispatcherServlet은 요청된 URL을 Handler Mapping 객체에 넘기고, 호출 해야할 Controller 메소드 정보를 얻습니다.
3. DispatcherServlet이 HandlerAdapter 객체를 가져오게 됩니다.
4. HandlerAdapter 객체의 메소드를 실행합니다.

    (자세히 설명하면 HandlerMapping은 DispatcherServlet로부터 전달된 URL을 바탕으로 HandlerAdapter 객체를 포함하는 HandlerExecutionChain 객체를 생성하며, 이후 DispatcherServlet이 HandlerExecutionChain 객체로부터 HandlerAdapter 객체를 가져와서 해당 메소드를 실행하게 된다.)

5. Controller 객체는 비즈니스 로직을 처리하고, 그 결과를 바탕으로 뷰(Front)에 전달할 객체를 Model 객체에 저장합니다. 또한 Controller 객체는 DispatcherServlet에게 view name을 리턴합니다.
6. DispatcherServlet은 view name을 View Resolver에게 전달하여 View 객체를 얻습니다.
7. DispatcherServlet은 View 객체에 화면표시를 의뢰합니다.
8. View 객체는 해당하는 뷰(Front)을 호출하며, 뷰(Front)는 화면표시에 필요한 객체를 Model 객체로부터 가져와 화면에 표시합니다.

## Reference

- [https://devinside.tistory.com/74](https://devinside.tistory.com/74)
- [https://starkying.tistory.com/entry/Spring-MVC-동작원리-구성요소](https://starkying.tistory.com/entry/Spring-MVC-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC-%EA%B5%AC%EC%84%B1%EC%9A%94%EC%86%8C)


### 목차
- Spring Framework의 동작순서와 구조(1)
- [Spring Framework의 동작순서와 구조(2)](https://pandamun.github.io//2022-05-03-Spring-Framework%EC%9D%98-%EB%8F%99%EC%9E%91%EC%88%9C%EC%84%9C%EC%99%80-%EA%B5%AC%EC%A1%B0(2)/)
