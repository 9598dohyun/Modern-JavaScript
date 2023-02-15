# 19.4~19.11 장

## ✅ ’리터럴 표기법에 의해 생성된 객체’의 생성자함수와 프로토타입

<생성자 함수로 생성한 객체>

```jsx
const obj = new Object();
console.log(obj.constructor === Object); // true
```

- 질문 : 객체에는 constructor 프로퍼티가 없지만 .으로 접근 가능한 이유는?<br/>
    → 프로토타입 체인을 통해 __proto__로 프로토타입 객체에 접근해서 
프로토타입 객체가 가지고 있는 constructor 프로퍼티를 활용한 것이다.
    
<br/>

<객체 리터럴로 생성한 객체>

```jsx
const obj = {};
console.log(obj.constructor === Object); // true
```

- 객체 리터럴로 생성된 객체도 프로토타입 객체가 존재한다<br/>
→ 즉, 리터럴 표기법(객체 리터럴, 함수 리터럴, 배열 리터럴 등)에 의핸 생성된 객체도 === 생성자 함수에 의해 생성된 객체와 같다고 생각해도 무방.<br/>
(리터럴로 만든 것과 생성자 함수를 이용하여 만든 객체는 다른 면모도 있지만, 프로토타입 관련 내용상에선 그냥 같다고 봐도 무방하다.)

**→ 결론, 리터럴 표기법이나 생성자 함수에 의해 생성된 객체거나 결국 모든 객체는 생성자 함수와 연결되어 있고, 프로토타입 객체도 있다!**

<br/>

## ✅ 프로토타입의 생성 시점

프로토타입 (객체)는 “생성자 함수가 생성”되는 시점에 생성된다. (생성자 함수와 프로토타입 객체는 쌍이기 때문)

### 1. “사용자 정의” 생성자 함수의 경우

- “함수 객체를 생성”하는 시점에 프로토타입 객체가 생긴다.
- non-constructor(es6메서드 축약표현 ,화살표 함수)는 프로토타입 객체가 생성되지 않는다 (생성자 함수로 호출할 수 없기 때문에)

### 2. “빌트인 생성자 함수”의 경우

- “전역 객체가 생성”되는 시점에 프로토타입 객체가 생긴다.
- 전역 객체는 코드가 실행되기 이전에 js엔진에 의해 생성되는 특수한 객체다 (브라우저: window, node.js - global객체)

### ✔️ 구체적인 과정 (함수 선언문의 경우)

- 런타임 이전 <br/>
: 1)생성자 함수 객체 or 전역 객체 생성 + 2)프로토타입 객체 생성 + 3)프로토타입 객체가 생성자 함수의 prototype프로퍼티에 바인딩 됨. ← 동시에 됨.
- 런타임 시 <br/>
: 인스턴스 객체를 생성 -> 생성된 인스턴스 객체의 [[prototype]] 내부 슬롯에 프로토타입 객체가 할당됨 (때문에 인스턴스 객체가 프로토타입 객체의 속성을 상속받을 수 있는 것이다.)

<br/>

 ✅ 19.6장. 객체 생성 방식과 프로토타입의 결정 ← SKIP 

<br/>

## ✅ 프로토타입 체인
- 생성자 함수에서 생성된 프로토타입 객체도 객체이기 때문에, 이 프로토타입의 객체의 프로토타입 객체는 결국 “Object.prototype”객체 이다.
- 코드 예제 - `3.toString();`  
: Object.prototype.toString() ← toString메서드 에서의 this는 넘버리터럴 3으로 지정됨.<br/>
: `Object.prototype.toString.call(넘버 인스턴스 객체 3 , 인자);`

<br/>

<프로토타입 체인과 스코프 체인의 상관관계>
- 프로토타입 체인 : “프로퍼티, 메서드”를 검색하기 위한 메커니즘
- 스코프 체인 : “(변수,함수 등)식별자”를 검색하기 위한 메커니즘

: 스코프 체인과 프로토타입 체인은 상관없이 별도로 동작하는게 아니라, 서로 협력하여 식별자와 프로퍼티를 찾는데 사용된다.

```jsx
// 1. 스코프 체인 : 지역 스코프 > 전역 스코프로 검색하며, me식별자를 검색한다.
const number = 3; 

// 2. 프로토타입 체인 : 넘버 리터럴 > Number prototype객체 > Object prototype 객체로 검색
number.toString(); 
```
<br/>


## ✅ 오버라이딩과 프로퍼티 섀도잉

- 오버라이딩 : 상위 객체(프로토타입 객체)가 가지고 있는 메서드를 하위 객체(인스턴스 객체)가 “재정의”하여 사용하는 방식. <br/>
(인스턴스 객체의 프로퍼티를 사용하기 때문에 체인으로 프로토타입 객체의 프로퍼티는 사용되지 않는다.)

- 프로퍼티 섀도잉 : 오버라이딩을 통해 프로토타입 객체의 프로퍼티가 가려지는 현상.

<br/>

## ✅ 프로토타입(객체)의 교체

<b><생성자 함수에 의한 교체></b>

constructor 연결 해제, 생성자 함수의 prototype는 연결 유지

- 프로토타입 객체를 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
- 생성자 함수의 prototype 프로퍼티가 프로토타입 객체를 가리키고 있는것은 동일하다. (생성자 함수 선언시 프로토타입 객체와 바인딩 되므로)

```jsx
const Person = (fuction(){
		function Person(name){
			this.name = name;
		}
		
		Person.prototype = { // 교체: 프로토타입 객체를 객체리터럴로 바꿈
			sayHello(){console.log("hello") // ※ 메서드 축약형
			} 
		};
	}()
)

const me = new Person('Lee');

//
console.log(me.constructor === "Person"); // false ✔️ <- 프로토타입 객체와 Person간에 constructor 연결이 끊어져서
console.log(me.constructor === "Object"); // true <- 프로토타입 체인으로 올라가다보면 결국 Object가 종점.
```

- 다시 끊어진 constructor 프로퍼티와 생성자 함수를 연결하는 로직
: 바꿀 프로토타입 객체에 constructor프로퍼티를 추가한다.

```jsx
const Person = (fuction(){
		function Person(name){
			this.name = name;
		}
		
		Person.prototype = { // 프로토타입 객체를 객체리터럴로 바꿈
				constructor : Person // constructor 프로퍼티와 생성자 함수 간의 연결을 설정 ✔️
			sayHello(){console.log("hello") 
		};

	}()
)

const me = new Person('Lee');

//
console.log(me.constructor === "Person"); // true <- ✔️
console.log(me.constructor === "Object"); // true <- 프로토타입 체인으로 올라가다보면 결국 Object가 종점.
```

<br/>

<b><인스턴스 객체에 의한 교체></b> <br/>
constructor 연결 해제, 생성자 함수의 prototype도 연결 해제됨.

- 프로토타입 객체를 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
- 생성자 함수의 prototype 프로퍼티가 프로토타입 객체와의 연결도 파괴된다.

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Lee');
		
const parent = {
	sayHello(){console.log("hello") // ※ 메서드 축약형
}
 
Object.setPrototypeOf(me,parent); // 교체: me인스턴스 객체의 프로토타입 객체를 parent로 바꿈
// me.__proto__ = parent (동일 교체로직)
 

//
console.log(me.constructor === "Person"); // false ✔️
console.log(me.constructor === "Object"); // true  
```

- 다시 끊어진 constructor와 생성자 함수의 prototype 를 연결하는 로직<br/>
: 바꿀 프로토타입 객체에 constructor프로퍼티를 추가한다.

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Lee');
		
const parent = {  
	constructor : Person // constructor와 생성자함수 연결 ✔️
	sayHello(){console.log("hello") 
};

Person.prototype = parent; // 생성자 함수의 prototype와 프로토타입 객체 연결 ✔️
 
Object.setPrototypeOf(me,parent); // 교체 
// me.__proto__ = parent (동일 교체로직)
 

//
console.log(me.constructor === "Person"); // true ✔️ <- 프로토타입 객체와 Person간에 constructor 연결이 끊어져서
console.log(me.constructor === "Object"); // true <- 프로토타입 체인으로 올라가다보면 결국 Object가 종점.
```

⇒ 결론 : 프로토타입 교체를 통해 상속 관계를 직접 변경하는 것은 번거로움. “따라서 프로토타입 객체를 직접 교체하지 않는게 좋음”.

(클래스 사용하는게 더 직관적으로 상속 관계 구현할 수 있음)

<br/>

## ✅ instanceof 연산자

- 사용 : `인스턴스 객체 instanceof 생성자함수`
- 프로토타입 객체의 “constructor”가 가리키는 생성자 함수를 가리키는게 아니라, <br/>생성자 함수의 “prototype”에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인하는 것이다!
    
    ```jsx
    function Person(name) {
      this.name = name;
    }
    
    const me = new Person("Lee");
    
    console.log(me instanceof Person); // true  ← me인스턴스 객체의 프로토타입 체인 상에 Person.prototype이 바인딩 됐는지 확인
    console.log(me instanceof Object); // true ← me인스턴스 객체의 프로토타입 체인 상에 Object.prototype이 바인딩 됐는지 확인
    
    // -----------------------------------------------------
    // <프로토타입 교체> "constructor"와 "prototype" 연결은 해제된다.
    const parent = {};
    
    me.__proto__ = parent; 
    
    console.log(me instanceof Person); // false!!
    console.log(me instanceof Object); // true
    
    // -----------------------------------------------------
    // <다시 생성자 함수의 prototype과 교체된 프로토타입 객체 연결>
    Person.prototype = parent; 
    
    console.log(me instanceof Person); // true!!
    console.log(me instanceof Object); // true
    
    console.log(parent.constructor); // Parent가 아닌 Object!!
    ```
    
<br/>

## ✅ 직접 상속

인스턴스 객체가 상속받을 “프로토타입 객체”를 직접 지정하는 것임.

1. Object.create
2. 객체 리터럴 내부에서 __proto__ 에 의한 직접 상속
