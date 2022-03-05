---
layout: post
title: (JAVA) Stream 생성방법
subtitle: Java8에서부터 지원하는 기술인 Stream를 생성해보자
category: java
---
기본적으로 스트림 API는 컬렉션, 배열, 파일, 비어있는 스트림등 다양한 데이터 소스에서 생성이 가능합니다.

### 컬렉션(Collection)

컬렉션 타입(Collection, List, Set)은  내장된 stream 메소드를 이용하여 스트림을 생성할수 있습니다.

```java
List<String> list = Arrays.asList("apple","banana","cat","dog");
// of()메소드 : 자바 9부터 추가됨
// List<String> list = List.of("apple","banana","cat","dog");
Stream<String> stream = list.stream();
```

### 배열(Array)

Arrays 클래스의 stream 메소드 또한 배열을 통해 스트림을 생성해줍니다.

```java
Arrays.stream(T[] arr);
//매개 변수로 온 배열의 요소를 그대로 스트림으로 전환합니다.

Arrays.stream(T[] arr, int startInclusive, int endExclusive);
//매개 변수로 온 배열의 요소를 startInclusive 번째 인덱스부터 endExclusive 인덱스까지만
//추출하여 스트림으로 전환함
```

예제 코드

```java
String[] arr = new String[]{"apple","banana","cat","dog"};

// 모든 인덱스를 그대로 스트림으로 전환
Stream<String> stream1 = Arrays.stream(arr);

// 1번째 인덱스부터 2번째 인덱스만 선택함
Stream<String> stream2= Arrays.stream(arr, 1, 2);

```

### 비어있는 스트림(Empty Stream)

Stream.empty() 메소드를 사용하여 비어있는 스트림을 생성할수 있습니다.

```java
Stream <String> emptystream = Stream.empty();
```

### Stream.builder()

Stream.builder 메소드를 사용하여 스트림을 생성할수 있으며 직접적으로 원하는 값을 넣을수 있습니다. 마지막으로 builder 메소드를 호출하여 스트림을 생성합니다.

```java
Stream<String> builderstream =
							 Stream.<String>builder()
											.add("apple")
											.add("banana")
											.add("cat")
											.add("dog")
											.build(); // ["apple","banana", "cat", "dog"]
```

### Stream.iterate()

iterate() 메소드를 사용하면 초기값과 람다를 인수로 받아 스트림을 생성할수 있습니다.

초기값에서 람다식을 거쳐 반환된 값을 다시 람다식을 거쳐 무한 스트림을 만들수 있기 때문에 limit 메소드로 크기를 제한합니다.

```java
Stream<Integer> iterateStream = Stream.iterate(0, x -> x + 1).limit(3); // 0,1,2
```

### Stream.generate()

generate() 메소드는 매개 변수로 함수를 받아 함수에서 리턴되는 객체가 스트림을 생성합니다.

함수는 무한하게 호출되기 때문에 무한 스트림을 만들수 있기 때문에 generate() 메소드 역시 limit 메소드로 크기를 제한합니다.

```java
Stream<Integer> generateStream = Stream.generate(() -> 1).limit(3);

Stream<Double> randomStream = Stream.generate(Math::random);
```

### 파일(Files.list, Files.lines)

파일의 행(line)을 요소로 하는 스트림을 생성하기 위해 java.nio.file.Files 클래스에는 list(), lines() 메소드가 정의 되어 있습니다.

list() 메소드는 path를 lines()메소드는 파일내의 각 라인을 스트림으로 생성합니다.

```java
Path path = Paths.get("~");
Stream<Path> list = Files.list(path);

Path filePath = Paths.get("~.txt");
Stream<String> lines = Files.lines(path);
```
