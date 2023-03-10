# Chapter 4 DataBase(데이터베이스)

# Section 4.5 인덱스

## 📌 인덱스(Index)

> 💡 인덱스(Index)는 데이터베이스의 테이블에 대해 미리 생성해 놓은 데이터의 위치 정보를 가지고 있는 특별한 자료구조입니다.
> 인덱스는 단순히 책의 특정한 부분을 빠르게 찾게해주는 책갈피의 역할을 하게됩니다.
>
데이터베이스에서 인덱스를 구현하기위해, 또 다른 테이블을 하나 더 생성하여 관리하게 됩니다. 인덱스를 생성하게 될수록 데이터베이스가 관리해야 될 데이터는 크게되지만, 데이터를 조회하는 효율을 월등히 좋아지게 되는
장점이 있습니다.

## 📌 인덱스 최적화 기법

### 1. 인덱스는 비용(Cost)입니다.

인덱스는 리스트 -> 컬렉션 순으로 탐색하게 됩니다. 그렇기 때문에, 1번이 아닌 2번의 조회로 데이터를 탐색하게 됩니다.  
그리고, 인덱스는 데이터를 저장하는 공간이기 때문에, 데이터를 저장하는 공간만큼의 비용이 발생하게 됩니다. 데이터를 저장하거나, 수정하게 되었을 때 여러개의 인덱스를 관리하는 모든 테이블에 데이터를 반영해야하는
문제점이 발생하게 됩니다.

### 2. 항상 테스팅하라.

인덱스의 최적화 기법은 현재 서비스 특징에 따라 달라지게 됩니다.  
해당하는 데이터를 사용하는 **사용자가 적은** 서비스와 MAU가 **몇백만**을 넘는 서비스는 관리의 중점이 달라지게 되는것입니다.

### 3. 복잡한 인덱스는 같음, 정렬, 다중 값, 카디널리티 순입니다.

인덱스를 생성할 때 같음, 정렬, 다중 값, 카디널리티 순으로 생성해야합니다.

1. 특정 값과 같음을 비교하는 `equal`이라는 쿼리가 존재한다면, 제일 먼저 인덱스를 생성합니다.
2. 정렬에 쓰는 필드라면 그 다음 인덱스로 생성합니다.
3. 다중 값을 출력해야 하는 필드, 즉 쿼리가 `>` 또는 `<` 등 많은 데이터를 출력해야하는 쿼리라면 나중에 인덱스를 생성합니다.
4. Unique(고유한)값의 정도를 카디널리티라고 합니다. 해당하는 카디너리티가 높은 순서를 기반으로 인덱스를 생성해야합니다.