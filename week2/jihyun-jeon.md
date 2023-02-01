# 08장. 제어문

## **✅**  조건문

switch문 보다 if…else를 사용하는게 좋음.

그러나 조건이 너무 많으면 switch를 쓰는게 좋음 ← 가독성 측면상 더 좋기 때문

<br/>

### switch문

- 기본 형태

```jsx
const month = 3;
let days = 0;

switch (month) {
  case 1:
    days = 1;
    break; // break를 써야 switch문이 종료됨.

  case 2:
    days = 2;
    break;

  default: // default문은 선택
    days = 3;
}

// console.log(days);
```

- 여러 개의 case를 하나의 조건으로 사용할 수도 있다.

```jsx
const month = 4;
let days = 0;

switch (month) {
  case 1:
  case 3:
  case 5:
    days = 31;
    break;

  case 4:
  case 6:
  case 9:
    days = 30;
    break;

  default:
    days = 29;
}

// console.log(days);
```

<br/>

## **✅**  반복문

for문은 반복 횟수가 명활할 때, while문은 반복 횟수가 불명확할 때 주로 사용함.

### while문 예제

```jsx
let num = 3;

while (num > 0) {
  num--;
  console.log(num); // 2 1 0
}
```

```jsx
let count = 0;

while (true) {
  count++;
  console.log(count);

  if (count === 3) {
    break; // while문 중단됨.
  }
}
```

### do while문 예제

코드 블록을 먼저 한번은 실행하고 조건식을 평가한다.

```jsx
let num = 4;

do {
  num--;
  console.log(num); // 3 2 1 0
} while (num > 0);
```

<br/>

## **✅**  break, continue

### break

: break를 사용하여 1)레이블 문, 2)반복문(for문, for…of, while,등), 3)switch문 의 loop을 중단시킬 수 있다.

: 이 세가지 문의 코드 블록 외에 break문을 사용하면 SyntaxError가 발생한다.

```jsx
if(true){
	break; // SyntaxError
}
```

### continue

: 반복문은 탈출하진 않지만, 현 지점의 코드 블록 실행을 중단하고 바로 반복문의 증감식으로 코드 실행이 이동된다.

<br/>
<br/>

<hr/>

# 9장. 타입 변환과 단축 평가

## ✅ 산술 연산자의 암묵적 타입 변환

- 산술 연산자의 피연산자는 숫자 타입이여야 한다.
- 그러나 피연산자가 숫자 타입이 아닌 경우에 그 형을 `"숫자형"`으로 암묵적 타입 변환한다.<br/>
  (이때, 피연산자를 숫자 타입으로 변환할 수 없는 경우 표현식의 평가 결과는 NaN이 된다.)

[이항 산술 연산자]

```jsx
// [예제1]
1 + true; // 2 (number타입)
1 + null; // 0 (number타입)

// [예제2]
6 - "2"; // 4 (number타입)

// [예제3]
"6" / "2"; // 3 (number타입)

// [예제4] - 피연산자를 숫자 타입으로 변환할 수 없는 경우, NaN
1 / "one" + // NaN (number타입)
  undefined + // NaN
  "a"; // NaN
```

[단항 산술 연산자]

- “숫자 타입이 아닌 피연산자”에 “(+,-)단항연산자”를 쓰면, `"숫자형"`으로 바뀜.

`+"1234"` // 1234 (문자열을 숫자형으로 타입 변환한다)<br/>
`+false` // 0 (불린타입을 숫자형으로 타입 변환한다)<br/>

`-'1234'` // -1234 (문자열을 숫자형으로 타입 변환한다)<br/>
`-true` // -1 (불린타입을 숫자형으로 타입 변환한다)<br/>

<br/>

### 📌 특이

: 덧셈 연산자(+)는 좀 특이함.

**덧셈 연산자(+)**

- 원칙
  : `산술 연산자`.  
  : 피연산자를 `"숫자형"`으로 타입 변환함

- 예외
  : “피연산자 중 하나 이상이 문자열인 경우” `문자열 연결 연산자`로 동작!<br/>
  : 피연산자를 `"문자열"`로 타입 변환함

```jsx
// <숫자 + 문자>
1 + "true"; // "1true"
2 + "1"; // "21"

// <불리언 + 문자>
true + ""; // "true"

// <null + 문자>
null + ""; // "null"

// <undefined + 문자>
undefined + ""; // "undefined"
```

## ✅ truthy, falsy value

헷갈리는 truthy 값들

- `[],{}, function(){}` - 빈 배열, 빈 객체, 빈 함수
- “0”, “false” - 빈 문자열이 아니므로

헷갈리는 falsy 값들

- null , undefined, NaN

<br/>

## ✅ 단축 평가

### 1. 논리 연산자를 사용한 단축 평가

- || : 왼쪽 피연산자가 평가 결과를 결정한다. (왼쪽이 true면 → 왼쪽값 그대로 반환)
- && : 오른쪽 피연산자가 평가 결과를 결정한다. (왼쪽이 true면, 오른쪽 평가 → 오른쪽값 그대로 반환)

```jsx
"Cat" || "Dog // "Cat"
false || "Dog" // "Dog"
"Cat" || false // "Cat"

let el = "";
el && el.value; // → "" (왼쪽이 falsy이므로 왼쪽값 그대로 출력)

"Cat" && "Dog // "Dog"
false && "Dog" // false
"Cat" && false // false
```

<br/>

### 2. 옵셔널 체이닝과 &&연산자

옵셔널 체이닝이 있기 전엔 &&논리연산자를 사용했다.

```jsx
let el = null;
let value = el && el.value; // &&논리연산자
let value = el?.value; // 옵셔널 체이닝
```

📌 **차이점**

- &&연산자 : `falsy값 인지`여부로 좌항 피연산자를 평가함
- 옵셔널 체이닝: 오직 `null또는 undefined 인지` 여부로 좌항 피연산자를 판단 (따라서 좌항 피연산자가 falsy값 이라도 우항 평가 가능)

```jsx
let el = "";
let value = el && el.length; // ""
let value = el?.length; // 0 (빈string 이여도 우항 프로퍼티 참조 가능)
```

<br/>

### 3. 옵셔널 체이닝과 ??(null 병합 연산자)

📌 **차이점**

- 옵셔녈 체이닝 : null, undefined가 “아닐때” 우항 참조 ,
- ?? : null, undefined “일 때” 우항 반환

```jsx
let el = null;
let value = el?.length; // undefined

let value = null || "b"; // "b"
```

📌 **공통점**

오직 `null또는 undefined 인지` 여부로 좌항 피연산자를 판단

```jsx
let el = "";
let value = el?.length; // 0  (falsy값이여서 null, undefined이 아니므로 우항 참조 가능)
let value = el ?? length; // "" (falsy값이라도 null, undefined가 아니므로 좌항 반환)
```

<br/>
<br/>

<hr/>

# 10. 객체 리터럴

## ✅  객체란

: 데이터를 의미하는 프로퍼티(property)와 데이터를 참조하고 조작할 수 있는 동작(behavior)을 의미하는 메소드(method)로 구성된 집합이다.<br/>
: 객체를 사용하면 여러 상태와 동작을 하나의 단위로 구조화 할 수 있어 유용함.

## ✅  객체의 형태

프로프티 키

: 모든 문자열 또는 심벌 값 <br/>
: 네이밍 규칙을 지킨 경우 따옴표 생략 가능 ex) name<br/>
: 그러나, 네이밍 규칙을 지키지 않은 경우 따옴표 꼭 붙여줘야 함. ex)"2name", “@name”

프로프티 값

: 프로퍼티 값에 “모든 자료형”이 허용된다.<br/>
: 때문에 함수도 프로퍼티 값으로 사용할 수 있는데, 이 경우 일반 함수와 구분하기 위해 “메서드”라고 부른다.

```jsx
// user라는 객체 선언함.

let user = {
  name: "John", //  키: "name"    , 값: "John" (자료형 : 문자열)
  age: 30, //  키: "age"     , 값: 30 (자료형 : 숫자형)
  activity: function () {
    //  키: "activity", 값: 함수 (자료형 : 함수형) <-메서드
    console.log("surfing");
  },
};
```

<br/>

## ✅  객체의 형태

### 1. 마침표 표기법

: 키 이름이 네이밍 규칙을 준수한 '유효한 변수 식별자’인 경우에 사용함.

```javascript
var person = {
  name: "Lee",
  age: 20,
};

// 1.마침표 표기법으로 접근
console.log(person.name); // Lee
console.log(person.age); // 20
```

### 2. 대괄호 표기법

: 대괄호 표기법 안에서 문자열을 사용할 땐 문자열을 **따옴표로** 묶어줘야 함! ex) ["name"] , [”age”]<br/>
: 따옴표가 없다면 (변수명과 같은) 식별자로 해석한다.

- 언제 사용하는지?

  **1) 프로퍼티 키가 네이밍 규칙을 준수한 이름일 땐, 마침표 표기법이든 대괄호 표기법이든 상관없음.**

  ```jsx
  var person = {
    name: "Lee",
    age: 20,
  };
  // 2.대괄호 표기법으로 접근도 가능
  console.log(person["name"]); // Lee
  console.log(person["age"]); // 20
  ```

    <br/>

  **2) 프로퍼티 키가 네이밍 규칙을 준수하지 않은 이름일 때**

  ```jsx
  var person = {
    "last^name": "Lee", // 식별자 이름에 ^라는 특수문자 안되는데 사용함 (네이밍 규칙 준수x)
    1: 20, // 식별자 이름 첫글자에 숫자형 안되는데 사용함 (네이밍 규칙 준수x)
  };

  console.log(person["last^name"]); // Lee
  console.log(person["1"]); // 20
  ```

    <br/>

  **3) 변수명으로 프로퍼티 키에 접근할 때**

  - [변수명] ← 따옴표 없이 바로 변수명 씀.
  - for (const key in menu) 에서 변수key에 menu객체의 프로퍼티 키가 할당됨.: `.key`로는 접근은 안됨. 객체 안에 key라는 프로퍼티 키가 있어야 접근 가능한 것임.

  ```jsx
  // menu 객체를 순회하면서 각 프로퍼티 값을 콘솔로 찍어봄.
  function multiplyNumeric() {
    const menu = {
      width: 200,
      height: 300,
      title: "My menu",
    };

    for (const key in menu) {
      console.log(menu[key]); // 200 , 300 , My menu
    }
  }

  multiplyNumeric();
  ```

<br/>

## ✅  es6에서 추가된 객체 리터럴 기능

### 📌 계산된 프로퍼티 이름

프로퍼티 키를 동적으로 생성하려면 : 키로 사용할 표현식을 “대괄호”로 묶어야 한다.

```jsx
let fix = "prop";
let i = 0;

// 예제1
let obj = {};
obj[fix + "-" + ++i] = i;

// 예제2
const obj = {
  [`${fix}-${++i}`]: i,
};
```

<br/>

### 📌 프로퍼티 축약 표현

1. 프로퍼티 "값"으로 변수를 사용하고

2. 프로퍼티 값과 키가 동일한 이름일 때

3. 프로퍼티 키를 생략할 수 있다.

```jsx
let x = 1,
  y = 2;

const obj = { x, y }; // {x:1,y:2}
```

<br/>

### 📌 메서드 축약 표현

객체에 메서드를 정의할 때 function 키워드 생략 가능한다.

```jsx
let obj = {
  name: "Lee",
  sayHi: function () {
    console.log("Hi");
  },
};

// 축약표현
let obj = {
  name: "Lee",
  sayHi() {
    console.log("Hi");
  },
};
```

---

<br/>

### 🔶 js에서의 객체

- js는 “객체 기반의 프로그래밍 언어”다.
- (원시값을 제외한) js를 구성하는 모든 것(함수, 배열, 정규 표현식 등)은 객체다. <br/> → 이는 객체 지향 프로그래밍과 연관된 개념

### 🔶 배열의 타입이 객체인 이유

`typeof [] → object`자바스크립트의 배열은 엄밀히 말해 일반적 의미의 배열이 아니다.자바스크립트의 배열은 일반적인 배열의 동작을 흉내낸 특수한 “객체”이다.

- 사실, 배열의 인덱스는 프로퍼티 키 이고, 배열의 요소는 프로퍼티 값이다.<br/>
  (참고 사이트 : [https://poiemaweb.com/js-array-is-not-arrray](https://poiemaweb.com/js-array-is-not-arrray))
- prototype chain: prototype chain 으로 인한 "자바스크립트의 모든 것은 객체다"라는 원리 때문에,배열은 확장된 객체인 것임!
