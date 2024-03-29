---
layout: post
title: JPA 다양한 연관관계 매핑
subtitle: 엔티티는 대부분 다른 엔티티와 연관관계가 있으며 이를 JPA에서 어떻게 매핑하는지에 대해 알아보자.
category: jpa
---

### 연관 관계의 핵심

방향 : 단방향(한쪽만 참조), 양방향(양쪽 모두 참조)

다중성 : 일대일(1:1), 다대일(N:1), 일대다(1:N), 다대다(N:N)

연관 관계의 주인 : 객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해줘야합니다.

### 객체와 테이블의 연관관계

데이터베이스는 외래키를 사용한 조인으로 양방향 관계가 가능하지만 객체는 참조로 연관관계를 맺고 참조용 필드가 있는 객체만 참조할수 있기때문에  단방향 관계입니다. 그러기 때문에 양방향 객체 참조를 할려면 단방향 관계 두개를 사용하여야 합니다.

### 연관관계의 주인

엔티티를 양방향으로 설정할시 객체의 참조는 둘인데 외래키는 하나입니다. 따라서 둘사이의 차이가 발생하는데요 이러한 차이로 JPA에서는 두 객체의 연관관계 중 하나를 정하여 외래키를 관리하는데 이것을 연관관계의 주인이라고 합니다.

## 다양한 연관관계 매핑

연관관계를 매핑하기 위하여 사용되는 어노테이션이 있습니다. 다음과 같습니다.

- @JoinColumn(name = “매핑할 외래키”) : 조인컬럼은 외래키를 매핑할때 사용하며 name 에는 키 이름을 지정합니다.
    - 속성
        - name : 매핑할 외래키 이름
        - referencedColumnName : 외래키가 참조하는 대상 테이블의 컬럼명
        - foreignKey(DDL) : 외래키 제약조건을 직접 지정할 수 있습니다.(테이블을 생성할 때만 사용합니다.)

- @ManyToOne : 다대일(N:1)관계 매핑 정보입니다.
    - 속성
        - optional : 기본값이 true이며 false로 설정할시 연관된 엔티티가 항상 있어야합니다.
        - fetch : 글로벌 페치 전략을 설정합니다.
        - cascade : 영속성 전이 기능을 사용합니다.
        - targetEntity : 연관된 엔티티의 타입 정보를 설정합니다.

- @OnetoMany : 일대다(1:N)관계 매핑 정보입니다.
    - 속성
        - mappedBy = 연관관계의 주인 필드를 선택한다
        - fetch : 글로벌 페치 전략을 설정합니다.
        - cascade : 영속성 전이 기능을 사용합니다.
        - targetEntity : 연관된 엔티티의 타입 정보를 설정합니다.


## @ManyToOne(다대일)_단방향

```java

public class Member{

	@Id
	@GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;

	private String username;

	// 연관 관계 매핑
	@ManyToOne
	@JoinColumn(name = "TEAM_ID")
	private Team team;

	//Getter, Setter
}

```



```java
@Entity
public class Team{

		@Id
		@GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;

		private String name;


		//Getter, Setter
}
```

## @ManyToOne(다대일)_양방향

- 다대일(N:1) 관계의 반대방향은 일대다(1:N)입니다. 그래서 ManytoOne 매핑의 반대방향은 OnetoMany입니다.
- 양방향 매핑은 복잡하며 단방향 매핑만으로도 테이블과 객체의 연관관계 매핑은 가능합니다.
- 단방향 매핑을 양방향으로 만들면 반대방향으로 객체 그래프 탐색 기능이 추가되어 조회를 쉽게 할수있도록 하기 위함입니다.
- 아래는 ManytoOne(양방향) 매핑 테스트 코드입니다.

```java
@Entity
public class Team{

		@Id
		@Column(name = "TEAM_ID")
		private Long id;

		private String name;

		// 추가
		@OnetoMany(mappedBy = "team")
		private List<Member> members = new ArrayList<Member>();

		//Getter, Setter
}
```

- 양방향 관계에서 외래키를 가지고있는 쪽인 연관 관계의 주인입니다.(연관관계의 주인은 외래키의 관리자입니다.)
- 양방향 관계에서 대부분 다(N)쪽이 외래키를 가지고 있습니다.

### 양방향 연관관계의 주의점

> 데이터베이스에 외래키값이 정상적으로 저장되지 않는다면 외래키를 관리하는 연관관계의 주인에는 값을 입력하지 않고 주인이 아닌곳에 값을 입력했는지 확인해보자                                                                             JPA를 사용하지 않는 순수한 객체상태에서는 양쪽 모두 값을 입력하지 않는다면 문제가 발생할수 있으므로  객체관점에서 양쪽 방향에 모두 값을 입력해주는 것이 가장 안전합니다.
>

## @OneToMany(일대다)_단방향

- 일대다(1:N)는 다대일(N:1) 반대 방향입니다. 보통 엔티티를 하나 이상 참조 할수 있으므로 자바 컬렉션을 사용합니다.
- 일대다(1:N) 단방향은 일(1)이 연관관계의 주인입니다
- 일대다 관계는 다(N) 쪽에 외래키가 있어 하지만 다(N)쪽에는 외래키를 매핑할수 있는 참조 필드가 없으며  반대쪽인 일(1)쪽에 참조필드가 있어 반대편 테이블의 외래키를 관리하는 특이한 모습을 나타냅니다.

아래는 OnetoMany(단방향) 매핑 테스트코드입니다.

```java
@Entity
public class Team{

		@Id
		@GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;

		private String name;

		@OneToMany
		@JoinColumn(name = "TEAM_ID") //MEMBER테이블의 TEAM_ID (FK)
		private List<Member> members = new ArrayList<Member>();

		//Getter, Setter
}
```

```java

public class Member{

	@Id
	@GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;

	private String username;


	//Getter, Setter
}

```

- 일대다(1:N) 단방향 관계를 매핑할때는 @JoinColumn을 명시해야합니다. 그렇게 하지 않는다면 JPA는 연결 테이블을 중간에 두고 연관관계를 관리하는 조인 테이블 전략을 기본으로 사용하여 매핑합니다.

### 일대다(1:N) 연관관계의 문제점

> 일대다(1:N) 연관관계의 문제점은 앞서 말해듯이 매핑한 객체가 관리하는 외래키가 다른테이블에 있다는것이다. 본인 테이블에 외래키가 있다면 엔티티의 저장과 연관관계 처리를 INSERT SQL 쿼리 한번에 끝낼수 있지만 다른 테이블에 외래키가 있다면 연관관계 처리를 위하여 UPDATE SQL 쿼리를 추가로 실행해야합니다.                                                                                                                                                               또한 성능 문제도 있지만 관리가 힘들다는 문제도 있습니다. 이를 해결하기 위한 방법은 일대다(1:N) 단방향이 아닌 다대일(N:1) 양방향 매핑을 사용하는것입니다.
>

## @OneToMany(일대다)_양방향

- 결론부터 말하면 일대다(1:N) 양방향 매핑은 존재하지 않습니다.
- 양방향 매핑에서는 @OneToMany는 연관관계의 주인이 될수 없습니다.
- 일대다 양방향은 일대다 단방향 매핑과 다대일 단방향 매핑(읽기전용)으로 만들수는 있다.

아래는 일대다 양방향 테스트 코드입니다.

```java
@Entity
public class Team{

	@Id
	@GeneratedValue
	@Column(name = "TEAM_ID")
	private Long id;

	private String name;

	@OneToMany
	@JoinColumn(name = "TEAM_ID")
	private List<Member> members = new ArrayList<Member>();

	//Getter, Setter
}
```

```java
@Entity
public class Member {

	@Id
	@GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;

	private String username;

	@ManyToOne
	@JoinColumn(name = "TEAM_ID", insertable = false, updatable = false) //읽기전용
	private Team team;
```

 일대다(1:N) 단방향 매핑 ,반대쪽인 다대일(N:1) 단방향 매핑 모두 TEAM_ID 외래키 컬럼을 매핑하여 같은키를 관리하므로 문제가 생길수 있으므로 반대편인 다대일(N:1) 단방향 매핑쪽을 insertable = false, updatable = false로 설정하여 읽기만 가능하게 하였습니다.

## @OneToOne(1:1)

- 일대일(1:1) 관계의 반대도 일대일(1:1) 관계이며 일대다(1:N), 다대일(N:1)은 항상 다(N)쪽이 외래키를 가졌지만 일대일(1:1) 관계는 어느쪽이나 외래키를 가질수 있습니다.
    - 주 테이블의 외래키 : 주 테이블쪽에 만 확인해도 대상 테이블과 연관관계가 있는지 알수 있음
    - 대상 테이블의 외래키 : 테이블 관계를 일대일에서 일대다로 변경할때 테이블 구조를 그대로 유지가능함

### 주 테이블의 외래키

- 주 테이블의 외래키(단방향) 테스트 코드입니다.

```java
@Entity
public class Member {

	@Id
	@GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;

	private String username;

	@OneToOne
	@JoinColumn(name = "LOCKER_ID")
	private Locker locker;

	....
}
```

```java
@Entity
public class Locker {

	@Id
	@GeneratedValue
	@Column(name = "LOCKER_ID")
	private Long id;

	private String name;

	....
}
```

- 주 테이블의 외래키(양방향) 테스트 코드입니다.

```java
@Entity
public class Member {

	@Id
	@GeneratedValue
	@Column(name = "MEMBER_ID")
	private Long id;

	private String username;

	@OneToOne
	@JoinColumn(name = "LOCKER_ID")
	private Locker locker;

	....
}
```

```java
@Entity
public class Locker {

	@Id
	@GeneratedValue
	@Column(name = "LOCKER_ID")
	private Long id;

	private String name;

	@OneToOne(mappedBy = "locker")
	private Member member;

	....
}
```

- 양방향이므로 연관관계의 주인을 정해야한다. 외래키를 가지고 있는 Member 테이블이 외래키를 가지고 있으므로 Member 엔티티의 Member.locker이 연관관계의 주인입니다. 반대편은 mappedBy를 선언하여 연관관계의 주인이 아니라고 설정했습니다.

### 대상 테이블의 외래키

- JPA에서는 대상테이블의 외래키가 있는 일대일(1:1)관계는 지원하지 않습니다.
- 대상테이블의 외래키(양방향) 테스트 코드입니다.

```java
@Entity
public class Member {
  @Id
	@GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long id;

  private String username;

  @OneToOne(mappedBy = "member")
  private Locker locker;

  ....
}
```

```java
@Entity
public class Locker {

	@Id
	@GeneratedValue
	@Column(name = "LOCKER_ID")
	private Long id;

	private String name;

	@OneToOne
	@JoinColumn(name = "MEMBER_ID")
	private Member member;

	....
}
```

## @ManyToMany(다대다)

- 정규화된 테이블 2개로 다대다 관계를 표한할수 없기때문에 일대다(1:N), 다대일(N:1) 관계로 풀어내는 연결테이블을 사용한다.
- JoinTable
    - 속성
        - name : 연결 테이블을 지정합니다.
        - joinColumns : 현재 방향인 엔티티와 매핑할 조인 컬럼 정보를 지정합니다.
        - inverseJoinColumns : 반대 방향 엔티티와 매핑할 조인 컬럼 정보를 지정합니다.
- 다대다(N:N) 단방향 테스트 코드입니다.

```java
@Entity
public class Member {

  @Id
	@GeneratedValue
  @Column(name = "MEMBER_ID")
  private Long id;

  private String username;

  @ManyToMany
  @JoinTable(name = "MEMBER_PRODUCT",
						 joinColumns = @JoinColumn(name = "MEMBER_ID"),
             inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
  private List<Product> products = new ArrayList<Product>();

  ....   
}
```

```java
@Entity
public class Product {

  @Id
  @Column(name = "PRODUCT_ID")
  private String id;

  private String name;

  ....   
}
```

- ManyToMany와 JoinTable을 사용하여 Member와 Product를 연결하는 Member_Product 엔티티 없이 Member_Product 테이블을 바로 매핑하였습니다.
- 다대다(N:N) 양방향 테스트 코드입니다.

```java
@Entity
public class Product {

  @Id
  @Column(name = "PRODUCT_ID")
  private String id;

  private String name;

	@ManyToMany(mappedBy = "products")
	private List<Member> members; //역방향 추가

  ....   
}
```

- 다대다 관계이기 때문에 역방향도 @ManyToMany를 사용하며 양쪽중 원하는곳에 mappedBy를 사용하여 사용되지 않는쪽을 연관관계의 주인으로 만듭니다.
