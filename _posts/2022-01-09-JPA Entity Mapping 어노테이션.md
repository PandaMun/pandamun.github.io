---
layout: post
title: JPA Entity Mapping
subtitle: 엔티티와 테이블을 정확히 매핑하기위해 어떠한 어노테이션을 사용하는지 알아보자
category: jpa
---

자바에서 어노테이션은 코드사이에 사용되어 특별한 기능을 수행하도록 하는 기술입니다.

엔티티와 테이블을 매핑하기 위하여 사용하는 어노테이션에 대해 알아보겠습니다.

```java
import javax.persistence.*;
import java.util.Date;

@Entity  // 테이블과 매핑할 클래스는 @Entity를 붙여야 JPA가 엔티티로써 관리합니다.
@Table(name = "USER") // USER 테이블과의 매핑
public class User{

		@Id
		@Column(name = "ID")
		private String id // 기본키(PK)설정, Id 객체필드를 ID컬럼에 매핑

		@Column(name = "NAME")
		private String username; // 객체 필드를 NAME 컬럼에 매핑

		private Integer age; // * Column 어노테이션을 생략시 기본값으로 매핑됩니다.
														//자세한 내용은 아래에 있습니다.
```

JPA는 데이터베이스 스키마를 자동으로 생성하는 기능을 지원합니다.

그래서 위 에있는 엔티티 클래스의 매핑정보를 보면 테이블의 구조를 파악 가능합니다.

### @Entity

- JPA를 사용하여 테이블과 매핑할 클래스는 Entity 어노테이션을 필수로 붙여야 JPA가 엔티티로써 관리를 할수 있습니다.
    - 속성
        - name : JPA에서 사용할 엔티티 이름을 지정하며 설정하지 않는다면 기본값인 클래스이름을 사용합니다. (보통 기본값 사용)
    - 특징
        - 기본생성자(파라미터가 없는 public, protected 생성자)는 필수로 작성해야하며 final, enum, interface, inner 클래스는 사용할수 없습니다.
        - 저장할 필드에 final을 사용해서는 안됩니다.

### @Table

- Table 어노테이션은 엔티티와 매핑할 테이블을 지정합니다, 생략시 엔티티 클래스 이름과 같은 테이블과 매핑합니다.
    - 속성
        - name : 매핑할 테이블의 이름
        - catalog : catalog 기능이 있는 데이터베이스에서 catalog를 매핑
        - schema : schema 기능이 있는 데이터베이스에서 schema를 매핑
        - uniqueConstraints : DDL(Data Definition Language) 생성시 유니크 제약조건을 만듦, 이 기능은 스키마 자동 생성 기능을 사용하여 DDL을 만들때만 사용됨

## 기본키 매핑

### @Id

- JPA가 객체를 관리할때 식별할 기본키를 지정합니다. (테이블 PK설정)

## 필드와 컬럼 매핑

### @Column

- 객체 필드와 테이블의 컬럼을 매핑합니다.
    - 속성
        - name : 맵핑할 테이블의 컬럼 이름을 정합니다.
        - nullable : NULL을 허용할지 정합니다.(속성 값을 false로 지정하면 DDL에 not null 제약조건을 추가할수 있습니다.)
        - length : 문자의 크기를 지정합니다. 기본값으로는 255가 입력됩니다.
        - unique : 제약조건을 걸때 사용합니다.
        - insertable : 엔티티 저장시 선언된 필드도 같이 저장합니다.


### @Enumerated

- 자바의 enum 타입을 매핑한다.
    - 속성
        - EnumType.ORDINAL : enum 순서를 데이터베이스에 저장(enum에 저장된 순서가 값이 되어 데이터베이스에 저장됩니다.)(기본값)
        - EnumType.STRING : enum 이름을 데이터베이스에 저장(이름 그대로 문자열 자체가 데이터베이스에 저장됩니다.)
    - 기본값이 ORIDNAL으로 사용할때 데이터베이스에 저장되는 데이터 크기가 작지만 enum의 순서를 바꾸게 되면 문제가 생기기 때문에 enum의 순서를 변경할수 없습니다.


### @Temporal

- 날짜 타입의 컬럼을 매핑할때 사용합니다.(데이터베이스에는 date(날짜), time(시간), timestamp(날짜와시간) 세가지 타입이 존재합니다.)
    - 속성
        - TemporalType.DATE : 날짜, 데이터베이스 date 타입과 매핑 (2022-01-09)
        - TemporalType.TIME : 시간, 데이터베이스 time 타입과 매핑( 10:11:12)
        - TemporalType.TIMESTAMP : 날짜와 시간, 데이터베이스 timestamp 타입과 매핑            (2022-01-09 10:11:12)

### @Lob
