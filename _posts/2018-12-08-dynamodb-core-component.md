---
layout: post
title:  "Dynamo DB Core Component"
date:   2018-12-08
---

<sub>이 문서는 https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/HowItWorks.html 를 요약한 내용아며, rdb 경험자 입장에서 작성 됨.</sub>

## Core Component 및 명칭
***

### table
우리가 알던 table과 동일한 개념. User, Car 등.

### items
table row 라고 이해하면 됨. no limit.

### Attributes
table field 라고 이해하면 됨.

### Primary Key
각 items 의 유니크한 식별자.

#### Partition Key
Primary Key의 일종이며 단일 키

#### Partition Key + Sort key
Primary Key의 일종이며 복합 키로 구성 됨

<br />

> 여기까지 내용은 일반 rdb 와 크게 다르진 않음.  
> 단, 복합키의 경우 딱 2개의 키만 사용가능하고, 명칭과 용도가 정해져 있다는게 특징.

<br />

### Partition 과 Partion Key

#### Partiion
데이터가 저장되는 물리적 장소  
테이블 하나당 여러개의 파티션을 사용하며 가능한 고루 분산되도록 저장하는게 특징.  
파티션이 부족하면 추가로 늘리는 등 자동으로 관리 됨. (늘릴 때 기존 데이터는 다시 분산되는지, 해시 함수는 변경되는지 아직 미확인.)

#### Partition Key
내부적인 해시함수 param 으로 사용되며, 키 값에 따라 저장될 파티션을 결정함.
![그림](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/images/HowItWorksPartitionKey.png)

### Secondary Indexes
인덱스를 사용하여, primary key 제외한 다른 대체 키 쿼리 가능.  
모든 인덱스는 테이블에 속하며, 이 테이블을 base table 이라 부름.  
base table 의 값이 변하면 속한 모든 인덱스의 값도 변함.  
<sub>https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/SecondaryIndexes.html 를 더 읽어볼 것.</sub> 

#### Global secondary index
최대 5개까지 생성 가능.  
인덱스가 테이블의 Partition key, sort Key 와 다를 수 있음.

#### Local secondary index
최대 5개까지 생성 가능.  
인덱스가 테이블의 Partion key 와 동일. sort key 는 다름.

### DynamoDB Streams
일단 간단히... 테이블의 데이터가 변화하면 이를 캡쳐하는 일종의 저장소.  
optional 한 기능.  
table name, event timestamp, meta data 등으로 구성 됨.  
24시간 동안 유효.  
aws lambda 와 함께 기존 rdb 의 트리거를 구현 가능.  
(이 말은 쉽게 이해가 됨. 스트림에 기록된 레코드를 중에 원하는 기능을 람다로 구현하면 될듯)  
