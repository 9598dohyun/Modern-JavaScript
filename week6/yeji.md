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
iterator.next();    // { value: 1, done: true }
iterator.next();    // { value: 2, done: true }
iterator.next();    // { value: 3, done: false }
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
    
    


