---
layout: post
title: JPA를 활용한 애플리케이션 개발
subtitle: 엔티티 매니저 팩토리와 엔티티 매니저를 생성하고 앤티티를 수정해보자
category: jpa
---


## persistence.xml


```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_0.xsd">
	<persistence-unit name="hello" >
			<class>hellojpa.Member</class>
		<properties>

			<!--필수 속성-->
			<property name="javax.persistence.jdbc.driver"
				value="com.mysql.jdbc.Driver" />
			<property name="javax.persistence.jdbc.user" value="root" />
			<property name="javax.persistence.jdbc.password" value="12341234" />
			<property name="javax.persistence.jdbc.url"
				value="jdbc:mysql://localhost:3306/pandamun" />
			<property name="hibernate.dialect"
				value="org.hibernate.dialect.MySQLDialect" />			
		</properites>
	</persistence-unit>
```


위와 같이 resources/META-INF/persistence.xml을 작성하였다.

다음은 Member 클래스 생성입니다.


```java
@Getter
@Setter
@Entity
public class Member {
    @Id
    private Long id;
    private String name;
}
```


생성 되었다면 jpa을 사용하여 쿼리문을 넣어보겠습니다.

데이터베이스에 등록하는 코드입니다.


```java
public class jpaMain {
    public static void main(String[] args) {
        //EntityManagerFactory 생성
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("hello");
        //EntityManager 생성
        EntityManager entityManager = entityManagerFactory.createEntityManager();
        //transaction 생성
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin(); // 트랜잭션 시작
        try {
            Member member = new Member(); // 객체 생성
            member.setId(1L); // 객체에 값 추가
            member.setName("pandamun");
            entityManager.persist(member); // 앤티티 매니저를 사용하여 객체 등록

            tx.commit();// 트랜젝션 커밋
        }catch (Exception e) { // 에러발생시
            tx.rollback(); // 트랜젝션 롤백
        }
        finally {
            entityManager.close(); // 앤티티 매니저 종료
        }
        entityManagerFactory.close(); // 앤티티 매니저 팩토리 종료

    }
}
```


- 엔티티 매니저 팩토리 생성

    JPA를 시작할려면 우선 persistence.xml의 설정 정보를 사용해서 엔티티 매니저 팩토리를 생성해야합니다.


    ```java
    EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("hello");
    ```


    여기서 hello라는 영속성 유닛을 찾아서 엔티티 매니저 팩토리를 생성합니다.

    이때 persistence.xml의 설정 정보를 읽어서 JPA를 동작시키기 위한 기반 객체를 만들고 JPA 구현체에 따라서는 데이터베이스 커넥션 풀도 생성하므로 엔티티 매니저 팩토리를 생성하는 비용은 아주 큽니다.

    따라서 엔티티 매니저 팩토리는 애플리케이션 전체에서 딱 한번만 생성하고 공유해야합니다.


- 엔티티 매니저 생성


    ```java
    EntityManager entityManager = entityManagerFactory.createEntityManager();
    ```


    앤티티 매니저 팩토리에서 매니저를 생성하며 앤티티 매니저를 사용하여 앤티티를 데이터베이스에 등록, 조회, 삭제, 조회 할수 있습니다.

    앤티티 매니저는 내부에 데이터 커넥션을 유지하며 데이터베이스와 통신합니다.

    앤티티 매니저는 데이터베이스 커넥션과 밀집한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안됩니다.

- 앤티티 매니저 종료


    ```java
    entityManager.close(); // 앤티티 매니저 종료
    ```


    사용이 끝난 앤티티 매니저를 종료 시킵니다.


- 앤티티 매니저 팩토리 종료


    ```java
    entityManagerFactory.close(); // 앤티티 매니저 팩토리 종료
    ```


    애플리케이션이 끝날때 앤티티 매니저 팩토리를 종료 시킵니다.


- JPA의 모든 데이터 변경은 트랜잭션 안에서 실행됩니다.

### 등록


```java
Member member = new Member(); // 객체 생성
member.setId(1L); // 객체에 값 추가
member.setName("pandamun");

entityManager.persist(member); // 앤티티 매니저를 사용하여 객체 등록
```


엔티티를 저장하려면 엔티티 매니저의 persist() 메소드에 저장할 엔티티를 넘기면 됩니다.

### 조회


```java
Member findMember = entityManager.find(Memeber.class, id);
```


find()는 데이터베이스의 테이블의 기본키 즉 id 값과 매핑한 식별자 값으로 엔티티를 조회하는 메소드입니다.

### 수정


```java
Member findMember = entityManager.find(Memeber.class, id);

findMember.setName("pandamun"); // 조회한 findMember 엔티티의 값을 변경합니다.
```


수정작업은 간단합니다. JPA는 어떤 엔티티가 변경되었는지 추적하는 기능을 가지고 있어 위처럼 엔티티의 값만 변경하면 update SQL이 생성됩니다.

### 삭제


```java
Member findMember = entityManager.find(Memeber.class, id);

entityManager.remove(findMember); // 조회한 findMember 엔티티를 삭제합니다.
```


조회한 findMember 엔티티를 엔티티 매니저의 remove 메소드에 넣어 삭제합니다.
