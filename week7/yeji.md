# 34장: 이터러블

## 34.1 이터레이션 프로토콜

순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의해 미리 약속한 규칙

이터레이션 프로토콜

- 이터러블 프로토콜 -> 준수한 객체를 이터러블 
  
  - (해석) 순회 가능한 객체를 이터러블이라고 하며, 이터러블이 따라야 하는 조건이 이터러블 프로토콜

- 이터레이터 프로토콜 -> 준수한 객체를 이터레이터
  
  - (해석) 반복자가 따르는 조건이 이터레이터 프로토콜 

### 34.1.1 이터러블

Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나, 프로토타입 체인을 통해 상속받은 객체

for...of 문으로 순회 가능 / 스프레드 문법 가능 / 배열 디스트럭처링 할당 가능

### 34.1.2 이터레이터

이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.

next 메서드를 호출 -> 이터러블 순회하며 결과 나타내는 iterator result object 반환

iterator result object의 value 프로퍼티는 순회중인 이터러블의 값, done은 이터러블의 순회 완료 여부를 나타냄.

```javascript
// 이터러블인 배열 선언
const array = [1, 2, 3];

// iterable
// 배열의 이터레이터(포인터)를 추출 ( 이터러블 프로토콜을 준수하기 때문에 이터레이터를 반환함 )
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수하기 때문에 next()를 호출할 수 있으며, value와 done을 키로 가진 객체를 반환함
iterator.next();    // { value: 1, done: false }
iterator.next();    // { value: 2, done: false }
iterator.next();    // { value: 3, done: false }
iterator.next();    // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

Array, String, Map, Set, TypedArray, arguments, DOM 컬렉션에서 빌트인 이터러블 제공

## 34.3 for ... of 문

이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.

```javascript
for (변수 선언문 of 이터러블) { ... }
```

```javascript
for (const item of [1, 2, 3]){
    console.log(item); // 1 2 3 
} 
```

> for ... in 문 비교
> 
> 모든 프로토타입의 프로퍼티 중에서
> 
> 프로퍼티 어트리뷰트 [[Enumerable]] 의 값이 true 인 프로퍼티를 
> 
> 순회하며 열거한다.

## 34.4 이터러블과 유사 배열 객체

1) 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고

2) length 프로퍼티를 갖는 객체 -> for 문 순회 가능

3) But, 이터러블이 아니라 for ...  of 문 순회 불가능 

4) 예외, arguments, NodeList, HTMLCollection은 유사배열이면서 이터러블이다

## 34.5 이터레이션 프로토콜의 필요성

다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여

> 데이터 공급자:  Array, String, Map/Set, DOM 컬렉션 등 다양한 이터러블

> 하나의 순회 방식:
> 
> - Symbol.iterator 메서드를 호출해 이터레이터 생성
> 
> - 이터레이터의 next 메서드를 호출해 이터러블 순회하며 iterator result object 반환 
> 
> - iterator result object의 value/done 프로퍼티 취득 

데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 

> 데이터 소비자; for... of, 스프레드 문법, 배열 디스트럭처링 할당, Map/Set 생성자...

데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.

## 34.6 사용자 정의 이터러블

이터레이션 프로토콜을 따르면 사용자 정의 이터러블 만드는 것도 가능. (책의 예시는 피보나치)

- 해당 예제는 지연 평가(필요할 때 되서야 데이터 생성)으로 불필요한 데이터를 미리 생성하지 않고, 무한한 표현도 가능 

# 35장: 스프레드 문법

하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐 개별적인 값들의 목록으로 만든다. 

스프레드 문법의 대상은 순회할 수 있는 **이터러블에 한정**. 

스프레드 문법의 결과물은 값이 아니며, **쉼표로 구분한 값의 목록을 사용하는 문맥**에서만 사용 가능.

- 함수 호출문의 인수 목록

- 배열 리터럴의 요소 목록
  
  - 배열을 concat할 때 ex) `[... [1, 2], ...[3, 4]]`
  
  - 배열을 splice 할 때 ex) `arr1.splice(1, 0, ...arr2);` 
  
  - 배열을 복사할 때 ex) `const copy = [... origin];`
  
  - 이터러블을 배열로 변환할 때 
  
  ```javascript
  function sum() {
      return [...arguments].reduce((pre, cur)=> pre + cur, 0);
  }
  ```

- 객체 리터럴의 프로퍼티 목록
  
  - 스프레드 문법의 대상은 이터러블이어야 하지만
    **스프레드 프로퍼티** 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다. 
  
  - 스프레드 프로퍼티: Spread properties in object initializers copies own enumerable properties from a provided object onto the newly created object.
    
    [GitHub - tc39/proposal-object-rest-spread: Rest/Spread Properties for ECMAScript](https://github.com/tc39/proposal-object-rest-spread)

# 36장: 디스트럭처링 할당

구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것 

## 36.1 배열 디스트럭처링 할당

```javascript
const arr = [1, 2, 3];
const [one, two, three] = arr; 
let x, y;
[x, y]= [1, 2]; 

// 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요 X
const [c, d] = [1]; // d === undefined
const [e, f] = [1, 2, 3];
const [g, ,h] = [1, 2, 3]; // g === 1, h ===3 

// 변수에 기본값 설정 가능
// 기본값보다 할당된 값이 우선
const [j, k = 10, l = 3] = [1, 2];
console.log(j, k, l); // 1, 2, 3

//Rest 요소 사용 가능
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

## 36.2 객체 디스트럭처링 할당

객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출해 1개 이상의 변수에 할당

할당의 대상(우변)은 객채여야 하며, 할당 기준은 프로퍼티

```javascript
// 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당
const {lastName, firstName} = {firstName: 'marie', lastName: 'kim'};

// 객체 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 
const {lastName: ln, firstName: fn} = {firstName: 'marie', lastName: 'kim'};

// 변수에 기본값 설정 가능 
const {firstName = 'marie', lastName} = {lastName: 'kim';}

// 원하는 프로퍼티만 추출하고 싶을 때 유용
const todo = {id: 1, content: 'HTML', completed: true};
const {id} = todo;

// 매개변수에도 활용 가능
function printTodo({ content, completed }) {
    console.log('${content}의 완료 상태는 ${completed}');
}
printTodo({id: 1, content: 'HTML', completed: true});

// 배열 요소가 객체일 때 배열 디스트럭쳐링 + 객체 디스트럭처링
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: false },
  { id: 3, content: 'JS', completed: false }
];

const [, { id }] = todos; // todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
console.log(id); // 2

// 중첩객체
const user = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul'
  }
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const { address: { city } } = user;
console.log(city); // 'Seoul'
```



# 37장: Set과 Map

## 37.1 Set

중복되지 않는 유일한 값들의 집합 

자바스크립트의 모든 값을 요소로 저장할 수 있다. 



- size 프로퍼티

- add 메서드 
  
  - 반환값: 새로운 요소가 추가된 Set 객체 - 메서드 체이닝 가능

- has 메서드

- delete 메서드 
  
  - 인수: 삭제하려는 요소의 '값'
  
  - 반환값: 불리언 - 메서드 체이닝 불가능
  
  - 존재하지 않는 요소를 삭제하려고 하면 에러 없이 무시)

- clear 메서드

- forEach 메서드
  
  - `forEach(현재 순회 중인 요소 값, 현재 순회 중인 요소 값, 현재 순회 중인 set 객체 자체)`

- 이터러블이므로 스프레드 문법, 배열 디스트럭처링의 대상이 될 수 있음.



집합 연산 - 프로토타입 메서드 직접 구현해야

- 교집합(intersection)

- 합집합(union)

- 차집합(difference)

- 부분집합, 상위 집합



## 37.2 Map

키와 값의 쌍으로 이뤄진 컬렉션.

키로 객체를 포함한 모든 값을 사용할 수 있으며, 이터러블이다. 

- 생성
  
  - 인수: 키와 값의 쌍으로 이뤄진 요소로 구성된 이터러블 
  
  - 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다 

- size 프로퍼티 

- set 메서드
  
  - 반환값: 새로운 요소가 추가된 Map 객체 - 메서드 체이닝 가능 

- get 메서드
  
  - 전달한 키를 갖는 요소가 없으면 undefined

- has 메서드 

- delete 메서드 
  
  - 존재하지 않는 키로 삭제하려 하면 에러없이 무시 
  
  - 반환값: 불리언 - 메서드 체이닝 불가 

- clear 메서드

- `forEach(현재 순회 중인 요소 값, 현재 순회 중인 요소 키, 현재 순회 중인 Map 객체 자체)`

- 이터러블이므로 스프레드 문법, 배열 디스럭처링 할당 가능 














