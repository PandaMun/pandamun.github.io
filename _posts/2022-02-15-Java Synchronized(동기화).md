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

### Synchronized Instance methods

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

### Synchronized Static methods

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
