# Chapter 3 운영체제(OS, Operating System)

# Section 3.3 프로세스와 스레드

## 📌 프로세스의 메모리 구조

프로세스의 메모리구조는 아래와 같습니다.

- `스택(Stack)`
- `힙(Heap)`
- `데이터 영역(BSS segment, Data segment)`
- `코드 영역(Code segment)`

### 1️⃣ 스택(Stack)

> 💡 스택(Stack)은 지역 변수, 매개 변수, 함수가 저장되고 컴파일 시에 크기가 결정되며 **동적**인 특징을 가집니다.
>

스택 영역은 함수가 재귀적으로 호출되어 동적으로 크기가 늘어나거나 줄어들 수 있습니다. 이때 힙과 스택 메모리 영역이 겹치지 않게 힙과 스택 사이의 공간을 비워 놓습니다.

### 2️⃣ 힙(Heap)

> 💡 힙(Heap)은 동적으로 할당된 메모리 영역으로, 프로그램이 실행되는 동안에 동적으로 메모리를 할당하고 해제할 수 있습니다.
>

힙은 동적 할당할 때 사용되고 런타임 시 크기가 결정됩니다.

### 3️⃣ 데이터 영역(BSS segment, Data segment)

> 💡 데이터 영역(BSS segment, Data segment)은 전역 변수, 정적 변수가 저장되고, 정적인 특징을 가지는 프로그램이 종료되면 사라지는 변수가 들어있는 영역입니다.
>

- BSS 영역(BSS segment)
    - 초기화 되지 않은 변수가 0으로 초기화 되어 저장되는 영역입니다.
- Data 영역(Data segment)
    - 0이 아닌 다른 값으로 할당된 변수들이 저장되는 영역입니다.

### 4️⃣ 코드 영역(Code segment)

> 💡 코드 영역(Code segment)은 프로그램에 내장되어 있는 소스 코드가 들어가는 영역입니다.
> 수정 불가능한 기계어로 저장되어 있고, 정적인 특징을 가집니다.
>
