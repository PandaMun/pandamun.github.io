---
layout: post
title: (JAVA)ThreadLocal
subtitle: 자바의 ThreadLocal 대해 알아보자
category: java
---

ThreadLocal의 개념을 정리하였습니다.

### ThreadLocal

ThreadLocal은 자바의 Class중 하나이며 thread-local 변수들을 제공합니다.

ThreadLocal은 Thread의 정보를 Key값으로 하여 Map의 형식으로 데이터를 저장한후 사용할 수 있는 자료구조를 가지고 있습니다.

ThreadLocal class 는 한 스레드에 의해서 읽고 쓰여질수 있는 변수이며 멀티 쓰레드 환경에서 각 스레드의 독립적인 변수를 가질수 있습니다.  사용에는 저장(set), 조회(get), 제거(remove) 메서드를 사용합니다.

### ThreadLocal 기본 사용법

1. ThreadLocal 객체 생성
2. set 메서드를 사용하여 현재 쓰레드의 로컬 변수에 값을 저장
3. get 메서드를 사용하여 현재 쓰레드의 로컬변수 값을 가져온다.
4. remove 메서드를 사용하여 현재 쓰레드의 로컬변수 값을 삭제한다.

### 사용법

[https://soft.plusblog.co.kr/28](https://soft.plusblog.co.kr/28) 에 정리되어있습니다.

### 활용

ThreadLocal은 한 스레드에서 실행되는 코드가 동일한 객체를 사용할수 있도록 해주기 때문에 관련한 코드에서 파리미터를 사용하지 않고 객체를 각자 가져다 쓸때 사용됨

- Spring Security에서 사용자 인증 정보를 전파할때 사용됨
- 스레드에 안전하게 보관해야할때 사용됨
- 스레드 기준으로 동작하는 기능을 구현할때 사용됨

### 주의 할점

ThreadLocal을 Thread pool 환경(미리 스레드를 만들어 놓은 환경)에서 사용할경우 ThreadLocal 변수의 사용이 끝나면 해당 데이터를 삭제해야합니다. 삭제 하지 않는다면 재사용 되는 스레드가 올바르지 않은 데이터를 참조할수 있다.

- Reference

    [https://javacan.tistory.com/entry/ThreadLocalUsage](https://javacan.tistory.com/entry/ThreadLocalUsage)
