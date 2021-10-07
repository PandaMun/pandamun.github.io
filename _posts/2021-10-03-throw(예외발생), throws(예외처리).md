---
layout: post
title: throw(예외발생), throws(예외처리)
subtitle: throws 그리고 throw 키워드에 대해 알아보자
author: Muntaeho(Pandamun)
category: java
comments: true
---

- throw

throw는 예외를 강제로 발생시킬때 사용합니다. 예를 들면

~~~java
public class test{
	public static void main(String[] args){
			try{
					throw new Exception(); // 예외 발생
				}catch (Exception e){ // 예외 처리 코드
							System.out.println(e);
				}
		}
}
~~~

<img class="post_image" src="{{ site.baseurl }}/img/post/Exception_결과.png" width="420"/>


위의 코드는 메인함수를 들어갈때 throw을 써서 생성한 Exception객체를 강제로 발생시켰습니다.

Exception이 강제로 발생하였기 때문에 위와 같이 catch문에서 발생한 예외을 출력하게 됩니다.

- throws

throws도 예외를 처리할때 사용하는데요

throws는 발생한 예외를 상위 메소드로 던진다고 생각하시면 될거같습니다.

메소드에서 처리하지 않은 예외를 호출한곳으로 떠넘기는 역활입니다. 예를 들면

~~~java
class Test {
    public void test(String a) throws NumberFormatException{
            int sum = Integer.parseInt(a);
    }
}
public class Practice {

	public static void main(String[] args) {

		Test test = new Test();
		try {
            test.test("A"); // 메소드 호출 , 호출한 곳에서 예외 처리
     }catch (NumberFormatException e) { // 예외 처리 코드
                System.out.println("숫자를 입력하지 않았습니다.");
     }
  }
}
~~~

위의 코드는 Test클래스를 별도로 만들어 test 메소드를 작성하였고 throws NumberFormatException을 달았으며 Int형 변수 sum을 생성후 전달받는 문자 a를 parseInt()로 숫자로 변경하여 저장하게 만들었습니다.

메인 메소드에서 test 객체의 test메소드를 호출하여 문자 "A"를 전달했습니다. 이렇게 된다면 A는 숫자로 형변환이 되지 않기 때문에 NumberFormatException 예외가 발생하게 됩니다. 여기서 발생된 예외는 catch블록에서 처리하게됩니다.

위와 같이 throws 키워드가 붙어있는 메소드는 try블록내에서 호출되어야합니다. 그래야 catch블록에서 떠넘겨 받은 예외를 처리할수 있습니다. 혹은 throws 키워드를 다시 사용하여 예외를 다시 떠넘겨야 합니다. 그렇지 않는다면 컴파일 에러가 발생하게 됩니다.
