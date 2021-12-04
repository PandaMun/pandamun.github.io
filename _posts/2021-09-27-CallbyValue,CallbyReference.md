---
layout: post
title: Call-by-Value, Call-by-Reference의 개념
subtitle: 값에 의한 호출과 참조에 의한 호출을 이해해보자
author: Muntaeho(Pandamun)
category: knowledge
comments: true
---

Call by Value 그리고 Call by Reference에 대한 차이는 현재도 많이 물어보는 회사 면접 단골 문제입니다.
Call by Value와 Call by Reference가 무엇인지 프로그래밍 언어에 따라 어떤 차이가 있는지 정리해보려고 합니다.

## Call by Value

- 함수가 호출될때 메모리에는 함수를 위한 임시 공간이 생성되는데요. 이때 Call by Value(값에 의한 호출)은 전달되는 변수의 값을 복사하여 함수의 인자로 전달하는데요 복사된 인자는 지역 변수의 특징을 지니며 이로 인해 함수 안의 인자 값이 변경되어도 외부의 인자 값은 변경되지 않습니다.

C++ 언어로 예를 들어 보겠습니다.

```cpp
#include <iostream>
using namespace std;

void swap(int num1,int num2){
		int temp = num1;
		num1 = num2;
    num2 = temp;

}
int main(){
		int a = 1, b = 2;
		swap(a,b);
		cout << "a : " << a << "," << "b : " << b << "\n";
		return 0;
}

```

위와 같은 코드를 실행 시키게되면 아래와 같은 값이 나오게 되는데요.

a : 1, b : 2

![CallbyValue.png](img/CallbyValue.png)

값이 swap되지 않고 그대로 출력되는 이유는 swap함수에서 값을 받아온후에 처리를 한후에 아무것도 넘기지 않기 때문입니다.

변수를 주소 또는 포인터등를 사용해서 가져오지 않았기 때문에 실질적으로 값이 교체가 되지 않고 swap함수내에서만 처리가 됩니다.

## Call by Reference

- Call by Reference(참조에 의한 호출)은 함수 호출시 인자로 전달되는 변수의 레퍼런스를 전달합니다.              즉 Call by Reference는 값대신 주소값을 전달하는 방식입니다.

추가적으로 C언어는 엄밀히 말하면 Call by Reference가 존재 하지 않습니다.

C언어의 방식은 주소값 자체를 복사해서 넘겨주기 때문에 Call by Reference 가 아니고 Call by Value라고 이해할수 있으며 이러한 방식을 C언어에서는 Call by adress 방식이라고 합니다.

```cpp
#include <iostream>
using namespace std;

void swap(int &num1,int &num2){
		int temp = num1;
		num1 = num2;
    num2 = temp;

}
int main(){
		int a = 1, b = 2;
		swap(a,b);
		cout << "a : " << a << ", " << "b : " << b << "\n";
		return 0;
}

```

![CallbyReference.png](img/CallbyReference.png)

위의 결과물을 보면 Call by reference는 값 대신 주소값을 전달하는 방식이기 때문에 함수가 호출되면 값이 swap 되는걸 확인할수 있습니다.
