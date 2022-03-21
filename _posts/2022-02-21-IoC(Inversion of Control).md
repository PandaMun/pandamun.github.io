---
layout: post
title: IoC(Inversion of Control), 제어의 역전
subtitle: IoC(Inversion of Control)에 대하여 알아보자
category: knowledge
---

## IoC(Inversion of Control)

제어의 역전 - IoC는 프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름제어를 받게 되는 소프트웨어 디자인 패턴을 말합니다. 줄여서 IoC (Inversion of Control)이라고 부른다. 전통적인 프로그래밍에서 흐름은 프로그래머가 작성한 프로그램이 외부 라이브러리의 코드를 호출해 이용한다. 하지만 제어 반전이 적용된 구조에서는 외부 라이브러리의 코드가 프로그래머가 작성한 코드를 호출한다.      - wikipedia -

라이브러리를 사용할때는 자신의 코드가 라이브러리 코드를 호출하지만 프레임워크를 사용할때에는 프레임 워크가 자신의 코드를 호출하는 점에서 제어가 역전되었다. 라고 볼수있다

## DI

IoC에 대해 정리하면서 빠지지 말아야할 내용중 하나가 DI입니다.  의존성을 관리하는 자체를 역전하였기 때문에 DI(Dependency Injection)을 일종의 IoC라고 볼수 있습니다.

DI는 클래스간의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는것을 말하며  DI (Denpendcy Injection - 의존성 주입)을 통하여 모듈간의 결합도가 낮아지고 유연성이 높아집니다.

### DI의 유형

마틴 파울러의 글을 보면 세가지의 의존성 주입 패턴을 소개하였습니다.

1. 생성자 주입 : 필요한 의존성을 모두 포함하는 클래스의 생성자를 만들고 그 생성자를 통해 의존성을 주입한다.
2. Setter를 통한 주입 : 의존성을 입력받는 Setter 메소드를 만들고 이를 통해 의존성을 주입한다.
3. Interface를 통한 주입 : 의존성을 주입하는 함수를 포함한 인터페이스를 작성하여 이 인터페이스를 구현함으로써 실행시에 이를 통하여 의존성을 주입한다

## Reference

- [https://martinfowler.com/articles/injection.html](https://martinfowler.com/articles/injection.html)
- [https://ko.wikipedia.org/wiki/제어_반전](https://ko.wikipedia.org/wiki/%EC%A0%9C%EC%96%B4_%EB%B0%98%EC%A0%84)
- [https://homesi.tistory.com/entry/IoCInversion-of-Control-제어의-역전](https://homesi.tistory.com/entry/IoCInversion-of-Control-%EC%A0%9C%EC%96%B4%EC%9D%98-%EC%97%AD%EC%A0%84)
