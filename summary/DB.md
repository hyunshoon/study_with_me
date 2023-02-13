
DB: 구조화된 형식으로 저장되고 데이터를 효율적으로 검색하고 조작할 수 있도록 설계된 조직화된 데이터 모음
DBMS: DB를 정의하고 사용하기 위한 시스템

------
## 데이터베이스 특징, 성능

### 데이터베이스의 특징

1. 데이터의 독립성
- 물리적 독립성: 데이터베이스와 데이터의 변경한다고 해서 애플리케이션을 변경할 필요는 없다
- 논리적 독립성: 데이터베이스는 논리적인 구조로 다양한 애플리케이션의 논리적 요구를 만족
2. 데이터의 무결성
- 다양한 경로를 통해 잘못된 데이터가 발생하는 경우의 수를 방지하는 기능. 데이터의 유효성 검사를 통해 무결성을 구현한다.
3. 데이터의 보안성
- 계정 관리 또는 접근 권한 설정 등으로 승인된 사용자만 접근할 수 있게 한다.
4. 데이터의 정합성
- 연관된 정보를 논리적인 구조로 관리함으로써 하나의 데이터만 변경했을 경우 발생할 수 있는 데이터의 불일치성 배제
5. 데이터 중복 최소화

### 데이터베이스의 성능

DB 성능 이슈는 디스크 I/O를 어떻게 줄이느냐에서 시작된다. 순차I/O가 랜덤I/O 보다 빠른데, 랜덤I/O 자체를 줄여주는 것이 데이터베이스 쿼리 튜닝이다.

### Schema

데이터 모델을 바탕으로 database의 구조를 기술 한 것

스키마는 데이터베이스를 설계할 때 정해지며 한 번 정해진 후에는 자주 바뀌지 않는다.

### State

데이터베이스에 있는 실제 데이터는 꽤 자주 바뀔 수 있다. 

특정 시점에 데이터베이스에 있는 데이터를 db state 혹은 snapshop이라고 한다.

----
## Index

인덱스는 색인이다. 조회를 빠르게 해준다.

### 사용 목적

인덱스가 없다면, 데이터를 조회할 때 O(N)의 시간이 걸린다. 인덱스가 있다면 O(logN)(B-tree)이 걸린다. 즉, 조건을 만족하는 튜플을 빠르게 조회하기 위해 사용한다.
정렬과 그룹핑도 빠르게 해준다.

### 인덱스 구현

#### B Tree 인덱스 알고리즘

일반적인 방법. 이진 탐색 트리 기반 자료구조.

#### Hash 인덱스 알고리즘

해시테이블을 사용하여 구현

- 조회 시 O(1)
- rehasing에 대한 부담
- **equality 비교만 가능, range 비교 불가능**
- multicolumn index의 경우 전체 attributes에 대한 조회만 가능

### 사용 예시

WHERE a=7 AND b=95 를 실행할 때 a=7 를 찾는 시간은 O(logN)이다. 하지만 a=7 AND b=95를 찾을 때는 a=7인 인덱스를 full scan 해야한다.

이를 빠르게 해주는 것이 멀티 인덱스 INDEX(a,b) 이다. 이렇게 하면 a를 기준으로 정렬하고 다음으로 b를 기준으로 정렬하기 때문에 b조건을 찾을 때도 Binary Search를 사용한다.

사용하는 query에 맞춰 적절하게 index를 생성해야 query가 빠르게 처리될 수 있다.

### Index 단점

인덱스를 생성하는 것은 따로 인덱스 테이블을 만드는 것

1. table을 수정할 때 마다 index 테이블도 수정해야 한다 -> 오버헤드 발생
2. 추가적인 저장 공간 차지

결론: 불필요한 index는 만들면 안된다.

### 기타

Covering Index: 조회하려는 attributes 를 index가 모두 cover할 때. -> 조회 성능이 더 빨라짐

Full scan이 더 좋은 경우
- table에 데이터가 조금 있을 때
- 조회하려는 데이터가 테이블의 상당 부분을 차지할 때 (예: 성별)

이미 데이터가 몇 백 만건 이상 있는 테이블에 인덱스를 생성하는 경우 시간이 몇 분 이상 소요될 수 있고 DB 성능에 안좋은 영향을 미칠 수 있다.

----
# DB 정규화

RDB에서 중복을 최소화하기 위해 데이터를 구조화하는 작업. 

데이터 중복, insertion, update, deletion anomaly를 최소화하기 위해 일련의 normal forms(NF)에 따라 relational DB를 구성하는 과정

### Normal forms

정규화 되기 위해 준수해야하는 rule

DB 정규화는 순차적으로 진행하며 normal form을 만족하지 못하면 만족하도록 테이블 구조를 조정한다. 앞 단계를 만족해야 다음 단계로 진행할 수 있다.

### DB 정규화 과정



----
# Transaction

무결성을 유지하며 상태를 변화시키는 기능을 수행하는 하나 이상의 작업 시퀀스. 트랜잭션은 ACID라고 부르는 4가지 규칙을 만족해야한다.

## 설명 예시

```
예를 들어 한 은행 계좌에서 다른 은행 계좌로 돈을 이체하는 거래를 생각해 보십시오. 또 다른. 거래에는 다음 단계가 포함될 수 있습니다.

원본 계정의 잔액을 검색합니다.
원본 계정에 송금을 완료하기에 충분한 금액이 있는지 확인합니다.원본 계좌에서 이체 금액을 공제합니다.
대상 계좌에 이체 금액을 추가합니다.
이러한 단계 중 하나라도 실패하면 데이터베이스가 일관된 상태로 유지되도록 전체 트랜잭션을 롤백하거나 실행 취소해야 합니다.
```
이 중 한 단계라도 에러가 발생하면 DB가 일관된 상태를 유지되도록 전체 트랜잭션을 롤백하거나 실행 취소해야한다.


## ACID

----
# RDB VS NoSQL

RDB는 엄격하게 정의된 Schema를 요구하는 table 기반 데이터 구조를 갖는다.
NoSQL은 table 형식이 아닌 비정형 데이터를 저장할 수 있고 유연한 데이터 구조를 가진다.

RDB는 데이터 중복이 없기 때문에 데이터 update가 많을 때 유리하다. NoSQL은 데이터 중복으로 인해 update 시 많은 컬렉션에서 수정이 필요하다. 따라서 update가 적고 조회가 많을 때 유리하다.

## NoSQL 

빅데이터 시대가 도래하면서 데이터의 크기가 훨씬 방대해지고, 관계형으로 처리하기 어려운 비정형 데이터들도 많아졌다. 이를 해결하기위해 등장한 것이 Key-value Storage System을 가지는 NoSQL 이다.

## 차이점

가장 큰 차이는 엄격한 스키마 구조를 가지느냐 가지지 않느냐이다.

RDB가 엄격한 Schema를 가짐으로써 얻는 장점

1. 명확한 데이터 구조 보장
2. 데이터 중복 X(무결성) -> 데이터 update 용이

단점

1. 데이터 구조가 유연하지 못하므로 Scale Out이 어렵다. 비용이 많이드는 Scale up을 사용함.
2. 시스템이 커질수록 Join 문이 복잡해진다.



Reference
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4