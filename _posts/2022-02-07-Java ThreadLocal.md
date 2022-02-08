---
layout: post
title: (JAVA) ThreadLocal
subtitle: 자바의 ThreadLocal 대해 알아보자
category: java
---

ThreadLocal의 개념을 정리하였습니다.

일반적으로 객체는 Heap이나 Stack 메모리 영역에 배치하며 Heap 영역에 배치하였을때 프로세스내의 모든 Thread에서 접근 가능하고 Thread내에 Stack영역에 배치하였을때 Thread 간 공유가 되지 않는다고 알고 있습니다.

## ThreadLocal

ThreadLocal은 자바의 Class중 하나이며 thread-local 변수들을 제공합니다.
