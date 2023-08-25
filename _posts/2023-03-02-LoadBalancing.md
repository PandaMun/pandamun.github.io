---
layout: post
title: LoadBalancing이란
subtitle: LoadBalancing란 무엇이며 어떠한 종류가 있는지 알아보자
category: knowledge
---

## 로드 밸런싱 (Load Balancing) 이란

둘 이상의 CPU 또는 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미합니다.

로드 밸런서는 서버에 가해지는 부하(Load) 를 분산(Balancing) 해주는 장치 또는 기술을 통칭합니다. 클라이언트와 서버 풀(Server Pool, 분산 네트워크를 구성하는 서버들의 그룹) 사이에 위치하며 한 대의 서버로 부하가 집중되지 않도록 트래픽을 관리해 각각의 서버가 최적의 퍼포먼스를 보일 수 있도록 합니다.

![https://post-phinf.pstatic.net/MjAxOTEyMTBfMjE3/MDAxNTc1OTU0ODk1ODQ3.-GJxkoK7Apn4l0K5L1OXN4NFGsseRoaNhW2r0KIQJdog.0BchcWEI-WS-uEb3iRRrD0JyO_6eZoIWh7xf4f4J2fMg.JPEG/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98.jpg?type=w1200](https://post-phinf.pstatic.net/MjAxOTEyMTBfMjE3/MDAxNTc1OTU0ODk1ODQ3.-GJxkoK7Apn4l0K5L1OXN4NFGsseRoaNhW2r0KIQJdog.0BchcWEI-WS-uEb3iRRrD0JyO_6eZoIWh7xf4f4J2fMg.JPEG/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98.jpg?type=w1200)

웹사이트에 접속하는 인원이 급격히 늘어나게 되면서 모든 트래픽을 감당하기엔 1대의 서버로는 부족한 상황이 발생하였습니다. 이러한 상황에 대해서 아래의 두 가지 방법을 통해 대응할 수 있습니다.

1) **Scale-up** : 하드웨어의 성능을 올리기

2) **Scale-out** : 여러 대의 서버가 나눠서 일하도록 만들기

그러나 Scale-up의 경우 하드웨어 향상 비용이 비싸고, 따라서 Scale-out 방법을 통해 서버를 여러 대 운영함으로써 무 중단 서비스를 제공하는 환경을 구성합니다.  이 때, 여러 서버에게 **균등하게** 트래픽을 분산 시켜주는 것이 바로 **로드 밸런싱 (Load Balancing)** 입니다.

## 로드 밸런싱 (Load Balancing) **에서 사용하는 주요 기술**

1) Health Check

서버들에 대한 주기적인 Health Check 를 통해 서버들의 장애 여부를 판단할 수 있습니다. 이로 인해 로드밸런서 있는 경우, 서버 몇 대에 이상이 생기더라도 다른 정상 동작 중인 서버로 트래픽을 보내주는 Fail-Over 가 가능하며, TCP/UDP 분석이 가능하기 때문에 방화벽의 역할도 수행할 수 있습니다.

2) NAT(Network Address Translation)

- SNAT (Source Network Address Translation)

    내부에서 외부로 트래픽이 나가는 경우, 내부 사설 IP 주소를 외부의 공인 IP 주소로 변환하는 방식입니다. 대표적인 예로는 집에서 사용하는 공유기가 있습니다.

- DNAT (Destinateion Network Address Translattion)

    외부에서 내부로 트래픽이 들어오는 경우, 외부 공인 IP 주소를 내부의 사설 IP 주소로 변환하는 방식입니다. 대표적으로는 로드밸런서가 있습니다.


3) DSR(Dynamic Source Routing protocol)

로드밸런서 사용 시 서버에서 클라이언트로 되돌아가는 경우 목적지를 클라이언트로 설정한 다음 네트워크 장비(스위치) 나 로드밸렌서를 거치지 않고 바로 클라이언트를 찾아가는 방식입니다. 이 경우, 로드밸런서의 부하를 줄여줄 수 있는 장점이 있습니다.

3) Tunneling

인터넷 상에서 눈에 보이지 않는 통로를 만들어 통신할 수 있게 하는 개념으로 데이터를 캡슐화해서 연결된 상호 간에만 캡슐화된 패킷을 구별해 캡슐화를 해제할 수 있습니다.

## 로드 밸런싱 (Load Balancing) 의 종류와 다양한 알고리즘

![https://blog.kakaocdn.net/dn/04KNd/btrDVMc2KFK/xbIFaZbqDRcc3ATXkDhQpk/img.png](https://blog.kakaocdn.net/dn/04KNd/btrDVMc2KFK/xbIFaZbqDRcc3ATXkDhQpk/img.png)

![https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2021/06/434.png](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2021/06/434.png)

네트워크 통신 시스템은 크게 일곱 가지의 계층 (OSI 7 layers, 개방형 통신을 위한 국제 표준 모델)으로 나뉩니다. 상위 계층에서 사용되는 장비는 하위 계층의 장비가 갖고 있는 기능을 모두 가지고 있으며, 상위 계층으로 갈수록 더욱 정교한 로드밸런싱이 가능합니다.

부하 분산에는 L4 로드밸런서와 L7 로드밸런서가 가장 많이 활용됩니다. L4 로드밸런서부터 포트(Port)정보를 바탕으로 로드를 분산하는 것이 가능하기 때문입니다. 한 대의 서버에 각기 다른 포트 번호를 부여하여 다수의 서버 프로그램을 운영하는 경우라면 최소 L4 로드밸런서 이상을 사용해야만 합니다.

### (1) L4 로드밸런싱

L4 로드밸런서는 L4 계층에서 동작하는 로드밸런서로 네트워크 계층(L3) 나 트랜트포트 계층(L4) 의 정보를 바탕으로 로드를 분산합니다.

IP 주소나 포트번호, MAC 주소, 전송 프로토콜 등에 따라 트래픽을 나누고 분산처리 하는 것이 가능합니다.

- **라운드로빈 방식(Round Robin Method)**
    - 클라이언트로부터 받은 요청을 로드밸런싱 대상 서버에 순서대로 할당받는 방식입니다.
    - 첫 번째 요청은 첫 번째 서버, 두 번째 요청은 두 번째 서버, 세 번째 요청은 세 번째 서버에 할당합니다.
    - 로드밸러닝 대상 서버의 성능이 동일하고 처리 시간이 짧은 애플리케이션의 경우, 균등하게 분산이 이루어지기 때문에 이 방식을 사용합니다.
- **가중 라운드로빈 방식(Weighted Round Robin Method)**
    - 각각의 서버마다 가중치를 매기고 가중치가 높은 서버에 클라이언트 요청을 우선적으로 배분합니다.
    - 주로 서버의 트래픽 처리 능력이 상이한 경우 사용되는 부하 분산 방식입니다.
    - 예를 들어 A라는 서버가 5 라는 가중치를 갖고 B라는 서버가 2 라는 가중치를 갖는다면, 로드밸런서는 라운드로빈 방식으로 A 서버에 5개 B 서버에 2개의 요청을 전달합니다.
- **IP 해시 방식(IP Hash Method)**
    - 클라이언트의 IP 주소 를 특정 서버로 매핑하여 요청을 처리하는 방식입니다.
    - 사용자의 IP를 해싱(Hashing, 임의의 길이를 지닌 데이터를 고정된 길이의 데이터로 매핑)해 로드를 분배하기 때문에 사용자가 항상 동일한 서버로 연결되는 것을 보장합니다.
- **최소 연결 방식(Least Connection Method)**
    - 요청이 들어온 시점에 가장 적은 연결상태를 보이는 서버에 우선적으로 트래픽을 배분합니다.
    - 동적인 분산 알고리즘으로 각 서버에 대한 현재 연결 수를 동적으로 카운트할 수 있고, 동적으로 변하는 요청에 대한 부하를 분산시킬 수 있습니다.
    - 자주 세션이 길어지거나, 서버에 분배된 트래픽들이 일정하지 않은 경우에 적합한 방식입니다.
- **최소 리스폰타임 방식(Least Response Time Method)**
    - 서버의 현재 연결 상태와 응답시간(Response Time, 서버에 요청을 보내고 최초 응답을 받을 때까지 소요되는 시간)을 모두 고려하여 트래픽을 배분합니다.
    - 가장 적은 연결 상태와 가장 짧은 응답시간을 보이는 서버에 우선적으로 로드를 배분하는 방식입니다.

⚡ **대규모 시스템에서 사용되는 알고리즘은 무엇일까요?**

단지 몇 개의 노드만 있다면 라운드 로빈 DNS 와 같은 방식이 가장 합리적이다. 로드 밸런서 자체의 비용도 높고 불필요한 복잡함을 증가시킬 수 있기 때문입니다. DNS 에서는 하나의 도메인 이름을 라운드 로빈 방식으로 여러 개의 IP 주소를 변환한다면 이것만으로 쉽게 부하 분산이 가능합니다.

하지만 라운드 로빈 방식 채택에는 3 가지 단점이 존재합니다.

1) 서버의 수 만큼 공인 IP 주소가 필요

부하 분산을 위해 서버의 대수를 늘리기 위해서는 그 만큼의 공인 IP 가 필요합니다.

2) 균등하게 분산되지 않음

대부분의 클라이언트에서는 DNS 서버의 부하를 줄이고 성능을 향상시키기 위해 일정 시간 동안 캐싱하기 때문에 부하 분산이 균등하게 발생하지 않습니다.

3) 서버 장애 감지되지 않음

웹 서버의 부하가 높아서 응답이 느려지거나 접속수가 꽉 차서 접속을 처리할 수 없는 상황인 지를 전혀 감지할 수가 없기 때문에 특정 서버에 장애가 발생하더라도 장애 여부가 감지되지 않아 서비스에서 해당 서버를 제거할 수 업습니다.

그렇기 때문에 대규모 시스템에는 **가중치 라운드로빈 알고리즘과 최소 연결방식**이 사용되고 있습니다. 이러한 알고리즘들은 네트워크 트래픽과 분산 요청을 제어하여서 자동 절체나 이상 노드 제거와 같은 신뢰성 관련한 기능을 제공합니다.

![https://post-phinf.pstatic.net/MjAxOTEyMTBfNCAg/MDAxNTc1OTU1MzY3OTM2.nG91HOEOh6Sc1AuUgbN3O4pcnEI-rh24UKSrrrjkrcsg.VcG18MidW4az7Oh0RQfRPLDBHNRyGayE1BsQxDImL3Ig.JPEG/L4-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200](https://post-phinf.pstatic.net/MjAxOTEyMTBfNCAg/MDAxNTc1OTU1MzY3OTM2.nG91HOEOh6Sc1AuUgbN3O4pcnEI-rh24UKSrrrjkrcsg.VcG18MidW4az7Oh0RQfRPLDBHNRyGayE1BsQxDImL3Ig.JPEG/L4-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200)

### (2) L7 로드밸런싱

L7 로드밸런서는 L4 로드밸런서의 기능을 포함할 뿐만 아니라 애플리케이션 계층(HTTP, FTP, SMTP)에서 로드를 분산하기 때문에 HTTP 헤더, 쿠키 등과 같은 사용자의 요청을 기준으로 특정 서버에 트래픽을 분산하는 것이 가능합니다.

쉽게 말해 패킷의 내용을 확인하고 그 내용에 따라 로드를 특정 서버에 분배하는 것이 가능한 것입니다. 위 그림과 같이 URL에 따라 부하를 분산시키거나, HTTP 헤더의 쿠키값에 따라 부하를 분산하는 등 클라이언트의 요청을 보다 세분화해 서버에 전달할 수 있습니다. 

또한 L7 로드밸런서의 경우 특정한 패턴을 지닌 바이러스를 감지해 네트워크를 보호할 수 있으며, DoS/DDoS와 같은 비정상적인 트래픽을 필터링할 수 있어 네트워크 보안 분야에서도 활용되고 있습니다.

- **URL 스위칭 방식 (URL Switching), 컨텍스트 스위칭 방식 (Context Switching)**
    - 특정 하위 URL 들을 특정 서버에서 처리하는 방식입니다.
    - 클라이언트가 요청한 특정 리소스에 대해 특정 서버 등으로 연결하는 방식입니다.
    - 예를 들어, 이미지 파일에 대해서는 확장자를 참조하여 별도로 구성된 이미지 파일이 있는 서버/스토리지로 직접 연결해 줄 수 있습니다.
- **쿠키 지속성 (Persistence With Cookies)**
    - 쿠키 정보를 바탕으로 클라이언트가 연결했던 동일한 서버에 계속 할당해 주는 방식입니다.
    - 특히 사설 네트워크에 있던 클라이언트의 IP 주소가 공인 IP 주소로 치환되어 전송 (X-Forwarded-For 헤더에 클라이언트 IP 주소를 별도 기록) 하는 방식을 지원합니다.

![https://post-phinf.pstatic.net/MjAxOTEyMTBfMjA1/MDAxNTc1OTU1MzgxODY5.odnG4CRES0e5bH7sOKyWRP1c8uO_XC4VX9A3HPeI1JQg.lNL2eJYbMz6NX1e5YFzfHDMQHn4YrdOJR2VYHmq5e1Ig.JPEG/L7-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200](https://post-phinf.pstatic.net/MjAxOTEyMTBfMjA1/MDAxNTc1OTU1MzgxODY5.odnG4CRES0e5bH7sOKyWRP1c8uO_XC4VX9A3HPeI1JQg.lNL2eJYbMz6NX1e5YFzfHDMQHn4YrdOJR2VYHmq5e1Ig.JPEG/L7-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200)

![LoadBalancing.png](/img/post/LoadBalancing.png)

## 로드 밸런서 (Load Balancer) 기본 동작 방식

1. 사용자의 브라우저에서 [ssafy.com](http://ssafy.com) 이라고 입력
2. 클라이언트에서 설정된 메인 DNS 서버로 [ssafy.com](http://ssafy.com) 의 IP 주소 문의
3. 메인 DNS 서버는 [ssafy.com](http://ssafy.com) 주소를 관리하는 별도의 DNS 서버에 IP 주소 문의
4. 별도 관리 DNS 서버는 로드밸런서의 IP(Virtual IP) 주소를 메인 DNS 서버에게 알려줌
5. 메인 DNS 서버는 획득한 IP(Virtual IP) 주소를 클라이언트에게 전송
6. 클라이언트에서 로드밸런서의 IP(Virtual IP) 주소로  HTTP 요청
7. 로드밸런서는 별도의 로드밸런싱 기법을 통해 서버에게 요청 전송
8. 서버의 작업 결과를 받은 로드밸런서는 전달받은 HTTP 결과를 클라이언트에게 전송
