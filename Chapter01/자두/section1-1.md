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


