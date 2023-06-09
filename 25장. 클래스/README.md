# 25장. 클래스

# 1. 클래스는 프로토타입의 문법적 설탕인가?

```jsx
// ES5 생성자 함수
var Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log('Hi! My name is ' + this.name);
  };

  // 생성자 함수 반환
  return Person;
}());

// 인스턴스 생성
var me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕
- 클래스는 생성자 함수보다 엄격, 생성자 함수에서는 제공하지 않는 기능도 제공
    - `new` 연산자 없이 호출 시 에러 발생
    - 상속을 지원하는 `extends` 와 `super` 키워드 제공
    - 호이스팅이 발생하지 않는 것처럼 동작
    - 모든 코드에는 암묵적으로 strict mode가 지정되어 실행, strict mode 해제 불가능
    - `constructor` , 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]` 의 값이 `false` → 열거되지 않음
- 새로운 객체 생성 메커니즘

# 2. 클래스 정의

- `class` 키워드를 사용하여 정의
- 파스칼 케이스 사용
    
    ```jsx
    // 클래스 선언문
    class Person {}
    ```
    
- 표현식으로 클래스 정의 가능
    
    ```jsx
    // 익명 클래스 표현식
    const Person = class {};
    
    // 기명 클래스 표현식
    const Person = class MyClass {};
    ```
    
- 클래스가 값으로 사용할 수 있는 일급 객체
    - 무명의 리터럴로 생성 가능 → 런타임에 생성 가능
    - 변수나 자료구조에 저장 가능
    - 함수의 매개변수에게 전달 가능
    - 함수의 반환값으로 사용 가능
- 0개 이상의 메서드만 정의 → `constructor` , 프로토타입 메서드, 정적 메서드
    
    ```jsx
    // 클래스 선언문
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name; // name 프로퍼티는 public하다.
      }
    
      // 프로토타입 메서드
      sayHi() {
        console.log(`Hi! My name is ${this.name}`);
      }
    
      // 정적 메서드
      static sayHello() {
        console.log('Hello!');
      }
    }
    
    // 인스턴스 생성
    const me = new Person('Lee');
    
    // 인스턴스의 프로퍼티 참조
    console.log(me.name); // Lee
    // 프로토타입 메서드 호출
    me.sayHi(); // Hi! My name is Lee
    // 정적 메서드 호출
    Person.sayHello(); // Hello!
    ```
    

# 3. 클래스 호이스팅

- 함수로 평가 가능
    
    ```jsx
    // 클래스 선언문
    class Person {}
    
    console.log(typeof Person); // function
    ```
    
- 런타임 이전에 먼저 평가되어 함수 객체를 생성 → `constructor`
- 클래스 정의 이전에 참조할 수 없음
    
    ```jsx
    console.log(Person);
    // ReferenceError: Cannot access 'Person' before initialization
    
    // 클래스 선언문
    class Person {}
    ```
    
- 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않음
    
    ```jsx
    const Person = '';
    
    {
      // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
      console.log(Person);
      // ReferenceError: Cannot access 'Person' before initialization
    
      // 클래스 선언문
      class Person {}
    }
    ```
    
- 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작

# 4. 인스턴스 생성

- 생성자 함수이며 `new` 연산자와 함께 호출되어 인스턴스 생성
    
    ```jsx
    class Person {}
    
    // 인스턴스 생성
    const me = new Person();
    console.log(me); // Person {}
    ```
    
    ```jsx
    class Person {}
    
    // 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
    const me = Person();
    // TypeError: Class constructor Person cannot be invoked without 'new'
    ```
    
- 기명 클래스 표현식의 클래스 이름을 사용해 인스턴스를 생성하면 에러가 발생
    
    ```jsx
    const Person = class MyClass {};
    
    // 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
    const me = new Person();
    
    // 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
    console.log(MyClass); // ReferenceError: MyClass is not defined
    
    const you = new MyClass(); // ReferenceError: MyClass is not defined
    ```
    

# 5. 메서드

- 0개 이상의 메서드만 선언 가능 → `constructor` , 프로토타입 메서드, 정적 메서드

## 1. `constructor`

- 인스턴스를 생성하고 초기화하기 위한 특수한 메서드
- 이름을 변경할 수 없음
    
    ```jsx
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    }
    ```
    
    ```jsx
    // 클래스는 함수다.
    console.log(typeof Person); // function
    console.dir(Person);
    ```
    
    ```jsx
    // 인스턴스 생성
    const me = new Person('Lee');
    console.log(me);
    ```
    
- `constructor` 내부에서 `this` 에 추가한 프로퍼티는 인스턴스 프로퍼티가 됨
    
    ```jsx
    // 클래스
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    }
    
    // 생성자 함수
    function Person(name) {
      // 인스턴스 생성 및 초기화
      this.name = name;
    }
    ```
    
- 클래스 정의가 평가되면 `constructor` 의 기술된 동작을 하는 함수 객체가 생성
- 클래스 내에 최대 한 개만 존재 가능
    
    ```jsx
    class Person {
      constructor() {}
      constructor() {}
    }
    // SyntaxError: A class may only have one constructor
    ```
    
- 생략 가능
    
    ```jsx
    class Person {}
    ```
    
- 생략 → 빈 `constructor` 가 암묵적으로 정의
    
    ```jsx
    class Person {
      // constructor를 생략하면 다음과 같이 빈 constructor가 암묵적으로 정의된다.
      constructor() {}
    }
    
    // 빈 객체가 생성된다.
    const me = new Person();
    console.log(me); // Person {}
    ```
    
- 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 `constructor` 내부에서 `this` 에 인스턴스 프로퍼티를 추가
    
    ```jsx
    class Person {
      constructor() {
        // 고정값으로 인스턴스 초기화
        this.name = 'Lee';
        this.address = 'Seoul';
      }
    }
    
    // 인스턴스 프로퍼티가 추가된다.
    const me = new Person();
    console.log(me); // Person {name: "Lee", address: "Seoul"}
    ```
    
- 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 다음과 같이 `constructor` 에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달
    
    ```jsx
    class Person {
      constructor(name, address) {
        // 인수로 인스턴스 초기화
        this.name = name;
        this.address = address;
      }
    }
    
    // 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
    const me = new Person('Lee', 'Seoul');
    console.log(me); // Person {name: "Lee", address: "Seoul"}
    ```
    
- 별도의 반환문을 갖지 않아야 함
    
    ```jsx
    class Person {
      constructor(name) {
        this.name = name;
    
        // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
        return {};
      }
    }
    
    // constructor에서 명시적으로 반환한 빈 객체가 반환된다.
    const me = new Person('Lee');
    console.log(me); // {}
    ```
    
- 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 `this` 가 반환
    
    ```jsx
    class Person {
      constructor(name) {
        this.name = name;
    
        // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
        return 100;
      }
    }
    
    const me = new Person('Lee');
    console.log(me); // Person { name: "Lee" }
    ```
    
- `constructor` 내부에서 `return` 문을 반드시 생략

## 2. 프로토타입 메서드

- 명시적으로 프로토타입에 메서드를 추가
    
    ```jsx
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }
    
    // 프로토타입 메서드
    Person.prototype.sayHi = function () {
      console.log(`Hi! My name is ${this.name}`);
    };
    
    const me = new Person('Lee');
    me.sayHi(); // Hi! My name is Lee
    ```
    
- 클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 `prototype` 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨
    
    ```jsx
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    
      // 프로토타입 메서드
      sayHi() {
        console.log(`Hi! My name is ${this.name}`);
      }
    }
    
    const me = new Person('Lee');
    me.sayHi(); // Hi! My name is Lee
    ```
    
- 클래스가 생성한 인스턴스는 프로토타입 체인의 일원
    
    ```jsx
    // me 객체의 프로토타입은 Person.prototype이다.
    Object.getPrototypeOf(me) === Person.prototype; // -> true
    me instanceof Person; // -> true
    
    // Person.prototype의 프로토타입은 Object.prototype이다.
    Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
    me instanceof Object; // -> true
    
    // me 객체의 constructor는 Person 클래스다.
    me.constructor === Person; // -> true
    ```
    
- 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 됨

## 3. 정적 메서드

- 인스턴스를 생성하지 않아도 호출할 수 있는 메서드
- 명시적으로 생성자 함수에 메서드를 추가
    
    ```jsx
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }
    
    // 정적 메서드
    Person.sayHi = function () {
      console.log('Hi!');
    };
    
    // 정적 메서드 호출
    Person.sayHi(); // Hi!
    ```
    
- 클래스에서는 메서드에 `static` 키워드를 붙여 정적 메서드로 정의
    
    ```jsx
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    
      // 정적 메서드
      static sayHi() {
        console.log('Hi!');
      }
    }
    ```
    
- 인스턴스로 호출하지 않고 클래스로 호출
    
    ```jsx
    // 정적 메서드는 클래스로 호출한다.
    // 정적 메서드는 인스턴스 없이도 호출할 수 있다.
    Person.sayHi(); // Hi!
    ```
    
- 인스턴스로 호출 불가 → 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않음, 인스턴스로 클래스의 메서드를 상속 불가
    
    ```jsx
    // 인스턴스 생성
    const me = new Person('Lee');
    me.sayHi(); // TypeError: me.sayHi is not a function
    ```
    

## 4. 정적 메서드와 프로토타입 메서드의 차이

- 자신이 속해 있는 프로토타입 체인이 다름
- 정적 메서드 : 클래스로 호출, 프로토타입 메서드 : 인스턴스로 호출
- 정적 메서드 : 인스턴스 프로퍼티 참조 불가, 프로토타입 메서드 : 인스턴스 프로퍼티 참조 가능
    
    ```jsx
    class Square {
      // 정적 메서드
      static area(width, height) {
        return width * height;
      }
    }
    
    console.log(Square.area(10, 10)); // 100
    ```
    
    ```jsx
    class Square {
      constructor(width, height) {
        this.width = width;
        this.height = height;
      }
    
      // 프로토타입 메서드
      area() {
        return this.width * this.height;
      }
    }
    
    const square = new Square(10, 10);
    console.log(square.area()); // 100
    ```
    
    ```jsx
    // 표준 빌트인 객체의 정적 메서드
    Math.max(1, 2, 3);          // -> 3
    Number.isNaN(NaN);          // -> true
    JSON.stringify({ a: 1 });   // -> "{"a":1}"
    Object.is({}, {});          // -> false
    Reflect.has({ a: 1 }, 'a'); // -> true
    ```
    
- 클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아 놓으면 이름 충돌 가능성을 줄여 주고 관련 함수들을 구조화할 수 있는 효과

## 5. 클래스에서 정의한 메서드의 특징

- `function` 키워드를 생략한 메서드 축약 표현 사용
- 클래스에 메서드를 정의할 때는 콤마가 필요 없음
- 암묵적으로 strict mode로 실행
- `for ... in` 문이나 `Object.keys` 메서드 등으로 열거 불가능 → 프로퍼티 어트리뷰트 `[[Enumerable]]` 의 값이 `false`
- non-constructor → `new` 연산자와 함께 호출 불가능

# 6. 클래스의 인스턴스 생성 과정

## 1. 인스턴스 생성과 `this` 바인딩

- `new` 연산자와 함께 클래스를 호출하면 `constructor` 의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체 생성
- `constructor` 내부의 `this` 는 클래스가 생성한 인스턴스를 가리킴

## 2. 인스턴스 초기화

- `constructor` 의 내부 코드가 실행되어 `this` 에 바인딩되어 있는 인스턴스를 초기화

## 3. 인스턴스 반환

- 완성된 인스턴스가 바인딩된 `this` 가 암묵적으로 반환
    
    ```jsx
    class Person {
      // 생성자
      constructor(name) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // Person {}
        console.log(Object.getPrototypeOf(this) === Person.prototype); // true
    
        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.name = name;
    
        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
      }
    }
    ```
    

# 7. 프로퍼티

## 1. 인스턴스 프로퍼티

- `constructor` 내부에서 정의
    
    ```jsx
    class Person {
      constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;
      }
    }
    
    const me = new Person('Lee');
    console.log(me); // Person {name: "Lee"}
    ```
    
- `constructor` 내부에서 `this` 에 인스턴스 프로퍼티를 추가
- 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화
    
    ```jsx
    class Person {
      constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name; // name 프로퍼티는 public하다.
      }
    }
    
    const me = new Person('Lee');
    
    // name은 public하다.
    console.log(me.name); // Lee
    ```
    

## 2. 접근자 프로퍼티

- 자체적으로는 값(`[[Value]]` 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티
    
    ```jsx
    const person = {
      // 데이터 프로퍼티
      firstName: 'Ungmo',
      lastName: 'Lee',
    
      // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
      // getter 함수
      get fullName() {
        return `${this.firstName} ${this.lastName}`;
      },
      // setter 함수
      set fullName(name) {
        // 배열 디스트럭처링 할당: "36.1. 배열 디스트럭처링 할당" 참고
        [this.firstName, this.lastName] = name.split(' ');
      }
    };
    
    // 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
    console.log(`${person.firstName} ${person.lastName}`); // Ungmo Lee
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
    // 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
    person.fullName = 'Heegun Lee';
    console.log(person); // {firstName: "Heegun", lastName: "Lee"}
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
    // 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
    console.log(person.fullName); // Heegun Lee
    
    // fullName은 접근자 프로퍼티다.
    // 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
    console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));
    // {get: ƒ, set: ƒ, enumerable: true, configurable: true}
    ```
    
    ```jsx
    class Person {
      constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
      }
    
      // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
      // getter 함수
      get fullName() {
        return `${this.firstName} ${this.lastName}`;
      }
    
      // setter 함수
      set fullName(name) {
        [this.firstName, this.lastName] = name.split(' ');
      }
    }
    
    const me = new Person('Ungmo', 'Lee');
    
    // 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
    console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
    // 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
    me.fullName = 'Heegun Lee';
    console.log(me); // {firstName: "Heegun", lastName: "Lee"}
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
    // 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
    console.log(me.fullName); // Heegun Lee
    
    // fullName은 접근자 프로퍼티다.
    // 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
    console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
    // {get: ƒ, set: ƒ, enumerable: false, configurable: true}
    ```
    
- `getter` 함수와 `setter` 함수로 구성
    - `getter` : 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
        - 메서드 이름 앞에 `get` 키워드를 사용해 정의
        - 참조 시에 내부적으로 `getter` 가 호출
        - 반드시 무언가를 반환
    - `setter` : 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
        - 메서드 이름 앞에 `set` 키워드를 사용해 정의
        - 할당 시에 내부적으로 `setter` 가 호출
        - 반드시 매개변수가 있어야 함
    
    ```jsx
    // Object.getOwnPropertyNames는 비열거형(non-enumerable)을 포함한 모든 프로퍼티의 이름을 반환한다.(상속 제외)
    Object.getOwnPropertyNames(me); // -> ["firstName", "lastName"]
    Object.getOwnPropertyNames(Object.getPrototypeOf(me)); // -> ["constructor", "fullName"]
    ```
    

## 3. 클래스 필드 정의 제안

- 클래스 기반 객체지향 언어에서 클래스가 생설할 인스턴스의 프로퍼티를 가리키는 용어
    
    ```jsx
    // 자바의 클래스 정의
    public class Person {
      // ① 클래스 필드 정의
      // 클래스 필드는 클래스 몸체에 this 없이 선언해야 한다.
      private String firstName = "";
      private String lastName = "";
    
      // 생성자
      Person(String firstName, String lastName) {
        // ③ this는 언제나 클래스가 생성할 인스턴스를 가리킨다.
        this.firstName = firstName;
        this.lastName = lastName;
      }
    
      public String getFullName() {
        // ② 클래스 필드 참조
        // this 없이도 클래스 필드를 참조할 수 있다.
        return firstName + " " + lastName;
      }
    }
    ```
    
- 자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 `constructor` 내부에서 `this` 프로퍼티 추가
- 자바스크립트의 클래스 몸체에는 메서드만 선언 가능
    
    ```jsx
    class Person {
      // 클래스 필드 정의
      name = 'Lee';
    }
    
    const me = new Person('Lee');
    ```
    
    ```jsx
    class Person {
      // 클래스 필드 정의
      name = 'Lee';
    }
    
    const me = new Person();
    console.log(me); // Person {name: "Lee"}
    ```
    
- 클래스 몸체에서 클래스 필드를 정의하는 경우 `this` 에 클래스 필드를 바인딩해서는 안됨
    
    ```jsx
    class Person {
      // this에 클래스 필드를 바인딩해서는 안된다.
      this.name = ''; // SyntaxError: Unexpected token '.'
    }
    ```
    
- 클래스 필드를 참조하는 경우 자바와 같은 클래스 기반 객체지향 언어에서는 `this` 를 생략할 수 있으나 자바스크립트에서는 `this` 를 반드시 사용
    
    ```jsx
    class Person {
      // 클래스 필드
      name = 'Lee';
    
      constructor() {
        console.log(name); // ReferenceError: name is not defined
      }
    }
    
    new Person();
    ```
    
- 클래스 필드에 초기값을 할당하지 않으면 `undefined` 가짐
    
    ```jsx
    class Person {
      // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
      name;
    }
    
    const me = new Person();
    console.log(me); // Person {name: undefined}
    ```
    
- 인스턴스를 생성할 때 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면 `constructor` 에서 클래스 필드 초기화
    
    ```jsx
    class Person {
      name;
    
      constructor(name) {
        // 클래스 필드 초기화.
        this.name = name;
      }
    }
    
    const me = new Person('Lee');
    console.log(me); // Person {name: "Lee"}
    ```
    
    ```jsx
    class Person {
      constructor(name) {
        this.name = name;
      }
    }
    
    const me = new Person('Lee');
    console.log(me); // Person {name: "Lee"}
    ```
    
- 함수는 일급 객체이므로 클래스 필드에 할당 가능 → 클래스 필드를 통해 메서드 정의 가능
    
    ```jsx
    class Person {
      // 클래스 필드에 문자열을 할당
      name = 'Lee';
    
      // 클래스 필드에 함수를 할당
      getName = function () {
        return this.name;
      }
      // 화살표 함수로 정의할 수도 있다.
      // getName = () => this.name;
    }
    
    const me = new Person();
    console.log(me); // Person {name: "Lee", getName: ƒ}
    console.log(me.getName()); // Lee
    ```
    
- 클래스 필드에 화살표 함수를 할당하여 화살표 함수 내부의 `this` 가 인스턴스를 가리키게 하는 경우
    
    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;
    
            // increase 메서드를 이벤트 핸들러로 등록
            // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
            // 하지만 increase는 화살표 함수로 정의되어 있으므로
            // increase 내부의 this는 인스턴스를 가리킨다.
            this.$button.onclick = this.increase;
    
            // 만약 increase가 화살표 함수가 아니라면 bind 메서드를 사용해야 한다.
            // $button.onclick = this.increase.bind(this);
          }
    
          // 인스턴스 메서드
          // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this를 가리킨다.
          increase = () => this.$button.textContent = ++this.count;
        }
        new App();
      </script>
    </body>
    </html>
    ```
    

## 4. `private` 필드 정의 제안

```jsx
class Person {
  constructor(name) {
    this.name = name; // 인스턴스 프로퍼티는 기본적으로 public하다.
  }
}

// 인스턴스 생성
const me = new Person('Lee');
console.log(me.name); // Lee
```

- 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출
    
    ```jsx
    class Person {
      name = 'Lee'; // 클래스 필드도 기본적으로 public하다.
    }
    
    // 인스턴스 생성
    const me = new Person();
    console.log(me.name); // Lee
    ```
    
- private 필드의 선두에는 `#` 을 붙여줌 → private 필드를 참조할 때도 `#` 을 붙여줌
    
    ```jsx
    class Person {
      // private 필드 정의
      #name = '';
    
      constructor(name) {
        // private 필드 참조
        this.#name = name;
      }
    }
    
    const me = new Person('Lee');
    
    // private 필드 #name은 클래스 외부에서 참조할 수 없다.
    console.log(me.#name);
    // SyntaxError: Private field '#name' must be declared in an enclosing class
    ```
    
- private 필드는 클래스 내부에서만 참조 가능
    
    ```jsx
    class Person {
      // private 필드 정의
      #name = '';
    
      constructor(name) {
        this.#name = name;
      }
    
      // name은 접근자 프로퍼티다.
      get name() {
        // private 필드를 참조하여 trim한 다음 반환한다.
        return this.#name.trim();
      }
    }
    
    const me = new Person(' Lee ');
    console.log(me.name); // Lee
    ```
    
- private 필드는 반드시 클래스 몸체에 정의
    
    ```jsx
    class Person {
      constructor(name) {
        // private 필드는 클래스 몸체에서 정의해야 한다.
        this.#name = name;
        // SyntaxError: Private field '#name' must be declared in an enclosing class
      }
    }
    ```
    

## 5. `static` 필드 정의 제안

- `static` 키워드를 사용하여 정적 필드를 정의할 수 없었음
    
    ```jsx
    class MyMath {
      // static public 필드 정의
      static PI = 22 / 7;
    
      // static private 필드 정의
      static #num = 10;
    
      // static 메서드
      static increment() {
        return ++MyMath.#num;
      }
    }
    
    console.log(MyMath.PI); // 3.142857142857143
    console.log(MyMath.increment()); // 11
    ```
    

# 8. 상속에 의한 클래스 확장

## 1. 클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의
    
    ```jsx
    class Animal {
      constructor(age, weight) {
        this.age = age;
        this.weight = weight;
      }
    
      eat() { return 'eat'; }
    
      move() { return 'move'; }
    }
    
    // 상속을 통해 Animal 클래스를 확장한 Bird 클래스
    class Bird extends Animal {
      fly() { return 'fly'; }
    }
    
    const bird = new Bird(1, 5);
    
    console.log(bird); // Bird {age: 1, weight: 5}
    console.log(bird instanceof Bird); // true
    console.log(bird instanceof Animal); // true
    
    console.log(bird.eat());  // eat
    console.log(bird.move()); // move
    console.log(bird.fly());  // fly
    ```
    
- 의사 클래스 상속 패턴을 사용하여 상속에 의한 클래스 확장을 흉내 내기도 함
    
    ```jsx
    // 의사 클래스 상속(pseudo classical inheritance) 패턴
    var Animal = (function () {
      function Animal(age, weight) {
        this.age = age;
        this.weight = weight;
      }
    
      Animal.prototype.eat = function () {
        return 'eat';
      };
    
      Animal.prototype.move = function () {
        return 'move';
      };
    
      return Animal;
    }());
    
    // Animal 생성자 함수를 상속하여 확장한 Bird 생성자 함수
    var Bird = (function () {
      function Bird() {
        // Animal 생성자 함수에게 this와 인수를 전달하면서 호출
        Animal.apply(this, arguments);
      }
    
      // Bird.prototype을 Animal.prototype을 프로토타입으로 갖는 객체로 교체
      Bird.prototype = Object.create(Animal.prototype);
      // Bird.prototype.constructor을 Animal에서 Bird로 교체
      Bird.prototype.constructor = Bird;
    
      Bird.prototype.fly = function () {
        return 'fly';
      };
    
      return Bird;
    }());
    
    var bird = new Bird(1, 5);
    
    console.log(bird); // Bird {age: 1, weight: 5}
    console.log(bird.eat());  // eat
    console.log(bird.move()); // move
    console.log(bird.fly());  // fly
    ```
    

## 2. `extends` 키워드

- 상속받을 클래스를 정의
    
    ```jsx
    // 수퍼(베이스/부모)클래스
    class Base {}
    
    // 서브(파생/자식)클래스
    class Derived extends Base {}
    ```
    
- 서브클래스 : 상속을 통해 확장된 클래스, 베이스 클래스, 부모 클래스
- 수퍼클래스 : 서브클래스에게 상속된 클래스, 파생 클래스, 자식 클래스
- 수퍼클래스와 서브클래스 간의 상속 관계 설정
- 클래스도 프로토타입을 통해 상속 관계 구현
- 수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성

## 3. 동적 상속

- 생성자 함수를 상속받아 클래스 확장
- `extends` 키워드 앞에는 반드시 클래스 위치
    
    ```jsx
    // 생성자 함수
    function Base(a) {
      this.a = a;
    }
    
    // 생성자 함수를 상속받는 서브클래스
    class Derived extends Base {}
    
    const derived = new Derived(1);
    console.log(derived); // Derived {a: 1}
    ```
    
- `[[Construct]]` 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용
    
    ```jsx
    function Base1() {}
    
    class Base2 {}
    
    let condition = true;
    
    // 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
    class Derived extends (condition ? Base1 : Base2) {}
    
    const derived = new Derived();
    console.log(derived); // Derived {}
    
    console.log(derived instanceof Base1); // true
    console.log(derived instanceof Base2); // false
    ```
    

## 4. 서브클래스의 `constructor`

- 클래스에서 `constructor` 를 생략 → 비어 있는 `constructor` 암묵적으로 정의
    
    ```jsx
    constructor() {}
    ```
    
- `constructor` 암묵적으로 정의
- `args` 는 `new` 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트
    
    ```jsx
    constructor(...args) { super(...args); }
    ```
    
- `super()` 는 수퍼클래스의 `constructor(super-constructor)` 를 호출하여 인스턴스 생성
- `Rest` 파라미터 : 함수에 전달된 인수들의 목록을 배열로 전달
    
    ```jsx
    // 수퍼클래스
    class Base {}
    
    // 서브클래스
    class Derived extends Base {}
    ```
    
    ```jsx
    // 수퍼클래스
    class Base {
      constructor() {}
    }
    
    // 서브클래스
    class Derived extends Base {
      constructor() { super(); }
    }
    
    const derived = new Derived();
    console.log(derived); // Derived {}
    ```
    

## 5. `super` 키워드

- 함수처럼 호출할 수도 있고 `this` 와 같이 식별자처럼 참조할 수 있는 특수한 키워드
- 수퍼클래스의 `constructor(super-constructor)` 를 호출
- 수퍼클래스의 메서드를 호출

### 1. `super` 호출

- 수퍼클래스의 `constructor(super-constructor)` 를 호출
    
    ```jsx
    // 수퍼클래스
    class Base {
      constructor(a, b) {
        this.a = a;
        this.b = b;
      }
    }
    
    // 서브클래스
    class Derived extends Base {
      // 다음과 같이 암묵적으로 constructor가 정의된다.
      // constructor(...args) { super(...args); }
    }
    
    const derived = new Derived(1, 2);
    console.log(derived); // Derived {a: 1, b: 2}
    ```
    
    ```jsx
    // 수퍼클래스
    class Base {
      constructor(a, b) { // ④
        this.a = a;
        this.b = b;
      }
    }
    
    // 서브클래스
    class Derived extends Base {
      constructor(a, b, c) { // ②
        super(a, b); // ③
        this.c = c;
      }
    }
    
    const derived = new Derived(1, 2, 3); // ①
    console.log(derived); // Derived {a: 1, b: 2, c: 3}
    ```
    
- 인스턴스 초기화를 위해 전달한 인수는 수퍼클래스와 서브클래스에 배분되고 상속 관계의 두 클래스는 서로 협력하여 인스턴스 생성
1. 서브클래스에서 `constructor` 를 생략하지 않는 경우 서브클래스의 `constructor` 에서는 반드시 `super` 호출
    
    ```jsx
    class Base {}
    
    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        console.log('constructor call');
      }
    }
    
    const derived = new Derived();
    ```
    
2. 서브클래스의 `constructor` 에서 `super` 를 호출하기 전에는 `this` 를 참조할 수 없음
    
    ```jsx
    class Base {}
    
    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        this.a = 1;
        super();
      }
    }
    
    const derived = new Derived(1);
    ```
    
3. `super` 는 반드시 서브클래스의 `constructor` 에서만 호출
    
    ```jsx
    class Base {
      constructor() {
        super(); // SyntaxError: 'super' keyword unexpected here
      }
    }
    
    function Foo() {
      super(); // SyntaxError: 'super' keyword unexpected here
    }
    ```
    

### 2. `super` 참조

- 메서드 내에서 `super` 를 참조하면 수퍼클래스의 메서드 호출 가능
1. 서브클래스의 프로토타입 메서드 내에서 함수는 수퍼클래스의 프로토타입 메서드를 가리킴
    
    ```jsx
    // 수퍼클래스
    class Base {
      constructor(name) {
        this.name = name;
      }
    
      sayHi() {
        return `Hi! ${this.name}`;
      }
    }
    
    // 서브클래스
    class Derived extends Base {
      sayHi() {
        // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
        return `${super.sayHi()}. how are you doing?`;
      }
    }
    
    const derived = new Derived('Lee');
    console.log(derived.sayHi()); // Hi! Lee. how are you doing?
    ```
    
- `super` 참조를 통해 수퍼클래스의 메서드를 참조하려면 `super` 가 수퍼클래스의 메서드가 바인딩된 객체, 수퍼클래스의 `prototype` 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 함
    
    ```jsx
    // 수퍼클래스
    class Base {
      constructor(name) {
        this.name = name;
      }
    
      sayHi() {
        return `Hi! ${this.name}`;
      }
    }
    
    class Derived extends Base {
      sayHi() {
        // __super는 Base.prototype을 가리킨다.
        const __super = Object.getPrototypeOf(Derived.prototype);
        return `${__super.sayHi.call(this)} how are you doing?`;
      }
    }
    ```
    
- `super` 는 자신을 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 가리킴
    
    ```jsx
    /*
    [[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
    [[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
    예를 들어, Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩되어 있다.
    따라서 Derived 클래스의 sayHi 메서드의 [[HomeObject]]는 Derived.prototype이고
    이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정된다.
    따라서 super.sayHi는 Base.prototype.sayHi를 가리키게 된다.
    */
    super = Object.getPrototypeOf([[HomeObject]])
    ```
    
- ES6의 메서드 축약 표현으로 정의된 함수만이 `[[HomeObject]]` 를 가짐
    
    ```jsx
    const obj = {
      // foo는 ES6의 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
      foo() {},
      // bar는 ES6의 메서드 축약 표현으로 정의한 메서드가 아니라 일반 함수다.
      // 따라서 [[HomeObject]]를 갖지 않는다.
      bar: function () {}
    };
    ```
    
    ```jsx
    const base = {
      name: 'Lee',
      sayHi() {
        return `Hi! ${this.name}`;
      }
    };
    
    const derived = {
      __proto__: base,
      // ES6 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
      sayHi() {
        return `${super.sayHi()}. how are you doing?`;
      }
    };
    
    console.log(derived.sayHi()); // Hi! Lee. how are you doing?
    ```
    
1. 서브클래스의 정적 메서드 내에서 함수는 수퍼클래스의 정적 메서드를 가리킴
    
    ```jsx
    // 수퍼클래스
    class Base {
      static sayHi() {
        return 'Hi!';
      }
    }
    
    // 서브클래스
    class Derived extends Base {
      static sayHi() {
        // super.sayHi는 수퍼클래스의 정적 메서드를 가리킨다.
        return `${super.sayHi()} how are you doing?`;
      }
    }
    
    console.log(Derived.sayHi()); // Hi! how are you doing?
    ```
    

## 6. 상속 클래스의 인스턴스 생성 과정

```jsx
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

### 1. 서브클래스의 `super` 호출

- 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성 위임
- 서브클래스의 `constructor` 에서 반드시 `super` 를 호출해야 하는 이유

### 2. 수퍼클래스의 인스턴스 생성과 `this` 바인딩

```jsx
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle
...
```

```jsx
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle

    // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
    console.log(this instanceof ColorRectangle); // true
    console.log(this instanceof Rectangle); // true
...
```

### 3. 수퍼클래스의 인스턴스 초기화

- 수퍼클래스의 `constructor` 가 실행되어 `this` 에 바인딩되어 있는 인스턴스를 초기화
    
    ```jsx
    // 수퍼클래스
    class Rectangle {
      constructor(width, height) {
        // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // ColorRectangle {}
        // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
        console.log(new.target); // ColorRectangle
    
        // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
        console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
        console.log(this instanceof ColorRectangle); // true
        console.log(this instanceof Rectangle); // true
    
        // 인스턴스 초기화
        this.width = width;
        this.height = height;
    
        console.log(this); // ColorRectangle {width: 2, height: 4}
      }
    ...
    ```
    

### 4. 서브클래스 `constructor` 로의 복귀와 `this` 바인딩

- `super` 가 반환한 인스턴스가 `this` 에 바인딩
- 서브클래스는 별도의 인스턴스를 생성하지 않고 `super` 가 반환한 인스턴스를 `this` 에 바인딩하여 그대로 사용
    
    ```jsx
    // 서브클래스
    class ColorRectangle extends Rectangle {
      constructor(width, height, color) {
        super(width, height);
    
        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this); // ColorRectangle {width: 2, height: 4}
    ...
    ```
    
- `super` 가 호출되지 않으면 인스턴스가 생성되지 않으며 `this` 바인딩도 불가능
- 서브클래스의 `constructor` 에서 `super` 를 호출하기 전에는 `this` 를 참조할 수 없는 이유

### 5. 서브클래스의 인스턴스 초기화

- `super` 호출 이후 서브클래스의 인스턴스가 초기화

### 6. 인스턴스 반환

- 완성된 인스턴스가 바인딩된 `this` 가 암묵적으로 반환
    
    ```jsx
    // 서브클래스
    class ColorRectangle extends Rectangle {
      constructor(width, height, color) {
        super(width, height);
    
        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this); // ColorRectangle {width: 2, height: 4}
    
        // 인스턴스 초기화
        this.color = color;
    
        // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
        console.log(this); // ColorRectangle {width: 2, height: 4, color: "red"}
      }
    ...
    ```
    

## 7. 표준 빌트인 생성자 함수 확장

- `extends` 키워드 다음에는 클래스뿐만이 아니라 `[[Construct]]` 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능
    
    ```jsx
    // Array 생성자 함수를 상속받아 확장한 MyArray
    class MyArray extends Array {
      // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
      uniq() {
        return this.filter((v, i, self) => self.indexOf(v) === i);
      }
    
      // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
      average() {
        return this.reduce((pre, cur) => pre + cur, 0) / this.length;
      }
    }
    
    const myArray = new MyArray(1, 1, 2, 3);
    console.log(myArray); // MyArray(4) [1, 1, 2, 3]
    
    // MyArray.prototype.uniq 호출
    console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
    // MyArray.prototype.average 호출
    console.log(myArray.average()); // 1.75
    ```
    
    ```jsx
    console.log(myArray.filter(v => v % 2) instanceof MyArray); // true
    ```
    
    ```jsx
    // 메서드 체이닝
    // [1, 1, 2, 3] => [ 1, 1, 3 ] => [ 1, 3 ] => 2
    console.log(myArray.filter(v => v % 2).uniq().average()); // 2
    ```
    
    ```jsx
    // Array 생성자 함수를 상속받아 확장한 MyArray
    class MyArray extends Array {
      // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
      static get [Symbol.species]() { return Array; }
    
      // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
      uniq() {
        return this.filter((v, i, self) => self.indexOf(v) === i);
      }
    
      // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
      average() {
        return this.reduce((pre, cur) => pre + cur, 0) / this.length;
      }
    }
    
    const myArray = new MyArray(1, 1, 2, 3);
    
    console.log(myArray.uniq() instanceof MyArray); // false
    console.log(myArray.uniq() instanceof Array); // true
    
    // 메서드 체이닝
    // uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
    console.log(myArray.uniq().average());
    // TypeError: myArray.uniq(...).average is not a function
    ```
