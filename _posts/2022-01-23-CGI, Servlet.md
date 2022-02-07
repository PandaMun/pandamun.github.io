---
layout: post
title: CGI, Servlet
subtitle: CGI의 개념과 서블릿과의 관계, 차이점
category: web
---


Servlet을 공부하던중 CGI에 대한 내용이 나와 CGI의 개념과 Servlet의 개념 그리고 차이점을 정리해 보았습니다.

### CGI(Common Gateway Interface)

CGI, 공용 게이트웨이 인터페이스는 웹 서버와 외부 프로그램간의 데이터를 주고 받는 방법에 대한 규격입니다.  (특정 플랫폼에 의존하지 않고 웹서버등으로 외부 프로그램을 호출하는 조합)

이 CGI 표준을 따라서 만들어진 프로그램을 CGI 프로그램 CGI 스크립트라고 합니다.

CGI는 서버측에서 웹 브라우저가 웹서버의 프로그램과 상호작용 할수 있게 해줍니다. 예를 들면 웹페이지가 데이터베이스를 쿼리하거나 사용자가 양식 정보를 서버에 제출하는 경우 CGI 스크립트가 호출됩니다.  

### Servlet

서블릿은 웹 브라우저 또는 HTTP 클라이언트와 HTTP 서버 간의 상호 작용을 용이하게 하는 중간 프로그램 역할을 하는 Java 기반 웹 구성 요소 입니다.

그렇다면 CGI와 Servlet의 차이는 무엇일까?

### 둘의 차이

| 비교 | CGI | Servlet |
| --- | --- | --- |
| 기본 | 프로그램은 기본 OS로 작성됩니다. | 자바 기반 프로그램 |
| 프로세스 생성 | 각 클라이언트 요청은 자체 프로세스를 생성 | 클라이언트 요청 유형에 따라 프로세스가 생성 |
| 실행 환경 | 별도의 프로세스 | JVM |
| 보안 | 서블릿에 비해 취약합니다. | 공격에 저항이 가능합니다. |
| 속도 | 느림 | 빠름 |
| 스크립트 처리 | 즉시 | 실행하기전에 스크립트가 번역되고 컴파일됩니다. |

### 정리

- CGI는 웹서버와 동적 컨텐츠를 만들어내는 프로그램간의 대화방법을 정의한 규격입니다.
- 보통 CGI가 사용될때는 C나 Perl로 프로그램을 만들어 사용하였으나 속도등 여러문제를 개선 시켜 나온 기술이 서블릿입니다.

### Reference

[https://techdifferences.com/difference-between-cgi-and-servlet.html](https://techdifferences.com/difference-between-cgi-and-servlet.html)

[위키백과(CGI)](https://ko.wikipedia.org/wiki/%EA%B3%B5%EC%9A%A9_%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)
