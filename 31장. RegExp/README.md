# 31장. RegExp

# 1. 정규 표현식(Regular Expression)이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(Formal Language)
- 패턴 매칭 기능 제공 : 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능
    
    ```jsx
    // 사용자로부터 입력받은 휴대폰 전화번호
    const tel = '010-1234-567팔';
    
    // 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
    const regExp = /^\d{3}-\d{4}-\d{4}$/;
    
    // tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
    regExp.test(tel); // -> false
    ```
    

# 2. 정규 표현식의 생성

- 정규 표현식 리터럴과 `RegExp` 생성자 함수 사용
- 패턴과 플래그로 구성
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 패턴: is
    // 플래그: i => 대소문자를 구별하지 않고 검색한다.
    const regexp = /is/i;
    
    // test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
    regexp.test(target); // -> true
    ```
    
- `RegExp` 생성자 함수를 사용하여 `RegExp` 객체 생성
    
    ```jsx
    /**
     * pattern : 정규 표현식의 패턴
     * flags : 정규 표현식의 플래그(g, i, m, u, y)
     */
    
    new RegExp(pattern[, flags])
    ```
    
    ```jsx
    const target = 'Is this all there is?';
    
    const regexp = new RegExp(/is/i); // ES6
    // const regexp = new RegExp(/is/, 'i');
    // const regexp = new RegExp('is', 'i');
    
    regexp.test(target); // -> true
    ```
    
- `RegExp` 생성자 함수를 사용하면 변수를 사용해 동적으로 `RegExp` 객체 생성 가능
    
    ```jsx
    const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;
    
    count('Is this all there is?', 'is'); // -> 3
    count('Is this all there is?', 'xx'); // -> 0
    ```
    

# 3. `RegExp` 메서드

## 1. `RegExp.prototype.exec`

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
- 매칭 결과가 없는 경우 `null` 반환
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/;
    
    regExp.exec(target); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
    ```
    
- 문자열 내의 모든 패턴을 검색하는 `g` 플래그를 지정해도 첫 번째 매칭 결과만 반환

## 2. `RegExp.prototype.test`

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/;
    
    regExp.test(target); // -> true
    ```
    

## 3. `RegExp.prototype.match`

- 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/;
    
    target.match(regExp); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
    ```
    
- `g` 플래그가 지정되면 모든 매칭 결과를 배열로 반환
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/g;
    
    target.match(regExp); // -> ["is", "is"]
    ```
    

# 4. 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용
    - `i` : Ignore Case, 대소문자를 구별하지 않고 패턴을 검색
    - `g` : Global, 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색
    - `m` : Multi Line, 문자열의 행이 바뀌더라도 패턴 검색을 계속함
    
    ```jsx
    const target = 'Is this all there is?';
    
    // target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
    target.match(/is/);
    // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
    
    // target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
    target.match(/is/i);
    // -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
    
    // target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
    target.match(/is/g);
    // -> ["is", "is"]
    
    // target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
    target.match(/is/ig);
    // -> ["Is", "is", "is"]
    ```
    

# 5. 패턴

- 문자열의 일정한 규칙을 표현하기 위해 사용
- `/` 로 열고 닫으며 문자열의 따옴표 생략
- 메타문자 또는 기호로 표현 가능

## 1. 문자열 검색

- 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
    const regExp = /is/;
    
    // target과 정규 표현식이 매치하는지 테스트한다.
    regExp.test(target); // -> true
    
    // target과 정규 표현식의 매칭 결과를 구한다.
    target.match(regExp);
    // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
    ```
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
    const regExp = /is/i;
    
    target.match(regExp);
    // -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
    ```
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 'is' 문자열과 매치하는 패턴.
    // 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
    const regExp = /is/ig;
    
    target.match(regExp); // -> ["Is", "is", "is"]
    ```
    

## 2. 임의의 문자열 검색

- `.` : 임의의 문자 한 개
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
    const regExp = /.../g;
    
    target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
    ```
    

## 3. 반복 검색

- `{m, n}` : 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열
- 콤마 뒤에 공백이 있으면 정상 동작하지 않음
    
    ```jsx
    const target = 'A AA B BB Aa Bb AAA';
    
    // 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
    const regExp = /A{1,2}/g;
    
    target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
    ```
    
- `{n}` : 앞선 패턴이 n번 반복되는 문자열, `{n, n}` 과 동일
    
    ```jsx
    const target = 'A AA B BB Aa Bb AAA';
    
    // 'A'가 2번 반복되는 문자열을 전역 검색한다.
    const regExp = /A{2}/g;
    
    target.match(regExp); // -> ["AA", "AA"]
    ```
    
- `{n, }` : 앞선 패턴이 최소 n번 이상 반복되는 문자열
    
    ```jsx
    const target = 'A AA B BB Aa Bb AAA';
    
    // 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
    const regExp = /A{2,}/g;
    
    target.match(regExp); // -> ["AA", "AAA"]
    ```
    
- `+` : 앞선 패턴이 최소 한번 이상 반복되는 문자열, `{1, }` 과 동일
    
    ```jsx
    const target = 'A AA B BB Aa Bb AAA';
    
    // 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
    const regExp = /A+/g;
    
    target.match(regExp); // -> ["A", "AA", "A", "AAA"]
    ```
    
- `?` : 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열, `{0, 1}` 과 동일
    
    ```jsx
    const target = 'color colour';
    
    // 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
    const regExp = /colou?r/g;
    
    target.match(regExp); // -> ["color", "colour"]
    ```
    

## 5. OR 검색

- `|` : or
    
    ```jsx
    const target = 'A AA B BB Aa Bb';
    
    // 'A' 또는 'B'를 전역 검색한다.
    const regExp = /A|B/g;
    
    target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
    ```
    
- 분해되지 않은 단어 레벨로 검색하기 위해서는 `+` 함께 사용
    
    ```jsx
    const target = 'A AA B BB Aa Bb';
    
    // 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
    // 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
    const regExp = /A+|B+/g;
    
    target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
    ```
    
- `[ ]` : 내의 문자는 or로 동작
    
    ```jsx
    const target = 'A AA B BB Aa Bb';
    
    // 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
    // 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
    const regExp = /[AB]+/g;
    
    target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
    ```
    
- 범위를 지정하려면 `[ ]` 내에 `-` 사용
    
    ```jsx
    const target = 'A AA BB ZZ Aa Bb';
    
    // 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
    // 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
    const regExp = /[A-Z]+/g;
    
    target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]
    ```
    
- 대소문자를 구별하지 않고 알파벳을 검색하는 방법
    
    ```jsx
    const target = 'AA BB Aa Bb 12';
    
    // 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
    const regExp = /[A-Za-z]+/g;
    
    target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
    ```
    
- 숫자를 검색하는 방법
    
    ```jsx
    const target = 'AA BB 12,345';
    
    // '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
    const regExp = /[0-9]+/g;
    
    target.match(regExp); // -> ["12", "345"]
    ```
    
    ```jsx
    const target = 'AA BB 12,345';
    
    // '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
    const regExp = /[0-9,]+/g;
    
    target.match(regExp); // -> ["12,345"]
    ```
    
- `\d` : 숫자, [0-9]와 동일
- `\D` : 숫자가 아닌 문자
    
    ```jsx
    const target = 'AA BB 12,345';
    
    // '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
    let regExp = /[\d,]+/g;
    
    target.match(regExp); // -> ["12,345"]
    
    // '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
    regExp = /[\D,]+/g;
    
    target.match(regExp); // -> ["AA BB ", ","]
    ```
    
- `\w` : 알파벳, 숫자, 언더스코어, [A-Za-z0-9_]와 동일
- `\W` : 알파벳, 숫자, 언더스코어가 아닌 문자
    
    ```jsx
    const target = 'Aa Bb 12,345 _$%&';
    
    // 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
    let regExp = /[\w,]+/g;
    
    target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]
    
    // 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
    regExp = /[\W,]+/g;
    
    target.match(regExp); // -> [" ", " ", ",", " ", "$%&"]
    ```
    

## 5. NOT 검색

- `[ ... ]` 내의 `^` : not
    
    ```jsx
    const target = 'AA BB 12 Aa Bb';
    
    // 숫자를 제외한 문자열을 전역 검색한다.
    const regExp = /[^0-9]+/g;
    
    target.match(regExp); // -> ["AA BB ", " Aa Bb"]
    ```
    

## 6. 시작 위치로 검색

- `[ ... ]` 밖의 `^` : 문자열의 시작
    
    ```jsx
    const target = 'https://poiemaweb.com';
    
    // 'https'로 시작하는지 검사한다.
    const regExp = /^https/;
    
    regExp.test(target); // -> true
    ```
    

## 7. 마지막 위치로 검색

- `$` : 문자열의 마지막
    
    ```jsx
    const target = 'https://poiemaweb.com';
    
    // 'com'으로 끝나는지 검사한다.
    const regExp = /com$/;
    
    regExp.test(target); // -> true
    ```
    

# 6. 자주 사용하는 정규표현식

## 1. 특정 단어로 시작하는지 검사

```jsx
const url = 'https://example.com';

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
```

```jsx
/^(http|https):\/\//.test(url); // -> true
```

## 2. 특정 단어로 끝나는지 검사

```jsx
const fileName = 'index.html';

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

## 3. 숫자로만 이루어진 문자열인지 검사

```jsx
const target = '12345';

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

## 4. 하나 이상의 공백으로 시작하는지 검사

- `\s` : `[\t\r\n\v\f]` 와 동일

```jsx
const target = ' Hi!';

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

## 5. 아이디로 사용 가능한지 검사

```jsx
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

## 6. 메일 주소 형식에 맞는지 검사

```jsx
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
```

```jsx
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```

## 7. 핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

## 8. 특수 문자 포함 여부 검사

```jsx
const target = 'abc#123';

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
(/[^A-Za-z0-9]/gi).test(target); // -> true
```

```jsx
(/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // -> true
```

- 특수 문자 제거 → `String.prototype.replace` 메서드 사용

```jsx
target.replace(/[^A-Za-z0-9]/gi, ''); // -> abc123
```
