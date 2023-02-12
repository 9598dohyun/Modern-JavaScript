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

- 데이터 프로퍼티: 키와 값으로 구성된 프로퍼티
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

# 18장: 함수와 일급 객체

## 일급 객체

일급 객체의 조건 

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성 가능하다.

- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.

- 함수의 매개변수에 전달할 수 있다.

- 함수의 반환값으로 사용할 수 있다.

함수가 일급 객체라는 것의 의미

- 객체와 동일하게 사용할 수 있음

- 객체는 값이므로 함수는 값과 동일하게 취급할 수 있음

- 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든지 리터럴로 정의할 수 있다

- 런타임에 함수 객체로 평가된다.

- 함수의 매개변수에 전달할 수 있다 -> 함수형 프로그래밍 가능 

함수와 일반 객체의 차이즘

- 일반 객체는 호출 못함 

- 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유

## 함수 객체의 프로퍼티

console.dir() 사용해 들여다보기

Object.getOwnPropertyDescriptors 메서드로 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트 확인 가능 

### arguments 프로퍼티

arguments 프로퍼티

- value(값): arguments 객체 
  
  - 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
  
  - 함수 내부에서 지역 변수처럼 사용돼 외부 참조 불가 

- ES3부터 표준에서 제외되었음. arguments 프로퍼티 대신 arguments 객체를 참조할 것. 

arguments 객체

- 인수를 프로퍼티 값으로 소유

- 프로퍼티 키는 인수의 순서를 나타냄 

- callee 프로퍼티는 함수 자신을 가리킴

- length 프로퍼티는 인수의 개수를 가리킴

```javascript
function multiply(x, y) {
    console.log(arguments);
    return x * y;
}
console.log(multiply(1, 2, 3));
/*
Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
0: 1
1: 2
2: 3
callee: ƒ multiply(x, y)
length: 3
Symbol(Symbol.iterator): ƒ values()
[[Prototype]]: Object
*/
// 2
```

arguments 객체의 할용

- 자바스크립트는 인수의 개수를 확인하지 않음 -> arguments.length로 인수 개수를 확인하고 함수의 동작을 달리 정의할 수 있음.

- 가변 인자 함수를 구현할 때 응용 

arguments 객체는 유사 배열 객체 

- 배열 메서드를 사용할 경우 에러가 발생

- (Before ES6) 배열 메서드를 사용하려면 간접 호출 해야(Function.prototype.call, Function.prototype.apply)

- (After ES6) Rest 파라미터 도입
  
  ```javascript
  function sum(...args) {
      return args.reduce((pre,cur) => pre + cur, 0);
  }
  ```

### caller 프로퍼티

ECMAScript 사양에 포함되는 비표준 프로퍼티. 표준화될 예정도 없음. 



### length 프로퍼티

함수를 정의할 때 선언한 매개변수의 개수 

<-> arguments.length는 인자읙 ㅐ수 

### name 프로퍼티

ES6에서 정식 표준이 됨, 함수의 이름을 나타냄.

익명 함수 표현식의 경우 ES5에서는 빈문자열을 갖고, ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다. (함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출.)



### \_\_proto_\_ 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. 이는 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

\_\_proto_\_ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티. 



### prototype 프로퍼티

constructor(생성자 함수로 호출할 수 있는 함수 객체)만이 소유하는 프로퍼티. 

함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.





# 19장: 프로토타입

자바스크립트는 멀티 패러다임 프로그래밍 언어.

- 명령형

- 함수형

- 프로토타입 기반

- 객체 지향 프로그래밍 지원

- **프로토타입 기반 객체지향**



> 클래스
> 
> - ES6 도입 but 기존의 프로토타입 기반 객체 지향 모델을 폐지하는 것 X
> 
> - 클래스도 함수이며 기존 프로토타입 기반 패턴의 문법적 설탕
> 
> - 그러나 생성자 함수보다 엄격하고, 생성자 함수에서 제공하지 않는 기능도 있어 단순한 문법적 설탕으로 보기보다는 새로운 객체 생성 메커니즘으로 보는 것이 더 합당 (??? 모순이 있잖슴)



# 객체지향 프로그래밍

객체: 속성을 통해 여러개의 값을 하나의 단위로 구성한 복합적인 자료구조

객체지향 프로그래밍: 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임 

- 객체의 **상태** 데이터와 상태 데이터를 조작할 수 있는 **동작**을 하나의 논라적인 단위로 묶어 생각

- 객체 = 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조

- 프로퍼티: 상태 데이터 / 메서드: 동작



## 상속과 프로토타입

상속: 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것 

자바스크립트는 **프로토타입을 기반**으로 상속을 구현 -> 불필요한 중복 제거 

##### Before

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

##### After

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```



## 프로토타입 객체

- 상속을 구현하기 위해 사용

- 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 슬롯의 값은 프로토타입의 참조다.

- 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다. 
  ex) 객체 리터럴에 의해 생서된 객체의 프로토타입은 Object.prototype
  생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype에 바인딩되어있는 객체 

- 모든 객체는 하나의 프로토타입을 갖고, 모든 프로토타입은 생서자 함수와 연결되어 있다.



![image](https://user-images.githubusercontent.com/80154058/144704014-2f03263c-d4cc-4704-84cb-b1650afd5d48.png)



### \_\_proto\_\_ 접근자 프로퍼티

- 모든 객체는 _\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

- 접근자 프로퍼티 특성
  
  - 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function) 즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티다.
  
  - Object.prototype의 접근자 프로퍼티 __proto__는 getter, setter(접근자 함수 = [[Get]], [[Set]] 프로퍼티 어트리뷰트에 할당된 함수)를 통해 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입을 취득하거나 할당한다. 

- 상속을 통해 사용 
  
  - 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티
  
  - 모든 객체는 상속을 통해 Object.prototype.\_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있다.

- 프로토타입을 왜 이 프로퍼티로 접근하는가
  
  - 상호 참조에 의해 프로토타입 체인이 생성(순환참조)되는 것을 방지
  
  -  프로토타입은 단방향 링크드 리스트로 구현되어야



### 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다. 

non-constructor (메서드 - ES6 메서드 축약 표현을 칭함, 화살표 함수)는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다. 



### 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constuctor 프로퍼티를 갖는다. 이는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다. 
