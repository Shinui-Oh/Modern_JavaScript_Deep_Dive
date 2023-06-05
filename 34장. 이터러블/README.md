# 34장. 이터러블

# 1. 이터레이션 프로토콜

- 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙
- 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 `for ... of` 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화
- 이터러블 프로토콜(Iterable Protocol) : 이터러블 프로토콜을 준수한 객체, `for ... of` 문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능
- 이터레이터 프로토콜(Iterator Protocol) : 이터러블의 `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환, 이터레이터 리절트 객체 반환, 이터레이터 프로토콜을 준수한 객체

## 1. 이터러블

- 이터러블 프로토콜을 준수한 객체
- `Symbol.iterator` 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체
    
    ```jsx
    const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';
    
    // 배열, 문자열, Map, Set 등은 이터러블이다.
    isIterable([]);        // -> true
    isIterable('');        // -> true
    isIterable(new Map()); // -> true
    isIterable(new Set()); // -> true
    isIterable({});        // -> false
    ```
    
- `for ... of` 문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능
    
    ```jsx
    const array = [1, 2, 3];
    
    // 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
    console.log(Symbol.iterator in array); // true
    
    // 이터러블인 배열은 for...of 문으로 순회 가능하다.
    for (const item of array) {
      console.log(item); // 1 2 3
    }
    
    // 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
    console.log([...array]); // [1, 2, 3]
    
    // 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
    const [a, ...rest] = array;
    console.log(a, rest); // 1, [2, 3]
    ```
    
    ```jsx
    const obj = { a: 1, b: 2 };
    
    // 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 않는다.
    // 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
    console.log(Symbol.iterator in obj); // false
    
    // 이터러블이 아닌 일반 객체는 for...of 문으로 순회할 수 없다.
    for (const item of obj) { // -> TypeError: obj is not iterable
      console.log(item);
    }
    
    // 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.
    const [a, b] = obj; // -> TypeError: obj is not iterable
    ```
    
- TC39 프로세스의 stage 4 단계에 제안되어 있는 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용 허용
    
    ```jsx
    const obj = { a: 1, b: 2 };
    
    // 스프레드 프로퍼티 제안(Stage 4)은 객체 리터럴 내부에서 스프레드 문법의 사용을 허용한다.
    console.log({ ...obj }); // { a: 1, b: 2 }
    ```
    

## 2. 이터레이터

- `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환
- 이터러블의 `Symbol.iterator` 메서드가 반환한 이터레이터는 `next` 메서드를 가짐
    
    ```jsx
    // 배열은 이터러블 프로토콜을 준수한 이터러블이다.
    const array = [1, 2, 3];
    
    // Symbol.iterator 메서드는 이터레이터를 반환한다.
    const iterator = array[Symbol.iterator]();
    
    // Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
    console.log('next' in iterator); // true
    ```
    
- 이터레이터의 `next` 메서드 : 이터러블의 각 요소를 순회하기 위한 포인터의 역할
- 이터레이터 리절트 객체(Iterator Result Object) 반환
    
    ```jsx
    // 배열은 이터러블 프로토콜을 준수한 이터러블이다.
    const array = [1, 2, 3];
    
    // Symbol.iterator 메서드는 이터레이터를 반환한다. 이터레이터는 next 메서드를 갖는다.
    const iterator = array[Symbol.iterator]();
    
    // next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를
    // 반환한다. 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
    console.log(iterator.next()); // { value: 1, done: false }
    console.log(iterator.next()); // { value: 2, done: false }
    console.log(iterator.next()); // { value: 3, done: false }
    console.log(iterator.next()); // { value: undefined, done: true }
    ```
    
- `value` 프로퍼티 : 현재 순회 중인 이터러블의 값
- `done` 프로퍼티 : 이터러블의 순회 완료 여부

# 2. 빌트인 이터러블

- 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블 제공
- `Array` : `Array.prototype[Symbol.iterator]`
- `String` : `String.prototype[Symbol.iterator]`
- `Map` : `Map.prototype[Symbol.iterator]`
- `Set` : `Set.prototype[Symbol.iterator]`
- `TypedArray` : `TypedArray.prototype[Symbol.iterator]`
- `arguments` : `arguments[Symbol.iterator]`
- `DOM` 컬렉션 : `NodeList.prototype[Symbol.iterator]` , `HTMLCollection.prototype[Symbol.iterator]`

# 3. `for ... of` 문

- 이터러블을 순회하면서 이터러블의 요소를 변수에 할당
    
    ```jsx
    for (변수선언문 of 이터러블) { ... }
    ```
    
    ```jsx
    for (변수선언문 in 객체) { ... }
    ```
    
- 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 `[[Enumerable]]` 의 값이 `true` 인 프로퍼티를 순회하며 열거
- 내부적으로 이터레이터의 `next` 메서드를 호출하여 이터러블을 순회하며 `next` 메서드가 반환한 이터레이터 리절트 객체의 `value` 프로퍼티 값을 `for ... of` 문의 변수에 할당
    
    ```jsx
    for (const item of [1, 2, 3]) {
      // item 변수에 순차적으로 1, 2, 3이 할당된다.
      console.log(item); // 1 2 3
    }
    ```
    
    ```jsx
    // 이터러블
    const iterable = [1, 2, 3];
    
    // 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터를 생성한다.
    const iterator = iterable[Symbol.iterator]();
    
    for (;;) {
      // 이터레이터의 next 메서드를 호출하여 이터러블을 순회한다. 이때 next 메서드는 이터레이터 리절트 객체를 반환한다.
      const res = iterator.next();
    
      // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true이면 이터러블의 순회를 중단한다.
      if (res.done) break;
    
      // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다.
      const item = res.value;
      console.log(item); // 1 2 3
    }
    ```
    

# 4. 이터러블과 유사 배열 객체

- 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 `length` 프로퍼티를 갖는 객체
    
    ```jsx
    // 유사 배열 객체
    const arrayLike = {
      0: 1,
      1: 2,
      2: 3,
      length: 3
    };
    
    // 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
    for (let i = 0; i < arrayLike.length; i++) {
      // 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
      console.log(arrayLike[i]); // 1 2 3
    }
    ```
    
- `Symbol.iterator` 메서드가 없기 때문에 `for ... of` 문으로 순회 불가능
    
    ```jsx
    // 유사 배열 객체는 이터러블이 아니기 때문에 for...of 문으로 순회할 수 없다.
    for (const item of arrayLike) {
      console.log(item); // 1 2 3
    }
    // -> TypeError: arrayLike is not iterable
    ```
    
- `arguments` , `NodeList` , `HTMLCollection` 은 유사 배열 객체이면서 이터러블
    
    ```jsx
    // 유사 배열 객체
    const arrayLike = {
      0: 1,
      1: 2,
      2: 3,
      length: 3
    };
    
    // Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다
    const arr = Array.from(arrayLike);
    console.log(arr); // [1, 2, 3]
    ```
    

# 5. 이터레이션 프로토콜의 필요성

- 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할

# 6. 사용자 정의 이터러블

## 1. 사용자 정의 이터러블 구현

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1]; // "36.1. 배열 디스트럭처링 할당" 참고
    const max = 10; // 수열의 최대값

    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고
    // next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur]; // "36.1. 배열 디스트럭처링 할당" 참고
        // 이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >= max };
      }
    };
  }
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출된다.
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}
```

```jsx
// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibonacci];
console.log(arr); // [ 1, 2, 3, 5, 8 ]

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [ 3, 5, 8 ]
```

## 2. 이터러블을 생성하는 함수

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블을 반환하는 함수. 수열의 최대값을 인수로 전달받는다.
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max };
        }
      };
    }
  };
};

// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
for (const num of fibonacciFunc(10)) {
  console.log(num); // 1 2 3 5 8
}
```

## 3. 이터러블이면서 이터레이터인 객체를 생성하는 함수

```jsx
// fibonacciFunc 함수는 이터러블을 반환한다.
const iterable = fibonacciFunc(5);
// 이터러블의 Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: true }
```

```jsx
// 이터러블이면서 이터레이터인 객체. 이터레이터를 반환하는 Symbol.iterator 메서드와
// 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
{
  [Symbol.iterator]() { return this; },
  next() {
    return { value: any, done: boolean };
  }
}
```

```jsx
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
  return {
    [Symbol.iterator]() { return this; },
    // next 메서드는 이터레이터 리절트 객체를 반환
    next() {
      [pre, cur] = [cur, pre + cur];
      return { value: cur, done: cur >= max };
    }
  };
};

// iter는 이터러블이면서 이터레이터다.
let iter = fibonacciFunc(10);

// iter는 이터러블이므로 for...of 문으로 순회할 수 있다.
for (const num of iter) {
  console.log(num); // 1 2 3 5 8
}

// iter는 이터러블이면서 이터레이터다
iter = fibonacciFunc(10);

// iter는 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```

## 4. 무한 이터러블과 지연 평가

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
      return { value: cur };
    }
  };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

- 지연 평가(Lazy Evaluation) : 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법
- 지연 평가 사용 시 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현 가능
