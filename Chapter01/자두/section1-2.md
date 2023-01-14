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
