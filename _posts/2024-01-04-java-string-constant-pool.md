---
layout: post
title: Java String constant pool에 대해서
category: java
---

본 내용으로 들어가기 전에  아래 코드를 보자.

```java

String a = "Hello World";
String b = new String("Hello World");
String c = "Hello World";

```


여기서 a,b 각 문자열 선언 방식의 차이는 무엇일까?

또한 a,b는 같은 객체일까? 아닐까?

위 질문에 나는 a,b 객체 선언의 차이 그리고 a,c가 같은 객체인지 아닌지 확답을 하지 못하였다.

아래 내용을 읽으면 알겠지만 위 질문에 대한 대답은 a,c는 문자열 리터럴 선언, b는 new 연산자를 이용한 문자열 인스턴스 선언이다.

그리고 a,c는 같은 문자열 리터럴을 가지고 있기 때문에 메모리 내에서 한번만 선언되고 이를 공유 하기 때문에 같은 객체라고 이야기 할수 있다.

## String Constant Pool

Java에서 문자열 리터럴을 저장하는 Heap 메모리 내 영역을 String Pool 또는 String Constant Pool 이라고 합니다.

Java에서 String의 가장 중요한 특징은 Immutable(불변)하다는 것입니다. 즉 한번 생성된 문자열은 프로그램이 실행되는 동안 동일하게 유지 된다는 의미로써 이러한 불변성은 힙 메모리 영역내 String Constant Pool로 인하여 달성됩니다.

```java

String Str1 = "Hello";

```

위와 같이 문자열을 선언 하게 되면 스택에 String 타입의 a 객체가 저장되고 힙에는 해당 객체의 문자열 값을 가진 인스턴스가 생성되고 Stack 영역에 저장되어있는 a에 주소값으로 연결되게 됩니다.

![StringConstantPool1.png](/img/post/StringConstantPool1.png)

그렇다면 동일한 값을 가지는 문자열 변수를 추가적으로 사용했을때에는 어떻게 될까요?

```java

String Str1 = "Hello";
String Str2 = "Hello";

```

![StringConstantPool2.png](/img/post/StringConstantPool2.png)

두 문자열 객체 모두 Stack영역에 생성되지만 Heap영역에 추가적으로 인스턴스가 생성되지 않고 “Hello”을 가진 인스턴스가 재사용 되게 됩니다.

이렇게 String contatnt pool은 Heap영역내의 캐시같은 역할을 수행하여 주로 메모리 사용량을 줄이고 메모리 내 기존 인스턴스의 재사용을 향상 시키기 위해 존재합니다.

```java

String Str1 = "Hello";
String Str2 = "Hello";
String Str3 = "World";

```

만약 문자열 객체에 다른값이 할당되면 Heap영역에 추가적으로 생성되게 됩니다.

![StringConstantPool3.png](/img/post/StringConstantPool3.png)

만약 new 키워드를 사용하여 인스턴스를 생성할경우에는 어떻게 될까요?


```java

String Str1 = new String("Panda");
String Str2 = new String("Panda");

```

new 키워드를 사용하여 인스턴스를 생성할 경우에는 String contant pool 외부에 인스턴스가 강제로 생성되어 캐싱 및 재사용이 불가능 합니다.

![StringConstantPool4.png](/img/post/StringConstantPool4.png)

### Reference

[String Constant Pool in Java - GeeksforGeeks](https://www.geeksforgeeks.org/string-constant-pool-in-java/)
