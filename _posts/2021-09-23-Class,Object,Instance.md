---
layout: post
title: 클래스, 객체, 인스턴스 무엇인가?
subtitle: 클래스,객체,인스턴스의 개념과 차이
categories: knowledge
---

## 클래스, 객체, 인스턴스의 개념

### 클래스(Class)
 - 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀입니다.
 - 클래스에 포함되는 title, author과 같은 변수는 속성(property)라고 하며 함수는 메소드(method)라고 합니다.

#### 예시

~~~java
// 클래스
public class Book {
  String title;
  String author;
  public void read(){...}
}
~~~


### 객체(Object)
 - 클래스가 붕어빵을 찍어낼수있는 틀이라면 틀에서 나오는 붕어빵이 객체라고 이해할수 있다.
 - 하나의 클래스로부터 수많은 객체를 생성할수 있다.

#### 예시

~~~java
 // 클래스
 public class Book {
   String title;
   String author;
   ...
 }
 //메인함수에 선언된 객체
   public static void main(String[] args){
     Book littlePrince,harrypotter; //객체

 }
~~~

### 인스턴스(Instance)
 - 클래스의 구조로 컴퓨터 저장공간에 할당된 실체를 의미합니다.
 - 클래스 타입으로 선언된 객체를 실제 메모리에 할당되었을때 인스턴스라고 한다.

#### 예시

 ~~~java
 // 클래스
 public class Book {
   String title;
   String author;
   ...
 }
 //메인함수에 선언된 객체
   public static void main(String[] args){
     Book littlePrince, harrypotter; //객체

     //객체 인스턴스화
     littlePrince = new Book(); // littlePrince는 Book클래스의 인스턴스(객체를 메모리에 할당)
     harrypotter = new Book(); // harrypotter는 Book클래스의 인스턴스(객체를 메모리에 할당)
   }
 ~~~
