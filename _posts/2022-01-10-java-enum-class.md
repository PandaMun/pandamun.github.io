---
layout: post
title: (JAVA) enum 클래스
subtitle: 자바의 열거형 enum 클래스에 대해 알아보자
category: java
---


### Enumeration

계절(봄, 여름, 가을, 겨울), 트럼프 카드 모양(스페이드, 하트, 클로버, 다이아몬드)등 한정된 값을 가지는 타입을 열거 타입(Enumeration Type)이라고 하며 이러한 열거 타입에 들어가는 값(ex. 클로버)을 열거상수(Enumeration constrant)라고 합니다.

### Enum을 통해 얻는 장점

- 코드가 단순해지면서 가독성이 좋아집니다.
- 인스턴스 생성과 상속을 방지하여 상수 값의 타입 안정성이 보장됩니다.
- 키워드 enum을 사용하기 떄문에 구현의 의도가 열거임을 알수 있습니다.

열거형을 사용하는 방법은 아래와 같습니다.

```java
public enum Trump{
	 SPADE,CLOVER,DIAMOND,HEART
}
```

아래처럼 추가 속성도 부여 할수 있습니다.

```java
public enum Trump{
	SPADE("스페이드",1),
	CLOVER("클로버",2),
	DIAMOND("다이아몬드",3),
	HEART("하트",4);

	private String Trumpname;
	private int score;

	private Trump(String Trumpname, int score){
	this.Trumpname = Trumpname;
	this.score = score;

	}

	public void getScore(){
		System.out.println(Trumpname + "의 점수는 " + score + "점 입니다.");
	}
}

Trump.SPADE.getScore();
```

결과값

```java
//출력
//스페이드의 점수는 1점입니다.
```

### Enum클래스의 특징

- Enum클래스의 열거상수는 모두 대문자로 선언합니다.

- Enum클래스에서 열거상수를 기본으로 선언한경우에 세미콜론(;)을 붙이지 않는다
    - 열거상수에 속성값을 추가한경우에는 세미콜론을 붙입니다.

- 상속을 지원하지 않습니다.
    - 자바는 다중상속을 지원하지 않기 때문에 java.lang.enum 클래스를 상속받는 Enum은 상속을 지원하지 않습니다.

- enum은 클래스처럼 생성자를 가질수 있습니다. 하지만 고정된 상수의 집합임으로 런타임이 아닌 컴파일 타임에 모든 값을 가지고 있어야합니다. 즉 다른 클래스에서 enum 타입에 접근하여 동적으로 값을 정해줄 수 없습니다. 따라서 컴파일시 타입 안정성이 보장됩니다.(enum 클래스 내에서도 new 키워드로 인스턴스 생성 불가능). 이 때문에 생성자의 접근 제어자를 private으로 설정해야하며 이렇게 되면 외부에서 접근 가능한 생성자가 없으므로 enum은 final 특성을 가지고 있습니다. 클라이언트에서 enum의 인스턴스를 생성할수도 없고 상속을 받을수도 없으므로 클라이언트 관점에서 인스턴스는 없지만 선언된 enum 상수는 존재하게 됩니다. 이러한 특징 때문에 enum은 싱글톤 패턴을 구현하는데 사용되기도 합니다.

### Enum 클래스 메소드

- values() 메소드
    - values() 메소드는 해당 열거체의 모든 상수를 저장한 배열을 생성하여 반환합니다.
    - 이 메소드는 자바의 모든 열거체에 컴파일러가 자동으로 추가해주는 메소드입니다.

```java
enum Direction{
		EAST,WEST,SOUTH,NORTH
	}
public class Example{
		public static void main(String[] args) {

				Direction[] arr = Direction.values();

				for (Direction dir : arr){
					System.out.println(dir);
				}
		}
}
```

결과 값

```java
//출력
//EAST
//WEST
//SOUTH
//NORTH
```

- ordinal() 메소드
    - ordinal() 메소드는 해당 열거상수가 정의된 순서를 반환합니다.
    - 이때 반환된 값은 열거상수가 정의된 순서입니다.


```java
enum Direction{
		EAST,WEST,SOUTH,NORTH
	}
public class Example{
		public static void main(String[] args) {

				int index = Direction.SOUTH.ordinal();
				System.out.println(index);
		}
}
```

결과 값

```java
//출력
//2
```

- valueOf() 메소드
    - valueOf() 메소드는 전달된 문자열과 일치하는 열거상수를 반환합니다.
    - 일치하는 원소가 없을경우 Illegalargumentexception 발생합니다.

```java
enum Direction{
		EAST,WEST,SOUTH,NORTH
	}
public class Example{
		public static void main(String[] args) {

				Direction dir = Direction.valueOf("WEST");
				System.out.println(dir);
		}
}
```

결과 값

```java
//출력
//WEST
```

## Reference

- Effective Java 3rd Edition
- opentutorials.org
- nextree.co.kr
