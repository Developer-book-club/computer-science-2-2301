# Chapter 3 운영체제(OS, Operating System)

# Section 3.3 프로세스와 스레드

## 📌 프로세스의 상태

### 1️⃣ 생성 상태(Create)

> 💡 생성 상태(Create)는 프로세스가 생성된 상태이며 `fork()`, `exec()` 함수를 통해 생성됩니다.
>

- `fork()`: 부모 프로세스가 자식 프로세스를 생성하는 함수입니다.
- `exec()`: 새로운 프로세스를 생성하는 함수입니다.

### 2️⃣ 대기 상태(Ready)

> 💡 대기 상태(Ready)는 메모리 공간이 할당되어 CPU 스케줄러로부터 CPU의 소유권을 넘어오기를 기다리는 상태입니다.
>

### 3️⃣ 대기 중단 상태(Ready Suspended)

> 💡 대기 중단 상태(Ready Suspended)는 메모리 부족으로 일시 중단된 상태입니다.
>

### 4️⃣ 실행 상태(Running)

> 💡 실행 상태(Running)는 CPU 소유권과 메모리 공간을 할당받아 인스트럭션(Instruction)을 수행하고 있는 상태입니다.
>

- 인스트럭션(`Instruction`): 컴퓨터에게 일을 시키는 단위이며, 기계어로 이루어져 있고 CPU가 실행할 수 있는 최소한의 명령어를 뜻합니다.

### 5️⃣ 중단 상태(Blocked)

> 💡 중단 상태(Blocked)는 특정 이벤트가 발생한 이후 기다리며 프로세스가 차단된 상태입니다.
>

- I/O 작업이나 시스템 호출이 발생한 경우, 프로세스는 중단 상태가 됩니다.
    - ex) 프린트 인쇄 시 프로세스가 일시적으로 멈추는 상태와 동일한 상태입니다.

### 6️⃣ 일시 중단 상태(Blocked Suspended)

> 💡 일시 중단 상태(Blocked Suspended)는 중단된 상태에서 프로세스가 실행되려할 때, 메모리가 부족하여 일시 중단된 상태입니다.
>

### 7️⃣ 종료 상태(Terminated)

> 💡 종료 상태(Terminated)는 메모리와 CPU 소유권을 모두 반환한 상태입니다.
>

종료 상태는 일반적으로 정상적인 종료와 비자발적인 종료 2가지가 존재합니다.

- 정상적인 종료(Normal Termination): 프로세스가 모든 작업을 마치고 종료되는 경우입니다.
- 비정상적인 종료(Abnormal Termination): 프로세스가 예기치 않은 이유로 종료되는 경우입니다.
    - 부모 프로세스가 자식 프로세스를 강제 종료(abort)시키는 경우
    - 자식 프로세스에 할당된 자원의 한계를 넘어서는 경우
    - 부모 프로세스가 종료되는 경우
    - 사용자가 `process.kill` 과 같은 명령어로 프로세스를 종료하는 경우가 존재합니다.
