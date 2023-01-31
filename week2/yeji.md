# 8장: 제어문

## 블록문 & 조건문

- 블록문: 0개 이상의 문을 {}로 묶은 것. 주로 제어문, 함수 정의할 때 사용.
- 조건문:
  - if...else
  - switch: C-family 는 대부분 지원하는 구문
  ```
  switch(표현식) {
      case 표현식1:
          어쩌구
          break; // fall through 되지 않게 탈출하게
      default:
          저쩌구;
  }
  ```
  fall through를 사용하며 이런 표현도 가능
  ```
  switch(표현식) {
    case 표현식1: 표현식 2: 표현식 3:
        어쩌구
        break;
    case 표현식 4: 표현식 5:
        저쩌구
        break;
    default:
        어쩌구 저쩌구
  }
  ```

## 반복문

반복문을 대체할 수 있는 것들

- forEach
- for ... in
- for ... of

1. for 문

```
for (let i = 0; i < 2; i++) {
    console.log(i);
}
```

2. while 문
3. do...while
   선 실행 후 조건문 평가

```
let count = 0;

do {
    console.log(count);
    count++;
} while (count < 3)
```

## break 문

레이블문, 반복문 탈출
이 외에 break 사용하면 SyntaxError(문법 에러)

### 레이블 문

식별자가 붙은 문

```
// foo라는 식별자가 붙은 레이블 블록문
foo: {
    console.log(1);
    break foo;
    console.log(2);
}
console.log('Done');
// 1 과 Done 출력
// break 가 없다면, 1 2 Done 출력
```

중첩된 for 문 탈출에 효과적, 이밖에는 권장X (흐름 복잡, 가독성 나빠짐)

```
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log('Done!');

// inner [0, 0]
// inner [0, 1]
// inner [0, 2]
// inner [1, 0]
// inner [1, 1]
// Done!
```

> inner [0, 0] inner [0, 1] inner [0, 2]만 출력되고 끝나야할 것 같은데 왜 계속 진행되지?

### continue 문

반복문의 코드 블록 실행을 현 지점에서 중단하고, 반복문의 증감식으로 실행 흐름을 이동

# 9장: 타입 변환과 단축 평가

타입 변환

- 명시적, 암묵적 변환 모두 기존의 원시 값을 사용해 다른 타입의 **새로운** 원시 값을 생성. 기존 원시값 직접 변경 X

## 암묵적 타입 변환

개발자의 의도와 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환.
새로운 타입 값을 만들어 **단 한 번 사용하고 버린다**.

### 불리언 타입으로 변환

- Truthy 값: 참으로 평가되는 값
- False 값: 거짓으로 평가되는 값
  ex) false, undefined, null, 0, -0, NaN, ''

## 명시적 타입 변환

### 문자열 타입으로 변환

- String 생성자 함수를 new 없이 호출 `Stirng(1); // "1"`
- Object.prototype.toString 메서드 `(1).toString(); // "1"`
- 문자열 연결 연산자 이용 `1 + ''; // "1"`

### 숫자 타입으로 변환

- Number 생성자 함수를 new 연산자 없이 호출 `Number('0'); // 0`
- parseInt, parseFloat 함수 `parseInt("0");`
- \+ 단항 산술 연산자

```
+true; //1
+'0'; //0
```

- \* 산술 연산자

```
false * 1; // 0
'0' * 1; // 0
```

### 불리언 타입으로 변환

- Boolean 생성자를 new 없이 호출 `Boolean('x'); // true`
- !부정 논리 연산자 두번 사용 `!!NaN; // false`

## 단축 평가

타입 변환하지 않고 **그대로** 반환
&&, || 연산자 표현식의 결과는 2개의 피연산자 중 어느 한쪽으로 평가

- &&: 두개의 피연산자가 모두 true일 때 true 반환

```
'Cat' && 'Dog' // 둘다 true 이므로 두 번째 피연산자 반환
```

- ||: 두개의 피연산자 중 하나만 true여도 true 반환

```
'Cat' || 'Dog' // 이미 'Cat'에서 true 이므로 첫 번째 연산자만 보고 반환
```

### 단축 평가를 활용한 구문

> 이해는 했지만 직관적이지 않아서 코드로 썼을 때 유용할지는 의문

1. 변수가 있는지 확인하고 프로퍼티 참조

```
var elem = null;
var value = elem && elem.value;
// elem이 있으면 value는 elem.value
// elem이 null이면 value는 null
```

2. 함수 배개변수에 기본값 설정

```
function getStringLength(str){
    str = str || ''; // str이 없으면 str = '';
    return str.length;
}
```

### 옵셔널 체이닝 연산자

- 옵셔널 체이닝 연산자 ?.가 도입되기 이전에는 논리연산자 &&를 사용한 단축 평가로 변수 null, define 체크

```
var value = elem && elem.value;
var value = elem?.value;
```

- 좌항 피연산자가 Falsy 값이어도 우항 프로퍼티 참조함

```
var str ='';
var length = str?.length;
console.log(length); // 0
```

### null 병합 연산자

??는 좌항의 피연산자가 null or undefined -> 우항 피연산자 반환, 아니면 -> 좌항 피연산자 반환
Falsy 값이 기본 값으로 유용한 경우 잘 사용할 수 있음 ex) 기본값으로 빈 문자열 설정하고 싶을 때

```
function setDefaultString(str) {
let foo = str ?? 'default string';
// 만약 let foo = str || 'default string'; 이었다면
// ''는 foo가 될 수 없지만, ??를 사용하면 ''도 기본값으로 쓸 수 있다
return foo;
};
```

## 10장: 객체 리터럴

객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조
원시 값은 immutable value <-> 객체 타입은 mutable value
함수는 일급 객체 이므로 프로퍼티 값(메서드)으로 사용 가능

### 객체 생성 방법

- 객체 리터럴
  - {}내 0개 이상 프로퍼티 정의
  - 변수 할당 시점 엔진이 객체 리터럴 해석해 객체 생성
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

### 프로퍼티

- 프로퍼티 키: 문자열 또는 심벌 값
  - 자바스크립트 네이밍 규칙을 따르는 프로퍼티 키는 '', "" 생략 가능
  - 문자열이 아니라면 문자열로 암묵적 타입변환
  - 프로퍼티 키를 중복 선언하면, 나중에 선언한 프로퍼티가 덮어씀. 에러X
- 프로퍼티 값: 자바스크립트 사용할 수 있는 모든 값
  - 존재하지 않는 프로퍼티에 값을 할당하면, 프로퍼티가 동적으로 생성된다
- delete: 프로퍼티를 삭제, 존재하지 않는 프로퍼티를 삭제하려고 해도 에러X

#### 접근 방법

- 마침표 표기법 `person.name`
- 대괄호 표기법 `person['name']`
  - 프로퍼티 키는 문자열이니, 문자열로 검색하기 -> 아니면 ReferenceError `person[name]`
  - 예외) 프로퍼티 키가 숫자로 이뤄진 문자열이라면 따옴표 생략 가능
- 존재하지 않는 프로퍼티에 접근하면 에러가 아니라 undefined
- 네이밍 규칙을 준수하지 않은 프로퍼티 키라면 반드시 **대괄호 표기법** 사용해야
  > 이상한 에러를 마주하지 않으려면 네이밍 규칙을 따르자!!!!

### 객체 리터럴의 확장 기능

#### 프로퍼티 키 생략

프로퍼티 값으로 변수를 생성하려하고, 프로퍼티 키 = 변수 이름이라면 **프로퍼티 키 생략** 가능

```
let x = 1;
let y = 2;

const obc = { x, y };
console.log(obj); // { x: 1, y: 2 };
```

#### 프로퍼티 키 동적 생성

표현식을 대괄호로 묶기(= 계산된 프로퍼티 이름)

```
obj[prefix + '-' + ++i] = i;

const obj = {
    [`${prefix}-${++i}`]:i
}
```

#### 메서드 축약 표현

ES5 프로퍼티 값으로 함수 선언식 할당

```
// ES5
var obj = {
  name: 'Lee',
  // 프로퍼티 sayHi의 값으로 할당된 것은 일반 함수(constructor)로 정의된 함수이다.
  // 현재 ECMAScript 사양에서 메서드로 인정되지 않는다.
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee

// ES5의 메서드 선언 방식은 현재 ECMAScript 사양에서는 일반 함수로 정의된다.
// 일반 함수는 constructor이므로 new로 인스턴스를 생성할 수 있다.
new obj.sayHi(); // sayHi {}
```

ES6 function 생략 가능, 이것만 메서드로 인정

```
// ES6 메서드 축약표현만 메서드로 인정된다.
const obj = {
  name: 'Lee',
  // 메소드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // 일반함수 호출: Hi! Lee
// 메소드는 non-constructor이므로 생성자 함수로 호출할 수 없다.
new obj.sayHi(); // Uncaught TypeError: obj.sayHi is not a constructor
```
