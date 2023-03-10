# Chapter 4 DataBase(데이터베이스)

# Section 4.2 ERD와 정규화 과정

## ERD(Entity Relationship Diagram)

> 💡 ERD(Entity Relationship Diagram)는 데이터베이스의 내의 릴레이션 간의 관계를 정의한 것 입니다.  
> ERD를 이용해 데이터베이스의 구성요소를 명확하게 인지할 수 있고, 기초적인 뼈대를 구성할 수 있게 됩니다.
>
<img width="1246" alt="스크린샷 2023-02-11 오후 6 07 42" src="https://user-images.githubusercontent.com/49636918/218250089-1ac0c450-7ab8-4c09-b8af-9ae4c4823c3a.png">

ERD는 구현할 Application의 요구 사항을 바탕으로 작성되고, 이를 바탕으로 데이터베이스를 구축하게 됩니다.  
하지만, ERD의 경우 이름과 같이 **엔티티**들의 **관계**를 **다이어그램**으로 표현한 것이므로 관계형 데이터베이스를 나타내기에 유용하지만, NoSQL과 같은 비정형 데이터를 나타내기에는 부족한 점이
있습니다.

## 정규화 (Normalization)

> 💡 정규화 (Normalization)는 잘못된 릴레이션 간의 관계로 인해 데이터베이스의 이상 현상이 발생하지 않도록 하거나, 효율적으로 사용하기 위해 릴레이션을 여러 개로 분리하는 것을 뜻합니다.
>
정규화를 하는 것이 무조건 장점이라고 볼 순 없습니다. 정규화를 하게되었을 경우 여러개의 테이블로 데이터들이 분산되는 상황이 많이 발생하게 되는데, 이때 특정 데이터를 조회하기 위해 하나의 쿼리에서 조인을 하게되어
성능이 저하될 수 있습니다.

### 이상 현상(Anomaly)

불필요한 데이터의 중복 현상으로 릴레이션에 대한 데이터 삽입, 수정 삭제를 할 때 발생하는 데이터 불일치 현상입니다.

### 정규화의 과정

정규화 과정은 정규형 원칙을 기반으로 정규형으로 만들어 가는 과정이고, 정규화된 정도는 아래와 같은 순서로 표현하게됩니다.

- 제1정규형
- 제2정규형
- 제3정규형
- BCNF(보이스/코드)정규형
- 제4정규형
- 제5정규형

### 제1정규형

> 💡 제1정규형은 릴레이션의 모든 도메인이 더 이상 분해될 수 없는 원자 값(Atomic Value)만으로 구성되어야 한다는 것을 뜻합니다.
>
하나의 기본키에 대해 여러개의 값을 가지는 반복 집합이 있을 경우, 해당하는 반복 집합을 제거해야할 것 입니다.

### 제2정규형

> 💡 제2정규형은 현재 릴레이션이 제1정규형인 상태이고, 부분 함수의 종속성을 제거한 상태를 나타냅니다.
>

- 부분 함수의 종속성 제거
    - 기본키가 아닌 모든 속성이 기본키에 완전 종속적인 것을 뜻 합니다.

### 제3정규형

> 💡 제3정규형은 현재 릴레이션이 제2정규형인 상태이고, 기본키를 제외한 모든 속성이 이행적 함수 종속상태(transitive FD)를 만족하지 않는 상태를 뜻합니다.
>

- 이행정 함수 종속
    - A -> B와 B -> C가 존재한다면, A -> C가 성립하게되는데, 이때 C 집합은 A에 이행적으로 함수 종속이 된 상태라고 뜻합니다.

### BCNF(보이스/코드)정규형

> 💡 BCNF(보이스/코드)정규형은 현재 릴레이션이 제3정규형인 상태이고, 함수 종속 관계를 제거하여, 모든 결정자가 후보키인 상태를 뜻 합니다.
>
