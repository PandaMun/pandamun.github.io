---
layout: post
title: (JAVA) Stream의 특징
subtitle: Java8에서부터 지원하는 기술인 Stream에 대해 알아보자
category: java
---

# Java Stream 정리

- Stream의 정의와 특징
- [Stream 생성방법](https://pandamun.github.io/2021-12-10-Java-Stream-%EC%83%9D%EC%84%B1%EB%B0%A9%EB%B2%95/)
- [Stream 중간연산](https://pandamun.github.io/2021-12-11-Java-Stream%EC%9D%98-%EC%A4%91%EA%B0%84%EC%97%B0%EC%82%B0/)
- [Stream 최종연산](https://pandamun.github.io/2021-12-12-Java-Stream%EC%9D%98-%EC%B5%9C%EC%A2%85%EC%97%B0%EC%82%B0/)

---

## Stream?

Java8에서 부터 지원하는 Stream은 다양한 데이터 소스 즉 배열이나 컬렉션(List, Map,Set)으로 원하는 값을 얻을때 요소들을 하나씩 참조하면서 반복적인 처리를 할수 있게 합니다. 그렇게 되면 복잡한 로직이나 코드를 가진 경우에 불필요한 for이나 if문들을 쓰지 않고 깔끔하게 처리가능하게 됩니다.

## Stream의 특징

1. Stream은  데이터소스로부터 데이터를 읽기만할뿐 기존 데이터를 변경하지 않습니다.

1. Stream은 Iterator처럼 일회용입니다. 최종연산 즉 결과값을 반환후에는 스트림이 닫혀 스트림을 사용할려고 하면 에러가 뜨게 됩니다. 사용할려면 다시 스트림을 생성해야 합니다.

1. 최종 연산 전까지 중간 연산이 수행되지 않고 지연됩니다.

## Stream의 구조

Stream은 생성 - 가공 -  결과값 반환 이 세가지로 나눌수 있는데요

스트림을 생성후  n번의 중간연산을 거치고 1번의 최종연산을 마치고 결과 값이 나오게 됩니다.

성능을 최적화 하기위하여 최종 연산을 하기전까지 중간 연산들이 지연(lazy) 되며 최종 연산시 모두 수행됩니다.

```java
stream.distinct().limit(5).sorted().forEach(System.out::println)
//       중복 제거 ,5개자르기,정렬(중간연산),    출력(최종연산)       
```

## Stream 생성

컬렉션 구현 클래스의 stream 메서드를 사용하여 생성합니다.

## 중간 연산

- 생성된 스트림을 원하는 형태로 알맞게 가공하는 연산
- 연산 결과가 스트림인 연산이며 반복적으로 적용 가능합니다.



## 최종 연산

- 연산 결과가 스트림이 아닌 연산으로써 단 한번만 적용 가능합니다.(스트림의 요소를 소모)
- 지연(lazy)되었던 모든 중간 연산들이 최종 연산시 모두 수행됩니다.
- 최종 연산시 모든 요소를 소모한 스트림은 더이상 사용이 불가능 합니다.
