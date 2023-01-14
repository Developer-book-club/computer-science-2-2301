## 싱글톤패턴
* 불변하는 1개의 객체를 정의하고 전역에서 이 객체를 공유.
* 보통 데이터베이스 클라이언트의 커낵션 풀을 싱글톤으로 만든다.

### 싱글톤패턴에서 의존성 주입하기
* 싱글톤패턴은 싱글톤을 사용하는 각 모듈간의 결합을 강하게 만든다.
* 의존성 주입으로 decoupling 가능하다.


[예시]

의존성 주입 전 
* `Model`은 `MysqlClient` 외에 다른 DBMS 클라이언트를 가질 수 없다.
* `MysqlClient` 에 `Model`이 의존한다. (mysql의 syntax가 변경되면 `Model.select()`도 변경해야 한다.)
```python
class MysqlClient:
    def __init__(self, host, port, user, password):
        self.host = host
        self.port = port
        self.user = user
        self.password = password


# 싱글톤 객체
mysql_client = MysqlClient("localhost", 3306, "root", "1234")


class Model:
    def __init__(self):
        self.client = mysql_client
    
    def select(self, query):
        if not query.startwith("SELECT"):
            raise Exception("wrong syntax")
        print("query to mysql...")
        return 1

model = Model()
model.select("SELECT id FROM item;")
```

의존성 주입 후
* mysql syntax 변경으로부터 `Model`이 자유롭다.
* `Model` 클래스 변경 없이 `MysqlClient` 말고도 다른 클래스 객체로 `Model` 객체를 만들 수 있다.

```python
class MysqlClient:
    def __init__(self, host, port, user, password):
        self.host = host
        self.port = port
        self.user = user
        self.password = password

    def select(self, query):
        if not query.startwith("SELECT"):
            raise Exception("wrong syntax")
        print("query to mysql...")
        return 1

    
class MongoClient:
    def __init__(self, host, port, user, password):
        self.host = host
        self.port = port
        self.user = user
        self.password = password

    def select(self, query):
        if "find" not in query:
            raise Exception("wrong syntax")
        print("query to mongo...")
        return 2


class Model:
    def __init__(self, client):
        self.client = client


model1 = Model(MysqlClient('localhost', 3306, 'root', '1234'))  # Model 에 MysqlClient 객체 주입
model2 = Model(MongoClient('localhost', 27017, 'root', '1234'))  # Model 에 MongoClient 객체 주입
```

## 팩토리패턴
* 클래스의 상속을 활용한 패턴.
* 부모 클래스에 클래스의 뼈대를 정의하고, 자식 클래스에서 객체 생성에 관한 구체적인 로직을 결정한다.


[예시]
* `MysqlClient.select()` 또는 `MongoClient.select()` 에서 변경이 발생해도 `DatabaseClient`는 변경하지 않는다.
```python
# DatabaseClient 에서 뼈대를 결정
class DatabaseClient:
    def __init__(self, host, port, user, password):
        self.host = host
        self.port = port
        self.user = user
        self.password = password

    def select(self, query):
        pass


# DatabaseClient를 상속받은 자식 클래스에서 구체적인 로직 수행.
class MysqlClient(DatabaseClient):
    def select(self, query):
        if not query.startwith("SELECT"):
            raise Exception("wrong syntax")
        print("query to mysql...")
        return 1

    
# DatabaseClient를 상속받은 자식 클래스에서 구체적인 로직 수행.
class MongoClient(DatabaseClient):
    def select(self, query):
        if "find" not in query:
            raise Exception("wrong syntax")
        print("query to mongo...")
        return 2


class Model:
    def __init__(self, client: DatabaseClient):
        self.client = client

model1 = Model(MysqlClient('localhost', 3306, 'root', '1234'))  # Model 에 MysqlClient 객체 주입
model2 = Model(MongoClient('localhost', 27017, 'root', '1234'))  # Model 에 MongoClient 객체 주입
```

### 전략 패턴
* 어떤 객체의 행위를 바꾸고 싶은 경우 행위의 변경없이 객체의 행위가 들고있는 전략(캡슐화된 로직)을 바뀌주는 패턴. (설명만 읽어서는 이해가 잘 안된다..)

[예시]
```python
class PaymentMethod:
    def __init__(self, username):
        self.username = username

    def pay(self):
        pass


# 카카오페이로 결제하는 전략
class KakaoPay(PaymentMethod):
    def __init__(self, username: str, kakao_user_id: int):
        super().__init__(username)
        self.kakao_user_id = kakao_user_id

    def pay(self):
        print(f"pay with kakao...{self.kakao_user_id}")


# 네이버페이로 결제하는 전략
class NaverPay(PaymentMethod):
    def __init__(self, username: str, naver_user_id: str):
        super().__init__(username)
        self.naver_user_id = naver_user_id

    def pay(self):
        print(f"pay with naver...{self.naver_user_id}")


class Item:
    def __init__(self, name):
        self.name = name


class Order:
    def __init__(self, items):
        self.items = items

    # 주문 결제
    def pay(self, payment_method: PaymentMethod):
        payment_method.pay()


# '주문 결제' 라는 행위(Order.pay() 메서드)의 변경없이 행위가 가지고 있는 전략 (KaKaoPay, NaverPay) 을 바꿔줌.
order = Order(items=[Item(name="의자"), Item(name="책상")])
order.pay(KakaoPay(username='자두', kakao_user_id=1234))
order.pay(NaverPay(username='살구', naver_user_id="ABCD1234"))
```
코드 실행결과
```
pay with kakao...1234
pay with naver...ABCD1234
```

## 옵저버 패턴
* 어떤 주체가 객체를 관찰하다가 객체의 상태에 변화가 발생하면 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴.
* 코드에서만 구현하는 게 아니라 백엔드 아키텍처에도 적용가능한 개념. 예) EDA(Event Driven Architecture) 에서 어떤 주체 = Producer, 옵저버 = Consumer.


## 프록시 패턴
* 어떤 객체에 접근하려고 할 때 객체 앞단에 인터페이스를 두고 인터페이스를 거쳐서 객체로 흐름을 전달하는 패턴.
* 코드에서만 구현하는 게 아니라 백엔드 아키텍처에도 적용가능한 개념. 예) Nginx, CloudFlare, CORS 해결 등

### Nginx
* 버퍼 오버플로우 방지.
  * 버퍼 오버플로우 : 많은 양의 데이터를 한꺼번에 전송하여 메모리에 rewrite 를 발생시키는 것.
* 정적 리소스 제공.
  * WAS는 동적 리소스만 담당하므로 로드를 덜 받는다.
* queue + thread pool 구조로 다수의 요청을 WAS 에게 안정적으로 전달한다.

### CloudFlare
* 전 세계에 분산된 서버에 정적 콘텐츠를 캐싱하여 각 지역에 빠르게 콘텐츠를 제공하는 CDN 서비스.
* DDOS 공격 방어, HTTPS 적용 가능.

### SOP (Same-Origin Policy)
* 자신의 도메인 외에 다른 도메인에서의 리소스 요청을 차단하는 정책.
* 다른 웹 사이트에서 무단으로 이미지, 동영상 같은 리소스를 가져가는 상황을 방지한다.
* 하지만 서브도메인 간의 리소스 공유를 어렵게 만든다. 
* SOP를 개선하기 위해 CORS가 생겼다.


[예시]
* `https://www.bookclub.co.kr` 으로 접속 시 HTML, CSS, javascript 등 프론트엔드 구성에 필요한 콘텐츠 제공.
* 프론트엔드에서는 `https://api.bookclub.co.kr` 로 서빙되는 WAS 에 동적 리소스 요청.
* WAS는 `https://api.bookclub.co.kr:80` 이 아닌 요청은 거부한다.
* 요청이 가능하려면 WAS의 오리진과(프로토콜 + 도메인 + 포트번호) 완전히 동일해야 한다.

### CORS (Cross-Origin Resource Sharing)
* 특정 오리진을 갖는 웹 호스트에 한해서만 서버가 리소스를 제공하도록 제한하는 정책
* 책에서는 프론트엔드에서 오리진을 바꾸는 프록시 서버를 두는 방법으로 CORS 준수하는 방법을 제시.
* 백엔드에서 CORS 가능한 오리진을 직접 설정하는 경우도 있다.


[예시]
* Fastapi 경우 CORS 정책을 셋팅하는 미들웨어가 있다.
* `www.bookclub.co.kr` 에서의 요청을 허용하려면 아래와 같이 설정할 수 있다.
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://www.bookclub.co.kr",
    "http://www.bookclub.co.kr:8080",
    "https://www.bookclub.co.kr",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```
---
CORS 정책 내에서 클라이언트가 서버에게 리소스를 받는 방식은 2가지가 있다.


#### CORS 단순 요청
`api.bookclub.co.kr` 에서 `www.bookclub.co.kr`으로부터 오는 요청을 허용한다는 것만 명시해주면 `www.bookclub.co.kr`에서는
아래 3가지만 지키면 된다.

1. `GET`, `HEAD`, `POST` 메서드를 사용한다.
2. Content-Type 헤더가 아래 3가지 중에 하나여야 한다.
   * `text/plain`
   * `application/x-www-form-urlencoded`
   * `multipart/form-data`
3. 표준에 정의되지 않은 커스텀한 헤더를 사용하지 않는다.

하지만 위에 제약은 요즘 널리 쓰이는 RESTful API 디자인에 한계를 주기 때문에 'CORS 사전 요청' 방식을 주로 사용한다.

#### CORS 사전 요청
* 클라이언트는 서버에게 반드시 사전 요청을 먼저 보내서 서버가 허용하는 메서드와 헤더 정보를 확인한다.


[예시]
`api.bookclub.co.kr/book/1` API Spec

* HTTP method: GET 
* path: `/book/{book_id}` 
* 표준에 정의되지 않은 헤더 외에 `user-id` 헤더를 받는다.

`www.bookclub.co.kr` 에서 `api.bookclub.co.kr/book/1` 이라는 엔드포인트로 리소스를 요청하기 전에 아래와 같이 사전 요청을 보낸다.
```
OPTIONS api.bookclub.co.kr/book/1
Headers {
  'Access-Control-Request-Method': GET
  'Access-Control-Request-Headers': user-id
}
```
사전요청에 문제가 없다면 서버에서는 아래와 같이 응답한다.
```
200 OK
Headers {
  Access-Control-Allow-Origin: http://www.bookclub.co.kr, http://www.bookclub.co.kr:8080, https://www.bookclub.co.kr
  Access-Control-Allow-Methods: *
  Access-Control-Allow-Headers: *
}
```
* `Access-Control-Allow-Origin`: 서버에서 허용하는 교차 도메인 목록
* `Access-Control-Allow-Methods`: 서버에서 허용하는 HTTP 메서드 목록
* `Access-Control-Allow-Headers`: 서버에서 허용하는 커스텀 헤더 목록

위 응답을 받고나면 클라이언트에서 실제로 필요한 리소스 요청을 수행한다.
```
GET api.bookclub.co.kr/book/1
Headers {
  'user-id': 1234
}
```
