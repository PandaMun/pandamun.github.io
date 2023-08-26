---
layout: post
title: Spring으로 보는 SSE(Server-Sent Events)
subtitle: SSE 통신의 개념과 Spring Framework에서의 사용법
category: spring
---

프로젝트를 진행하면서 실시간 랭킹 데이터를 클라이언트로 보내줘야하는 상황이 생겼습니다.

실시간으로 랭킹이 변하기 때문에 매번 클라이언트에서 api를 호출하여 랭킹 데이터를 확인하는것은 비효율적이라는 생각이 들었고 그렇다면 어떤 방법이 있을까? 라는 생각을하고 찾아보기 시작하였습니다.

1. WebSocket

    첫번째 방법으로 웹소켓을 먼저 떠올렸습니다. 웹소켓은 실시간 양방향 데이터 전송을 가능하게 하는 통신 프로토콜로써 일반적인 HTTP 프로토콜과는 달리 지속적인 연결을 유지하고 서버와 클라이언트 간의 데이터 전송이 빠르게 이루어지기 때문에 실시간 데이터를 받아야 하는 서비스에 어울리지 않을까? 라는 생각을 하였습니다.  

2. SSE

    두번쨰로는 Server-Sent Events(SSE)입니다

    SSE는 서버에서 클라이언트로의 단방향통신을 지원하는 기술로써 보통 실시간 데이터 전송이나 서버에서 발생하는 특정 이벤트에 클라이언트가 즉시 반응해야 하는경우 SSE 방식을 사용하여 데이터를 서버에서 클라이언트로 실시간으로 전송합니다.


제가 구현하려는 로직은 단순히 서버에서 클라이언트로 실시간 랭킹 순위 데이터를 전달하려는 목적이였기 때문에 웹소켓과 같은 양방향 통신이 아닌 SSE와 같은 서버에서 클라이언트로의 단방향 통신이 맞을것 같았으며 구현 또한 웹소켓에 비해 단순한 SSE를 사용하기로 생각하였습니다.

사용하기전 SSE에 대한 지식이 부족하여 공부하여 내용을 아래와 같이 정리하였습니다.

주로 참고한 사이트는 Tecoble 블로그의 Spring Server-Sent-Events 구현하기 게시물입니다.

[Spring에서 Server-Sent-Events 구현하기](https://tecoble.techcourse.co.kr/post/2022-10-11-server-sent-events/)

## SSE의 정의

Server-Sent Evnets(SSE)는 서버에서 클라이언트로 실시간으로 데이터를 전송하는데 사용되는 기술로써 SSE는 클라이언트가 데이터를 주기적으로 요청할 필요없이 서버에서 데이터를 전송할수 있습니다.

HTTP 프로토콜을 기반으로 하며, 실시간 업데이트 및 이벤트 알림에 적합합니다.

## SSE의 특징

1. 서버에서 발생하는 이벤트나 데이터 변경 사항을 클라이언트에게 실시간으로 전달할수 있습니다.
2. SSE는 HTTP 프로토콜을 사용하여 서버에서 클라이언트로의 단방향 통신을 지원합니다.
3. SSE 연결이 끊어졌을때 자동으로 재 연결을 시도하지만 클라이언트가 연결을 끊어도 서버에서 감지하기가 어렵습니다.

## SSE 통신 흐름

![SSE1.png](/img/post/SSE1.png)

1. Client에서 서버의 이벤트를 구독하기 위한 요청을 보냅니다.
    - 이벤트의 미디어 타입은 text/event-stream입니다.
    - Cache-Control : no-cache
        - 캐싱이란 일반적으로 자주 요청되는 리소스를 효율적으로 전달하기 위해 클라이언트나 중간 프록시 서버에 데이터를 임시저장하는것을 의미합니다. 그러나 실시간 이벤트의 경우 캐싱이 필요하지 않으며, 오히려 이벤트 전달에 지연이나 데이터 불일치가 발생할수 있기 때문에 no-cache를 사용하여 이벤트를 캐싱하지 않도록 지시합니다.

    ```jsx
    GET /connect HTTP/1.1
    Accept: text/event-stream
    Cache-Control: no-cache
    ```


2. 서버는 클라이언트의 요청에 대한 응답을 보내게 됩니다.
    - 응답의 미디어 타입은 text/event-stream입니다.
    - Transfer-Encoding는 HTTP 헤더 중 하나로서 클라이언트와 서버간 데이터 전송 방식을 결정하는 역할을 합니다. 이 헤더의 값은 전송하는 데이터의 인코딩 방식을 나타냅니다. 일반적으로 서버가 메시지 본문을 여러개의 조각(Chunk)로 나누어 전송함으로써 전체 데이터의 크기를 미리 알지 못해도 클라이언트는 이 조각들을 받아 조립하여 최종 데이터를 얻게 되는 chunked 방식을 사용합니다.

    ```jsx
    HTTP/1.1 200
    Content-Type: text/event-stream;charset=UTF-8
    Transfer-Encoding: chunked
    ```

3. 서버에서 클라이언트로 이벤트 전달

    - 클리이언트에서 구독을 하고 나면 서버는 해당 클라이언트에게 비동기적으로 데이터를 전송할수 있습니다. 이때 데이터는 UTF-8로 인코딩된 텍스트 데이터만 가능합니다.

## SpringFramework에서 사용해보자

Spring framework 4.2 부터 SSE 통신을 지원하는 SeeEmitter API를 제공하기 때문에 이를 사용해서 SSE 구독 요청에 대한 응답을 할수 있습니다.

아래는 자바 스프링으로 SSE를 구현하는 예시 코드입니다.

```java
import java.io.IOException;  
import lombok.extern.slf4j.Slf4j;  
import org.springframework.http.MediaType;  
import org.springframework.http.ResponseEntity;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RestController;  
import org.springframework.web.servlet.mvc.method.annotation.SseEmitter;  

@RestController  
@RequiredArgsConstructor
@Slf4j  
public class SseController {  

    private final List<SseEmitter> emitters = new CopyOnWriteArrayList<>();

    @GetMapping(value = "/connect", produces = MediaType.TEXT_EVENT_STREAM_VALUE)  
    public ResponseEntity<SseEmitter> connect() {  
        SseEmitter emitter = new SseEmitter();  
        sseEmitters.add(emitter);

        try {  
            emitter.send(SseEmitter.event()  
                    .name("connect")  
                    .data("connected!"));  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
        return ResponseEntity.ok(emitter);  
    }  
}
```

위 코드에서는 기본 생성자를 통해 만료시간이 디폴트로 정해져 있지만 만료 시간을 따로 설정할수 있습니다. 또한 생성된 SseEmitter 객체는 이벤트가 발생했을때 클라이언트로 이벤트를 전송하기위해 사용되므로 서버에서 저장하고 있어야합니다.

```java
SseEmitter emitter = new SseEmitter(60 * 1000L); // 만료시간 설정
sseEmitters.add(emitter); // SseEmitter 객체 저장
```

Emitter를 생성후에는 만료시간까지 데이터를 보내지 않는다면 재연결 요청시 503 Service Unvailable에러가 발생할수 있기 떄문에 처음 SSE를 연결시 더미 이벤트를 전송하는 것이  좋습니다.

이벤트 이름을 설정해주면 클라이언트에서 설정한 이름으로 이벤트를 받을수 있습니다.

```java
 emitter.send(SseEmitter.event()  
                    .name("connect")  
                    .data("connected!"));
```

그리고 클라이언트에서 해당 이벤트를 받을떄에는 아래와 같이 매핑된 URL로 EventSource 객체를 생성하고 이벤트 리스너를 등록해 해당 이름으로 매핑된 이벤트가 발생하였을때 로직이 수행됩니다.

```jsx
const sse = new EventSource("http://localhost:8080/connect"); // EventSource 객체 생성

sse.addEventListener('connect', (e) => { // connect라는 이름을 가진 이벤트를 받는다
	const { data: receivedConnectData } = e;
	console.log('connect event data: ',receivedConnectData);  // "connected
});
```

또한 시간이 만료되었을때는 onTimeout()이나 .작업이 만료됬을떄의 onCompletion()을 사용하여 콜백을 등록하여 Emitter 객체를 삭제할수 있습니다.

```jsx
emitter.onCompletion(() -> {
            log.info("만료");
            this.emitters.remove(emitter);    // 만료되면 리스트에서 삭제
        });
        emitter.onTimeout(() -> {
            log.info("time out");
            emitter.complete();
        });
```

현재 코드는 설명을 위해 따로 저장하지 않았지만 회원과의 매핑, 지금까지의 이벤트 발생 데이터 저장을 위해 EmitterRepository를 구현하여 데이터베이스에 Emitter 관련 데이터를 저장하고 삭제할수도 있다.

### 주의 할점

SseEmitter는 기본적으로 다중 스레드 환경에서 작동하며, 다중 스레드를 사용할때 동시성 문제가 발생하지 않게 하기위하여 CopyOnWriteArrayList나 ConcurrentHashMap등의 Thread-Safe한 자료구조를 사용하는것이 좋습니다.
