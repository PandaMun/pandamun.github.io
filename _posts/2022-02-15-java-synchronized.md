---
layout: post
title: (JAVA)Synchronized(동기화)
subtitle: 자바의 Synchronized 대해 알아보자
category: java
---

## Java의 동기화

멀티 스레드를 사용할때 중요하게 고려할점이 스레드간 동기화 문제입니다.

스레드간 공유하고 있는 data를 동기화가 제대로 되지 않은 상태에서 프로그램을 돌리게 되면 data의 안정성과 신뢰성을 보장할수 없습니다.

따라서 data가 멀티 스레드 프로그래밍에서 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없게(Thread safe)되기 위해서는 Synchronized 키워드를 사용하여 스레드간 동기화를 시켜줘야합니다.

## Synchronized 키워드

자바에서 지원하는 Synchronized 키워드는 멀티 스레드 프로그래밍에서 데이터에 대하여 해당 데이터를 사용하는 스레드만 접근이 가능하며 다른 스레드는 접근이 되지 않도록 막습니다.

일반적으로 Synchronized는 아래의 유형의 쓰입니다.

- Instance methods (인스턴스 메소드)
- Static method (스태틱 메소드)
- Code blocks inside instance methods (인스턴스 메소드 코드블록)
- Coce blocks inside static methods (스태틱 메소드 코드블록)

### Synchronized Instance methods(동기화된 인스턴스 메소드)

```java
public class MyCounter {

	private int count = 0;

	public synchronized void add(int value){
			this.count += value;
	}
}
```

add() 메소드 선언에서 Synchronized 키워드를 사용하였습니다.

인스턴스 메소드의 동기화는 메소드 단위로 동기화 되는것이 아닌 해당 메소드를 가진 인스턴스(객체) 기준으로 이루어집니다. 따라서 한 클래스가 동기화된 인스턴스 메소드를 가진다면 동기화는 이클래스의 한 인스턴스를 기준으로 이루어지며 한시점에 오직 한 스레드만이 동기화된 인스턴스 메소드를 실행할수 있습니다. 만약 인스턴스가 두개 이상이라면 한 인스턴스에 한 스레드만 메소드를 실행할수 있습니다.

### Synchronized Static methods(동기화된 스태틱 메소드)

```java
public static MyStaticCounter{

  private static int count = 0;

  public static synchronized void add(int value){
      count += value;
  }
}
```

이번엔 static키워드가 사용된 메소드에 Synchronized 키워드를 사용하였습니다.

static 메소드 동기화는 이 메소드를 가진 클래스의 클래스 객체 기준으로 이루어집니다. JVM 안에 클래스 객체는 클래스당 하나만 존재할수 있으므로 같은 클래스에 오직 한 스레드만 동기화된 Static 메소드를 실행할 수 있습니다.

### **Synchronized Blocks in Instance Methods(동기화된 인스턴스 메소드 코드블록)**

```java
public void add(int value){

    synchronized(this){
       this.count += value;   
    }
  }
```

전체 메소드에 Synchronized 키워드를 사용하는것보다는 때때로 메소드에 일부에만 사용할때가 바람직할때가 있습니다.

동기화 블록이 한 객체를 전달받고 있습니다. (this)

이는 add 메소드가 호출된 객체를 의미하는데요 이 동기화 블록 안의 전달된 객체를 모니터 객체(monitor object) 라고 합니다.

이 코드는 모니터 객체 기준으로 동기화가 이루어짐을 나타내고 있습니다. 동기화된 인스턴스 메소드는 해당 객체가 속한 객체를 모니터 객체로 사용합니다.

```java
public class MyClass {

    public synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }


    public void log2(String msg1, String msg2){
       synchronized(this){
          log.writeln(msg1);
          log.writeln(msg2);
       }
    }
  }
```

위 예시에서는 하나의 스레드는 두개의 동기화된 코드중 하나만을 실행할수 있습니다.

여기에서 두번째 동기화 블록의 모니터객체가 this가 아닌 다른 객체였다면 스레드는 한 시점에서 각 메소드를 실행할수 있습니다.

### **Synchronized Blocks in Static Methods(동기화된 스태틱 메소드 코드 블록)**

```java
public class MyClass {

    public static synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }


    public static void log2(String msg1, String msg2){
       synchronized(MyClass.class){
          log.writeln(msg1);
          log.writeln(msg2);  
       }
    }
  }
```

같은 시점에서 오직 하나의 스레드만 이 두 메소드중 어느쪽이든 실행 가능합니다. 두번째 동기화 블로그이 괄호에 Myclass.class 가 아닌 다른 객체를 전달한다면 스레드는 동시에 각 메소드를 실행할수 있습니다.

### 정리

- Synchronized 키워드는 Race condition이 발생할수 있는 코드를 동기화하여 1개의 스레드만 코드를 수행할수 있게 보장하는것입니다. Synchronized를 사용하여 효율적일수 있지만 상황에 따라  비효율적일수 있습니다. 상황에 따라서 잘 사용하는것이 중요합니다.

### Reference

- [https://codechacha.com/ko/java-synchronized-keyword/](https://codechacha.com/ko/java-synchronized-keyword/)
- [http://tutorials.jenkov.com/java-concurrency/synchronized.html](http://tutorials.jenkov.com/java-concurrency/synchronized.html)
- [https://parkcheolu.tistory.com/15](https://parkcheolu.tistory.com/15)
