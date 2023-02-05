# 11. 원시 값과 객체의 비교

### 📌 요약

|                                            | 원시 타입                 | 객체 타입                       |
| ------------------------------------------ | ------------------------- | ------------------------------- |
| 변수에 할당하면?                           | 실제 값이 저장됨          | 참조값이 저장됨                 |
| 각 타입의 값                               | 원시값 : 변경 불가능한 값 | 객체 타입의 값 : 변경 가능한 값 |
| 원시값을 갖는 변수를 다른 변수에 할당하면? | 원시값이 복사됨.          | 참조값이 복사됨.                |

<br/>

※ 변수명(식별자) : 변수 값이 저장되는 ‘메모리 공간’을 지칭하기 위해 붙인 이름이고 <br/>
※ 변수의 값은 : 메모리에 저장된 ‘데이터 자체’이다.

---

<br/> 
<br/>

## ✅ 원시타입

### 1️⃣ **원시값을 변수에 할당하면? - 값을 저장함.**

```jsx
let score = 80;
```

<br/> 
<b>< 코드 생성과정 ></b><br/>

1. 새로운 메모리 주소에 공간을 만들고 <br/>

2. score식별자는 그 주소를 가르키게 된다. <br/>
3. 메모리 공간에 80이라는 “값”을 할당한다. <br/>

- 식별자(score)는 “값(80)"이 아니라, 값이 담긴 “메모리 주소"를 기억하고 있는 것임.

<br/>

### 2️⃣ 원시값은 변경 불가능 하다.(불변성)

- 원시값은 불변성을 띈다. 때문에 재할당을 통해 값을 교체할 수 있음.<br/><br/>
- 메모리에 저장되있는 <u>"원시값"은 변경할 수 없지만</u>, <br/> 재할당을 통해 새로운 메모리 공간을 확보하고 그곳에 재할당한 값을 저장한 후, <br/> <u>변수 식별자가 참조하던 "메모리 공간의 주소가 변경"</u>되어 재할당 되는 것임.<br/>
  (원시값이 변경 불가능한 값이기 때문에 변수가 참조하던 메모리 공간의 주소가 변경되는 것!)
  <br/>
  <br/>
- 예제코드

  ```jsx
  var num;
  num = 80;
  num = 100;
  ```

  ![https://velog.velcdn.com/images/jhplus13/post/983a5bec-d768-4a8a-b95f-5c60d9e0bb0a/image.png](https://velog.velcdn.com/images/jhplus13/post/983a5bec-d768-4a8a-b95f-5c60d9e0bb0a/image.png)

<br/>

※ 문자열과 불변성<br/>
: 문자열은 원시값이므로 값을 변경할 수 없다.(변수에 새로운 문자열을 할당하는 재할당은 가능)

```
let str = "hello";
str[0] = "j";
console.log(str) // "hello"
```

<br/> 

### 3️⃣ 값에 의한 전달

```jsx
let score = 80;
let copy = score;

// [p1]
console.log(score); // 80
console.log(copy); // 80

score = 100;

// [p2]
console.log(score); // 100
console.log(copy); // 80 !!
```

### [point.1] 원시값을 갖는 변수를 다른 변수에 할당하면? - “값”이 복사됨.

: copy변수에 원시값을 갖는 변수인 score을 할당하면, copy에는 score의 “원시값”이 복사되어 전달됨.

### [point.2]

: score변수와 copy변수는 같은 값(80)을 갖지만, 다른 메모리 공간에 저장된 <u>별개의 값</u>이다!<br/>
때문에 score값을 재할당해도 copy변수의 값에는 영향을 주지 않는 것임!

![https://velog.velcdn.com/images/jhplus13/post/4afa8d33-bb06-4856-a64c-1a8233504f26/image.png](https://velog.velcdn.com/images/jhplus13/post/4afa8d33-bb06-4856-a64c-1a8233504f26/image.png)

<br/> 
<br/>

## ✅ 참조타입

### 1️⃣ reference type을 변수에 할당하면? - 참조값이 저장됨

```jsx
let person = { name: "Lee" };
```

<b>< 코드 생성과정 ></b>

1. 메모리 주소(0x001)에 공간을 만들어지고<br/>
2. 변수 식별자는 그 메모리 주소(0x001)를 가르키게 된다.<br/>
3. 다른 메모리 공간(0x002)에 실제 객체값이 담긴다.<br/>
4. 메모리 공간(0x001)에 저장된 참조값이 이 실제 객체를 가르키게 된다.<br/>

<br/>

<b>< 객체값을 참조하는 과정 ></b>

- 변수 키워드가 기억하는 메모리 주소를 통해 메모리 공간(0x001)에 접근하면, 참조값에 접근할 수 있다.
- 참조값은 <u>실제 객체 값이 저장된 메모리 공간을 가리키는 주소</u>이다.
- 이렇게 참조값을 통해 실제 객체에 접근할 수 있는 것이다!
  ![https://velog.velcdn.com/images/jhplus13/post/e02f9549-36fb-4d7b-94b7-942a7a044e7d/image.png](https://velog.velcdn.com/images/jhplus13/post/e02f9549-36fb-4d7b-94b7-942a7a044e7d/image.png)

<br/> 
<br/>

### 2️⃣ 참조값은 변경 가능하다.

원시값은 변경 불가능한 값이므로 재할당을 통해 새로운 메모리에 원시값을 새로 생성해야 했지만,<br/>
참조값은 변경 가능한 값이므로 기존 메모리에 있는 객체를 <u>직접 수정할 수 있다.</u>

```jsx
let person = { name: "Lee" };

person.name = "Kim";
person.adress = "Seuol";
```

<br/> 
<br/>

### 3️⃣ 참조에 의한 전달

```jsx
let person = { name: "Lee" };
let copy = person; // 얕은 복사, person의 참조값을 복사한 것임.
```

원시타입은 원시”값”자체가 복사되어 전달되지만, 참조타입은 “참조값(reference)”가 복사되어 전달된다.

때문에 person과 copy는 메모리 주소는 다르지만, <u>동일한 참조값을 공유</u>하고 있게 된다.

따라서 person과 copy 중 한쪽에서 객체를 수정하면 <u>서로 영향을 받게 된다.</u>

![https://velog.velcdn.com/images/jhplus13/post/cf94c86f-9ca6-4fe9-9d2b-acd797ef479f/image.png](https://velog.velcdn.com/images/jhplus13/post/cf94c86f-9ca6-4fe9-9d2b-acd797ef479f/image.png)

<br/>

### 🔆 값이 같은 두 객체의 비교

```jsx
let obj1 = { a: 1 };
let obj2 = { a: 1 };

console.log(obj1 === obj2); // false
console.log(obj1.a === obj2.a); // true
```

- 두 객체는 내용은 같지만 다른 메모리에 저장된, 즉 "참조값이 다른 별개의 객체"다.
- 그치만 객체 내용이 같아서 동일한 내용이 나오는 것이다.

---

<br/> 

## ✅ 얕은 복사, 깊은 복사

얕은 복사와 깊은 복사로 생성된 원본과 복사본은 참조 값이 다른 별개의 객체다.

하지만 중첩 객체인 경우

- 얕은 복사 : 참조 값을 복사한다.
- 깊은 복사 : 중첩된 객체까지 모두 복사해서 원시 값처럼 완전한 복사본을 만든다.

```jsx
const obj = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...obj }; // {y:1}객체는 같은 값을 공유하게 된다.

// 깊은 복사
const c2 = JSON.parse(JSON.stringify(obj));
```

---

<br/> 
<br/>

# 12. 함수

✅ 이상적인 함수는 한 가지 일만 해야하고, 가급적 작게 만들어야 한다.

<br/>
✅ 함수를 사용해야 하는 이유

- 코드의 재사용성 : 함수는 몇 번이든 호출 할 수 있으므로
- 유지보수의 편의성
- 코드의 가독성

<br/>
✅ 함수 리터럴

- js에서 함수는 “객체”이다. (함수리터럴은 new Function으로 인해 생성된, 인스턴스 “객체”이기 때문)
- 함수 리터럴은 “값”이다. (따라서 함수리터럴을 변수에 할당할 수 있음.)
- 함수 이름, 매개변수 ← 네이밍 규칙 준수해야 함

```jsx
var x = function (a, b) {
  return a + b;
};
```

<br/>

## ✅ 함수 정의

함수 호출은 함수 이름이 아니라, <u>함수 객체를 가리키는 식별자</u>로 사용해야 한다.

```jsx
const add = function foo(x, y) {
  return x + y;
};

// 1. 함수명으로 호출
foo(1, 2); // ReferenceError

// 2.함수 객체를 가르키는 식별자로 호출
add(1, 2); // 3
```

<br/>

### 1. 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.<br/>

따라서 <U>함수 몸체 외부에서 함수 이름으로 함수를 참조할 수 없어</u> 함수를 호출할 수 없다.

```jsx
function foo() {
  console.log("some");
}
foo(); // foo라는 함수이름으로 함수를 호출 할 수 없어야 함. 그러나 호출 가능함.
```

### 2. 그러나 foo함수가 호출 가능한 원리

- js엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름으로 식별자를 암묵적으로 생성하고, 그 식별자에 함수 객체를 할당한다.<br/>
- 따라서 사실 함수는 함수 이름으로 호출하는 것이 아니라, <u>“함수 객체를 가리키는 식별자”로</u> 호출하는 것이다.<br/>
  → js 엔진은 함수 선언문을 함수 표현식으로 변환해 함수 객체를 생성함.

<img src="https://velog.velcdn.com/images/jhplus13/post/b6cd28e0-4c50-4f6a-9e28-cb71328d52c6/image.png" width="400"/>

<br/>

## ✅ 함수 호이스팅

### 1. 함수선언문: 호이스팅이 가능함

- 코드 실행 순서<br/>
런타임 이전 : 1)함수 객체가 메모리에 생성 → 2)함수 이름과 동일한 이름의 식별자(test1)를 암묵적으로 생성 → 3) 식별자에 생성된 “함수 객체”를 할당함<br/>

- 호이스팅이 가능한 이유<br/>
: <U>런타임 이전에 이미 함수 객체가 생성되고 식별자에 할당까지 완료</u>되있다. <br/>
   때문에 함수 선언문 이전에 해당 함수를 참조할 수 있는 것이고, 이는 즉 호이스팅이 가능하다는 말이다.<br/>
  (사실 변수 선언 뿐만 아니라 함수선언문 , class 키워드를 사용한 선언문 등 "모든 선언문"은 런타임 이전에서 먼저 준비되기 때문에 호이스팅 가능함.)

  ```jsx
  test1(); //aa출력, (호이스팅 가능ㅇ)

  function test1() {
    console.log("aa");
  }
  ```

### 2.함수표현식: 호이스팅이 불가능.

- 함수 표현식으로 함수를 정의하면, 함수 호이스팅이 아니라 “변수 호이스팅”이 발생한다.<br/>

- 코드 실행 순서<br/>
런타임 이전 : 1) 변수 선언 실행 → 2) undefined로 초기화 → 런타임 이후: 3) 변수 값(함수 객체) 할당
- 호이스팅 불가한 이유<br/>: 함수 객체 값 할당은 런타임에 평가되므로 함수 표현식 이전에 해당 함수를 참조하면 undefined로 평가되고, 즉 호이스팅이 불가하다.

```jsx
test2(); //error남,호이스팅 불가능

const test2 = function () {
  console.log("show");
};
```

## ✅ 매개변수와 인수

- 매개변수는 함수 몸체 내에서 변수와 동일하게 취급된다.
- 인수가 할당되지 않은 매개변수는 undefined 이다.
- 인수가 초과되면 그냥 버려지는 것은 아니고, 모든 인수는 arguments객체의 프로퍼티로 보관된다.

```jsx
function add (a,b){
console.log(arguments); //  Arguments(3) [1, 2, 5]
	return a + b;
}*

console.log(add(1,2,5)) // 3
```

- 매개변수에 기본값 할당하는 방법

  - 방법 1) 단축평가 사용

    ```jsx
    function add(a, b, c) {
      a = a || 0;
      b = b || 0; // b의 기본값은 0이 된다.
      c = c || 0; // c의 기본값은 0이 된다.
      return a + b + c;
    }

    console.log(add(1));
    ```

  - 방법 2) 매개변수에 기본값 지정

    ```jsx
    function add(a = 0, b = 0, c = 0) {
      // 기본값 설정
      return a + b + c;
    }

    console.log(add(1));
    ```

- 매개변수틑 갯수에 제한이 없지만 최대 3개 이상을 넘지 않을 것을 권장한다.

## ✅ Javascript의 인자전달 Call by value(값에 의한 호출) , Call by reference(참조에 의한 호출)

- 원시 타입인 인수는 "값 자체가 복사"되어 "별도의 값"이 매개변수에 전달됨.<br/>
  따라서 값에 의한 호출은 원본에 영향을 주지 않는다.
  <br/>

- 참조 타입인 인수는 "참조값이 복사"되어 매개변수에 전달됨. 따라서 하나의 "값을 공유"하게 됨.<br/>
  따라서 참조에 의한 호출은 원본에 영향을 준다. (원본값도 바뀜)

  ```jsx
  function ChageVal(primitive, obj) {
    primitive += 100;
    obj.name = "Kim";
  }

  let num = 100; // 원본값 유지 (num과 primitive 변수는 별개의 값이므로)
  let person = { name: "Lee" }; // 원본값 바뀜 (참조값이 공유되어 하나의 값을 공유한것이므로)

  ChageVal(num, person);
  ```

## ✅ 순수함수란?<br/>
: 외부 상태에 의존하지 않고 외부상태를 변경하지 않는 함수, 즉 부수 효과가 없는 함수<br/>

: 동일한 인수가 전달되면 언제나 동일할 값을 반환하는 함수다.<br/>

: 순수함수를 통해 부수효과를 최대한 억제하여 오류를 피해야 한다.

- 비순수 함수 예제1

```jsx
let person = { name: "Lee" }; // 원본값 바뀜 (참조값이 공유되어 하나의 값을 공유한것이므로)

function ChageVal(obj) {
  obj.name = "Kim"; // 매개변수를 통해 객체를 전달받으면 비순수 함수가 된다.
}

ChageVal(person);
```

- 비순수 함수 예제2

```jsx
let num = 0;

function increase() {
  num++; // 함수 내부에서 외부의 값을 직접 참조하고 변경하고 있음. 따라서 비순수함수!
}

increase();
```

---

<br/>

## 다양한 함수의 형태

✅ 화살표 함수

특징

1.  this 바인딩 방식이 다름

  - 화살표 함수는 항상 상위 스코프의 this를 상속받습니다.
  - 화살표 함수는 bind(), call()과 같은 메서드로 this를 바인딩 할 수 없습니다.
  <br/>

2.  prototype 프로퍼티가 없음
    <br/>

3.  arguments 객체를 생성하지 않음

<br/>

✅ 즉시 실행 함수

- 익명 함수를 사용하는 것이 일반적이다.
- 즉시 실행 함수에 함수 이름을 지어주더라도, 외부에서 재사용 할 수 없고 그냥 즉시 실행 함수로만 작동된다.
- 형태

  ```jsx
  // 기본 형태1
  (function(){
    console.log("IIFE");
  }());

  // 기본 형태1-2. 즉시 실행 함수에도 인자를 전달할 수 있다.
  let res = (function(a,b){
      return a+b
    }(1,2)
  )  

  // 기본 형태2 <- 실제 이 형태가 젤 일반적임.
  (function () {
      console.log("IIFE");
  })();

  // 기본 형태3 - 화살표 함수로도 사용 가능하다
  (() => {
      console.log("IIFE");
  })();
  ```
<br/>
✅ 재귀 함수

- 함수 내부에서 함수 이름을 사용해 자기 자신을 호출할 수 있다.

- 자신을 무한 재귀 호출하기 때문에 탈출 조건을 반드시 만들어야 한다.
- 대부분의 재귀 함수는 for문이나, while문으로 구현 가능하다.
- 장점 : 반복되는 처리를 반복문 없이 구현할 수 있다. / 단점 : 무한 반복에 빠질 수 있고, 스택 오버 플로 에러 발생시킬 수 있다.

  ```jsx
  function factorial(num) {
    if (num === 1) {
      return 1;
    }

    return num * factorial(num - 1);
  }

  console.log(factorial(3)); // 6
  console.log(factorial(5)); // 120
  console.log(factorial(6)); // 720
  ```
<br/>
✅ 콜백 함수

- 콜백 함수 : 어떤 함수의 “매개변수로 전달되는” 함수
- 고차함수 : 매개변수를 콜백함수를 “전달받은” 함수, 함수를 반환값으로 반환하는 함수

- 코드 예제
  ```jsx
  btnEl.addEventListener("click", () => console.log("clicked btn"));
  setTimeout(() => console.log("1초 경과"), 1000);

  repeat(5, (i) => console.log(i)); // 콜백함수를 익명 함수 리터럴로 정의하면서 고차함수(repeat)에 전달하는 것이 일반적이다.
  let result = [1, 2, 3].map((num) => num * 2); // 콜백함수를 사용하는 고차함수 map
  let result = [1, 2, 3].reduce((acc, cur) => (acc += cur)); // 콜백함수를 사용하는 고차함수 reduce
  ```
