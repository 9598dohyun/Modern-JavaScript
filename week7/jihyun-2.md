# 34.이터러블


# 35장. 스프레드 문법

스프레드 문법은 값을 생성하는 연산자가 아님, 따라서 변수에 할당 불가

## 사용할 수 있는 대상

- for…of로 순회할 수 있는 이터러블 <br/>
: Array, String, Map,Set, Dom컬렉션(HTMLCollection,NodeList ←유사배열) , arguments ←유사배열

## 사용할 수 없는 대상

- 이터러블 아닌 일반객체는 스프레드 문법 대상x <br/>
: `console.log(…{a:1,b:2})` // TypeError

## 사용

1. 함수 인수를 목록으로 받을 때 
: ex&#41; `Max.Math(…arr)`

2. 객체 리터럴 프로퍼티 목록 
: ex&#41; `{…obj1, …obj2}` <br/>
: es6이전엔 Object.assign() 사용하여 객체 병합했음.

3. 배열 리터럴 요소 목록 
: ex&#41;  `const copy = […arr]` <br/>
  - 유사 배열 객체인 arguments를 배열로 변환하는 방법 <br/>
    1&#41; `Array.prototype.slice.call(arguments)` ← es6 이전. <br/>
    2&#41; 스프레드 문법 사용 `[...arguments]` <br/>
- splice메서드 에서의 사용 예제 <br/>
    
    ```jsx
    const arr1 = [1,4];
    const arr2 = [2,3];
    console.log(arr1.splice(1,0,...arr2)); // [1,2,3,4]
    ```
    
- 이터러블이 아닌 유사 배열 객체를 배열로 만드는 법 <br/>
: spread 문법 사용 불가 <br/>
: Array.from() 사용 ㅇ (ex. `Array.from(유사배열객체) // 배열로 반환` )

## rest파라미터 vs 스프레드 문법 차이

: 이 둘은 반대의 개념임 <br/>

- rest파라미터 : 나열된 인수 목록을 하나의 배열로 묶어 전달받음. <br/> `function test(...params){console.log(params)} // [1,2,3,]` <br/>
- 스프레드 문법: 배열과 같이 묶여있는 것의 값을 나열시킴. `test(…arr)`

---

# 36. 디스트럭처링 할당

사용 : 배열과 같은 이터러블 , 객체에서 필요한 값만 추출해서 변수로 받고싶을 때 유용

## 배열 디스트럭처링 할당

- 우변은 이터러블 이여야 함
- 좌변은 이름은 상관x, 배열의 “순서”대로 할당됨

```jsx
const [x,y] = [1,2];
const [x,y="a"] = [1,2]; // [1,2] <- 기본값을 지정할 수 있음
```

## 객체 디스트럭처링 할당

- 좌변은 순서는 상관x, 객체 프로퍼티 키 “이름”이 같아야 함

```jsx
const user = {firstName:"duboo", lastName:"Jeon"}

const {firstName:firstName, lastName:lastName} = user  

// 축약 표현
const {firstName,lastName} = user
```

- 다른 이름으로 받는 법

```jsx
const user = {firstName:"duboo", lastName:"Jeon"}

const {firstName:f1,lastName:f2} = user // f1 = "duboo" , f2 = "Jeon"
```

- 기본값 지정할 수 있음

```jsx
const user = {firstName:"duboo"}

const {firstName,lastName="Jeon"} = user // f1 = "duboo" , f2 = "Jeon"
```

## 배열 디스트럭처링 + 객체 디스트럭처링 혼용

```jsx

const arr = [{a:1,b:2},{c:3,d:4},{e:5,f:6}];

const [,secondObj]=arr;  // secondObj ={c:3,d:4}
const [,{c}]=arr;  // c = 3
```

## rest파라미터와 디스트럭처링

```jsx
const [x, ...y] = [1,2,3,4] // x = 1, y = [2,3,4]
const {x,...rest} = {x:1, y:2, z:3}; // x = 1, rest = {y:2, z:3}
```

---

# 37장. Set, Map

## < Map: 객체와 유사함>

### ✅ 객체와 차이

1) 객체 : 이터러블x ↔ Map객체 : 이터러블ㅇ

2) 객체 : 프로퍼티명(키값)에 문자,심벌값만 가능 ↔ Map객체 : 프로퍼티 키에 객체를 포함한 모든 값 가능

### ✅ Map 객체의 생성

- 이터러블을 인수로 전달받아 Map객체를 생성함 
```jsx
new Map(); // {} <- Map생성자 함수로 인한 인스턴스 객체가 생김

new Map([['key1', 'value1'],['key2', 'value2'],]); // {'key1' => 'value1', 'key2' => 'value2'}
```

### ✅ Map의 프로퍼티,메소드

1. 요소 개수 확인 - Map.prototype.size
2. 요소 추가 - Map.prototype.set(key,value)

: 반환값 - 새로운 요소가 추가된 Map객체를 반환함.
- 객체처럼 중복된 키를 갖는 요소는 Map객체에 요소로 될 수 없음.
- 문자열뿐 아니라 모든 값을 키로 사용할 수 있음.

```jsx
const mapObj = new Map();
mapObj.set('key1', 'value1').set('key2', 'value2'); // {'key1' => 'value1', 'key2' => 'value2'} 
``` 
<Br/>
3. 요소 취득 - Map.prototype.get(key)

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };
const mapObject = new Map([
  [lee, 'designer'],
  [kim, 'developer'],
]);

//  mapObject = [{{name: 'Lee'} => "designer"} , {{ name: 'Kim' } => "developer"}]
console.log(mapObject.get(kim)); // developer
```

4. 요소 검색 - Map.prototype.has(key)
: Map객체에 해당 key가 존재하면 true, 아니면 false를 반환

```jsx
console.log(mapObject.has(kim)); // true
```

5. 요소 삭제 - Map.prototype.delete(key);
: 반환값 - 삭제 성공 여부를 나타내는 불리언 값 반환됨.

```jsx
mapObject.delete(lee); // true
console.log(mapObject); // mapObject = [{{ name: 'Kim' } => "developer"}]
```

6. 요소 일괄 삭제 - `Map.prototype.clear();`
7. 요소 순회
    
    1) forEach((현재 순회중인 요소의 값, 현재 순회중인 요소의 키, 현재 순회중인 객체자체)=>{})
    
    2) for...of
    
8. Map객체는 이터러블임.
    
    1) 때문에 스프레드 문법 쓸 수 있음.
    
    ```jsx
    console.log([...MAPOBJ]); // [['a', '1'] , ['b', '2'] , ['c', '3']]
    ```
    
    2)배열의 디스트럭처링 할 수 있음.
    
    ```jsx
    const [k, v] = MAPOBJ;
    // console.log(k, v); // ['a', '1']  ['b', '2']
    ```
    

## < Set : 배열과 유사함>

### ✅ 배열과 차이

1.중복값 사용 안됨. 2.인덱스로 접근할 수 없음. 3.때문에 요소 순서에 의미가 없음 4. Map객체 순회시 순서를 보장함(객체처럼 자동 정렬 되지 않고 추가된 순서를 따름)

- Set객체는 객체이지 배열은 아님. 따라서 배열처럼 사용하고 싶으면 배열로 변환 필요
- Set 사용 : 배열의 중복된 요소를 제거할 때 유용함.

### ✅ Set 객체의 생성

- 이터러블(배열,객체 등 순회가능한 것)을 인수로 전달받아 set객체를 생성함

```jsx
new Set(); // {} <- Set생성자 함수로 인한 인스턴스 객체
new Set([1, 2, 3, 4, 5]); // {1, 2, 3, 4, 5}
```

### ✅ Set의 프로퍼티,메소드

### 1. 중복된 값은 Set객체에 요소로 저장되지 않음.

```jsx
new Set([1, 1, 2, 2, 3, 4, 5]); // {1, 2, 3, 4, 5}
new Set("hello"); // {"h","e","l","o"}
```

### 2.요소의 개수 확인 - Set.prototype.size

### 3.요소 추가 - Set.prototype.add();

: 배열처럼 “모든 값을 ” 요소에 넣을 수 있음.

: 반환값 - 새로운 요소가 추가된 Set객체를 반환함.

```jsx
const setObj = new Set();
setObj.add(1).add(true).add(['a', 'b', 'c']); 
// setObj = {1, true, ["a","b","c"]}
```

### 4.요소 검색 - Set.prototype.has();

: 반환값 - set객체에 해당 요소가 존재하면 true, 아니면 false를 반환

```jsx
console.log(setObj.has(1)); // true
```

### 5.요소 삭제 - Set.prototype.delete();

: Set객체는 인덱스를 갖지 않음. 따라서 삭제하려는 “요소값”을 인수로 전달해야함.

: 반환값 - 삭제 성공 여부를 나타내는 불리언 값 반환됨.

```jsx
setObj.delete(true); // true
```

### 6.요소 일괄 삭제 - Set.prototype.clear();

### 7.요소 순회

**1&#41;forEach** : Set객체는 순서에 의미가 없어, forEach 콜백함수에 인덱스 인자를 갖지 않음.


**2&#41;for...of**

### 8.Set객체는 이터러블임.

1	&#41; 따라서 **스프레드 문법** 쓸 수 있음.

```jsx
const setObject1 = new Set([1, 2, 3, 4, 5]); // setObject1 = {1, 2, 3, 4, 5}
console.log([...setObject1]); // [1, 2, 3, 4, 5]
```

2	&#41; 배열의 **디스트럭처링** 할 수 있음.

```jsx
const [a, b, ...rest] = setObject1;
console.log(a, b, rest); // 1 , 2 ,[3, 4, 5]
```

<Br/>

### ✅ new Set()으로 인한 이터러블 “객체”를 → 배열로 바꾸는 법

- Set객체는 객체이지 배열은 아님. 따라서 배열처럼 사용하고 싶으면 배열로 변환 필요

 1. Array.from()

 2. 전개 연산자 사용함.