# Chapter 1 - 디자인 패턴과 프로그래밍 패러다임

# 디자인 패턴(Design Pattern)

## 개론

<aside>
💡 **디자인 패턴**이란, 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 ‘규약’ 형태로 만들어 놓은 것이다.

</aside>

많은 사람들이 프로그램을 개발하면서 여러가지의 문제와 끝없는 고민을 하게 만드는 상황을 매번 겪습니다. 그런 상황을 겪으면서 어느순간 반복적으로 발생하는 문제들 중 특정 문제들의 해결방법이 동일하다는 것을 깨닫게 되고, 이런 해결 방법들을 묶어서 **디자인 패턴(Design Pattern)**이라는 이름으로 관리하게 됩니다.

## 싱글톤 패턴(Singleton Pattern)

> **싱글톤 패턴(Singleton Pattern)**은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다.
> 

싱글톤 패턴은 여러개의 모듈에서 **동일한 하나의 인스턴스**를 가져야하는 상황에서 많이 사용됩니다.

예를들어서 A, B 모듈이 데이터베이스를 연결해야하는 상황에서 각각의 모듈이 데이터베이스를 연결하게 되면, 모듈의 갯수만큼 연결을 시도하는 문제가 발생할 수 있습니다.

- 📝  **싱글톤 패턴을 사용하지 않은 데이터베이스 연결 구조 구현**
    
    ```jsx
    const Sequelize = require("sequelize");
    
    class MySQL {
      // Sequelize를 이용해 MySQL 서버와 연결합니다.
      static connection() {
        return new Sequelize("singleton_pattern_DB", "username", "password",);
      }
    }
    
    class aModule {
      constructor() {
        // A Module에서 MySQL과 연결합니다.
        this.sequelize = MySQL.connection();
      }
    }
    
    class bModule {
      constructor() {
        // B Module에서 MySQL과 연결합니다.
        this.sequelize = MySQL.connection();
      }
    }
    ```
    
    현재 A, B 모듈은 MySQL 클래스가 제공하는 `connection` 메서드를 이용해 데이터베이스를 연결합니다.
    
    `connection` 메서드는 단순히 지정된 `database`, `username`, `password`를 이용해 데이터베이스에 연결을 시도합니다. 여러개의 모듈에서 `connection` 메서드를 여러번 호출하더라도 호출한 횟수마다 데이터베이스에 연결을 시도하게됩니다.
    
    해당 상황에서는 모듈이 추가될 때 마다 데이터베이스에 연결을 시도하는 횟수가 증가하게 되어 의도치 않은 어플리케이션의 속도 저하가 발생하게 됩니다.
    
- 📝  **싱글톤 패턴을 사용한 데이터베이스 연결 구조 구현**
    
    ```jsx
    const Sequelize = require("sequelize");
    
    let dbInstance = null;
    
    class MySQL {
      // Sequelize를 이용해 MySQL 서버와 연결합니다.
      static connection() {
        if (!dbInstance) {
          // dbInstance가 생성되어 있지 않은경우 인스턴스를 생성하고 반환합니다.
          dbInstance = new Sequelize("singleton_pattern_DB", "username", "password",);
        }
        
        // dbInstance가 생성되어 있을 경우 인스턴스를 생성하지 않고, 반환합니다.
        return dbInstance;
      }
    }
    
    class aModule {
      constructor() {
        // A Module에서 sequelize를 이용해 MySQL과 연결합니다.
        this.sequelize = MySQL.connection();
      }
    }
    
    class bModule {
      constructor() {
        // B Module에서 sequelize를 이용해 MySQL과 연결합니다.
        this.sequelize = MySQL.connection();
      }
    }
    ```
    
    싱글톤 패턴을 사용한 경우 MySQL 클래스에서 단 한번만 인스턴스를 생성하여 `dbInstance` 변수에 저장하게 되고, 각각의 모듈에서는 생성된 인스턴스를 반환 받도록 구현하게 됩니다.
    
    그로인해 데이터베이스와 연결하는 행위는 `dbInstance` 변수가 존재하지 않을 때 단 1번만 실행하게 되고, 의도하지 않은 행위가 발생하지 않는 장점을 가지게 됩니다.
    

## 팩토리 패턴(Factory Pattern)

> **팩토리 패턴(Factory Pattern)**은 객체의 생성 부분을 떼어내 추상화한 패턴이며, 상속 관계에 있는 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.
> 

**팩토리 패턴(Factory Pattern)**은 실제로 구현되고 사용되는 상위 클래스와 단순히 생성만 하는 하위 클래스로 분리가 되기 때문에 느슨한 결합을 가지게 되고, 객체를 생성하는 로직이 하위 클래스에만 존재하므로 코드 리팩터링을 하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가하게 되는 장점을 가지고 있습니다.

- 📝  **Coffee 클래스와 CoffeeFactory 클래스를 이용한 팩토리 패턴 구현**
    
    ```jsx
    class Latte {
      constructor() {
        this.name = 'latte'
      }
    }
    class Espresso {
      constructor() {
        this.name = 'espresso'
      }
    }
    
    class DefaultCoffeeFactory {
      static createCoffee() {  }
    }
    class LatteFactory extends DefaultCoffeeFactory {
      constructor() { super() }
    
      static createCoffee() {
        return new Latte();
      }
    }
    class EspressoFactory extends DefaultCoffeeFactory {
      constructor() { super() }
    
      static createCoffee() {
        return new Espresso();
      }
    }
    
    const factoryObj = { LatteFactory, EspressoFactory };
    class CoffeeFactory {
      static createCoffee(type) {
        const factory = factoryObj[type];
        return factory.createCoffee();
      }
    }
    
    (() => {
      // Latte 클래스의 인스턴스를 생성합니다.
      const coffee = CoffeeFactory.createCoffee("LatteFactory");
      // 커피 이름을 출력합니다.
      console.log(coffee.name);
    })()
    ```
    
    팩토리 패턴으로 인해 상위 클래스에 구현되어 있는 `CoffeeFactory`가 중요한 뼈대를 결정하고, 하위 클래스인 `LatteFactory`, `EspressoFactory`가 구체적인 인스턴스를 생성하는 내용을 결정하고 있습니다.
    

## 전략 패턴(Strategy Pattern) | 정책 패턴(Policy Pattern)

> **전략 패턴(Strategy Pattern)**은 객체의 행위를 변경하고 싶은 경우 “**직접**” 수정하지 않고 전략이라 부르는 “**캡슐화한 알고리즘**”을 컨텍스트(Context) 내부에서 변경하여 상호 교체가 가능하게 만드는 패턴이다.
> 

- **📝  카드 결제 시스템을 이용한 전략 패턴 구현**
    
    ```tsx
    interface PaymentStrategy {
      pay(amount: number): void;
    }
    
    class KAKAOCardStrategy implements PaymentStrategy {
      private readonly name: string;
      private readonly cardNumber: string;
      private readonly cvv: string;
      private readonly dateOfExpiry: string;
    
      constructor(name: string, cardNumber: string, cvv: string, expiryDate: string) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
      }
    
      public pay(amount: number): void {
        console.log(`${amount} paid using KAKAOCard`);
      }
    }
    
    class LUNACardStrategy implements PaymentStrategy {
      private readonly emailId: string;
      private readonly password: string;
    
      constructor(emailId: string, password: string) {
        this.emailId = emailId;
        this.password = password;
      }
    
      public pay(amount: number): void {
        console.log(`${amount} paid using LUNACard`);
      }
    }
    
    class Item {
      private readonly name: string;
      private readonly price: number;
    
      constructor(name: string, cost: number) {
        this.name = name;
        this.price = cost;
      }
    
      getName() {
        return this.name;
      }
    
      getPrice() {
        return this.price;
      }
    }
    
    class ShoppingCart {
      private items: Array<Item>;
    
      constructor() {
        this.items = [];
      }
    
      addItem(item: Item): void {
        this.items.push(item);
      }
    
      removeItem(item: Item): void {
        const index = this.items.indexOf(item);
        this.items.splice(index, 1);
      }
    
      calculateTotal(): number {
        let sum = 0;
        for (const item of this.items) {
          sum += item.getPrice();
        }
        return sum;
      }
    
      pay(paymentMethod: PaymentStrategy): void {
        const amount = this.calculateTotal();
        paymentMethod.pay(amount);
      }
    }
    
    (() => {
      const cart = new ShoppingCart();
    
      const a = new Item("ItemA", 100);
      const b = new Item("ItemB", 300);
    
      cart.addItem(a);
      cart.addItem(b);
    
      // pay by LUNA Card
      cart.pay(new LUNACardStrategy(
        "archepro84@gmail.com",
        "archeproPassword"
      ));
    
      // pay by KAKAO Card
      cart.pay(new KAKAOCardStrategy(
        "이용우",
        "4890-1010-2013-8484",
        "991",
        "23/02"
      ));
    })()
    ```
    
    `**ShoppingCart**` 클래스는 사용자가 장바구니에 담은 상품들을 관리하는 클래스입니다.
    사용자가 장바구니에 담은 정보들을 바탕으로 최종 금액을 산출하여 **pay 메서드**를 이용해 결제를 진행합니다.
    
    `**ShoppingCart**` 클래스의 **pay 메서드**는 `**PaymentStrategy**` 클래스가 가지고 있는 **pay 메서드**에 최종 결제 금액을 전달할 뿐 어떠한 로직을 수행하지 않습니다.
    
    `**KAKAOCardStrategy**` 또는 `**LUNACardStrategy**` 클래스 내부에서 어떠한 로직으로 결제를 수행하는지 `**ShoppingCart**` 클래스는 어떠한 영향도 받지 않게되고, “**캡슐화된 알고리즘**”을 각각의 클래스 내부에서 바꿔주며 상호 교체가 가능하게 됩니다.
    

## 옵저버 패턴(Observer Pattern)

> **옵저버 패턴(Observer Pattern)**은 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해서 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴이다.
> 

**옵저버 패턴(Observer Pattern)**은 **주체**와 **옵저버**란 개념이 새롭게 설명되는데, 여기서 **주체**란 객체(subject)의 상태 변화를 주시하고 있는 **관찰자**이고, **옵저버(Observer)**들이란 이 객체(subject)의 상태 변화에 따라 전달되는 메서드 등을 기반으로 “**추가 변화 사항**”이 생기는 객체들을 의미합니다.

여기서, 주체와 객체를 따로 두지않고, 상태가 변경되는 객체를 기반으로 패턴을 구축하기도 합니다.

주로 **분산 이벤트 핸들링 시스템**을 구현하거나 **발행/구독(Pub/Sub)** 모델에서 많이 사용됩니다. 
→ 여기서 발행/구독(Pub/Sub)모델은 대표적으로 [MQTT 프로토콜](https://mqtt.org/)을 이용하는 [Rabbit MQ](https://www.rabbitmq.com/tutorials/tutorial-one-javascript.html)에서 사용됩니다. 

- **📝  Topic을 이용한 Pub/Sub 옵저버 패턴 구현**
    
    ```tsx
    interface Observer {
      update(): void;
    }
    
    interface Subject {
      register(obj: Observer): void;
    
      unregister(obj: Observer): void;
    
      notifyObservers(): void;
    
      getUpdate(obj: Observer): string;
    }
    
    // Topic: Observer 패턴의 주체이자 객체
    class Topic implements Subject {
      private observers: Set<Observer>;
      private message: string;
    
      constructor() {
        this.observers = new Set;
        this.message = "";
      }
    
      register(obj: Observer): void {
        if (!this.observers.has(obj)) this.observers.add(obj);
      }
    
      unregister(obj: Observer): void {
        this.observers.delete(obj);
      }
    
      notifyObservers() {
        this.observers.forEach(observer => observer.update());
      }
    
      getUpdate(obj: Observer): string {
        return this.message;
      }
    
      postMessage(msg: string): void {
        console.log(`Message sended to Topic: ${msg}`);
        this.message = msg;
        this.notifyObservers();
      }
    }
    
    class TopicSubscriber implements Observer {
      private readonly name: string;
      private topic: Subject;
    
      constructor(name: string, topic: Subject) {
        this.name = name;
        this.topic = topic;
      }
    
      update(): void {
        const msg = this.topic.getUpdate(this);
        console.log(`${this.name} :: got message >> ${msg}`);
      }
    }
    
    (() => {
      const topic = new Topic();
      const a = new TopicSubscriber("a", topic); // 이름과 Topic을 지정하여 Subcriber 생성
      const b = new TopicSubscriber("b", topic); // 이름과 Topic을 지정하여 Subcriber 생성
      const c = new TopicSubscriber("c", topic); // 이름과 Topic을 지정하여 Subcriber 생성
    
      topic.register(a); // Topic에 Subscriber 등록
      topic.register(b); // Topic에 Subscriber 등록
      topic.register(c); // Topic에 Subscriber 등록
    
      topic.postMessage("Hello post Message by topic"); // Topic에 등록된 Subscriber에게 메시지 Publish
    
      /**
       * Message sended to Topic: Hello post Message by topic
       * a :: got message >> Hello post Message by topic
       * b :: got message >> Hello post Message by topic
       * c :: got message >> Hello post Message by topic
       */
    })();
    ```
    
    `Topic` 클래스는 **구독(Sub)**할 `TopicSubscriber`클래스를 이용해 구독자들을 전달받고, 해당 `Topic`을 구독하고 있는 모든 구독자에게 메시지를 **전송(Pub)**합니다.
    
    예를들어 `Topic` 클래스가 DB 서버와 상태를 감시하던 중 서버와의 연결이 끊기게 되었을 경우 `Topic` 클래스를 구독하는 모든 구독자들에게 DB의 연결 상태를 전달하여, 더이상 DB에 접근하지 못하도록 만들수도 있을 것 입니다.
    

## 프록시 패턴(Proxy Pattern)과 프록시 서버(Proxy Server)

> **프록시 패턴(Proxy Pattern)**은 대상 객체(subject)에 접근하기 전 해당 접근에 대한 흐름을 가로채 해당 객체 앞단의 인터페이스 역할을 하는 디자인 패턴입니다.
> 

**프록시 패턴(Proxy Pattern)**은 대표적으로 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용됩니다.

**프록시 서버(Proxy Server)**는 대표적으로 캐싱 시스템, **CDN(Content Delivery Network)**으로 사용됩니다.

- **❓ 프록시(Proxy) 객체란?**
    
    > **프록시(Proxy) 객체**는 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체이며, Javascript에서 프록시 객체는 `**target**`, `**handler**`두 개의 매개변수를 가진다.
    > 
    - `**target**`: 프록시 할 대상을 가리킵니다.
    - `**handler**`: 프록시 객체의 `**target**` 동작을 가로채서 정의할 동작들이 정해져 있는 함수입니다.
    
    ```tsx
    const handler = {
      get: (target, key) => {
        return key === "name" ? `${target.a} ${target.b}` : target[key]
      }
    }
    
    const p = new Proxy({ a: "Archepro84", b: "Is Developer" }, handler);
    console.log(p.name); // Archepro84 Is Developer
    console.log(p["name"]); // Archepro84 Is Developer
    ```
    
    `new Proxy`로 선언한 **객체(object)**에 `a`와 `b`라는 속성에 문자열을 담고, `handler`에 “`name`” 프로퍼티에 접근하려 할 때 객체의 `a`와 `b`를 합쳐 문자열을 만들도록 구현하였습니다.
    
    `p` 변수는 `name` 속성을 선언하지 않고도 해당하는 프로퍼티가 존재하는 것 처럼 문자열을 만들어 반환합니다.
    
- **❓ 프록시(Proxy) 서버란?**
    
    > **프록시 서버(Proxy Server)**는 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 시스템이나 응용 프로그램을 가리킨다.
    > 
- **📝  프록시(Proxy) 객체를 이용한 옵저버 패턴 구현**
    
    ```jsx
    function createReactiveObject(target, callback) {
      const proxyHandler = {
        /**
         * @param {Object} obj - the target object
         * @param {String} prop - the property Key
         * @param value - the property value
         * @returns {Boolean} - true if the property was successfully set
         * **/
        set(obj, prop, value) {
          if (value !== obj[prop]) {
            const prev = obj[prop];
            obj[prop] = value;
            callback(`${prop}가 ${prev}에서 ${value}로 변경되었습니다.`);
          }
          return true;
        }
      }
    
      const proxy = new Proxy(target, proxyHandler);
      return proxy;
    }
    
    const a = {
      "YoungWoo": "Solo"
    }
    
    const b = createReactiveObject(a, console.log);
    b.YoungWoo = "Solo";
    b.YoungWoo = "Couple";
    
    /**
     * { YoungWoo: 'Solo' } YoungWoo Solo
     * { YoungWoo: 'Solo' } YoungWoo Couple
     * YoungWoo가 Solo에서 Couple로 변경되었습니다.
     * **/
    ```
    
    `createReactiveObject` 함수를 이용해 새로운 프록시 객체를 할당 받습니다.
    
    해당 프록시 객체의 `proxyHandler` 객체를 할당하고, `set()` 메서드를 이용해 특정 프로퍼티에 대한 접근을 가로채고있습니다.
    
    `set()` 함수에서 전달받은 `prop`, `value`를 이용해 지정한 형식의 `console.log`를 출력하고 있습니다.
    

## 이너레이터 패턴(Iterator Pattern)

> **이너레이터 패턴(Iterator Pattern)**은 이터레이터(Iterator)를 이용해 컬렉션(Collection)의 요소들에 접근하는 패턴이다.
> 

- **📝  Javascript의 Map, Set 객체를 이용한 이터레이터 패턴 구현**
    
    ```jsx
    const mapObject = new Map();
    const setObject = new Set();
    
    mapObject.set('a', 1);
    mapObject.set('b', 2);
    mapObject.set('c', 3);
    
    setObject.add(1);
    setObject.add(2);
    setObject.add(3);
    
    for (const mapItem of mapObject) console.log(mapItem);
    for (const setItem of setObject) console.log(setItem);
    
    /**
     * [ 'a', 1 ]
     * [ 'b', 2 ]
     * [ 'c', 3 ]
     * 1
     * 2
     * 3
     */
    ```
    
    `Map`과 `Set`을 각각 `mapObject`, `setObject` 변수에 할당하였습니다.
    
    두 변수에 할당된 자료형식은 각각 다르지만 `for … of`문 이라는 **[이터레이터 프로토콜](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)**을 통해 같은 방식으로 반복문을 순회하는 것을 확인 할 수 있습니다.
    
    → **[이터레이터 프로토콜](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)**은 **[이터러블(Iterable)한 객체](https://ko.javascript.info/iterable)들을 순회할 때 쓰이는 규칙**입니다.
    

## 노출모듈 패턴(Revealing Module Pattern)

> **노출모듈 패턴(Revealing Module Pattern)**은 [즉시 실행 함수(IIFE)](https://developer.mozilla.org/ko/docs/Glossary/IIFE)를 통해 **private**, **public** 과 같은 [접근 제어자(Access Modifier)](https://www.typescripttutorial.net/typescript-tutorial/typescript-access-modifiers/)를 만드는 패턴이다.
> 

**Javascript**는 **Typescript**, **Java**와 다르게 접근 제어자가 존재하지 않습니다.

그렇기 때문에 노출모듈 패턴을 통해 **priavate**, **public** 과 같은 접근 제어자를 구현합니다.

이런 방식으로 구현한 접근 제한 방식은 **[클로저(Closure)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)**라고 부릅니다.

- **📝  Javascript의 클로저(Closure)를 이용한 노출모듈 패턴 구현**
    
    ```jsx
    const closureIIFE = (() => {
      const a = 1;
      const b = () => 2;
      const public = {
        c: 2,
        d: () => 3,
      }
    
      return public;
    })();
    
    console.log(closureIIFE);
    console.log(closureIIFE.a);
    console.log(closureIIFE.b);
    console.log(closureIIFE.c);
    console.log(closureIIFE.d);
    console.log(closureIIFE.d());
    
    /**
     * { c: 2, d: [Function: d] }
     * undefined
     * undefined
     * 2
     * [Function: d]
     * 3
     */
    ```
    
    `closureIIFE`는 즉시 실행 함수(IIFE)를 이용해 구현된 변수입니다.
    
    여기서 `a`, `b` 변수는 접근 제어자 중 **private** 과 동일한 제어 범위을 가집니다.
    
    하지만, 해당 함수는 `public` 객체를 **return** 하는데, `public` 객체에 있는 `c`, `d` 프로퍼티는 접근 제어자 중 **public**과 동일한 제어 범위를 가집니다.
    

## MVC 패턴(Model, View, Controller Pattern)

> **MVC 패턴(Model, View, Controller Pattern)**은 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인패턴이다.
> 

**MVC 패턴(Model, View, Controller Pattern)**은 대표적으로 React.js, Spring 프레임워크 등 다양한 라이브러리에서 사용되고있습니다.

**모델(Model)**: 어플리케이션의 데이터베이스, 상수, 변수 등을 나타냅니다.

**뷰(View)**: 사용자 인터페이스 (User Interface) 요소를 나타냅니다.

**컨트롤러(Controller)**: 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당하고, 모델과 뷰의 생명 주기도 관리합니다.

## **MVP 패턴(Model, View, Presenter Pattern)**

> **MVP 패턴(Model, View, Presenter Pattern)**은 MVC 패턴으로부터 파생되었으며 컨트롤러(Controller)가 프레젠터(Presenter)로 교체된 패턴이다.
> 

뷰(View)와 프레젠터(Presenter)는 1:1 관계이므로 MVC 패턴보다 강결합을 지닌 패턴입니다.

## MVVM 패턴(Model, View, ViewModel Pattern)

> **MVVM 패턴(Model, View, ViewModel Pattern)**은 MVC 패턴으로부터 파생되었으며 컨트롤러(Controller)가 뷰모델(View Model)로 교체된 패턴이다.
> 

**MVVM 패턴(Model, View, ViewModel Pattern)**은 커맨드와 데이터 바인딩을 가지는 것이 특징입니다. 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스트(Unit Test) 코드를 구성하기 쉽습니다.

- ❓ **뷰모델(View Model)이란?**
    
    > **뷰모델(View Model)**은 뷰(View)를 더욱 추상화 한 계층입니다.
    > 
- ❓ **커맨트(Command)란?**
    
    > **커맨트(Command)**는 여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법입니다.
    > 
- ❓ **데이터 바인딩(Data Binding)이란?**
    
    > **데이터 바인딩(Data Binding)**은 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법으로, 뷰모델을 변경하면 뷰가 변경됩니다.
    >