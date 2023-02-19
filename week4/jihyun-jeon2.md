# 19.12장 ~ 
## ✅ 정적 프로퍼티/메서드

- 생성자 함수(객체)가 소유한 프로퍼티,메서드를 “정적 프로퍼티,메서드”라고 함.
- 정적 메서드는 인스턴스를 생성하지 않아도 바로 호출할 수 있다.
- 생성자 함수가 생성한 인스턴스로는 참조할 수 없다. (프로토타입 체인상에 존재하지 않으므로)

```jsx
function Person(name){
	this.name = name;
}

Person.prototype.sayHello = function(){}; // 프로토타입 메서드
Person.staticMethod = function(){}; // 정적 메서드

const me = Person("jihyun");

Person.staticMethod(); // 정적 메서드는 생성자 함수로 호출 가능
me.staticMethod(); // 정적 메서드는 인스턴스 객체로 호출x
```

- 형태 : Object.함수() ← 정적 메서드 / Object.prototype.함수() ← 프로토타입 메서드

<br/>

## ✅ 프로퍼티 존재 확인 방법

방법1. in연산자

- 형태 :  `key in object`
- 객체의 프로토타입 체인 상에 존재하는 모든 프로퍼티 검색 가능함.

```jsx
const person = {name:"Kim", age:20};

console.log('name' in person); // true
console.log('age' in person); // true
console.log('toString' in person); // true ✔️
```

방법2. Object.prototype.hasOwnProperty 메서드

- 객체의 프로토타입 체인 상에 존재하는 모든 프로퍼티 검색X (프로토타입으로 상속받은 프로퍼티는 false를 반환)

```jsx
const person = {name:"Kim", age:20};

console.log(person.hasOwnProperty.('name')); // true
console.log(person.hasOwnProperty.('toString')); // false ✔️
```

<br/>

## ✅ 프로퍼티 열거

방법1. for … in문

- 순회대상 객체의 프로퍼티 뿐 아니라, “상속받은 프로토타입 객체”의 프로퍼티까지 열거한다. 
(→ 자세히 말하면, 상속받은 프로토타입 객체의 프로퍼티 어트리뷰트 [[Enumerable]]값이 true인 프로퍼티만 가능함.)
- 순회하며 프로퍼티를 열거할 때 순서를 보장하지 않는다. 프로퍼티 키가 숫자면 자동으로 오름차순 순으로 정렬해서 나온다.

```jsx
const obj = {2:"b",1:"a",3:"c",z:"d",y:"e"};

for(let key in obj){
	console.log(key); // 1,2,3,z,y
}
```

방법2. Object.key()/values()/entries() 메서드

- 객체 자신의 고유 프로퍼티만 열거하기 위해서는 for…in문 보다, 이 방법이 낫다.

---
 #  20장 strict mode
 읽기만, 생략

---
 #  21장 빌트인 객체
 ## ✅  자바스크립트 객체의 분류

1. 표준 빌트인 객체 <br/>
: ECMAScript 사양에 정의된 객체, 전역 객체의 프로퍼티여서 별도 선언없이 앱 전역에서 참조 가능<br/>
: 브라우저, Node.js 환경과 관계없이 사용 가능<br/>
: 예) Object, Array, Symbol, Math,Function, Promise, Proxy, JSON, Error…<br/>

2. 호스트 객체<br/>
: 브라우저, Node.js 환경에서 추가로 제공하는 객체<br/>
: 브라우저 환경에서는 -  클라이언트 사이드 web API 를 제공 ( fetch, Canvas,DOM,Web Storage,History API 등..)<br/>
: Node.js 환경에서는 - Node.js 고유의 API 를 제공<br/>

3. 사용자 정의 객체

## ✅ 표준 빌트인 객체

- Math, Reflect, JSON 을 제외한 모든 표준 빌트인 객체를 생성자 함수이다.
    
    1) 생성자 함수 객체인 표준 빌트인 객체는 : “프로토타입(객체에 있는) 메서드”와 “정적 메서드”를 제공<br/>
      (생성자함수의 prototype 프로퍼티에 프로토타입 객체가 바인딩 되있기 때문에 프로토타입 객체의 메서드 사용 가능)
    
    2) 생성자가 아닌 함수 객체인 표준 빌트인 객체는 : “정적 메서드”만 제공
    
    ```jsx
    const num = new Number(1.5) // {1.5}
    
    console.log(num.toFixed()); // toFixed는 Number.prototype의 "프로토타입 메서드"
    console.log(Number.isInteger(0.5)); // iseInteger는 Number의 "정적 메서드"임
    ```
    

## ✅ 원시값과 래퍼 객체

- 원시값을 객체처럼 사용하면(예: `"hello".length`), 그 원시값을 인스턴스 객체로 변환된다.
- 그리고 그 인스턴스 객체는 “래퍼 객체”이다.  (래퍼 객체란 : 원시값에 객체처럼 접근하면 생성되는 임시 객체)
- 인스턴스 객체로 되기 때문에 프로토타입 메서드를 상속받아 사용할 수 있다.
- 이후 객체처럼 사용이 끝나면 다시 원시값으로 돌리고, 래퍼 객체는 가비지 컬렉션의 대상이 된다.
- null , undefined는 래퍼 객체를 생성하지 않는다. 따라서 객체처럼 사용할 수 없음.

```jsx
const str = "Hello";

console.log(str.length); // 원시타입인 문자열이 인스턴스 객체로 변환되고, 래퍼 객체가 됨.
console.log(str.toUpperCase()) // String 프로토타입 객체의 메소드를 상속 가능.

// 다시 원시값으로 돌리고, 래퍼 객체는 가비지 컬렉션 대상이 됨.
```

## ✅ 전역 객체

- 최상위 객체임. 자바스크립트 엔진에 의해 어떠한 객체보다도 먼저 생성
- 전역 개체는 개발자가 의도적으로 생성할 수 없음
- 전역 객체의 프로퍼티를 참조할 때 window(global)은 생략 가능
- 여러개 script를 통해 js코드를 분리해도 하나의 전역 객체 window를 공유하게 된다.
- 전역 객체의 프로퍼티
    - 표준 빌트인 객체 (Object, Array, Symbol, Math,Function, Promise, Proxy, JSON, Error…)
    - 호스트 객체( 클라이언트 사이드 web API, Node.js 고유의 API ) 제공. 환경에 따라 추가적인 프로퍼티를 갖는다.
    - var 로 선언한 전역 변수는 전역 객체의 프로퍼티가 됨.
    (※ let, const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 보이지 않는 블록(전역 렉시컬 환경의 선언적 환경 레코드)에 존재하게 된다. )
    - (선언하지 않는 변수에 값 할당한)암묵적 전역도 전역 객체의 프로퍼티가 됨.
    - 전역 함수도  전역 객체의 프로퍼티가 됨.
        
        ```jsx
        var foo = 1; // var로 선언한 전역 변수
        bar = 1; // 전역 변수가 아니라 전역 객체의 프로퍼티가 됨 (window.bar = 1)
        ```
        
- 빌트인 전역 프로퍼티 (전역 객체의 프로퍼티) : Infinity , NaN, undefined 등
- 빌트인 전역 함수 (전역 객체의 메서드) : isFinite, isNaN, parseFloat 등

---

 #  22장 this
 ### ✅ this란

- 함수 자신이 속한 객체 또는 함수 자신이 생성할 인스턴스 객체
- this가 필요한 이유 : 함수 자신이 속한 객체나 자신이 생성할 인스턴스를 가리킬 수 있기 위해서.
- strict mode가 적용된 일반 함수 내에서의 this는 window가 아닌 “undefined”가 된다.<br/>
(this는 자신이 속한 객체를 가리키기 위함인데, 그냥 window가 나오는건 의미가 없기 때문에 undefined가 나오는 것)

### ✅ this는 함수를 "호출하는 방식"에 따라 달라진다.

- 함수 “호출” 시점에, 함수 “호출 방식”에 따라 this는 동적으로 다르게 결정된다.
- 함수 호출시, arguments 객체와 this가 함수 내부에 전달된다.

### 1. 콜백함수, 중첩함수 등 어떠한 함수라도 일반 함수로 호출되면, 그 함수 내부의 this는 "전역객체, window" 가 바인딩 된다.

- 일반함수호출
    
    ```jsx
    function foo(){
     console.log(this); // window
      function bar(){
        console.log(this); // window
       }
       bar();
     }
    foo();
    ``
    ```
    <br/>

- 메서드로 정의한 “중첩함수”도 일반함수로 호출되면 중첩함수 내부의 this는 window임.
    
    ```jsx
    const obj = {
    	value: 100;
    
    	foo(){
    	console.log(this); // [p1] obj객체 {value:100, foo : fn()}
    	function bar(){
    		console.log(this) // [p2] window
    	   }
    	bar(); // [p2]일반함수 호출
        }
    
    }
    
    obj.foo();	// [p1]메서드 호출
    ```
    <br/>

- “콜백함수”도 일반 함수로 호출되면 콜백함수 내부의 this도 window가 바인딩 됨.
    
    ```jsx
    	const person = {
        	name : "Lee"
    
          	foo(){
              //foo함수 안의 콜백함수는 일반함수 호출임. 따라서 this는 window임.
           	 	setTimeout(function(){
            		console.log(this.name);
            		}, 100)
           		 }
    
        	};
    
    person.foo();
    ```
    
<br/>

### 2.메서드 호출

- 메서드 내부의 this는 , 메서드를 호출한 객체(쉽게말하면 점.앞에 있는 객체)가 this가 된다.
    
    ```jsx
    const obj = {
    	name : "Lee";
      	foo(){
        	 return this.name; // "Lee"
        }
      }
    
      obj.foo(): // 메서드 foo를 호출한 객체인 obj가 this가 됨.
    ```
    
<br/>

### 3.생성자 함수로 호출

- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스 객체에 바인딩 됨.
    
    ```jsx
     function Circle(num){
       this.radius : num;
       this.diameter = function(){
       		return this.radius * 2;
       }
     }
    
    new Circle(5); // {radius : 5, diameter : fn}
    new Circle(10); // {radius : 10, diameter : fn}
    ```
<br/>

### 4. 이벤트 핸들러 함수 내에서의 this

: 이벤트 핸들러를 바인딩한 dom element 요소

<br/>

### 5. arrow함수 내에서의 this

arrow함수 내에서의 this는 한단계 위의 스코프의 this가 됨.

```jsx
var value = 1;

const obj = {
	value : 100,
	foo(){setTimeout(()=>console.log(this.value))} // 화살표 함수의 스코프가 아닌 한단계 위인 foo함수 스코프의 this가 바인딩 됨.
}
// obj.foo() // 100
```

## 6. apply, call, bind

**1) 형태 - `apply(thisArg,[1,2,3])` , `call(thisArg, 1,2,3)` , `bind(thisArg, 1,2,3)`**

- apply(this로 사용할 객체, 함수에게 전달할 인자를 “배열” 형식으로 전달) ← 함수를 호출함
- call (this로 사용할 객체, 함수에게 전달할 인자를 “쉼표”로 구분하여 전달) ← 함수를 호출함
- bind (this로 사용할 객체, 함수에게 전달할 인자를 “쉼표”로 구분하여 전달)  ← 함수를 호출하지 않음.(명시적으로 호출해 줘야함.)

**2) apply,call 과 bind 차이**

- apply,call: 함수를 실행할 때, (함수의 컨텍스트) this 값을 바꾸어 함수를 호출함.
- bind: 어떤 함수의 this값을 영구적으로 바꾼 새로운 함수를 만들어 반환함. (함수가 호출되는건 아님)

**3) 예제**

- apply,call 예제
    
    ```jsx
    // < 기본예제 >
     function test(){
       console.log(arguments)
      return this;
     }
    
    const obj = {a:1}; //this로 지정할 객체
    
    test(); // window
    test.apply(obj, [1,2,3]); // arguments(3) [1,2,3] // {a:1}
    test.call(obj, 1,2,3); // arguments(3) [1,2,3] // {a:1}
    ```
    
    ```jsx
    // < 유사 배열을 실제 배열로 복사 > // (유사배열은 slice메서드를 바로 사용할 수 없지만, 이런 방식으로 사용 가능)
    function convert(){
    	const arr = Array.prototype.slice.call(arguments); // slice메서드 안의 this를 argument로 지정함.
    	const arr = Array.prototype.slice.apply(arguments);
    }
    
    convert(1,2,3)
    ```
    

- bind 예제1
    
    ```jsx
    function test(){
     return this;
    }
    
    const obj = {a:1};
    
    test.bind(obj);   // bind메서드는 this를 지정만 하고, 함수를 호출하지 않음
    test.bind(obj)(); // 따라서 명시적으로 호출해야 한다
    ```
    

- bind 예제2<br/>
:콜백함수가 “일반함수"로 호출되기 때문에 this는 window인데, bind로 this를 person으로 직접 지정해줌.<br/>
: [질문] bind 안의 this가 어떻게 person을 가리키는건지?<br/>
 &nbsp; [답변] bind는 foo함수에서 실행아 된다. 때문에 foo 함수 내부의 this는 person이기 때문에, bind의 this에는 person 객체가 되는 것임.
    
    ```jsx
    const person = {
     name : "Lee",
      foo(){
    	// foo함수 내부의 this는 foo메서드를 호출한 객체인 person이다.
      	setTimeout(function(){
        	console.log(`hello,${this.name}`);
        }.bind(this),1000) 
      }
    }
    
    person.foo(); // hello,Lee
    ```