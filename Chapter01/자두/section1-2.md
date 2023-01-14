## 프로그래밍 패러다임

### 고차함수
* 함수를 인자로 받거나 리턴하는 함수를 고차 함수라고 한다.
* 어떤 언어에서 고차함수 정의가 가능하려면 그 언어에서 함수를 일급 객체로 취급해야 한다.

### 일급객체
* 변수에 할당할 수 있어야 한다.
* 함수의 파라미터로 넘길 수 있어야 한다.
* 다른 함수의 리턴값으로 쓰일 수 있어야 한다.

[예시]
```python
# retry 함수는 wrapper 라는 함수를 리턴.
def retry():
    def wrapper(func):
        for _ in range(3):
            func()
    return wrapper


def print_hello():
    print("hello")

# retry 함수가 리턴한 wrapper 함수는 print_hello 함수를 인자로 받음.
retry()(print_hello)
```
실행결과
```
hello
hello
hello
```

## OOP (Object Oriented Programming) 설계원칙 - SOLID

### SRP (Single Responsibility Principle, 단일 책임 원칙)
* 하나의 클래스는 하나의 책임만 가져야 한다. (어떤 클래스를 변경하는 이유는 한 가지여야 한다.)

### OCP (Open Closed Principle, 개방 폐쇄 원칙)
* 새로운 요구사항을 구현해야 할 때 기존 코드를 변경하지 않으면서 새로운 기능으로 확장 가능해야 한다. 

### LSP (Liskov Substitution Principle, 리스코프 치환 원칙)
* 상속관계에 있는 두 클래스가 있을 때 자식클래스 객체는 부모클래스 객체를 대체할 수 있어야 한다.
* 부모클래스 객체를 생성하고 변경하는 주체가 가지고 있는 부모클래스에 대한 가정을 자식클래스에서도 충족해야 한다.

### ISP (Interface Segregation Principle, 인터페이스 분리 원칙)
* 범용적인 인터페이스 하나를 서로 다른 목적이 있는 주체들이 사용하게 되면 주체들 사이에 간섭이 생긴다.
* 따라서 각 주체들이 원하는 구체적인 인터페이스를 만들어서 주체들 간의 간섭을 최소화한다.

### DIP (Dependency Inversion Principle, 의존 역전 원칙)
* 고수준 모듈은 저수준 모듈에 의존해서는 안된다.
* 예) 비즈니스 로직을 담당하는 클래스는 데이터베이스 쿼리, 캐싱, external API call 과 같은 세부적인 역할을 담당하는 클래스에 의존해서는 안된다.
* 세부적인 역할을 담당하는 클래스는 추상화시켜서 비즈니스를 담당하는 클래스의 의존을 최소화 해야 한다.

---
부가자료 출처
* [일급객체란?](https://velog.io/@reveloper-1311/%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4First-Class-Object%EB%9E%80)
* [객체지향 프로그래밍의 5가지 설계 원칙, 실무 코드로 살펴보는 SOLID](https://mangkyu.tistory.com/194)

---
### chapter1 읽고 느낀 점
* 이해하기 쉽게 설명하는 만큼 실무에 적용할 만한 예시, 예제코드는 부족했다.
* 너무 추상적으로 설명하는 것들도 있어서 부가자료를 찾아봐야 했다. 
* 주로 사용하는 언어가 파이썬이다보니 자바스크립트, 자바에 국한된 개념에는 관심이 잘 안갔다... 
* 그래도 애매하게 알고 있던 개념과 명명하지 않고 무심하게 쓰던 패턴들을 다시 짚고 넘어갈 수 있었다.
