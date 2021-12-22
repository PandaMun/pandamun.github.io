---
layout: post
title: 데이터 독립성 레벨(Isolation Levels)이란?
subtitle: 데이터 독립성 레벨이 무엇이고 어떤 단계가 있는지 알아보자
author: Muntaeho(Pandamun)
category: database
comments: true
---


## 독립성 레벨(Isolation Levels)?

---

주어진 트랜젝션이 병행수행되는 트랜젝션들의 부분으로 허용될수 있는 간섭의 정도(degree of interface)로 설명할수 있습니다.

직렬성을 보장하는 Serializable 레벨을 포함하여 아래의 종류들이 존재합니다.

- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable

![데이터베이스 독립성 레벨 단계.png](/img/post/데이터베이스 독립성 레벨 단계.png)

### 1. Serializable

---

트랜젝션의 직렬성을 보장하는 단계로써 트랜젝션이 완료될때까지 해당되는 모든 데이터의 대한 수정 및 입력이 불가능합니다.  

### 2.Repeatable Read

---

트랜젝션이 완료될때까지 해당되는 모든 데이터의 대한 수정이 불가능합니다. 하지만 이 단계에서부터는 phantom read 현상이 발생됩니다.

### 3.Read Commited

---

많은 DBMS가 Default로 채택하고 있는 레벨로써 이 Level 에서부터는 트랜젝션의 작업이 수행되는 동안에는 해당 트랜젝션의 데이터에 Shared Lock이 걸리게 됩니다.

현재레벨에서부터 Non-repeatable read 현상이 발생합니다. 또한 phantom read 현상은 존재합니다.

### 4.Read Uncommited

---

가장 낮은 레벨으로써 데이터를 읽을때 lock을 사용하지 않습니다. 그래서 트랜젝션에서 처리중인, 아직 커밋되지 않은 데이터를 다른 트렌젝션이 읽는것을 허용합니다.

또한 위의 phantom read, Non-repeatable read 현상은 여전히 존재하며 Dirtyread 현상이 발생합니다.

### 낮은 단계의 독립성 레벨을 사용할때 발생하는 현상

---

- Dirty Read(부정 판독)
- Non-repeatable Read(비반복 판독)
- Phantom Read(가상판독)

아래는 각 레벨에서 발생하는 현상을 표로 나타냈습니다.

| ISOLATION LEVEL | DIRTY READ | NON-REPEATABLE READ | PHANTOM READ |
| --- | --- | --- | --- |
| READ UNCOMMITTED | YES | YES | YES |
| READ COMMITTED | NO | YES | YES |
| REPEATABLE READ | NO | NO | YES |
| SERIALIZABLE | NO | NO | NO |

1. Dirty Read(부정판독)

    Dirty Read는 아직 커밋되지 않은 데이터를 다른 트랜젝션에서 읽을수 있도록 허용할때 발생합니다.

    트랜잭션 1이 특정 행의 갱신을 수행하고 난후 트랜젝션 2이 그행을 검색한뒤 원래 트랜잭션 1이 롤백하였을떄 트랜젝션 2는 더이상 존재하지 않는,존재하지 않았던 행을 본케이스가 됩니다.

2. Non-Repeatable Read

    Non-Repeatable Read은 한 트랜젝션내에서 같은 쿼리를 두번수행할때 중간에 다른 트랜젝션이 값을 수정및 삭제할시 두번의 쿼리값이 상이하게 나오는 비 일관성 이 발생하는 현상입니다.

3. phatom read(가상판독)

    phantom read(가상판독) 현상은 다른 트렌젝션에서 동일한 조건을 만족하는 새로운 행을 삽입한다고 했을경우 기존 트랜젝션에 존재하지 않았던 새로운 행이 추가되는 현상이다.

    이런 phantom read를 막기위해서 상위 단계인 Serializable 단계에서는 처리중인 데이터를 얻는데 사용되는 접근 경로(acess path)에 lock를 걸게 됩니다.
