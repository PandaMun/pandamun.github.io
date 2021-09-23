---
layout: post
title: 클래스, 객체, 인스턴스 무엇인가?
subtitle: 클래스,객체,인스턴스의 개념과 차이
author: Muntaeho(Pandamun)
category: knowledge
comments: true
---

# 클래스, 객체, 인스턴스의 개념

### 클래스(Class)
 - 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀입니다.

#### 예시

~~~java
// 클래스
public class Book {
  String title;
  String author;
}
~~~

### 객체(Object)
 - 클래스에서 정의한것을 토대로 메모리에 할당된것입니다.
 - 프로그램에서 사용되는 데이터 또는 식별자에 의해 참조되는 공간을 의미합니다.

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
     Book littlePrince, HarryPotter; //객체

 }
~~~

### 인스턴스(Instance)
 - 클래스의 구조로 컴퓨터 저장공간에 할당된 실체를 의미합니다.

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
     Book littlePrince, HarryPotter; //객체

     //객체를 인스턴스
     littlePrince = new Book(); // littlePrince는 Book클래스의 인스턴스(객체를 메모리에 할당)
     HarryPotter = new Book(); // HarryPotter는 Book클래스의 인스턴스(객체를 메모리에 할당)
   }
 ~~~
