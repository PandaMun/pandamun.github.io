---
layout: post
title: (JPA) 지연 로딩(LAZY)과 즉시 로딩(EAGER)
subtitle: 지연 로딩(LAZY)과 즉시 로딩(EAGER)란?
categories: JPA
---

## 지연 로딩과 즉시 로딩

### 즉시 로딩(EAGER)

```java
@Entity
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "team_id")
    private Team team;
```

위코드는 Member 엔티티와 Team 엔티티는 ManyToOne(다대일) 관계를 즉시로딩으로 작성하였습니다.



```java
@ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "team_id")
    private Team team;
```

위와같이 @ManyToOne(fetch = FetchType.EAGER)으로 즉시로딩을 설정하면 JPQL로 Member 엔티티 조회하게 되면 Team를 조회하는 시점에 Member 엔티티와 연관된 Team까지 불러오는 쿼리를 날려 한꺼번에 데이터를 불러오게 됩니다.

### 지연 로딩(LAZY)

```java
@Entity
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;
```

위코드는 Member 엔티티와 Team 엔티티는 ManyToOne(다대일) 관계를 지연로딩으로 작성하였습니다.

```java
@ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;
```

위와같이 @ManyToOne(fetch = FetchType.LAZY)으로 지연로딩을 설정하면 JPQL로 Member 엔티티 조회하게 되면 즉시로딩과는 달리 Team을 조회하는 쿼리가 생성되지 않고 Member만 조회하는 쿼리만 나가고 실제로 Team을 사용하는 시점에 Team을 조회하는 쿼리가 나가게 됩니다.

### 모든 연관관계는 지연로딩으로 설정하면 좋다.

- 즉시로딩( EAGER )은 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어렵다. 특히 JPQL을 실행할 때 N+1
문제가 자주 발생한다.
- 실무에서 모든 연관관계는 지연로딩( LAZY)으로 설정해야 한다.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, fetch join 또는 엔티티 그래프 기능을 사용한다.
- @XToOne(OneToOne, ManyToOne) 관계는 기본이 EAGER이므로 직접 LAZY으로 설정해야 한
다.

## Reference

- 자바 ORM 표준 JPA 프로그래밍
