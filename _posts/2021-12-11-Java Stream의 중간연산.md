---
layout: post
title: (JAVA) Stream의 중간연산
subtitle: Java8에서부터 지원하는 기술인 Stream의 중간연산을 알아보자
category: java
---

# Java Stream 정리

- [Stream의 정의와 특징](https://pandamun.github.io/2021-12-09-Java-Stream%EC%9D%98-%ED%8A%B9%EC%A7%95/)
- [Stream 생성방법](https://pandamun.github.io/2021-12-10-Java-Stream-%EC%83%9D%EC%84%B1%EB%B0%A9%EB%B2%95/)
- Stream 중간연산
- [Stream 최종연산](https://pandamun.github.io/2021-12-12-Java-Stream%EC%9D%98-%EC%B5%9C%EC%A2%85%EC%97%B0%EC%82%B0/)

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
//1
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.skip(2).forEach(System.out::print); // 345
//2
Stream<String> stream = Arrays.stream(new String[]{"apple","banana","cat","dog"});
stream.skip(2).forEach(System.out::println); //cat dog
```

## limit()

limit 메소드는 스트림내 저장할 요소의 개수를 제한할때 사용합니다.

```java
Stream<T> limit(long maxSize)
```

예제

```java
//1
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.limit(3).forEach(System.out::print); // 123
//2
Stream<String> stream = Arrays.stream(new String[]{"apple","banana","cat","dog"});
stream.limit(2).forEach(System.out::println); //apple banana
```

## filter()

filter 메소드는 스트림내 요소들을 조건(Predicate)에 따라 필터링을 하는 메소드입니다.

```java
Stream<T> filter(Predicate<? super T> predicate)
```

```java
//1
IntStream stream = Intstream.rangeClose(1,5);//12345
stream.filter(i -> i >= 3).forEach(System.out::print); //345
//2

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

- sorted 메소드는 지정된 Comparator로 스트림을 정렬합니다. 하지만 Comparator 대신 람다식을 사용하는것이 가능합니다. Comparator을 지정하지 않는다면 스트림 기본 정렬 기준(Comparable)으로 정렬합니다.

예제코드

```java
Stream<String> stream = Arrays.stream(new String[]{"cat","banana","apple","dog"});
stream.sorted().forEach(System.out::println); //apple banana cat dog
```

## map()

map 메소드는 스트림내 저장된 요소중 원하는 값만 추출하거나 특정 형태로 변환할때 사용합니다.

```java
Stream<R> map(Function<? super T, ? extends R> mapper)
```

예제

```java
Stream<String> stream = Arrays.stream(new String[]{"apple","banana","cat","dog"});
stream.map(i -> i.toUpperCase()).forEach(System.out::println); // 대문자로 변환
//APPLE BANANA CAT DOG
```

## peek()

peek 메소드는 연산과 연산 사이에 처리된 스트림내 요소를 확인할때 사용하며 원래 스트림의 요소를 소모하지 않기 때문에 중간에 결과를 확인할때 유용하게 사용됩니다.(미리보기)

```java
Stream<String> stream = Arrays.stream(new String[]{"cat","banana","apple","dog"});
stream.map(i -> i.toUpperCase()).peek(i -> System.out.println("변환후 : " + i))
.forEach(System.out::println);
//변환후 : APPLE
//APPLE
//변환후 : BANANA
//BANANA
//변환후 : CAT
//CAT
//변환후 : DOG
//DOG
```

위코드에서 알수 있는접은 peek 연산은 단말연산(forEach)이 수행되지 않으면 실행이 되지 않는다는걸 알수 있습니다.

## flatMap()

flatMap 메소드는 Array나 Object로 감싸져 있는 모든 원소를 단일 원소 스트림으로 반환합니다.

![flatMap.png](/img/post/flatMap.png)

```java
String[][] arrs = new String[][]{
        {"Laptop", "Phone"}, {"Mouse", "Keyboard"}
};

List<String> arrstream = Arrays.stream(arrs)
        .flatMap(arr -> Arrays.stream(arr))
        .collect(Collectors.toList());

System.out.println(arrstream);
//[Laptop, Phone, Mouse, Keyboard]
```
