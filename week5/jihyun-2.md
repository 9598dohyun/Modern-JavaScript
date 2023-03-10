# 24. 클로저

클로저는 js의 고유 개념이 아니라, 함수형 프로그래밍 언어에서 사용되는 개념이다. 

## 렉시컬 스코프

- 스코프 체인은 : 실행 컨텍스트의 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조” 를 통해 상위 렉시컬 환경과 연결되는 것이고
- 이런 상위 스코프에 대한 참조는 함수가 "정의" 되는 시점에 결정된다. 따라서 <br/>
함수를 어디서 호출했는지가 아닌, 어디서 “정의” 했는지에 따라 상위 스코프가 결정된다. ← 이것이 바로 렉시컬 스코프<br/>

<hr/>

## 함수 객체의 내부 슬롯 [[Environment]]

- 함수 객체에 있는 내부 슬롯 [[Environment]]을 알면 : 렉시컬 스코프가 작동되는 원리을 알 수 있음.
- 그 원리 한줄 요약 <br/>
: (런타임 이전에) 함수 객체가 생성될 때, 함수 객체의 내부 슬롯 [[Environment]] 에는 상위 스코프가 저장된다.<br/> 때문에 함수가 정의된 위치에 따라 상위 스코프가 정해지는 렉시컬 스코프가 작동할 수 있는 것임<br/>

1. 함수 정의를 평가하여 함수 객체를 생성할 때 함수 객체의 내부 슬롯 [[Environment]]에는 “현재 실행중인 실행 컨텍스트의 렉시컬 환경 즉, 상위 스코프”를 가리키게 된다.<br/>
(함수 객체를 생성하는 시점은 상위 함수가 실행되고 있는 시점이기 때문)
<br/>

2. 또한 함수가 호출되었을 때(정확히 말하면 호출 후 실행 직전?[질문]) 생성된 함수 실행 컨텍스트의 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”도 “현재 실행중인 실행 컨텍스트의 렉시컬 환경 즉, 상위 스코프”를 가리키게 된다.

- 그림 24-1 참조

📌 코드 예시 

```css
const x = 1;

function foo(){
	const x = 10;
	bar();
}

function bar(){
	console.log(x);
}

foo();
bar();
```

1. 전역 코드 평가 시점에 foo, bar 함수 객체 생성 -> window의 메서드로 등록됨.
2. 그 함수 객체의 내부슬롯 [[Environment]] 에는 global 렉시컬 환경이 참조되고
3. foo, bar 함수가 호출되면 (정확히는 호출 후 실행되기 직전을 말하는 것 같음) ←[질문]
    
    : foo,bar 함수 실행 컨텍스트 생성 > 함수 렉시컬 환경 생성 > “외부 렉시컬 환경에 대한 참조”에  global 렉시컬 환경이 참조가 할당됨.
    

→ 이런식으로  함수가 정의된 위치에 따라 함수가 실행되기 이전에 이미 상위 스코프가 결정된다.(렉시컬 스코프)

<hr/>

## 클로저와 렉시컬 환경

- 클로저란 : 1)중첩 함수가 외부 함수 보다 더 오래 유지되고 있고, 2)중첩 함수에서 이미 실행이 종료된 외부 함수의 스코프의 식별자를 참조할 수 있는 현상
- 클로저가 될 수 없는 경우
    - 외부함수 보다 중첩 함수의 생명 주기가 더 짧은 경우 클로저 아님.
    - 이미 종료된 외부함수의 스코프의 어떤 식별자도 참조 하지 않는 함수는 클로저 아님
- 함수는 상위 스코프를 기억하므로 모든 함수는 클로저다.

📌  클로저 예제코드

```jsx
const x = 1;

function outer(){
	const x = 10;
	const inner = function(){console.log(x)}; // 클로저1 
	return inner
}

const innerFunc = outer(); // 클로저2
innerFunc(); // 10
```

클로저1 

: 외부함수(outer)가 소멸해도 반환된 중첩 함수(inner)는 outer의 변수를 참조할 수 있다.

- outer함수가 실행되어 inner를 반환하면 outer는 실행 컨텍스트 스택에서 제거된다.<Br/>
그러나 outer함수의 렉시컬 환경까지 소멸하는 것은 아님. 따라서 클로저 가능
- 렉시컬 환경의 값을 누군가 참조하고 있다면 그 렉시컬 환경은 사라지지 않는다.

클로저2 

: inner함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 inner함수의 실행 컨텍스트 스택이 제거되도 <Br/>inner함수의 렉시컬 환경은 소멸하지 않는다. 따라서 클로저 가능

(생각 : 스택은 단순히 실행 컨텍스트의 "실행 순서"만 관리할 뿐이고, 렉시컬 환경은 실행 컨텍스트의 정보들(식별자,스코프체인)을 관리하는 것이다.)
<hr/>

## 클로저 활용

- 전역 변수로 상태를 선언한다면, 여러 함수에서 모두 접근할 수 있기 때문에 의도치 않은 상태 변경이 될 수 있음.
- 클로저를 활용한 은닉을 활용하면 “특정 함수에서만 그 상태에 접근하고 변경”할 수 있어서, <br/>
상태가 “의도치 않게 변경되지 않도록” 하여 "상태를 안전하게" 변경하고 유지할 수 있다.

은닉 예제

```jsx
const increase = (function(){
		let num = 0; 
		
		return function(){
				return num++
			}
	})()

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3 
```

- 즉시실행함수의 실행 컨텍스트 스택은 없어졌지만, 중첩함수에서 외부함수(즉시실행함수)의 식별자를 참조하고 있으므로, 렉시컬 환경은 없어지지 않는다.
- num변수는 은닉 되어있다. 따라서 num 변수에의 접근은 increase함수를 통해서만 가능하고, 전역 변수처럼 외부에서 직접 접근할 수 없다.

<Br/>
<hr/>

## 캡슐화와 정보 은닉

- 캡슐화 : 여러 객체에서 공통적으로 쓰는 메서드를 하나로 묶는 것 ()<br/>
- 캡슐화는 객체의 특정 프로퍼티와 메서드를 감추기 위해서 사용되기도 함(정보 은닉)<br/>
: 객체 외부에서 함부로 접근하면 안되는 프로퍼티나 메소드에 직접 접근할 수 없도록 한다.<br/>
: 특정 프로퍼티에 대한 접근을 미리 정해진 메소드들을 통해서만 가능하게 하는 것.<br/>
: 이를 통해 프로그래밍의 안전성을 높일 수 있다.<br/>
 

- 캡슐화를 통한 정보은닉 코드 예제

```jsx
function Person(name,age){
	this.name = name;
	let _age = age; // private. 이 변수는 생성자함수 외부에서 참조하거나 변경할 수 없다.

	this.sayHi = function(){console.log(`${this.name},${this._age}`)}
}

const person1 = new Person("kim",20); 
person1.name // 접근ㅇ
person1._age // 접근 불가능
```

- 그러나 js는 정보 은닉을 완전하게 지원하지 않는다고 한다.

## 자주 발생하는 실수

let,const를 사용한 반복문은, 블록을 반복할 때 마다 새로운 “렉시컬 환경”을 생성한다.

for문이 반복될 때마다 독립적인 새로운 렉시컬 환경을 생성하여 식별자 값을 유지한다.

이때 그 코드 블록 내에서 정의한 함수가 있다면 그 함수의 상위 스코프는 for문을 돌면서 코드 블록별로 생성된 새로운 렉시컬 환경이다.

만약 코드 블록 내에 정의한 함수가 없다면, 새로운 렉시컬 환경은 아무도 참조하지 않기 떄문에 바로 가비지 컬렉션의 대상이 된다.

✔️ let,const 블록 스코프이기 떄문에, 블록 단위로 실행 컨텍스트가 생성된다.<br/>
✔️ 함수 단위로 실행 컨텍스트가 생성된다고 생각했는데, es6에 let,const가 들어오면서 블록단위로 생성되기도 한다.