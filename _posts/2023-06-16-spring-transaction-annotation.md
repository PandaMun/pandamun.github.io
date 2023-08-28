---
layout: post
title: 간단하게 @Transactional에 대한 핵심원리와 사용방법에 대해 알아보자
subtitle: Transaction 어노테이션을 사용하는 방법에 대해 알아보자.
author: pandamun
category: spring
---

그동안 프로젝트와 취업 준비등으로 너무 바빠 블로그에 글을 자주 포스팅해야지 하면서도 제대로 운영을 하지 못하였지만 앞으로는 적어도 3주에 게시글 한개씩은 포스팅할 예정이다.

@Transactional은 사실 프로젝트를 진행할때 아주 많이 사용하는  Annotaion으로 전파 유형, 롤백 규칙, 격리 수준 등의 기능을 지원해주는 아주 좋은  Annotaion입니다.

이러한 Annotation이 Spring 핵심원리에 늘 언급되는 PSA, AOP가 사용되었다라는걸 알고 있지만 실제로 정확히 이야기해봐라고 했을때는 바로 노련하게 말이 나오지 않아 설명을 할수 없다는건 제대로 알고있지 않다는것이기 때문에 복기이자 완벽한 이해를 위해 정리해보았습니다.

첫번째로 트랜잭션이란게 무엇이냐?

## Transaction이란?

---

트랜잭션이란, 데이터베이스의 상태를 변화시키기 위해 수행되는 작업의 단위입니다.

몇몇 사람들이 오해하는것이 트랜잭션이 쿼리문 하나를 의미하는것이라고 생각하는것이다.

예를 들어 어떤 유저가 게시판에 글을 올린다고 가정을 해봅시다.

게시판에 글을 올리고 그후 다시 게시판에 돌아왔을때 게시판에 자신의 글이 올라와 있는걸 볼수 있습니다.

이러한 작업을 데이터베이스 관점에서 생각하면 Insert문을 이용하여 게시판에 글을 등록하고 다시 Select 작업을 통해 최신 정보를 불러오게 됩니다. 이러한 Insert, Select 문의 작업을 합친 하나의 작업단위를 하나의 트랜잭션이라고 말할수 있다.

트랜잭션에 대해서는 알았다면 @Transactional 이 어떠한 역할을 하는가?

## @Transactional의 역할

---

@Transactional이라는 annotation이 붙게 되면 Spring은 포인트 컷의 대상으로 자동 등록하고 트랜잭션 관리 대상이된다.

> 여기서 포인트컷 대상으로 자동 등록된다는 이야기는 해당 부분에 부가적인 기능을 정의한 Advice가 결합되어 새로운 프록시 객체가 생성되어 호출된다는 의미이다.
이해가 되지 않는다면 블로그에 정리된 AOP 관련 글을 읽어보는걸 추천한다.


[Spring 코드로 보는 AOP(Aspect Oriented Programming)](https://pandamun.github.io/post/AOP(Aspect-Oriented-Programming)-spring)

### Transaction의 커밋과 롤백

---

@Transaction을 사용하게 되면  커밋과 롤백 기능을 제공합니다.

어떤 경우에 해당 기능이 사용되느냐? 바로 UncheckedException이 발생될때 입니다.

**UncheckedException?**
> UncheckedException은 RuntimeException 클래스와 그 하위 클래스를 포함하는 Exception을 의미합니다. 컴파일러가 예외 처리를 강제하지 않고, 개발자가 선택적으로 예외처리를 할수 있습니다. 즉 try-catch 블록을 사용하지 않아도 **컴파일 오류**가 발생하지 않는 에러들입니다.

**CheckedException?**
> CheckedException은 UncheckedException과 반대로 컴파일러가 예외 처리를 강제하며, 반드시 try-catch 블록을 사용하거나 해당 메서드에서 throws문으로 상위로 예외를 던져야 합니다.


여기서 UncheckedException을 롤백 대상으로 잡은 이유로는 스프링에서 데이터 엑세스 기술의 예외는 런타임 에러로 전환해서 던지기 때문입니다. 또한 해당 Exception은 예외 처리를 강제하지 않고 예측 불가능한 Exception이기 때문에 문제가 발생하면 Rollback후 개발자가 해당 문제를 빠르게 수정할수 있게 하기 위함이라고 생각합니다.

### 트랜잭션 Isolation Level 설정

---

```java
@Service
public class TransactionalExampleService {

    @Transactional(isolation = Isolation.READ_COMMITTED)
    public void performTransactionalOperations() {
        // 여기에 트랜잭션으로 묶을 작업들을 구현합니다.
        // 이 메서드 내에서 실행되는 모든 데이터베이스 작업은 격리 수준이 READ_COMMITTED로 적용됩니다.
    }
}
```

위의 예제에서 @Transactional(isolation = Isolation.READ_COMMITTED) 부분에서 isolation 속성을 사용하여 격리 수준을 READ_COMMITTED로 설정하였습니다. 이제 이 메서드 안에서 수행되는 모든 데이터베이스 작업은 READ_COMMITTED 격리 수준으로 처리됩니다.

스프링 프레임워크에서 지원하는 다른 Isolation 옵션들로는 DEFAULT, READ_UNCOMMITTED, REPEATABLE_READ, SERIALIZABLE 등이 있으며, 필요에 따라 이러한 옵션을 사용하여 격리 수준을 설정할 수 있습니다.

추가적으로 Transaction의 격리수준에 대한 지식이 부족하다면 하단의 링크를 통해 다시한번 공부하자

[데이터 독립성 레벨(Isolation Levels)이란?](https://pandamun.github.io/post/database-isolation-levels)

### 트랜잭션의 전파 속성 설정

---

트랜잭션의 시작과 종료는 Connection 객체로 이루어집니다

트랜잭션 시작시 Hikari에서 Connection을 가져와서 작업을 진행하고 커밋을 하면 Hikari에 커넥션을 반납합니다.

그렇다면 여러 트랜잭션을 사용할때는 어떻게 될까요?

```java
@Test
void double_commit() {
  log.info("트랜잭션1 시작");
  TransactionStatus tx1 = txManager.getTransaction(new DefaultTransactionAttribute());
  log.info("트랜잭션1 커밋");
  txManager.commit(tx1);

  log.info("트랜잭션2 시작");
  TransactionStatus tx2 = TxManager.getTransaction(new DefaultTransactionAttribute());
  log.info("트랜잭션2 롤백");
  txManager.rollback(tx2);
}
```

JDBC는 자바에서 데이터베이스에 접속하고 데이터베이스와의 통신을 제공하는 자바 표준 API입니다.  이러한 JDBC는 중요한 설정중 하나는 “AutoCommit” 입니다. 해당 옵션은 데이터베이스 연결에서 트랜잭션을 자동으로 커밋할지에 대한 설정입니다.
트랜잭션을 사용자가 컨트롤하기 위해서는 해당 옵션을 false로 설정하여야 commit() 메서드를 호출하기 전까지 데이터베이스에 변경사항이 반영되지 않습니다.

```java
public void executeQuery() throws SQLException {
    Connection connection = dataSource.getConnection();
    connection.setAutoCommit(false);
    // 트랜잭션 시작
    ...
}
```

스프링을 이용하게 되면 내부적으로 커넥션을 가지고 있는 아래와 같은 추상화된  PlatformTransactionManager를 이용하게 됩니다

```java
package org.springframework.transaction;

public interface PlatformTransactionManager {

	TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;

	void commit(TransactionStatus status) throws TransactionException;


	void rollback(TransactionStatus status) throws TransactionException;

}
```

아래와 같이 DefaultTransactionDefinition 객체를 사용하여 트랜잭션 동작방식의 4가지 속성을 정의할수 있고 해당 정의된 인스턴스 설정으로 트랜잭션 매니저 내부에서 트랜잭션이 실행되게 됩니다.

```java
public void executeQuery() throws SQLException {

	DefaultTransactionDefinition transactionDefinition = createTransactionDefinition();    

	TransactionStatus status = transactionManager.getTransaction(createTransactionDefinition(new DefaultTransactionDefinition()));
    // 트랜잭션 시작
				try {

						....
            this.transactionManager.commit(status);
        } catch (RuntimeException e) {
            this.transactionManager.rollback(status);
            throw e;
        }


}
private DefaultTransactionDefinition createTransactionDefinition() {
        DefaultTransactionDefinition transactionDefinition = new DefaultTransactionDefinition();

        // 전파속성
        transactionDefinition.setPropagationBehavior(DefaultTransactionDefinition.PROPAGATION_REQUIRED);

        // 격리수준
        transactionDefinition.setIsolationLevel(DefaultTransactionDefinition.ISOLATION_DEFAULT);

        // timeout
        transactionDefinition.setTimeout(30);

        // Read-only
        transactionDefinition.setReadOnly(false);

        return transactionDefinition;
    }

```

**여기서 PlatformTransactionManager에 대해 알고가자**

> Spring에서 PlatformTransactionManager은 다양한 종류의 DataSource와 TransactionManager을 지원하기위해 설계된 최상위 인터페이스입니다.
아래 이미지를 보게 되면 JPA(Java Persistence API)나 JDBC(Java Database Connectivity)를 사용하기 위한 다양한 PlatformTransactionManager 구현체들이 있는걸 확인할수 있습니다.

![spring-platform-transaction-manager.png](/img/post/spring-platform-transaction-manager.png)

이처럼 Spring에서 @Transactional 을 사용하게되면 필요한 PlatformTransactionManager 구현체가 해당 DataSource에 맞게 Dependency Injection을 통해 주입되어 사용됩니다.
즉 @Transactional은 코드에서 트랜잭션 관련 로직을 분리하고 필요한 트랜잭션 매니저를 선택할수 있도록 도와줍니다
위와 같은 개념은 PSA(Portable Service Abstraction)입니다. PSA는 Spring의 핵심원리중 하나로 다양한 서비스에 접근하는 방법을 추상화하여 일관된 방식으로 사용할수 있도록 돕습니다.


### 트랜잭션 전파속성

---

Spring에서 제공하는 선언적 트랜잭션(@Transactional)은 여러 트랜잭션을 묶어서 커다란 하나의 트랜잭션 경계를 만들수 있습니다.

위처럼 2개의 Transaction이 순서대로 커넥션을 사용후 반납하는 방식 즉 동기적으로 작동합니다.

그렇다면 만약 2개의 Transaction이 동기적이 아닌 한 트랜잭션 내에 다른 트랜잭션이 존재하는 경우에는 어떻게 할까요?

![spring-transaction1.png](/img/post/spring-transaction1.png)

해당 경우에는 트랜잭션 정책에 따라 동작 방식이 결정되는데요.

스프링은 트랜잭션의 범위를 정하기 위하여 AOP를 사용하여 트랜잭션 관련 작업을 추상화한 @Transactional을 사용합니다.

해당 어노테이션은 여러 트랜잭션을 묶어 하나의 트랜잭션을 만들수 있으며 이미 트랜잭션이 진행중일때 추가로 진행되는 트랜잭션을 어떻게 할지 결정할수도 있다.

이것이 전파 속성(Propagation)입니다.

- 한 트랜잭션이 진행중일떄 추가 트랜잭션 진행을 어떻게 처리할지 결정하는것

해당 속성에 따라 기존 트랜잭션에 참여할수도, 별도로 진행할수도, 아니면 에러를 발생시킬수 있습니다.

![spring-transaction2.png](/img/post/spring-transaction2.png)

여기서 물리 트랜잭션, 논리 트랜잭션이라는 용어가 나옵니다.

물리 트랜잭션은 1개의 커넥션 객체를 사용하여 실제 데이터 베이스의 트랜잭션을 사용한다는 의미이며, 논리 트랜잭션은 트랜잭션 매니저를 사용하여 스프링이 트랜잭션을 처리하는 단위 입니다.

위에 그림에서는 2개의 트랜잭션이 한 물리 트랜잭션안에 존재합니다. 위 경우에는 모든 논리 트랜잭션이 커밋되어야 물리 트랜잭션이 커밋되며, 하나의 논리 트랜잭션이라도 롤백이 된다면 물리 트랜잭션 내에 있는 모든 트랜잭션은 롤백되게 됩니다.

그렇다면 스프링에서 제공하는 트랜잭션의 전파속성은 어떤것이 있을까?

```java
@Transactional(propagation = Propagation.REQUIRED)
```

아래와 같이 7개가 존재합니다.

- Propagation.REQUIRED: 이미 시작된 트랜잭션이 있으면 그 트랜잭션을 사용하고, 없으면 새로운 트랜잭션을 시작합니다.
- Propagation.SUPPORTS: 이미 시작된 트랜잭션이 있으면 그 트랜잭션을 사용하고, 없으면 트랜잭션 없이 실행됩니다.
- Propagation.MANDATORY: 이미 시작된 트랜잭션이 있으면 그 트랜잭션을 사용하고, 없으면 예외가 발생합니다.
- Propagation.REQUIRES_NEW: 항상 새로운 트랜잭션을 시작합니다.
- Propagation.NOT_SUPPORTED: 트랜잭션 없이 실행됩니다. 이미 시작된 트랜잭션이 있어도 일시 중단시킵니다.
- Propagation.NEVER: 트랜잭션 없이 실행됩니다. 이미 시작된 트랜잭션이 있으면 예외가 발생합니다.
- Propagation.NESTED: 중첩된 트랜잭션을 시작합니다. 부모 트랜잭션에 영향을 받으며 중첩된 트랜잭션이 커밋되거나 롤백됩니다.



![spring-transaction3.png](/img/post/spring-transaction3.png)

REQUIRED는 디폴트 속성으로 모든 트랜잭션 매니저가 지원하는 속성입니다.

### 참고자료

- 스프링 DB 데이터 접근 활용 기술
- 망나니 개발자
