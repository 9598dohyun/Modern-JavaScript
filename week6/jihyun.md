# 26장. 클래스 

클래스는 es6에서 생긴 개념.

<객체 생성 방식>

방식1. 프로토타입 기반 객체지향 모델(프로토타입 기반 객체 생성 패턴), 프로토타입 기반 생성자함수

방식2. 클래스 기반 객체지향 모델(클래스 기반 객체 생성 패턴), 클래스 기반 생성자함수

→ 이 둘은 프로토타입 기반의 인스턴스 객체를 생성한다.<br/>
→ 둘 다 프로토타입 체인 구조는 똑같이 생성되는데, 단지 객체 생성 방식만 다를뿐<br/>
→ 이전 생성자 함수 기반 객체 생성 방식에 비해 class가 간편하기 때문에 “문법적 설탕”, “편의 문법” 이라고 표현하는 것 

## 클래스 정의

- 클래스는 “함수”다. `console.log(typeof class Person{} ); // function`<br/>
- 함수 중에서도 constructor 함수(생성자 함수로 호출 할 수 있는 함수)이다. 즉, “생성자 함수”이다.<br/>
- 클래스는 “값”으로 사용할 수 있는 “일급 객체”다. → 따라서 표현식으로도 사용 가능

```jsx
class Person{} // 클래스 선언문

// 클래스 표현식 (일반적인 사용법은 아님)
const Person = class{}; // 익명
const Person = class MyClass{}; // 기명 
```

- 기본예제 - 클래스 기반 생성자 함수

```jsx
class Person{
// 1. constructor 생성자 :인스턴스를 생성, 인스턴스를 초기화 함.
	constructor(name){
		this.name = name;
	}

// 2. 프로토타입 메서드
	sayHi(){
		console.log(`hello ${this.name}`)
	}

// 3. 정적 메서드
	static sayHello(){
		console.log(`hello`)
	}
}

const p1 = new Person("Kim") // 인스턴스 생성
p1.sayHi(); // 프로토타입 메서드 호출
Person.sayHello(); // 정적 메서드 호출
```

- 기본예제 - 프로토타입 기반 생성자함수

```jsx
function Person{
	this.name = name; // 1.인스턴스를 초기화 함.
}

// 2. 프로토타입 메서드
Person.prototype.sayHi = function(){console.log(`hello ${this.name}`)}

// 3. 정적 메서드
Person.sayHello = function(){console.log(`hello`)}

const p1 = new Person("Kim") // 인스턴스 생성
p1.sayHi(); // 프로토타입 메서드 호출
Person.sayHello(); // 정적 메서드 호출
```

---

## 클래스의 인스턴스 생성 과정

1. new 연산자와 함께 클래스를 “호출” 후 그 “런타임 직전”에
2. 2-1)constructor 내부에서 암묵적으로 빈 객체(인스턴스 객체) 생성 → 인스턴스 객체와 prototype 객체 연결,  2-2) this 바인딩 됨.
3. constructor 실행되면서, 인스턴스에 프로퍼티 추가 (인스턴스 초기화)
4. 인스턴스 객체가 바인딩된 this가 암묵적으로 반환

## 클래스 호이스팅

※ 클래스는 런타임 이전에 함수 객체를 생성하고, 동시에 프로토타입도 생성된다.

- 클래스 선언문도 호이스팅이 발생한다. ← 모든 선언문은 런타임 이전에 실행되기 때문
그러나 TDZ 때문에 호이스팅이 발생하지 않는 것 처럼 보인다. (let, const 로 선언한 변수의 호이스팅과 같음)

---

## 인스턴스 객체 생성

- 클래스는 생성자함수 이다.
- 일반 함수는 new 연산자를 사용하면 생성자 함수로, 사용하지 않으면 일반함수로 작동 되지만,
클래스는 반드시 new 연산자와 함께 호출해야한다. (클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이기 때문)
- 클래스 이름은 클래스 몸체 내부에서만 유일한 식별자이기 때문에, 변수명으로 인스턴스를 생성해야한다.
    
    ```jsx
    const Person = class MyClass{}
    
    new Person(); // o
    new MyClass(); // x
    ```
    

---

## 메서드

- 메서드 종류 : 1) constructor 2) 프로토타입 메서드 3) 정적 메서드
- 클래스 몸체 내부에는 메서드를 아예 안써도, 쓴다면 이 3개 메서드만 사용할 수 있다.
- 클래스의 메서드는 “프로터타입 객체”의 메서드가 된다.

<br/>

**1. constructor - 인스턴스 객체를 생성하고, 프로퍼티를 초기화함.** 

- constructor 내부의 this는 클래스로 생성된 “인스턴스 객체”를 지칭
- 위 원리를 통해 인스턴스 객체의 프로퍼티를 추가할 수 있음.
- constructor 는 인스턴스 객체를 초기화 한 후 , 그 인스턴스 객체를 암묵적으로 반환한다.
그러나 명시적으로 return을 쓴다면 명시적으로 반환한 객체가 반환된다.(원시값을 리턴하는 경우는 무시됨). 
따라서 constructor 내에서 return문 쓰지 말기
- constructor를 생략하면 암묵적으로 빈 constructor가 정의되어, 빈 객체를 생성한다.
    
    ```jsx
    class Person{}
    
    class Person{
    	constructor(){} // 암묵적으로 constructor가 정의됨
    }
    
    new Person(); // {} 빈 객체 생성
    
    ```
    
- 엄밀히 말하면 constructor는 클래스 내부의 단순한 메서드가 아니라, 클래스가 평가되어 생성한 함수 객체의 일부이다.
- constructor는 클래스 내에 “한 개만” 존재 가능

※ 프로토타입 기반의 생성자 함수에서 나오는 constructor와 클래스에서의 constructor는 별개의 개념임.

<br/>

**2. 프로토타입 메서드**

- “인스턴스의 프로토타입 객체”에 존재하는 메서드가 됨 (프로토타입 체인 원리 이용)
- 프로토타입 기반 생성자함수와 마찬가지로 클래스로 생성된 인스턴스 객체도 프로토타입 체인의 일원이 된다. 
클래스는 단순히 생성자 함수의 역할일 뿐인 것임. 
    
	<br/>

**3. 정적 메서드**

- 정적 메서드는 : 인스턴스를 생성하지 않고도 호출할 수 있는 메서드 (ex: `Person.sayHello()`)
- 정적 메서드는 “클래스 생성자 함수”가 바로 가지고 있는 메서드가 됨.
- 따라서 인스턴스 객체로 호출 불가. 인스턴스 객체는 정적 메서드를 상속받을 수 없다.
- 메서드에 static 키워드를 붙여주면 된다. 
    

---

## 프로토타입 메서드와 정적 메서드 차이

1. this 바인딩
- 프로토타입 메서드 : 프로토타입메서드를 호출한 객체는 “new연산자로 생성된 인스턴스 객체”임. 따라서 메서드 내부의 this는 인스턴스 객체를 의미.
- 정적 메서드 : 프로토타입메서드를 호출한 객체는 “Class 생성자 함수”임. 따라서 메서드 내부의 this는 class함수 임.
2. 프로토타입 체인
- 프로토타입 메서드 : 인스턴스 객체가 프로토타입 객체의 메서드에 접근할 수 있음, 인스턴스 객체의 프로퍼티를 참조할 수 있음
- 정적 메서드 : 인스턴스 객체가 정적 메서드를 참조할 수 없음, 인스턴스 객체의 프로퍼티를 참조할 수 없음

→ 정적 메서드 사용<br/>
: this를 사용하지 않는 메서드는 정적 메서드로 정의 (인스턴스를 생성하지 않고도 바로 사용 가능해서 , 예: `Math.round()`)<br/>
: 전역에서 사용할 함수를 전역 함수로 정의하지 않고도 메서드로 구조화 할 때 유용함.

---

## 접근자 프로퍼티

- 자체적으로 값을 갖지 않는다.
- getter 함수와 setter 함수로 구성됨
- 함수를 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용됨. (참조하면 내부적으로 getrer,setter함수가 호출됨)
- getter함수 : 무언가를 취득할 때 사용. 반드시 무언가를 반황해야 함.
- setter함수 : 무언가를 프로퍼티에 할당할 때 사용. 반드시 단 하나의 매개변수가 있어야 함.
- 클래스의 메서드가 “프로토타입 객체”의 메서드인 것 처럼. 접근자 프로퍼티도 “프로토타입 객체”의 프로퍼티가 된다.

```jsx
// getter, setter 함수 예제
class Person{
	constructor(){
		this.firstName = firstName;
		this.lastName = lastName;		
	}

	get fullName(){
		return `${this.firstName} ${this.lastName}`
	}
	
	set fullName(name){
		this.firstName = name;
	}
}

const me1 = new Person("Jihyun","Jeon");
me1.fullName; // getter함수 호출. "Jihyun Jeon"
me1.fullName("baboo"); // setter함수 호출. firstName 변경됨.
```

---

## 클래스 필드 정의 제안

<클래스 필드란 >

- 인스턴스 객체의 “프로퍼티”를 의미.(일반적인 의미의 객체 에서의 프로퍼티를 클래스에서는 클래스 필드라 칭하는 것임)
- constsructor와 클래스 필드 사용<br/>
    : 인스턴스 객체 생성시 외부 값으로 클래스 필드를 초기화 한다면 - 1)constructor에서 인스턴스 프로퍼티 초기화,<br/>
    : 인스턴스 객체 생성시 외부 값으로 클래스 필드를 초기화할 필요 없다면 - 1) constructor에서 인스턴스 프로퍼티 초기화, 2)클래스 필드 정의 제안 방식
    

<클래스 필드 정의 제안>

- “클래스 생성자 함수 몸체” 내에서 클래스 필드 선언할 수 있다. (최신 브라우저, 최신 nodejs에서 지원됨)
- this에 클래스 필드를 바인딩하면 안됨. (this는 constructor나 메서드 내에서만 유효)
- 클래스 필드를 참조하는 경우에만 this사용.
    
    ```jsx
    class Person{
    	name = "Kim"; // <- 클래스 필드를 "생성자함수 몸체"에 선언
    }
    
    const me1 = Persion();
    ```
    
    ```jsx
    class Person{
    	this.name = "kim"; // x 
    	name = "kim"; // o
    
    	constructor(){
    		console.log(this.name); // o
    	}
    }
    ```
    

---

## private 필드 정의 제안

인스턴스 객체의 프로퍼티는 언제나 public해서 외부에서 다 접근 가능함. 그러나 클래스 필드(인스턴스 객체의 프로퍼티)를 private로 정의할 수 있다.

- 클래스 필드의 선두에 # 붙이고, 참조시도 # 붙임.
- private 필드는 “클래스 몸체”에서 정의해야 함. constructor에서 정의 불가.
- private 필드는 클래스 내부에서만 접근 가능. 
클래스 외부에서 참조 불가(인스턴스 객체를 통해 접근 불가), 자식 클래스 내부에서 접근 불가

```jsx
class Person{ 
	#name = "kim"; // private 필드 정의
    
	constructor(name){
		console.log(this.#name = name); // private 필드 참조
	}
}

const me1 = new Person("kim");
console.log(me1.#name); // x <- private 필드는 클래스 외부에서 참조 불가
```

---

## 상속에 의한 클래스 확장

<클래스 상속 기본 예제>

```jsx
class Animal{
	constructor(age,weight){
		this.age = age;
		this.weight = weight;
	}
	
	eat(){return "eat"};
	move(){return "move"};
}

// Bird클래스가 Animal클래스를 상속
class Bird **extneds** Animal{
	fly(){return "fly"};
}

// Dog클래스가 Animal클래스를 상속
class Dog **extneds** Animal{
	run(){return "run"};
}
```

<extends 키워드>

- 역할 : 수퍼클래스와 서브클래스 간 상속 관계를 설정하는 것
- 수퍼클래스와 서브클래스는 인스턴스 객체의 프로토타입 체인 뿐 아니라, 클래스 간의 프로토타입 체인도 생성함.<br/>
따라서 프로토타입 메서드 뿐 아니라 정적 메서드(생성자 함수에 바로 있는 메서드)도 모두 상속 가능
 
- extends는 1)클래스 뿐 아니라 2)생성자 함수 3)모든 표현식 도 상속받아 클래스를 확장할 수 있다.

```jsx
// <생성자 함수>
Base(){
// 
}

class Derived extends Base{
//
}

// <모든 표현식>
class Derived extends (condition ? Base1 : Base2 );
```

<서브클래스의 constructor>

- 서브클래스에서 constructor를 생략하면<br/>
:  `contructor(...args){super(...args)}`가 암묵적으로 정의됨<br/>
: 이후 new 연산자로 클래스 호출시 전달한 인수가 rest 파라미터로 받게되고, super가 자동으로 호출되서 실행된다.<br/>
- 서브 클래스, 수퍼클래스 둘다 constructor를 생략하면
    
    ```jsx
    class Base{
    	constructor(){}
    }
    
    class Derives{
    	contructor(...args){super(...args)}
    }
    ```
    

## super 키워드

호출과 참조를 할 수 있는 특수한 키워드

<super 호출시>

- 서브클래스에서 super를 호출하면 “수퍼클래스의 constructor”가 호출됨.
- 수퍼클래스의 constructor에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면, 서브클래스에서 constructor 생략가능
- 서브클래스에서 constructor 생략하지 않는 경우, 서브클래스에서 반드시 spuer호출해야 함.
- super호출 전에 this 참조할 수 없음
- super는 서브클래스의 constructor에서만 호출 해야함. (일반 함수나, 수퍼클래스에서 호출 불가)

```jsx
class Base{
	constructor(a,b){
		this.a = a; // this는 new연산자로 생성한 인스턴스 객체(d1)
		this.b = b;
	}
}

class Derived extends Base{
	constructor(a,b,c){
		super(a,b)
		this.c = c; // this는 new연산자로 생성한 인스턴스 객체(d1)
	}
}

const d1 = new Derives(1,2,3); // {a:1, b:2, c:3}
```

<super 참조시>

- 서브클래스의 메서드 내에서 super를 참조하면 “수퍼클래스의 메서드”가 호출됨.
- super가 적혀있는 메서드가 바인딩된 프로토타입 객체의 __proto__에 있는 프로토타입 객체를 참조해야 함.
super는 “수퍼클래스의 prototype객체”를 가리켜야 함.
- 메서드 축약 표현으로 정의된 함수만이 super를 참조할 수 있다. (+super는 클래스에서만 쓰이는게 아니여서, 객체 리터럴도 super참조 가능함.)

```jsx
class Base{
	constructor(name){
		this.name = name
	}

	sayHi(){
		return `Hi ${this.name}.`;
	}
}

// 서브
class Derived extends Base{
	sayHi(){
		return `${super.sayHi()} how are you doing?`; // super는 Base.prototype객체를 가리킴. 따라서 super.sayHi는 Base클래스의 sayHi 메서드인 것.
	}
}

const d1 = new Derived("Kim"); // 
d1.sayHi(); // Hi Kim. how are you doing? 
```

---

## 상속 클래스의 인스턴스 생성 과정

1. new 연산자로 서브클래스가  호출되면 
2. 서브클래스의 constructor 내부의 super가 호출 (→ 수퍼클래스로  실행 이동)
3. 수퍼클래스에서 빈 객체 즉 "인스턴스 객체” 생성 
4. 수퍼클래스의 constructor 실행되면서 
5. 수퍼클래스가 생성한 인스턴스 객체와 this 바인딩 됨. (수퍼클래스의 constructor내부의 this는 “인스턴스 객체”를 가리키게 됨.)
    
    ```
    !! 인스턴스 객체는 내부적으로 수퍼 클래스에서 생성한다. 그러나 new로 호출된 클래스는 서브클래스다.!! 
    따라서 결론적으로 인스턴스 객체를 서브클래스가 생성한 것 처럼 처리된다.
    → 때문에 수퍼클래스의 this도, 서브클래스의 this도 “new 연산자로 생성한 인스턴스 객체”가 되는 것임.
    ```
    
6. 수퍼클래스 인스턴스 초기화 : 인스턴스 객체에 프로퍼티 할당
7. 서브클래스의 constructor로 복귀 > this바인딩 ← 이때 수퍼클래스가 반환한 인스턴스 객체를 this바인딩에 그대로 사용함
    
    ```
    !! super가 호출되지 않으면 수퍼클래스에서 인스턴스 객체가 생성되지 않음.
    → 때문에 super를 호출하기 전에는 this를 참조할 수 없는 것임!
    ```
    
8. 서브클래스 인스턴스 초기화
9. 인스턴스 객체 암묵적 반환


---
# 26장. ES6 함수의 추가 기능
ES6 이전에는 생성자 함수든, 콜백함수든 모든 함수는 사용 목적에 따라 구분없이 사용되었다. <br/>
그러나 ES6에서는 함수를 사용 목적에 따라 세 가지로 구분했다.

<br/>

일반 함수     - 생성자 함수(constructor)O / super X / arguments O

메서드         - 생성자 함수(constructor)X / super O / arguments O

화살표 함수 - 생성자 함수(constructor)X / super X / arguments X

<br/>

## 화살표 함수

: 화살표 함수는 this 바인딩이 존재하지 않음. 

- 따라서 화살표함수의 this는 스코프 체인을 통해 상위 스코프의 this가 됨.
- 따라서 call,apply,bind 메서드를 통해 화살표 함수 내부의 this를 바꿀 수 없다.

<br/>

## arguments와 rest파라미터

arguments : 유사 배열 

```jsx
function sum(){
console.log(arguments); // Arguments(5) [1, 2, 3, 4, 5] <- 유사배열
Array.prototype.slice().call(arguments); // slice메서드 내부의 this는 arguments로 지정해줌
}

sum(1,2,3,4,5);

// 배열.slice()를 쓰면 메서드를 호출한 객체인 배열이 this로 되는데,
// arguments는 유사배열이라 slice메서드가 없어서 arguments.slice()로 할 수 없기때문에 직접 call로 this를 지정해주는 것임.
```

rest파라미터 : 배열로 받음

```jsx
function sum(...args){
	console.log(args); // [1,2,3]
}

sum(1,2,3)
```