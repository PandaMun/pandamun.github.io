---
layout: post
title: (JAVA) Stream의 단말연산
subtitle: Java8에서부터 지원하는 기술인 Stream의 최종연산을 알아보자
category: java
---
단말연산은 처리한 스트림을 결과로 만들어주는 연산으로써 아래와 같은 종류들이 있습니다.

## forEach()

중간연산이 peek()과 달리 스트림의 요소를 소모하는 최종연산중 하나로 스트림을 출력하는 용도로 많이 사용됩니다.

forEach()는 스트림의 각 요소에 대한 작업을 수행하여

## collect()

collect 작업은 스트림을

### count()

요소의 개수를 구할수 있습니다.

```java
long number = IntStream.of(1,2,3,4,5).count() // 5
```

## reduce()

 reduce연산은 모든 스트림 요소를 처리하여 값으로 도출하는 연산으로써 세가지 버전을 가지고 있습니다.

```java
Optional<T> reduce(BinaryOperator<T> accumulator); // 첫번째

T reduce(T identity, BinaryOperator<T> accumulator); // 두번째

<U> U reduce(U identity, BiFunction<U, ? super T, U>
			accumulator, BinaryOperator<U> combiner); // 세번째
```

## allMatch()

allMatch 작업은 스트림의 요소 모두 조건에 만족하면 true 를 반환합니다.

## anyMatch()

anyMatch 작업은 스트림의 요소가 조건에 하나라도 만족한다면 true를 반환합니다.

## noneMatch()

noneMatch 작업은 allMatch 작업과는 달리 스트림의 요소가 모두 조건에 일치하지 않을때 true를 반환합니다.

## min()

min 연산은 제공된 비교기(comparator)에 따라 스트림의 최소 요소를 반환하는 연산입니다.

## max()

max 연산은 min 작업과 유사하게 제공된 비교기(comparator)에 따라 스트림의 최대 요소를 반환하는 연산입니다.

# Java Stream 정리

- [Stream의 정의와 특징](https://pandamun.github.io/2021-12-09-Java-Stream%EC%9D%98-%ED%8A%B9%EC%A7%95/)
- [Stream 생성방법](https://pandamun.github.io/2021-12-10-Java-Stream-%EC%83%9D%EC%84%B1%EB%B0%A9%EB%B2%95/)
- [Stream 중간연산](https://pandamun.github.io/2021-12-11-Java-Stream%EC%9D%98-%EC%A4%91%EA%B0%84%EC%97%B0%EC%82%B0/)
- Stream 단말연산
