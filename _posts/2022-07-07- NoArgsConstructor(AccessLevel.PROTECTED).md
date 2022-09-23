---
layout: post
title: NoArgsContructor(AccessLevel.PROTECTED)에 대해서
subtitle: Lombok 라이브러리가 제공하는 생성자 어노테이션의 종류와 종류중 하나인 NoArgsConstructor의 PROTECTED AccessLevel을 쓰는 상황과 이유에 대해서 알아보자
category: web
---

NoargsConsturctor(AcessLevel)에 대해 알기전 생성자 어노테이션에는 무엇이 있으며 어떠한 역활이 있는지 확인하고 넘어가겠습니다.

### 생성자 어노테이션(Lombok)

@AllArgsConstructor : 모든 필드값을 파라미터로 받는 생성자를 만들어주는 어노테이션

@RequiredArgsConstructor : final이나 @NonNull인 필드값만 파라미터로 받는 생성자를 만들어주는 어노테이션

@NoArgsConstructor : 파라미터가 없는 기본 생성자를 만들어주는 어노테이션

```java
@NoArgsConstructor
@RequiredArgsConstructor
@AllArgsConstructor
public class User{

	@NonNull
	private String name;

	@NonNull
	private int age;

	private String adress;
}

//User(); -> @NoArgsConstructor
//User(name, age); -> @RequiredArgsConstructor
//User(name, age, adress); -> @AllArgsConstructor
```

이 포스팅의 주제인 파라미터가 없는 기본 생성자를 만들어주는 어노테이션인 NoArgsConstructor의 PROTECTE AccessLevel 을 왜 사용할까에 대해서 정리하겠습니다.

### @NoArgsContructor(AccessLevel.PROTECTED)

NoArgsContstruct(AcessLevel.PROTECTED)는 Entity나 DTO에서 많이 사용합니다.

그이유는 기본 생성자의 접근 제어를 PROTECTED로 설정 해놓게 되면 무분별한 객체 생성에 대해 체크할 수 있습니다.(코드를 제약하는 스타일로 짜는게 설계와 유지보수에 도움이 됩니다)

예를 들어 기본적인 Board 클래스에대해 생각해보면 Board클래스에 들어가야할 값은 subject, content, writer 정도만 있다고 하자

```java
@Getter
@Setter
@NoArgsConstruct
public class Board{
	private String subject;
	private String content;
	private String writer;
}
```

이런 상황에서 기본 생성자가 public이라면 아래와 같은 상황이 발생하게 됩니다.

```java
public static void main(String[] args){
	Board board = new Board();
	board.setSubject("제목");
	board.setContent("내용");
}
//불완전한 객체
```

그렇기 때문에 아래와 같이 해결합니다.

```java
@Getter
@Setter
@NoArgsConstruct(access = AccessLevel.PROTECTED)
public class Board{
	private String subject;
	private String content;
	private String writer;

	public static Board createBoard(String subject,
																		String content, String writer){
		Board board = new Board();
		board.setSubject(subject);
		board.setContent(content);
		board.setWriter(writer);

	return board;

	}
}
```

위처럼 수정하게 된다면 IDE 단계에서 누락을 방지 할수 있기 때문에 훨씬 수월하게 작업할수 있게 됩니다.


- Reference
    [https://cobbybb.tistory.com/14](https://cobbybb.tistory.com/14)
