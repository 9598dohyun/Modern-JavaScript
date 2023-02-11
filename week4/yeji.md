# 16장: 프로퍼티 어리뷰트

## 내부 슬롯과 내부 메서드

([[...]])으로 감싼 이름들  
JS는 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출하는 방법 제공X  
ex) 예외: 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가짐 - `__proto__`로 간접적으로 접근  가능  
  
## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체  

프로퍼티 어트리뷰트: 자바스크립트 엔진은 프로퍼티를 생성할 때 기본값으로 자동 정의하는 프로퍼티의 상태  

- 프로퍼티 상태: 프로퍼티 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부  
- 자바스크립트 엔진이 관리하는 내부 상태값인 내부 슬롯 `[[Value]], [[Writable]], [[Enumerable]], [[Configurable]]`
- 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드로 간접적으로 확인 가능  

  - 첫 번째 매개변수에는 객체의 참조, 두 번째 매개변수는 프로퍼티의 키
  - 반환값 프로퍼티 디스크립터, 존재하지 않는 프로퍼티나 상속받은 프로퍼티를 요구하면 undefined
  - ex) `Object.getOwnPropertyDescriptor(person, 'name')`
  
## 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티: 키와 값읓로 구성된 프로퍼티
- 접근자 프로퍼티: 자체적으로 값을 갖지 않고, 다른 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 데이터 프로퍼티

- [[Value]]
- [[Writable]]: rkqtdml qusrud rksmdduqn,  false면 readonly
- [[Enumerable]]: false면 for...in 문이나 Object.keys 메서드 등으로 열거할 수X
- [[Configurable]]  
  \- false면 프로퍼티 삭제, 어트리뷰트 값 변경 금지  
  \- 단, [[Writable]]이 true면 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것 가능

### 접근자 프로퍼티

- [[Get]]: 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다  
- [[Set]]: 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 떄 호출되는 접근자 함수, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다  
- [[Enumerable]]: 데이터 프로퍼티의 [[Enumerable]] 값과 같다
- [[Configurable]]: 데이터 프로퍼티의 [[Configurable]] 값과 같다

```javascript
const person = {
  // 데이터 프로퍼티
  firstName: 'Hyeong Kyeom',
  lastName: 'Kim',
  
  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당
    [this.firstName, this.lastName] = name.split(' ');
  }
};
person.fullName = "Marie Kim";
console.log(person.fullName); // Marie Kim
```

접근자 프로퍼티의 동작 방식(getter라고 가정했을 때)

1. 프로퍼티 키 유효한지 확인(문자 or 심벌인지)
2. 프로토타입체인에서 프로퍼티 검색
3. 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 검색
4. 프로퍼티 어트리뷰트 [[Get]]의 값 getter 를 호출해 그 결과 반환

> **프로토타입**
> 객체의 부모 객체 역할을 하는 객체  
> **프로토타입 체인** 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조  

16-07 예제..?

## 프로퍼티 정의

새로운 프로퍼티를 추가하면서, 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존  프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것.  
생략했을 때 기본값은 어트리뷰트마다 다르지만 undefined나 false.  
`Object.defineProperty`는 하나의 프로퍼티만 정의, `Object.defineProperties`는 여러 개의 프로퍼티 한번에 정의  

```javascript
const person = {};
Object.defineProperty(person, 'firstName', {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
}) 
```

## 객체 변경 방지

객체 변경을 방지하는 다양한 메서드들.

### 객체 확장 금지 `Object.preventExtensions`

프로퍼티 추가(프로퍼티 동적 추가, Object.defineProperty) 금지  
Object.isExtensible 메서드로 확장이 가능한 객체인지 확인 가능

### 객체 밀봉 `Object.seal`

프로퍼티 추가, 삭제, 프로퍼티 어트리뷰트 재정의 금지  
Read, Write 만 가능  
Object.isSealed 메서드로 밀봉된 객체인지 확인 가능  
\* 예제 16-11 참고

### 객체 동결 `Object.freeze`

프로퍼티 추가, 삭제, 어트리뷰트 재정의, 프로퍼티 값 갱신 금지  
Read만 가능
Object.isFrozen 메서드로 동결된 객체인지 확인 가능

### 불변 객체

앞의 세 변경 방지 메서드는 '얕은 변경 방지'로 직속 프로퍼티만 변경 방지, 중첩 객체에 영향을 주지는 못한다.  
Object.freeze 메서드로 객체를 동결해도 중첩 객체까지 동결할 수는 없다.  
중첩 객체까지 동결하여 '불변 객체'를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```javascript
function deepFreeze(target) {
    if (target && typeof target === 'object' && !Object.isFrozen(target)) {
        Object.freeze(target);
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}
```

# 17장: 생성자 함수에 의한 객체 생성

생성자 함수: new 연산자와 함께 호출해 객체를 생성하는 함수 (new 없으면 일반 함수로 동작)  
인스턴스: 생성자 함수에 의해 생성된 객체

## 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

장: 직관적이고 간편  
단: 단 하나의 객체만 생성, 여러 개 생성하려면 매번 같은 프로퍼티 기술해야  

### 생성자 함수에 의한 객체 생성 방식의 장점

객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용해 프로퍼티 구조가 동일한 객체 여러 개 생성 가능

#### this

자기 참조 변수. 가리키는 값은 함수 호출 방식에 따라 동적으로 전달.

- 일반함수: 전여 객체
- 메서드: 메서드를 호출한 객체
- 생성자: 생성자 함수가 생성할 인스턴스

```
function foo() {
    console.log(this);
}

foo(); // window

const obj = { foo };
obj.foo; // obj

const inst = new foo(); //inst

```

### 생성자 함수의 인스턴스 생성 과정

인스턴스 생성(필수), 인스턴스 초기화(옵션)  

인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환  

- 바인딩된 this가 암묵적으로 반환
- return으로 다른 객체를 명시적으로 반환하면, this가 아닌 return 문에 명시된 객체 반환
- 명시적으로 원시값을 반환하면, 이는 무시되고 this가 반환
- => 생성자 함수에서는 return 문을 반드시 생략해야

```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩

    // 2. this에 바인딩되어있는 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
    // 3. 완성된 인스턴스가 바인딩된 this로 반환
}
// 인스턴스 생성!
```

### 내부 메서드  [[Call]]과 [[Construct]]

함수 선언문, 함수 표현식으로 정의한 함수는 생성자로도 호출할 수 있음.  
함수는 일반 객체와 달리 **호출** 할 수 있다.  

#### 함수 객체만을 위한 내부 메서드

- [[Call]]: 일반 함수로서 호출될 때, 이를 가진 함수 객체 callable  
- 모든 함수 객체는 callable
- [[Constuct]]: 생성자 함수로서 호출될 때, 이를 가진 함수 객체 constructor(<-> non-constructor)  
  - 모든 함수가 [[Construct]]를 갖는 건 아님. constuctor일 수 있고, non-constructor 일 수도 있다.

### constructor vs non-constructor

- constructor: 함수 선언문, 함수 표현식, 클래스(클래스도 함수다!!!)
- non-constructor: 메서드(ES6 메서드 축약 표현을 칭함), 화살표 함수  

```javascript
var obj= {
    // 메서드 축약 표현. 프로퍼티 키를 입력하지 않는다.
    sayHi() {
        console.log('Hi');
    }
}
new obj.sayHi(); // TypeError, 메서드 축약표현은 non-constructor다
```

### new 연산자

new 없이 호출하면 일반 함수로 [[Call]] 호출.  
new 함께 호출하면 생성자 함수로 동작, [[Construct]] 호출. (단, 호출하는 함수가 constructor여야 함)  

\* new 없이 호출했을 때 this는 전역 객체를 가리키기 때문에, 전역 객체의 프로퍼티와 메서드를 생성하는 효과가 날 수도 (예제 17-18)  
=> 생성자는 파스칼 케이스(첫 문자 대문자)로 명명해, 일반 함수와 구별해야

### new.target

- 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용, 메타 프로퍼티라고 부름
- new 연산자와 함께 생성자로 호출됐는지 확인할 수 있음
  - 생성자 함수로 호출 된 경우: new.target은 함수 자신
  - 일반 함수로 호출된 경우: new.target은 undefined

```javascript
function Circle(radius) {
    // 이 함수가 new와 함꼐 호출된 것이 아니라면
    if (!new.target) {
        // 직접 재귀 호출로 인스턴스를 생성해 반환한다
        return new Circle(radius);
    }
    ...
}
```

### 스코프 세이프 생성자 패턴

new.target은 ES6에서 도입된 최신 문법. new.target을 사용할 수 없다면 스코프 세이프 생성자 패턴을 사용하자.

```javascript
function Circle(radius) {
    if (!(this instance of Circle)) {
        return new Circle(radius);
    }
    ...
}
```

빌트인 생성자 함수는 new 연산자와 함께 호출되었는지 확인한 후 적절한 값 반환  

- Object와 Function 생성자 함수는 new 없이 호출해도 new 연산자와 함께 호출했을 떄와 동일하게 동작
- String, Number, Boolean 생성자 함수는 new 없이 호출하면 객체가 아닌 문자열, 숫자, 불리언 값을 반환 -> 타입 변환으로 쓸 수 있음.
