---
layout: post
title: JPA(Java Persistence API)란?
subtitle: 객체와 RDB을 맵핑하는 기술인 JPA(ORM)에 대해서 알아보자
category: JPA
---


## JPA?

- Java Persistence API
- 자바 진영의 ORM 표준

## ORM?

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터 베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 객체와 테이블을 매핑하여 패러다임의 불일치를 해결합니다.
- 대중적인 언어에는 대부분 ORM 기술이 존재

### 패러다임의 불일치

---

객체지향 프로그래밍은 추상화, 캡슐화,상속성,다형성 등의 시스템의 복잡성을 제어 할수 있는 다양한 장치들을 제공합니다.

관계형 데이터베이스는  데이터 중심으로 구조화, 집합적인 사고가 필요하며 객체지향 프로그래밍에서 제공하는 추상화, 상속성등의 개념이 존재하지 않습니다.

그래서 패러다임의 차이 즉 패러다임 불일치 문제가 발생합니다.이를 해결하기위해서 개발자는 복잡한 과정을 거쳐 불일치를 해결해야 한다.

JPA는 이런 패러다임의 불일치 문제를 개발자 대신 해결해준다.

---

## JPA 동작 방식

---

애플리케이션과 JDBC 사이에서 동작합니다.



![JPA_main.png](/img/post/JPA_main.png)

JPA가 JDBC API를 사용하여 DATABASE에 SQL을 주고 받는다고 생각하시면 됩니다.

### 객체 저장

---

![JPA_persist.png](/img/post/JPA_persist.png)

```java
jpa.persist(member)
```

PERSIST 메소드를 사용하여 Entity를 넘겨 받아 Entity 분석, Insert SQL를 생성, JDBC API 사용, 패러다임 불일치를 해결합니다.

### 객체 조회

---

![JPA_find.png](/img/post/JPA_find.png)

```java
Member member = jpa.find(memberId)
```

find()로 식별자를 넘기면 Member객체를 분석하서 Select SQL을 생성, DB에서 가져온 결과를 객체로 만들어 반환, 패러다임의 불일치를 해결합니다.

### 객체 수정

---

```java
member.setName(”변경할 이름”)
```

트랜젝션 안에서만 데이터를 수정하면 데이터를 커밋하는 시점에서 자동으로 변경한 점을 찾아 업데이트 쿼리가 발생합니다.

### 객체 삭제

---
```java
jpa.remove(member)
```


### JPA의 성능 최적화 기능

---

#### 1차 캐시와 동일성(identity)보장
- 같은 트렌젝션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
    - 같은 트랜젝션 안에서 똑같은 식별자를 조회할시 첫번쨰는 쿼리가 발생하지만 두번쨰는 jpa안의 캐시에서 값을 가지고 옴


    ```java
    String memberId = "100";
    Member m1 =jpa.find(Member.class, memberId); // SQL 1번만 실행
    Member m2 =jpa.find(Member.class, memberId); // 캐시

    println(m1 == m2) // true
    ```



- DB 독립성 레벨이 Read Commit이여도 애플리케이션에서 Repeatable Read 보장
    - DB Isolation level이 높아질수록 성능이 떨어집니다(DB동시성, 직렬성) 3단계에서 2단계로 줄여도 JPA 애플리케이션에서 3단계를 보장해줍니다.
    - [데이터베이스 독립성 레벨(Isolation Levels)](https://pandamun.github.io/database/2021/12/21/Database_Isolation_levels.html)


#### 트랜젝션을 지원하는 쓰기 지연
- 트랜젝션을 커밋할 때까지 Insert SQL을 모음


```java
transaction.begin(); // [트랜잭션] 시작

member.persist(MemberA);
member.persist(MemberB);
member.persist(MemberC);
// 여기까지 Insert SQL을 데이터베이스에 보내지 않습니다.

transaction.commit(); // [트랜젝션] 커밋
// 커밋하는 순간 데이터베이스에 Insert SQL을 모아서 보냅니다.
```


- JDBC BATCH SQL 기능을 사용하여 한번에 SQL 전송
    - jpa에 존재하는 hibernate 옵션을 사용하면 JDBC BATCH SQL 기능을 사용하여 여러번의 네트워크가 아닌 한 네트워크에 SQL을 모아서 보내게 됩니다. 그후 커밋을 하게 됩니다.
- UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화
- 트랜젝션 커밋시 UPDATE, DELETE SQL을 실행하고 바로 커밋


```java
transaction.begin(); // [트랜잭션] 시작

changeMember(MemberA);
changeMember(MemberB);
비즈니스 로직 수행(); // 비즈니스 로직 수행동안 DB 로우 락이 걸리지 않습니다.

transaction.commit(); // [트랜젝션] 커밋
// 커밋하는 순간 데이터베이스에서 UPDATE,DELETE SQL을 보내게 됩니다.
```


#### 지연 로딩
- 지연 로딩
    - 엔티티가 실제 사용될때 로딩합니다.


```java
Member member = memberDAO.find(memberId);// SELECT * FROM MEMBER
Team team = member.getTeam();
String teamName = tema.getName(); // 실제로 조회할때 가져온다. SELECT * FROM TEAM
```


- 즉시로딩
    - JOIN SQL로 한번에 연관된 엔티티까지 미리 조회
    - 즉시 로딩을 사용할시 예상치 못한 SQL이 발생할수 있다.


```java
Member member = memberDAO.find(memberId);// SELECT M.*, T.* FROM MEMBER JOIN TEAM..
Team team = member.getTeam();
String teamName = tema.getName();
```


JPA에서는 지연로딩과 즉시로딩을 모두 지원합니다.
