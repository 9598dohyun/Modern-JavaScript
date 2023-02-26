# 24장: 클로저

## 24.1 렉시컬 스코프

렉시컬 스코프: 함수를 어디에 "정의"했는지에 따라 상위 스코프를 결정

```javascript
const x = 1;

function foo() {
    const x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo(); // 1
bar(); // 1
```

 

## 24.3 클로저와 렉시컬 환경

클로저

- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다

- 이때 외부 함수의 실행 컨택스트는 실행 컨택스트의 스택에서 제거되지만, 외부 함수의 렉시컬 환경까지 소멸하는 것은 아니다

- 중첩 함수라고 모두 클로저는 아니다 -> 클로저의 조건
  
  - 상위 스코프의 식별자를 참조해야
    
    - 브라우저는 참조하고 있는 식별자만 기억한다
    
    - 클로저가 참조하고 있는 상위 스코프의 변수는 '자유 변수'라고 부름
  
  - 외부 함수보다 충첩 함수가 더 오래 유지되어야



## 24.5 캡슐화와 정보 은닉

클로저는 접근 제한자가 없는 자바스크립트에서 private 하게 만드는 법

```javascript
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

생성자 함수가 여러개 인스턴스를 생성하면 \_age 가 변화됨

```javascript
onst me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

그러나 정보 은닉을 완벽하게 지원하는 것은 아니며, 향후 private 필드가 추가될 예정




