# 24장 클로저
- 클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합이다.
```js
const x = 1;

function outer(){
    const x= 10;
    function inner(){
        console.log(x);
    }
    inner();
}
outer();
```
#### 위 경우 지역 inner함수가 ourter의 변수인 x를 참조할수 있지만 inner함수가 outer 내부에서 정의된 함수가 아니라면 x변수에 접근할 방법이 없다.
#### 이것은 js가 렉시컬스코프를 따르는 언어이기때문이다.

## 24.1 렉시컬 스코프
- 어디서 호출했는지가 아닌 어디(내/외부)에 정의했는지 에 따라 상위크코프를 정의한다.

## 24.2 함수 객체의 내부슬롯 [[Environment]]
- 함수는 자신의 내부슬롯에 자신이 정의된 환경(상위스코프의 참조)를 저장한다!!
  - 함수객체 내부슬롯에 저장된 현재 실행중인 실행컨텍스트의 렉시컬환경의 참조가 상위스코프다!!!
  - 힘수객체는 상위스코프를 자신이 존재하는 한 기억한다.

## 24.3 클로저와 렉시컬환경
```js
const x = 1;

function outer(){
    const x =10;
    const inner = function (){console.log(x);};
    return inner;
}
const innerFun = inner;

innerFun(); // 10
```
- 외부함수(outer)보다 중첩함수가 더 오래유지되는 경우 중첩함수는 생명주기가 종료된 외부함수의 변수를 참조할수 있다.
  - oute의 경우 inner가 반환된느 시점에 생명주기가 종료된다.(실행컨텍스트 스택에서 제거)
  
1. 전역렉시컬환경을 outer 함수 객체의 내부슬롯에 상위스코프를 저장한다.
2. outer함수를 호출하면 outer 함수의 렉시컬환경이 생성되고 내부슬롯에 저장된 전역렉시컬환경을 '외부렉시컬환경에 대한 참조'에 할당
3. inner(중첩함수)가 평가된다. 이때 자신의 내부슬롯에 outer함수의 렉시컬환경을 상위스코프로서 저장된다.

- 이때 outer함수 실행컨텍스트는 제거되지만 렉시컬환경까지는 사라지지 않는다.
-  가비지컬렉터는 누군가 참조하고 있는 메모리는 해제하지 않는다.

### 클로저는 중첩함수가 상위스코프의 식별자를 참고하고 있고 중첩함수가 외부함수보다 오래 유지되는 경우에 한정하는 것을 의미한다.
## 24.4 클로저의 활용
- 클로저는 상태가 의도치않게 변경되지 않도록 은닉하고 특정함수에게만 변경을 허용하도록 하는데 사용한다.
```js
const increase = (function (){
    let num = 0;
    return function (){
        return ++num;
    };
}());
```
- 즉시 실행함수를 활용하여 간단히 클로저 함수를 구현했다.
   - 즉시실행함수는 호출되고 바로 사라지지만 반환된 클로저는 increse변수에 할댕되어 할당된다.
   - 반환된 변수는 상위스코프(increse의 렉시컬 혼경)를 기억하고 있다. 
- 한번만 실행되므로 변수가 호출될때마다 초기화될일이 없다. 

## 24.5 캡슐화와 정보은닉
- 캡슐화: 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할수 있는 메서드를 하나로 묶는것
- 정보은닉: 프로퍼티나 메서드를 감출 목적으로 사용하는 것
### 접근제한자(public,private,protected)를 선언하여 공개범위 한정 가능
- pubic : 클래스외부에서 참조가능
- private: 참조불가능
#### 객체의 모든 프로퍼티와 메서드는 기본적으로 public이다.

24-16과 24-18의 차이를 모르겠음
인스턴스 메서드의 중복생성을 막겠다는 건 이해가 되는데 결과론적으로 별로 달라지는게 없어보임..

## 24.6 실수모음
- var문으로 for문을 선언할 경우 함수레벨스코를 갖기 때문에 전역변수다.(24-20참고)
- const,let을 사용하여 블록레벨스코프를 갖도록 하고 for문의 반복문 코드를 실행할때마다 새로운렉시컬환경을 생성한다.(15-02 209pg 참조)(근데 왜 숫자의 나열이 왜 갯수로 저장이 될까? )
   - 반복문의 코드부ㅡㄹ록내부에서 함수를 정의할때 의미가 있다. 
- 고차함수를 사용하자(이는 27장에서)

- 함수레벨스코프: 함수의 코드블록만을 지역스코프로 인정 (모두 전역변수가 된다.)
- 블록레벨 스코프 : let키워드로 선언한 변수는 모든 코드불록을 지역스코프로 인정된다.