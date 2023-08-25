---
layout: post
title: HTTP/HTTPS란
subtitle: HTTP/HTTPS란 무엇이며 어떠한 차이점이 있는지 알아보자
category: knowledge
---

## HTTP

- HTTP는  “HyperText Transfer Protocol의 약자로써 하이퍼텍스트를 교환하기위한 통신규약입니다.” 라고 정의할수 있습니다.
- 80번 포트를 사용합니다.

### HTTP의 특징

- HTTP는 TCP/IP 4계층 중 응용계층에 있으며 기본적으로 Request/Response 구조로 이루어져 있습니다. 클라이언트가 HTTP Request를 보내면 서버가 HTTP Response를 보냅니다.
- HTTP의 요청과 응답은 독립적입니다. 클라이언트에서 요청을 보낸후 응답을 받았다면 그후에 요청을 보낼때는 전에 보낸 요청과 응답에대해서는 알지 못합니다.

### HTTP의 구조

- HTTP의 구조는 Request인지 Response인지에 따라 다르며 아래는 구조 사진입니다.

![Request_Message_Format.png](/img/post/Request_Message_Format.png){: .align-center}
            *Request Message Format*


- Request Line
    - 요청방법(method) : GET, POST, HEAD, PUT, CONNECT, UNLINK,...
    - 요청 URL : 해당 request가 전송되는 URL
    - HTTP Version : HTTP 버전
- Header Line
    - Request의 대한 추가 정보를 가지고 있는 부분입니다.(Request Body의 길이 등)
    - 각 헤더 항목에는 이름 : 값 형식의 구성으로 이루어져있습니다.
- Blank Line : 헤더의 끝을 의미함
- HTTP Body : 해당 Request의 내용  단, 요청 메소드가 POST가 아니면, 항상 비어있는채로 전달

![Response_Message_Format.png](/img/post/Response_Message_Format.png){: .align-center}
            *Response Message Format*

- Status Line(Response Line)
    - HTTP Version : HTTP 버전
    - Status code/Status phrase : 응답상태를 나타내는 코드/ 응답상태를 간략하게 설명해주는 부분                                                                           ( 예시 : 200 OK : 요청 성공, 400 Bad Request : 요청오류)
- Header Line
    - Response와 동일하다
    - Response 에서만 사용되는 헤더값이 존재한다(Server : Apache/2.2.22)
- Blank Line : 헤더의 끝을 의미함
- HTTP Body : Request와 동일함

![HTTP_HEADER.png](/img/post/HTTP_HEADER.png)

### HTTP의 단점

암호화 되지 않는 평문 통신이기 때문에 패킷을 탈취당할수 있으며 변조 가능성이 높습니다.

## HTTPS

위에 언급한 문제를 해결하고자 HTTP의 보안을 강화한 버전으로써 네트워크 상에서 중간에 제3자가 정보를 볼수 없도록 소켓 부분을 SSL(보안소켓계층),TLS(전송보안계층)로 대체함으로써 서버와 브라우저사이에 안전하게 암호화된 연결을 만들수 있게 도와줍니다.

### 암호화 방식

SSL은 두가지 암호화 기법을 혼용하여 사용하고 있습니다.

- 대칭키
    - 동일한키로 암호화와 복호화를 할수있는 방식의 암호화 기법입니다.
    - 대칭키가 유출되면 암호의 내용을 복호화하여 공격이 가능해지기 때문에 대칭키 전달이 어렵습니다.
- 공개키
    - 비밀키와 공개키를 사용하여 암호화하는 방법으로써 정보를 전송하는측이 공개키로 암호화를 한다면 수신쪽이 비밀키로 복호화를 하는 방식입니다.
    - 여기서 안전한점은 복호화에 필요한 비밀키를 통신으로 보낼 필요가 없기 때문에 안전합니다.

### HTTPS 단점

암복호화 과정으로 인하여 하드웨어 리소스(CPU, Memory) 소비가 불가피해지며 SSL을 사용함으로써 네트워크 리소스 소비가 늘어나므로 HTTP 통신보다 속도가 느립니다.
