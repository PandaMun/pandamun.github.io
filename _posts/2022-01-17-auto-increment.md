---
layout: post
title: AUTO_INCREMENT 기능과 초기화 및 재정렬
subtitle: AUTO_INCREMENT 기능과 사용법에 대해 알아보자
category: database
---


### AUTO_INCREMENT란

auto_increment는 테이블의 컬럼에 적용이 가능하며 자동 증가합니다.

기본으로 auto_increment 속성을 지닌 컬럼은 INT 형으로 작성해야합니다.

> 문자열 + 숫자로 된 값에서 숫자만 증가하는 컬럼을 생성하고 싶다면 별도의 테이블과 트리거를 사용하여 적용할수 있습니다.                                                [참고자료](https://stackoverflow.com/questions/17893988/how-to-make-mysql-table-primary-key-auto-increment-with-some-prefix)입니다.
>

AUTO_INCREMENT 컬럼에 값을 넣지 않아도 자동적으로 시퀀스 넘버가 증가하지만 만약 AUTO_INCREMENT 속성 컬럼에 다른값(최댓값)을 넣을때 해당 컬럼의 시퀀스 넘버는 자동으로 해당 컬럼의 최대값으로 수정됩니다.

테이블의 기본키를 선택하는 전략은 2가지가있습니다.

- 자연키
    - 비즈니스에 의미가 있는키(SSN, Email)
- 대리키
    - 비즈니스와 관련 없이 임의로 만들어진키(Oracle sequence, auto_increment, 키생성 테이블)

AUTO_INCREMENT는 이중 두번째 대리키 전략을 선택할때 사용되며 사용시 설정된 컬럼에 대해 레코드 삽입시 기본키가 자동으로 증가합니다.

사용 방법은 아래와 같습니다.

### AUTO_INCREMENT 속성값 추가

```sql
//MySQL 구문
CREATE TABLE USER(
	id int NOT NULL auto_increment,
	email varchar(255) NOT NULL,
	phone_number varchar(255) NOT NULL,
	PRIMARY KEY(id)
)
```

### AUTO_INCREMENT 초기화 및 순서대로 정렬

auto_increment 속성을 부여하여 컬럼값을 자동으로 증가시켰습니다.

하지만 데이터가 중간에 삭제가 된다면 한번 증가된 값은 다시 조정되지 않습니다.

그래서 이러한 컬럼 값을 초기화 하기위한 방법은 아래와 같습니다.

이때 (새로 시작할 시퀀스 번호)이 AUTO_INCREMENT의 값중 가장 큰 값보다 커야합니다.

```sql
ALTER TABLE (테이블명) AUTO_INCREMENT = (새로 시작할 시퀀스 번호);
```

현재 테이블의 상태입니다.

![AUTO_INCREMENT_01.png](/img/post/AUTO_INCREMENT_01.png)

여기서 새로 시작할 시퀀스 넘버을 위와 같이 6으로 설정한후 INSERT을 하면 아래와 같습니다.

![AUTO_INCREMENT_02.png](/img/post/AUTO_INCREMENT_02.png)

또한 아래와 같이 AUTO_INCREMENT로 설정된 컬럼이 레코드 삭제등과 같은 이유로 복잡해질때 순서대로 정렬 또한 가능합니다.

![AUTO_INCREMENT_03.png](/img/post/AUTO_INCREMENT_03.png)

아래와 @CNT를 0으로 설정한후 UPDATE문으로 컬럼을 재정렬합니다.

```sql
SET @CNT = 0;
UPDATE USER SET id = @CNT:=@CNT+1;
```

아래와 같이 순서대로 정렬된것을 볼수 있습니다.

![AUTO_INCREMENT_04.png](/img/post/AUTO_INCREMENT_04.png)

다음으로 AUTO_INCREMENT 초기화를 하여 새로 시작할 시퀀스 번호로 컬럼 값을 지정할수 있습니다.

```sql
ALTER TABLE (테이블명) AUTO_INCREMENT = (새로 시작할 시퀀스 번호);
```
