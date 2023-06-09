# 08장. 제어문(Control Flow Statement)

- 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용
- 코드의 실행 흐름을 인위적으로 제어 가능
- `foreach, map, filter, reduce` 같은 고차 함수를 사용한 함수형 프로그래밍 기법 → 제어문의 사용 억제 및 복잡성 해결

# 1. 블록문(Block Statement / Compound Statement)

- 0개 이상의 문을 중괄호로 묶은 것
- 코드 블록, 블록
- 하나의 실행 단위로 취급
- 제어문이나 함수를 정의할 때 사용
- 문의 끝에 세미콜론을 붙이는 것이 일반적
- 블록문의 끝에는 세미콜론을 붙이지 않음
    
    ```jsx
    // 블록문
    {
      var foo = 10;
    }
    
    // 제어문
    var x = 1;
    if (x < 10) {
      x++;
    }
    
    // 함수 선언문
    function sum(a, b) {
      return a + b;
    }
    ```
    

# 2. 조건문(Conditional Statement)

- 주어진 조건식(Conditional Expression)의 평가 결과에 따라 코드 블록의 실행을 결정
- 불리언 값으로 평가될 수 있는 표현식

## 1. `if ... else` 문

- 논리적 참 또는 거짓에 따라 실행할 코드 블록 결정
    
    ```jsx
    if (조건식) {
    	// 조건식이 참이면 이 코드 블록이 실행된다.
    } else {
    	// 조건식이 거짓이면 이 코드 블록이 실행된다.
    }
    ```
    
    ```jsx
    if (조건식1) {
    	// 조건식1이 참이면 이 코드 블록이 실행된다.
    } else if (조건식2) {
    	// 조건식2가 참이면 이 코드 블록이 실행된다.
    } else {
    	// 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
    }
    ```
    
    ```jsx
    var num = 2;
    var kind;
    
    // if 문
    if (num > 0) {
      kind = '양수'; // 음수를 구별할 수 없다
    }
    console.log(kind); // 양수
    
    // if...else 문
    if (num > 0) {
      kind = '양수';
    } else {
      kind = '음수'; // 0은 음수가 아니다.
    }
    console.log(kind); // 양수
    
    // if...else if 문
    if (num > 0) {
      kind = '양수';
    } else if (num < 0) {
      kind = '음수';
    } else {
      kind = '영';
    }
    console.log(kind); // 양수
    ```
    
    ```jsx
    var num = 2;
    var kind;
    
    if (num > 0) kind = '양수';
    else if (num < 0) kind = '음수';
    else kind = '영';
    
    console.log(kind); // 양수
    ```
    
    ```jsx
    // x가 짝수이면 result 변수에 문자열 '짝수'를 할당하고, 홀수이면 문자열 '홀수'를 할당한다.
    var x = 2;
    var result;
    
    if (x % 2) { // 2 % 2는 0이다. 이때 0은 false로 암묵적 강제 변환된다.
      result = '홀수';
    } else {
      result = '짝수';
    }
    
    console.log(result); // 짝수
    ```
    
    ```jsx
    var x = 2;
    
    // 0은 false로 취급된다.
    var result = x % 2 ? '홀수' : '짝수';
    console.log(result); // 짝수
    ```
    
    ```jsx
    var num = 2;
    
    // 0은 false로 취급된다.
    var kind = num ? (num > 0 ? '양수' : '음수') : '영';
    
    console.log(kind); // 양수
    ```
    

## 2. `switch` 문

- 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮김
- 상황을 의미하는 표현식을 지정하고 콜론으로 마무리
    
    ```jsx
    switch (표현식) {
    	case 표현식1:
    		switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    		break;
    	case 표현식2:
    		switch 문의 표현식과 표현식2이 일치하면 실행될 문;
    		break;
    	default:
    		switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
    }
    ```
    
- 다양한 상황에 따라 실행할 코드 블록을 결정할 때 사용
    
    ```jsx
    // 월을 영어로 변환한다. (11 → 'November')
    var month = 11;
    var monthName;
    
    switch (month) {
      case 1: monthName = 'January';
      case 2: monthName = 'February';
      case 3: monthName = 'March';
      case 4: monthName = 'April';
      case 5: monthName = 'May';
      case 6: monthName = 'June';
      case 7: monthName = 'July';
      case 8: monthName = 'August';
      case 9: monthName = 'September';
      case 10: monthName = 'October';
      case 11: monthName = 'November';
      case 12: monthName = 'December';
      default: monthName = 'Invalid month';
    }
    
    console.log(monthName); // Invalid month
    ```
    
- 폴스루(Fall Through) : 문을 실행한 후 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 이후의 모든 case 문과 default 문을 실행
- break 문은 코드 블록에서 탈출하는 역할
    
    ```jsx
    // 월을 영어로 변환한다. (11 → 'November')
    var month = 11;
    var monthName;
    
    switch (month) {
      case 1: monthName = 'January';
        break;
      case 2: monthName = 'February';
        break;
      case 3: monthName = 'March';
        break;
      case 4: monthName = 'April';
        break;
      case 5: monthName = 'May';
        break;
      case 6: monthName = 'June';
        break;
      case 7: monthName = 'July';
        break;
      case 8: monthName = 'August';
        break;
      case 9: monthName = 'September';
        break;
      case 10: monthName = 'October';
        break;
      case 11: monthName = 'November';
        break;
      case 12: monthName = 'December';
        break;
      default: monthName = 'Invalid month';
    }
    
    console.log(monthName); // November
    ```
    
    ```jsx
    var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
    var month = 2;
    var days = 0;
    
    switch (month) {
      case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
      case 4: case 6: case 9: case 11:
        days = 30;
        break;
      case 2:
        // 윤년 계산 알고리즘
        // 1. 연도가 4로 나누어떨어지는 해(2000, 2004, 2008, 2012, 2016, 2020...)는 윤년이다.
        // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해(2000, 2100, 2200...)는 평년이다.
        // 3. 연도가 400으로 나누어떨어지는 해(2000, 2400, 2800...)는 윤년이다.
        days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
        break;
      default:
        console.log('Invalid month');
    }
    
    console.log(days); // 29
    ```
    

# 3. 반복문(Loop Statement)

- 조건식의 평가 결과가 참인 경우 코드 블록 실행
- 조건식이 거짓일 때까지 반복

## 1. `for` 문

- 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행
    
    ```jsx
    for (변수 선언문 또는 할당문; 조건식; 증감식) {
    	조건식이 참인 경우 반복 실행될 문;
    }
    ```
    
    ```jsx
    for (var i = 0; i < 2; i++) {
      console.log(i);
    }
    ```
    
    ```jsx
    for (var i = 1; i >= 0; i--) {
      console.log(i);
    }
    ```
    
- 어떤 식도 선언하지 않으면 무한루프
    
    ```jsx
    // 무한루프
    for (;;) { ... }
    ```
    
- 중첩 `for` 문 가능
    
    ```jsx
    for (var i = 1; i <= 6; i++) {
      for (var j = 1; j <= 6; j++) {
        if (i + j === 6) console.log(`[${i}, ${j}]`);
      }
    }
    ```
    

## 2. `while` 문

- 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행
- 반복 횟수가 불명확할 때 주로 사용
- 조건문의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료
    
    ```jsx
    var count = 0;
    
    // count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
    while (count < 3) {
      console.log(count); // 0 1 2
      count++;
    }
    ```
    
    ```jsx
    // 무한루프
    while (true) { ... }
    ```
    
    ```jsx
    var count = 0;
    
    // 무한루프
    while (true) {
      console.log(count);
      count++;
      // count가 3이면 코드 블록을 탈출한다.
      if (count === 3) break;
    } // 0 1 2
    ```
    

## 3. `do ~ while` 문

- 코드 블록을 먼저 실행하고 조건식을 평가
- 무조건 한 번 이상 실행
    
    ```jsx
    var count = 0;
    
    // count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
    do {
      console.log(count);
      count++;
    } while (count < 3); // 0 1 2
    ```
    

# 4. `break` 문

- 코드 블록 탈출
- 레이블 문, 반복문(for, for ~ in, for ~ of, while, do ~ while) 또는 switch 문의 코드 블록 탈출
    
    ```jsx
    if (true) {
      break; // Uncaught SyntaxError: Illegal break statement
    }
    ```
    
- 레이블 문(Label Statement) : 식별자가 붙은 문
    
    ```jsx
    // foo라는 레이블 식별자가 붙은 레이블 문
    foo: console.log('foo');
    ```
    
- 레이블 문으로 프로그램 실행 순서 제어
    
    ```jsx
    // foo라는 식별자가 붙은 레이블 블록문
    foo: {
      console.log(1);
      break foo; // foo 레이블 블록문을 탈출한다.
      console.log(2);
    }
    
    console.log('Done!');
    ```
    
    ```jsx
    // outer라는 식별자가 붙은 레이블 for 문
    outer: for (var i = 0; i < 3; i++) {
      for (var j = 0; j < 3; j++) {
        // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
        if (i + j === 3) break outer;
        console.log(`inner [${i}, ${j}]`);
      }
    }
    
    console.log('Done!');
    ```
    
    ```jsx
    var string = 'Hello World.';
    var search = 'l';
    var index;
    
    // 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 문자열의 개별 문자가 'l'이면
      if (string[i] === search) {
        index = i;
        break; // 반복문을 탈출한다.
      }
    }
    
    console.log(index); // 2
    
    // 참고로 String.prototype.indexOf 메서드를 사용해도 같은 동작을 한다.
    console.log(string.indexOf(search)); // 2
    ```
    

# 5. `continue` 문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름 이동
- break 문처럼 반복문을 탈출하지 않음
    
    ```jsx
    var string = 'Hello World.';
    var search = 'l';
    var count = 0;
    
    // 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
      if (string[i] !== search) continue;
      count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
    }
    
    console.log(count); // 3
    
    // 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
    const regexp = new RegExp(search, 'g');
    console.log(string.match(regexp).length); // 3
    ```
    
    ```jsx
    for (var i = 0; i < string.length; i++) {
      // 'l'이면 카운트를 증가시킨다.
      if (string[i] === search) count++;
    }
    ```
    
    ```jsx
    // continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이면 카운트를 증가시킨다.
      if (string[i] === search) {
        count++;
        // code
        // code
        // code
      }
    }
    
    // continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이 아니면 카운트를 증가시키지 않는다.
      if (string[i] !== search) continue;
    
      count++;
      // code
      // code
      // code
    }
    ```
