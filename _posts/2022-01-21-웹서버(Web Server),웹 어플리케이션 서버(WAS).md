---
layout: post
title: 웹서버(Web Server), 웹 어플리케이션 서버(WAS)
subtitle: 웹서버(Web Server), 웹 어플리케이션 서버(WAS)의 개념과 차이점에 대해 알아보자
category: web
---


## Web Server

![web_server.png](/img/post/web_server.png)

HTTP를 통해 웹 브라우저에서 요청하는 HTML, Image 등을 전송해주는 서비스 프로그램을 말합니다.

- 하드웨어 측면
    - 웹서버의 소프트웨어와 컴포넌트 파일(데이터)들을 저장하는 컴퓨터
    - 컴포넌트 파일에는 html,image,css stylesheets, javascript file 등이 있습니다.
    - 웹서버는 인터넷에 연결되어 웹에 연결된 기기들이 컴포넌트파일(데이터)를 주고받을수 있게 합니다.
- 소프트웨어 측면
    - 웹사용자가 어떻게 호스트 파일에 접근하는지를 관리합니다.

기본적인 단계에서 브라우저가 HTTP를통해 파일을 요청하고 요청이 올바른 웹서버(하드웨어)에 도달하였다면, HTTP서버(소프트웨어)는 요청한 문서를 HTTP를 이용해 보내줍니다.

- 정적 웹서버 : HTTP서버(소프트웨어)있는 컴퓨터(하드웨어)로 구성되어있습니다.
    - 서버에 미리 저장된 HTML,Image같은 파일이 그대로 전달되는 웹페이지입니다.
- 동적 웹서버 : 정적 웹서버와 추가적인 소프트웨어(WAS,데이터베이스)로 구성되어있습니다.
    - 상황에 따라 서버에 저장되어있는 데이터를 가공한후 전달되는 웹페이지입니다.(들어온 요청에 따라 동적으로 만들어진 컨텐츠)

## WAS(Web Application Server)

![web_application_server.png](/img/post/web_application_server.png)


HTTP를 통해 웹서버가 요청을 받으면 애플리케이션 대한 로직을 수행하여 웹서버로 반환해주는 소프트웨어입니다.

웹서버와 DMBS 사이에서 동작하는 미들웨어로써 컨테이너 기반으로 동작합니다.

### WAS의 역할

- WAS 는 Web Server와 Web Container 로 이루어져있습니다.(WAS = Web Server + Web Container)
- WAS가 가지고있는 Web Server도 정적인 컨텐츠를 처리하는데 큰 성능 차이는 없습니다.

그럼에도 불구하고 WAS와 Web Server를 따로 분리하여 사용하는 이유가 무엇인가?

### Web Sever & WAS 분리 이유


![web_server&was.png](/img/post/web_server&was.png)


1. WAS와 Web Sever의 역할을 나눠 부하를 방지합니다.
    - WAS가 혼자 모든 요청을 처리할수 있지만 WAS의 부담이 커지게 됩니다. 그래서 기능을 분리하여 부하를 줄일수 있도록 Web Server와 WAS를 분리합니다.
2. 물리적으로 분리하여 보안을 강화합니다.
    - 클라이언트가 바로 WAS로 붙으면 뒤에 있는 Database 정보를 쉽게 알수 있게 됩니다.
    - Web Server를 WAS 앞단에 배치하여 데이터 리소스를 안전하게 보호할수 있습니다.
3. Web server에 여러대의 WAS를 연결할수 있습니다.
    - 규모가 큰 서비스에서는 하나의 웹서버에 여러개의 WAS를 두어 처리하는 요청을 배분하여 안정적인 서비스를 운영할수 있게 합니다.

    WAS는 요청이 들어오면 쓰레드가 서블릿을 실행시켜 동작합니다.

    WAS의 동시처리가 될려면 쓰레드가 효율적으로 동작해야하는데  

    요청 마다 쓰레드가 생성된다면 아래와 같은 단점이 발생하게 됩니다.

    - 장점
        - 동시 요청을 처리할수 있다
        - 리소스(CPU, 메모리)가 허용할때 까지 처리 가능
        - 하나의 쓰레드가 지연되어도 나머지 쓰레드는 정상적으로 동작합니다.
    - 단점
        - 쓰레드 생성비용은 비쌈
        - 요청이 올때마다 쓰레드를 생성하여 서블릿을 실행한다면 응답속도가 늦어짐
        - 쓰레드는 컨텍스트 스위칭 비용이 발생
        - 쓰레드 생성에 제한이 없다.
        - 고객의 요청이 너무 많이 오면 CPU, 메모리 임계점을 넘어서 서버가 죽을수도 있다.

    보통 WAS는 쓰레드풀(요청이 올때마다 스레드 생성 및 종료를 하는것이 아닌 사용자가 설정해둔 개수만큼 스레드를 생성해놓은 상태에서 작업합니다.)형태로 작업합니다.

    쓰레드 풀

    - 특징
        - 필요한 쓰레드를 쓰레드 풀에 보관하고 관리한다.
        - 쓰레드 풀에 생성 가능한 쓰레드의 최대치를 관리한다.
    - 사용
        - 쓰레드가 필요하면 이미 생성되어 있는 쓰레드를 쓰레드 풀에서 꺼내서 관리한다.
        - 사용을 종료하면 쓰레드 풀에 해당 쓰레드를 반납한다.
        - 최대 쓰레드가 모두 사용중이여서 쓰레드 풀에 쓰레드가 없다면 기다리는 요청은 거절하거나 특정 숫자 만큼만 대기하도록 설정할수 있다.
    - 장점
        - 쓰레드가 미리 생성되어있기때문에 쓰레드를 생성하고 종료하는 비용이 절약되고 응답 시간이 빠르다.
        - 생성 가능한 쓰레드의 최대치가 있으므로 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리할수 있다.

    ### 실무 팁

    WAS의 튜닝 포인트는 최대 쓰레드 수 입니다.

    너무 높게 설정하면 CPU,메모리 리소스 임계점 초과로 서버가 다운될수 있고

    너무 낮게 설정하면 서버 리소스는 여유롭지만 클라이언트 입장에서는 응답이 지연되 답답함

    적정 쓰레드 숫자는 성능 테스트로 찾음

    멀티 쓰레드 환경이므로 싱글톤 객체(서블릿, 스프링 빈)는 주의해서 사용
