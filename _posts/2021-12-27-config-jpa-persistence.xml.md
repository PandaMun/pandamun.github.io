---
layout: post
title: JPA persistence.xml 설정
subtitle: persistence.xml을 설정해보자
category: jpa
---

# persistence.xml 설정

JPA는 persistence.xml을 사용하여 필요한 설정 정보를 관리합니다.

이 설정파일이 META-INF/persistence.xml 클래스 패스 경로에 있으면 별도의 설정없이 JPA가 인식할수 있습니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	version="2.1">
	<persistence-unit name="jpabook" >
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



```xml
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	version="2.1">
```


설정파일은 persistence로 시작합니다 . 이곳에 XML 네임스페이스와 사용할 버전을 지정합니다.


```xml
<persistence-unit name = "jpabook">
```


JPA 설정은 영속성 유닛(persistence-unit)이라는것부터 시작하는데 일반적으로 연결할 데이터베이스당 하나의 영속성 유닛을 등록한다.

영속성 유닛에는 고유한 이름을 부여해야합니다 여기서는 jpabook이라는 이름을 사용했습니다.


### JPA 표준 속성

- javax.persistence.jdbc.driver : JDBC 드라이버
- javax.persistence.jdbc.user : 데이터베이스 접속 아이디
- javax.persistence.jdbc.password : 데이터베이스 접속 패스워드
- javax.persistence.jdbc.url : 데이터베이스 접속 URL


### Hibernate 속성

- hibernate.dialect : 데이터베이스 방언(dialect) 설정

이름이 javax.persistence로 시작되는 속성은 JPA 표준 속성으로 특정 구현체에 종속되지 않는다. 반면에 hibernate로 시작하는 속성은 Hibernate 전용 속성이므로 Hibernate에서만 사용할수 있습니다.


### Dialect(데이터베이스 방언)

- Database Dialect(데이터베이스 방언)은 (지역별 사투리가 있는것처럼) 각 DBMS가 지원하는 SQL 문법이 조금씩 다르다는점과 각 DBMS에서 지원하는 특정 기능을 칭합니다.
- 애플리케이션 개발자가 특정 DBMS에 종속되는 기능을 많이 사용하다보면 나중에 DBMS를 교체하기가 어렵다. 하지만 이런 문제를 해결하고자 Hibernate를 포함한 대부분의 JPA구현체들은 다양한 DMBS 방언 클래스를 제공합니다.
