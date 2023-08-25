---
layout: post
title: 페이지 교체 알고리즘
subtitle: 페이지 교체 알고리즘의 개념과 종류
category: os
---

운영체제 수업시간에 배운내용이지만 기억이 잘 나지 않아서 한번더 정리해봤습니다.

페이지 교체 알고리즘이란?

페이징 기법으로 메모리를 관리하는 운영체제에서 필요한 페이지가 주기억장치에 적재되지 않아 페이지 부재가 발생할때 새로운 페이지를 할당하기위하여 현재 할당된 페이지중 어떤 페이지와 교체할지 결정하는 방법 알고리즘입니다.

페이지 교체 알고리즘의 종류

- FIFO(First in First out) ,선입선출, 현재 페이지중 가장 먼저 들어와 있었던, 가장 오래된 페이지 교체
- LRU (Least Recently Used), 가장 오랫동안 사용되지 않은 페이지 교체
- LFU (Least Frequently Used) : 사용된 횟수, 사용빈도 가 가장 적은 페이지 교체
- OPT(Optimal) : 앞으로 가장 오랫동안 사용되지 않을 페이지 교체
- MFU(Most Frequently Used) : 사용된 횟수가 가장 많은 페이지 교체
- NRU(Not Recently Used) : 최근에 가장 오랫동안 사용되지 않은 페이지 교체

### FIFO(First in First out)

![FIFO.png](/img/post/FIFO.png)

FIFO 알고리즘은 선입 선출, 먼저 들어온 페이지를 먼저 내보냅니다.

위의 사진처럼 6번페이지를 넣을시에 가장 먼저 들어온 1번페이지를 내보내며 그이후에 마찬가지로 먼저 들어온 순서대로 내보냅니다.

- 장점
    - 이해가 쉽고 구현이 간단합니다.
- 단점
    - 언제나 좋은 성능을 내지 않습니다.

        ![Belady's Anomaly.png](/img/post/Beladys_Anomaly.png)

        위의 그래프를 보았을때 프레임의 개수가 3개일때보다 4개일때가 더 많은 페이지 부재가 발생하였습니다.

        일반적으로 많은 프레임을 가지게 되면 성능과 효율이 올라갈것이라고 예상하지만 그렇지 않습니다. 이러한 현상을 벨라디의 변칙(Belady’s Anomaly)라고 합니다.


### OPT(Optimal)

![optimal.png](/img/post/optimal.png)

최적(Optimal) 알고리즘은 앞으로 가장 오랫동안 사용되지 않을 페이지를 교체 하는 알고리즘으로써 가장 최고의 효율과 성능을 내는 알고리즘입니다.

- 장점
    - 페이지 교체수가 적기때문에 성능이 뛰어납니다.
- 단점
    - 앞으로 사용할 페이지(미래)를 알고있어야 하기 때문에 실제 활용이 불가능합니다.

### LRU(Least Recently Used)

![LRU.png](/img/post/LRU.png)

LRU 알고리즘은 가장 오랫동안 사용되지 않은 페이지를 교체하는 알고리즘으로써 최적 알고리즘을 실제로 구현하기 어려우므로 비슷한 방식을 사용하여 효과를 낼수 있도록 하였습니다. 앞으로 사용할 페이지를 알수없기 때문에 가장 오랫동안 사용되지 않은 페이지는 앞으로 사용할 확률이 적을것이다 란 예측으로 만들어진 알고리즘입니다.

### LFU(Least Frequently Used)

![LFU.png](/img/post/LFU.png)

LFU 알고리즘은 사용된 횟수가 가장 적은 페이지를 교체하는 알고리즘으로써 사용된 횟수가 같아 교체할 페이지가 여러개 일경우 LRU 알고리즘과 같이 가장 오래동안 사용되지 않은 페이지를 선택하여 교체합니다.

- 단점
    - 초반에 한페이지를 많이 사용하다가 나중에 사용하지 않는 경우 사용한 횟수가 이미 높기 때문에 사용하지 않더라도 메모리에 남아있기 때문에 문제가 될수 있습니다.

### MFU(Most Frequently Used)

![MFU.png](/img/post/MFU.png)

MFU 알고리즘은 사용할 횟수가 많은 페이지일수록 앞으로 사용할 확률이 적다는 예측으로 LFU 알고리즘과 반대로 참조횟수가 가장 많은 페이지를 교체하는 알고리즘입니다.

### NRU(Not Recently Used),NUR(Not Used Recently),

클럭 알고리즘(NRU)은 LRU와 비슷한 알고리즘으로써 마찬가지로 최근에 사용하지 않은 페이지를 교체한다.

LRU 알고리즘은 가장 오래전에 참조된 페이지를 교체하지만 클럭 알고리즘은 오랫동안 사용되지 않은 페이지중 하나를 교체합니다

가장 최근에 참조되지 않은 페이지를 교체한다는 점에서 비슷하지만 교체되는 페이지의 참조시점이 가장 오래되었다는것을 보장하지 않습니다.

![NRU.png](/img/post/NRU.png)

기본적으로 동작방식은 아래와 같습니다.

참조비트와 변형비트 두개의 비트를 사용합니다.

프레임내에서 페이지가 참조되거나 변형될때 각 비트는 1로 설정되며 참조되거나 변형되지 않았다면 각 비트를 0으로 교체합니다.

또 다시 한바퀴를 지났을때 비트의 순서와 상관없이 큰 값을 기준으로  0인 경우 교체대상이 되는 알고리즘입니다.

## Reference

- [https://medium.com/pocs/페이지-교체-page-replacement-알고리즘-650d58ae266b](https://medium.com/pocs/%ED%8E%98%EC%9D%B4%EC%A7%80-%EA%B5%90%EC%B2%B4-page-replacement-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-650d58ae266b)
- [https://eunhyejung.github.io/os/2018/07/24/operatingsystem-study15.html](https://eunhyejung.github.io/os/2018/07/24/operatingsystem-study15.html)
