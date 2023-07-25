---
layout: post
title: Spring 코드로 보는 AOP(Aspect Oriented Programming)
subtitle: AOP의 개념과 사용법
category: web
---

## AOP란?

AOP(Aspect Oriented Programming), 관점지향 프로그래밍은 어떠한 로직을 기준으로 핵심 관심사와 부가적인 관심사를 나누어서 해당 관점을 기준으로 모듈화하는 프로그래밍 패러다임입니다.

이렇게 말하면 처음에는 무슨말인지 싶었다. 그래서 자세히 예를 들어 정리해보겠습니다.

아래와 같이 서비스가 존재합니다.

![SpringAOP1.png](/img/post/SpringAOP1.png)

위와 같이 계좌이체, 대출승인, 이자계산이라는 핵심 기능이 존재하고 해당 핵심 기능마다 공통적으로 작동해야하는 부가기능이 존재합니다. 로깅, 보안, 트랜잭션이 그러한 기능들입니다.

그렇다면 이러한 부기기능을 어떻게 판별할것이냐?

그러기 위해서 다음과 같은 개념을 알고 있어야합니다.

![SpringAOP2.png](/img/post/SpringAOP2.png)

### JoinPoint

조인포인트는 타킷의 코드가 실행할때 나타날수 있는 여러시점으로써 포인트컷으로 지정될수 있는 후보들입니다.

- 예외가 발생하는 시점
- 생성자 호출 시점
- 필드값 접근 시점
- 메서드 실행 시점

하지만 Spring AOP는 조인 포인트는 항상 메서드 실행시점만을 의미합니다. 그이유로는 Spring AOP는 프록시 기반의 AOP 구현체로써 프록시 객체를 생성하여 핵심 비즈니스 로직을 감싸는 방식으로 AOP를 구현합니다. **즉 Spring AOP는 런타임 시점(정확히 인스턴스 메서드 호출 시점)에 프록시 객체를 생성하여 메서드 호출을 가로채는 방식으로 동작하기 때문에 static 메서드나 생성자 호출등의 같은 클래스 레벨의 작업에는 AOP를 적용하는것이 어렵습니다.**

하지만 프록시 객체를 대신 호출하여 작업을 수행하기 때문에 대상 메서드의 코드를 변경하지 않고도 추가기능(Advice)를 분리하여 추가할수 있다는 장점을 가지고 있습니다.

이를 통해 코드의 재사용성과 유지보수성이 높아지게 됩니다.

만약 컴파일 시점이나 JVM 클래스 로드 시점에 AOP를 동작시키기 위해서는 **AspectJ**와 같은 도구를 사용하여 AOP를 구현하는것을 추천합니다.

> **AspectJ**
.aj파일을 이용한 assertj 컴파일러를 추가로 사용하여 컴파일 시점이나 JVM 클래스 로드 시점에 조작합니다.
그렇기 때문에 런타임 시점에는 영향을 미치지 않습니다.( 컴파일이 완료된 후에는 성능에는 영향이 없다)
제가 생각했을때 사용시점은 다르지만 좀더 넓은 범위 즉 자바코드에서 동작하는 모든 객체에 대해 AOP를 적용할수 있는 기술이라고 생각합니다. (어렵다….)
>

### Weaving

간단하게 설명하면 핵심로직과 부가기능을 합쳐 새로운 프록시 객체를 생성하는 과정을 의미합니다.

예로 A라는 객체에 추가적인 기능(Advisor)을 추가한 새로운 프록시 객체가 생성되는데 앞으로 이 프록시 객체가 A객체를 호출되는 시점에 사용되는데 이때 프록시 객체가 생성되는 과정을 의미합니다.

이때 시점에 따라 Compile, load Time, Runtime으로 나뉘며 위에서 설명한것과 같이 Spring AOP는 런타임 시점에서 프록시 객체가 생성됩니다.

1. 컴파일 (Compile Time) Weaving:
    - AspectJ 컴파일러를 사용하여 핵심 로직과 어드바이스를 결합하는 방식입니다.
    - AspectJ는 Java 언어에 추가적인 기능을 제공하고, AspectJ 컴파일러를 사용하여 AspectJ 코드를 일반 Java 코드로 컴파일할 수 있습니다.
    - 컴파일 시간 Weaving은 코드가 컴파일되는 단계에서 Aspect를 적용하므로 런타임 성능에 영향을 주지 않습니다.
2. 클래스 로드(Load Time) Weaving:
    - 클래스 로더를 확장하여 Aspect를 대상 클래스에 주입하는 방식입니다.
    - 클래스 로드 시점에 Aspect를 적용하므로 런타임 성능에 약간의 오버헤드가 발생할 수 있습니다.
    - 보통 XML 파일 또는 JVM 옵션을 통해 클래스 로드 시간 Weaving을 설정합니다.
3. 런타임(Runtime) Weaving:
    - 대상 객체의 인스턴스를 생성할 때 Aspect를 주입하는 방식입니다.
    - Spring AOP에서는 주로 프록시를 사용하여 런타임 Weaving을 구현합니다.
    - 런타임 Weaving은 대상 객체를 감싸는 프록시를 생성하고, 프록시가 어드바이스를 적용하여 대상 객체를 감싸는 방식으로 동작합니다.

### Pointcut

Advice가 적용될 위치 또는 끼어들수 있는 시점을 의미하며 JoinPoint의 상세한 스펙(표현식)을 의미합니다.

예로 “HelloService의 sayHello메서드의 진입 시점에 호출할것”

### Advice

타켓(특정 메서드)에 적용할 부가기능을 의미합니다.

보통 해당 메서드가 Advice를 나타내는 Annotation의 종류는 Around, before, after가 있습니다.

- @Before
    - 해당 Annotation은 대상 메서드의 실행 이전에 실행되는 Advice를 정의합니다.
    - 대상 메서드 실행 이전에 사전작업을 수행합니다.
    - 코드로 확인하면 아래와 같습니다.
    - Advice 코드

    ```java
    @Aspect
    @Component
    public class BeforeAdviceExample {

        @Before("execution(* com.example.demo.MyService.beforeTest(..))")
        public void beforeAdvice() {
            System.out.println("Before advice: Going to execute the method");
        }
    }
    ```

    - 타켓 코드

    ```java
    @Service
    public class MyService {
        public String beforeTest(String name) {
            return "Test:  " + name + "!";
        }
    }
    ```

    - 결과

    ```java
    "Before advice: Going to execute the method"
    "Test: pandamun!"
    ```

- @After
    - 해당 Annotation은 대상 메서드의 실행 이후에 실행되는 Advice를 정의합니다.
    - 대상 메서드 실행 이후에 사후 작업을 수행합니다.
    - 코드로 확인하면 아래와 같습니다.
    - Advice 코드

    ```java
    @Aspect
    @Component
    public class AfterAdviceExample {

        @Before("execution(* com.example.demo.MyService.afterTest(..))")
        public void beforeAdvice() {
            System.out.println("After advice: Method execution completed.");
        }
    }
    ```

    - 타켓 코드

    ```java
    @Service
    public class MyService {
        public String afterTest(String name) {
            return "Test:  " + name + "!";
        }
    }
    ```

    - 결과

    ```java
    "Test: pandamun!"
    "After advice: Method execution completed."
    ```

- @Around
    - 해당 어노테이션은 대상 메서드 실행 전후에 작업을 수행하는 가장 유연한 Annotation으로 타겟 메서드를 감싸 호출 전후에 Advice 기능을 수행합니다.
    - 해당 Annotation이 붙은 메서드의 반환 값은 Object여야 합니다. 그 이유로는 @Around 어노테이션은 핵심 로직을 가로채기 때문에, 실제로 대상 메서드를 호출하는 부분을 어드바이스 메서드 내에서 직접 처리해야 합니다. 이 때 대상 메서드의 반환 값을 어드바이스 메서드가 받아야 후속 처리를 수행할 수 있습니다. 따라서 반환 타입을 Object로 지정하여 대상 메서드의 반환 값을 받아오게 됩니다.
    - 아래는 타겟메소드의 실행시간을 측정하는 Advice 코드입니다.
    1. 먼저 MeasureExecutionTime이라는 Annotation을 정의 합니다.

    ```java
    // Annotation 적용 대상 지정(Method)
    @Target(ElementType.METHOD)
    // Annotation 정보를 유지할 범위 지정(Runtime)
    // 즉 실행 중에도 Annotation 정보를 읽을수 있다.
    @Retention(RetentionPolicy.RUNTIME)
    // @interface: 사용자 정의 어노테이션 정의를 시작하는 키워드입니다.
    public @interface MeasureExecutionTime {
    //특별한 속성을 가지고 있지 않기 떄문에 Annotation 존재 여부 확인을 위해 사용됨
    }
    ```

    2. 그후 해당 어노테이션을 범위로 하는 Advice 코드를 작성합니다.

    ```java
    @Aspect
    @Component
    public class MeasureExecutionTimeAdvice {

    		//@MeasureExecutionTime이 적용된 메서드에만 동작한다.
        @Around("@annotation(com.example.demo.MeasureExecutionTime)")
        public Object measureMethodExecutionTime(ProceedingJoinPoint joinPoint)
    																																throws Throwable {
            long startTime = System.currentTimeMillis();
            Object result = joinPoint.proceed();
            long endTime = System.currentTimeMillis();
            long executionTime = endTime - startTime;
            System.out.println("Execution time of method " +
    						joinPoint.getSignature().getName() + ": " + executionTime + "ms");
            return result;
        }
    }
    ```

    해당 메서드내에서 대상 메서드를 호출하는 코드는 ProceedingJoinPoint 인터페이스의 proceed() 메서드를 호출하여 수행합니다.

    3. 시간 측정을 원하는 대상 메서드를 작성하여 적용합니다.

    ```java
    @Service
    public class MyService {

        @MeasureExecutionTime
        public void doSomething() {
            // 시간 측정이 필요한 비즈니스 로직
        }
    }
    ```


위와 같이 advice를 적용할 타켓을 적용할 지점은 여러 표현식(PointCut)을 사용할수 있습니다.

1. Execution Pointcut 표현식
    - execution(<접근제어자> <반환타입> <패키지이름>.<클래스이름>.<메서드이름>(<파라미터>) throws <예외>)
    - 가장 일반적으로 사용되는 Pointcut 표현식으로, 메서드 실행 시점을 기준으로 대상 메서드를 지정합니다.
2. Bean Pointcut 표현식
    - bean(<빈이름>)
    - 특정 빈에만 어드바이스를 적용하고자 할 때 사용하는 Pointcut 표현식입니다.
3. Annotation Pointcut 표현식
    - @target(<어노테이션타입>),@within(<어노테이션타입>),@annotation(<어노테이션타입>)
    - 특정 어노테이션이 적용된 대상을 지정하여 어드바이스를 적용할 때 사용하는 Pointcut 표현식입니다.
4. Args Pointcut 표현식
    - args(<파라미터타입>)
    - 특정 파라미터 타입을 가지는 대상 메서드를 지정하여 어드바이스를 적용할 때 사용하는 Pointcut 표현식입니다.
5. Combination Pointcut 표현식
    - 여러 Pointcut 표현식을 조합하여 복잡한 대상을 지정할 수 있습니다.
    - &&: AND
    - ||:  OR
    - !:  NOT


### Reference
  - [망나니 개발자 블로그](https://devinside.tistory.com/74)
 akd(https://mangkyu.tistory.com/175)
