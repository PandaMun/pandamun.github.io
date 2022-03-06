---
layout: post
title: (JAVA) Stream의 중간연산
subtitle: Java8에서부터 지원하는 기술인 Stream의 중간연산을 알아보자
category: java
---
생성된 스트림을 필터링하거나 원하는 형태로 가공하는 연산입니다.                                                   

중간 연산의 특징중 하나로 중간 연산의 결과값은 스트림을 반환하기 때문에 이어서 호출하는 메소드 체이닝이 가능합니다.                                            

또한 중간연산은 전에 언급했듯이 성능을 위하여 지연 연산이기 때문에 모든 중간 연산을 합친 다음 합친 연산을 마지막으로 처리합니다.

## skip()

Skip 메소드는 스트림내의 첫번째 인자부터 설정한 n개의 인자를 제외한 나머지를 반환합니다.

```java
Stream<T> skip(long n)
```

예제

```java
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.skip(2).forEach(System.out::print); // 345
```

## limit()

limit 메소드는 스트림내 저장할 요소의 개수를 제한할때 사용합니다.

```java
Stream<T> limit(long maxSize)
```

예제

```java
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.limit(3).forEach(System.out::print); // 123
```

## filter()

filter 메소드는 스트림내 요소들을 조건(Predicate)에 따라 필터링을 하는 메소드입니다.

```java
Stream<T> filter(Predicate<? super T> predicate)
```

```java
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.filter(i -> i >= 3).forEach(System.out::print); //345
```

## distinct()

distinct 메소드는 스트림내 저장된 요소중 중복된 요소를 제거할때 사용합니다.

```java
Stream<T> distinct()
```

예제

```java
IntStream stream = Intstream.of(1,1,2,2,3,3,4,4,5,5);
stream.distinct().forEach(System.out::println); // 1,2,3,4,5
```

## sorted()

sorted 메소드는 스트림내 저장된 요소를 정렬할때 사용합니다.

```java
Stream<T> sorted()

Stream<T> sorted(Comparator<? super T> comparator)
```

- comparator

## map()

map 메소드는 스트림내 저장된 요소중 원하는 값만 추출하거나 특정 형태로 변환할때 사용합니다.

```java
Stream<R> map(Function<? super T, ? extends R> mapper)
```

예제

```java
IntStream stream = IntStream.of(1,2,3,4,5);

```

## peek()

peek 메소드는 연산과 연산 사이에 처리된 스트림내 요소를 확인할때 사용하며 원래 스트림의 요소를 소모하지 않기 때문에 중간에 결과를 확인할때 유용하게 사용됩니다.

## flatMap()

flatMap 메소드는 스트림의 요소가 배열일경우 즉 중첩된 구조를 가진 스트림을 단일 구조 스트림으로 만들어 줍니다.
