---
layout: post
title: (JAVA) Stream의 단말연산
subtitle: Java8에서부터 지원하는 기술인 Stream의 최종연산을 알아보자
category: java
---

# Java Stream 정리

- [Stream의 정의와 특징](https://pandamun.github.io/2021-12-09-Java-Stream%EC%9D%98-%ED%8A%B9%EC%A7%95/)
- [Stream 생성방법](https://pandamun.github.io/2021-12-10-Java-Stream-%EC%83%9D%EC%84%B1%EB%B0%A9%EB%B2%95/)
- [Stream 중간연산](https://pandamun.github.io/2021-12-11-Java-Stream%EC%9D%98-%EC%A4%91%EA%B0%84%EC%97%B0%EC%82%B0/)
- Stream 최종연산

---

단말연산은 처리한 스트림을 결과로 만들어주는 연산으로써 아래와 같은 종류들이 있습니다.

## forEach()

forEach 작업은 해당 스트림을 순회합니다.

하지만 forEach 메소드는 병렬 스트림을 사용하였을때 순서를 보장할수 없기 때문에 순서대로 순회하려면 forEachOrdered 메소드를 사용해야합니다.

예제 코드

```java
Stream<String> stream = Stream.of("apple","banana","cat","dog");
stream.forEach(System.out::println);
```

## collect()

collect 연산은 인수로 전달되는 Collectors 객체에 구현된 방법대로 스트림의 요소를 수집합니다.

Collectors 클래스에 여러방법이 클래스메소드로 정의되어 있어 원하는 방식에 따라 사용이 가능합니다.

그외에 사용자가 직접 Collectors 인터페이스를 구현하여 수집방식을 정의할수 있습니다.

Collectors클래스에 정의되어 있는 메소드는 아래와 같습니다.

- Stream을 배열이나 컬렉션으로 변환 : toArray(), toCollection(),toList(),toSet(), toMap()

    예제 코드

    ```java
    Stream<String> fruit = Stream.of("apple","banana","pineapple","lime");
     List<String> fruitList = fruit.collect(Collectors.toList());

    System.out.println(fruitList);
    ```


- Stream의 요소를 특정조건에 따라 그룹화 : groupingBy(), paritioningBy()

    예제 코드

    ```java
    //partioningBy을 사용하여 요소별 글자수의 짝수 홀수에 따라 나누어 저장
    Stream<String> fruit = Stream.of("apple","banana","pineapple","lime");

    Map<Boolean, List<String>> patition =
    		fruit.collect(Collectors.partitioningBy(i -> (i.length() % 2 == 0));

    List<String> even = patition.get(true);
    List<String> odd = patition.get(false);

    System.out.println("짝수는 : " + even);
    System.out.println("홀수는 : " + odd);

    ```


- Stream 연산결과를 문자열 하나로 합치기 : joining()

    예제 코드

    ```java
    //joining()
    Stream<String> fruit = Stream.of("apple","banana","pineapple","lime");

    String joinfruit = fruit.collect(Collectors.joining());

    System.out.println(joinfruit); // applebananapineapplelime
    ```


- Stream 요소의 통계 또는 연산작업 수행 : Counting(), maxBy(), minBy(), toMap()

    예제 코드

    ```java
    //counting() 요소의 개수 구하기
    Stream<String> fruit = Stream.of("apple","banana","pineapple","lime");

    long count = fruit.collect(Collectors.counting());

    System.out.println(count);  // 4

    ```


## reduce()

 reduce연산은 모든 스트림 요소를 처리하여 값으로 도출하는 연산으로써 아래와 같습니다.

```java
Optional<T> reduce(BinaryOperator<T> accumulator);

T reduce(T identity, BinaryOperator<T> accumulator);
```

1. 인자가 하나만 있는 형태로 BinaryOperator라는 인자를 사용하는데 이는 두개의 같은 타입 요소를 인자로 받아 동일한 타입의 결과를 반환하는 함수형 인터페이스를 사용합니다.

```java
List<Integer> list = List.of(1,2,3,4,5);
Optional<Integer> result = list.stream().reduce((a, b) -> a + b);
result.ifPresent(s -> System.out.println("result : " + s)); // result : 15
```

1. 두번째는 두개의 인자를 받아서  위와 동일한 동작을 하지만 초기값을 지정해줄수 있습니다.

```java
List<Integer> list = List.of(1,2,3,4,5);
Integer result = list.reduce(1, (a, b) -> a + b);
System.out.println("result : " + result); // result : 7
```

## allMatch()

allMatch 작업은 스트림의 요소 모두 조건에 만족하면 true 를 반환합니다.

예제 코드

```java
IntStream number = IntStream.of(11, 22, 33, 44);

System.out.println(number.allMatch(i -> (i % 11) == 0)); // true (모두 11의 배수)
```

## anyMatch()

anyMatch 작업은 스트림의 요소가 조건에 하나라도 만족한다면 true를 반환합니다.

예제 코드

```java
IntStream number = IntStream.of(11, 22, 33, 44);
// i 는 21 보다 작다
System.out.println(number.anyMatch(i ->  i < 21)); // true (11이 만족)
```

## noneMatch()

noneMatch 작업은 allMatch 작업과는 달리 스트림의 요소가 모두 조건에 일치하지 않을때 true를 반환합니다.

예제 코드

```java
IntStream number = IntStream.of(11, 22, 33, 44);
// 요소의 숫자가 50보다 크다
System.out.println(number.anyMatch(i ->  i > 50)); // true (모두 조건에 일치하지 않음)
```

## Count()

Count 메소드는 해당 스트림의 요소의 총 개수를 long 타입으로 반환합니다.

예제 코드

```java
IntStream stream = IntStream.of(1,2,3,4,5,6,7,8,9);
Long count = stream.count()

System.out.print(count);// 9
```

## min()

min 연산은 제공된 비교기(comparator)에 따라 스트림의 최소 요소를 반환하는 연산입니다.

예제 코드

```java
IntStream stream = IntStream.of(1,2,3,4,5,6,7,8,9);
int min = stream.min().getAsInt();

System.out.print(min);// 1
```

## max()

max 연산은 min 작업과 유사하게 제공된 비교기(comparator)에 따라 스트림의 최대 요소를 반환하는 연산입니다.

예제 코드

```java
IntStream stream = IntStream.of(1,2,3,4,5,6,7,8,9);
Long max = stream.max().getAsInt();

System.out.print(max);// 9
```

## sum()

sum 메소드는 해당 스트림의 요소의 총합를 반환합니다.

예제코드

```java
IntStream stream = IntStream.of(1,2,3,4,5,6,7,8,9);
int sum = stream.sum()

System.out.print(sum); //45
```

## average()

average 메소드는 해당 스트림의 요소의 평균를 반환합니다.

예제코드

```java
DoubleStream stream = DoubleStream.of(1,2,3,4,5,6,7,8,9);
Double avg = stream.average().getAsDouble();

System.out.print(avg);// 5.0
```
