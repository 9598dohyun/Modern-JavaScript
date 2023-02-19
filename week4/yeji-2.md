## 19.11 직접 상속

### Object.create

```javascript
/** 
/* 지정된 프로토타입 및 프로퍼티를 같은 새로운 객체를 생성하여 반환한다.
/* @params {Object} prototype - 생성할 객체의 프로토타입으로 지정할 개체
/* @params {Object} [properiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
/* @return {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
/* 
Object.create(prototype, [, properiesObject];
```



### 객체 리터럴 내부에서 \_\_proto\_\_에 의한 직접 상속

```javascript
const myProto = { x : 10 };
const obj = {
    __proto__: myProto;
};


```



## 19.12 정적 프로퍼티/메서드

- 생성자 함수 객체가 소유한 프로퍼티/메서드

- 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다(생성자 함수로 참조/호출 가능)



## 19.13 프로퍼티 존재 확인

- `프로퍼티 키 in 객체`: 객체 내 특정 프로퍼티 존재하는지 확인

- `Reflect.has(객체, 프로퍼티 키)`

- `객체.hasOwnProperty(프로퍼티 키)`



## 19.14 프로퍼티 열거

- `for (변수 선언문 in 객체) {...}` : 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거, 상속받은 프로퍼티도 열거

- `Object.keys` : 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환

- `Object.values` (ES8 도입): 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환 

- `Object.entries` (ES8 도입): 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍을 배열에 담아 반환





# 20장: strict mode

적용하고 싶은 범위에 `'use strict';` 추가

전역에 적용하는 것은 바람직 하지 않다. 외부 서드파티 라이브러리가 non-strict mode일 수도 있기 때문.

함수 단위로 적용하는 것도 일관성이 없으므로, 즉시 실행함수로 감싼 스크립튼 단위로 적용.

=> 그냥 ESLint 를 쓰자



## strict mode가 발생시키는 에러

- 암묵적 전역 -> ReferenceError(선언하지 않은 변수)

- delete로 변수, 함수, 매개변수를 삭제 -> SyntaxError

- 중복된 매개변수 이름 -> Syntax Error

- with 문(전달된 객체를 스코프 체인에 추가)  -> SyntaxError 



## strict mode 적용에 의한 변화

- 일반 함수의 this 가 undefined에 바인딩됨

- 매개변수에 전달된 인수 재할당해 변경해도 arguments 객체에 반영 안됨



# 21장: 빌트인 객체

## 자바스크립트의 객체 분류

- 빌트인 객체: ECMAScript에 정의된 객체, 애플리케이션 전역의 공통 기능

- 호스트 객체: 브라우저, Node.js 환경 등에서 추가로 제공하는 객체(ex. DOM, Canvas, XMLHttpREques, fetch...)

- 사용자 정의 객체: 사용자가 직접 정의한 객체



## 표준 빌트인 객체

- Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error....

- 대부분 인스턴스를 생성할 수 있는 생성자 함수 객체(Math, Reflect, JSON 제외)

- 프로토타입으로 빌트인 프로토타입 메서드 제공
  ex) `numObj.toFixed();`  numObj가 Number 생성자로 만든 객체일 때, 숫자를 반올림해 문자열 반환

- 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 인스턴스 메서드 제공
  ex) `Number.isInteger(0.5);// false`



## 원시값과 래퍼 객체

- 원시값: 객체가 아니므로 프로퍼티나 메서드를 가질 수 없음

- 래퍼 객체: **문자열, 숫자, 불리언 **값에 대해 객체처럼 접근하면 생성되는 임시 객체
  
  -> 래퍼 객체 처리가 종료되면, 원래 상태인 원시값으로 돌아오고 래퍼 객체는 GC 대상이 됨
  
  -> 래퍼 객체가 있기 때문에 굳이 new String, new Number, new Boolean 이렇게 인스턴스 생성할 필요가 없다. 

```javascript
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환
console.log(str.length);//2
```



## 전역 객체

- 런타임 이전 자바스크립트 엔진이 가장 먼저 생성하는 특수한 객체

- 어떤 객체에도 속하지 않는 최상위 객체

- 브라우저에서는 window, 노드에서는 global

- 전역 객체 생성자 함수는 제공되지 않음 - 의도적 생성 X

- 전역 객체 프로퍼티를 참조할 때는 window/global 생략 가능

- 암묵적 전역(선언하지 않은 var 변수)은 전역 변수가 아니라 전역 객체의 프로퍼티 



### 빌트인 전역 프로퍼티

Infinity, NaN, undefined



### 빌트인 전역 함수

### eval

```javascript
/**
* 주어진 문자열 코드를 런타임에 평가 또는 실행한다.
* @param {string} code - 코드를 나타내는 문자열
* @returns {*} 문자열 코드를 평가/실행한 평가값
*/

eval(code)
```

- strict mode가 아니면, code는 그 위치에 원래 있던 것처럼 동작함.

- stritc mode면, code는 eval 함수 자신의 자체적인 스코프를 형성함.

- 사용자가 입력한 콘텐츠 실행 위험 & 엔진에 의해 최적화 수행X -> eval 사용 금지



### 그 외 전역 함수

- isFinite

- isNaN

- parseFloat: 문자열을 실수로 해석

- parseInt: 문자열을 정수로 해석, 두 번째 인수로 진법 나타내는 기수 전달 가능(16진수는 기본적으로 해석 가능)

- encodeURI: 완전한 문자열 URI를 이스케이프 처리를 위해 인코딩(쿼리스트링 구분자는 인코딩 X)

- decodeURI: 이스케이프 이전으로 디코딩 

- encodeURIComponent: URI 구성 요소를 이스케이프 처리를 위해 인코딩 (쿼리스트링 구분자 = ? & 도 인코딩)

- decodeURIComponent: URI 구성 요소 디코딩



### 암묵적 전역

- 전역 객체의 프로퍼티 추가 -> delete 연산자로 삭제 가능

- 변수 X -> 변수 호이스팅 발생 X

```javascript
console.log(x); // undefined, 변수 호이스팅
console.log(y); // ReferenceError


var x = 10;
function foo() {
    y = 20;
}
```



# 22장: this

자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수 

- 자바스크립트 엔진에 의해 암묵적으로 생성

- 함수를 호출하면 arguments 객체와 this 가 암묵적으로 함수 내부에 전달 

- this 바인딩은 함수 호출 방식에 의해 동적으로 결정



## 함수 호출 방식과 this 바인딩

c.f. 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점 상위 스코프 결정 / this 바인딩은 함수 호출 시점에 결정



- 일반 함수(중첩 함수, 콜백 함수 포함): 전역 객체 바인딩
  
  - 헬퍼함수인 중첩 함수, 콜백 함수의 this 가 외부 함수의 this와 다른 건 문제 있음
  
  - 솔루션 1) 외부 함수의 this 를 다른 변수로 정의하고(예를 들면 that), 그 변수를 받아 쓰는 방법이 있음
  
  - 솔루션 2) Function.prototype.apply/call/bind 메서드로 명시적으로 this 를 바인딩할 수도 있음

- 메서드: 메서드를 호출한 객체
  
  - 메서드를 일반 함수로 호출하면, 일반 함수이기 때문에 this는 전역 객체

- 생성자 함수: 생성자 함수가 미래에 생성할 인스턴스
  
  - 생성자 함수를 new 없이 일반 함수로 쓰면, this는 전역 객체
    
    ```javascript
    function Circle(radius) {
        this.radius = radius;
        this.getDiameter = function () {
            return 2 * this.radius;
        };
    }
    
    const circle = Circle(15);
    console.log(radius); //15
    ```

- Function.prototype.apply/call/bind
  
  - apply, call 메서드
    
    - 기능: 함수 호출 +첫 번째 인수로 전달한 객체를 this로 바인딩
    
    - (apply는 인수를 배열로 묶어서 전달)
  
  - bind 메서드
    
    - 기능: this로 사용할 객체만 전달, 함수 호출 X
    
    - 중첩 함수, 콜백 함수의 this 를 외부함수의 this와 일치하도록 할 때 많이 사용 
    
    ```javascript
    채ㅜㄴㅅ 
    ```
    
    












