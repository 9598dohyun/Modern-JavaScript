# 25장 실행 컨텍스트

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- 기존 프로토타입 기반 패턴(프로토 체인과 같은)을 클래스 기반 패턴처럼 사용할 수 있다.

#### 클래스와 생성자 함수는 유사하지만 차이점이 있다.
1. 클래스를 new연산자없이 호출하면 에러가 발생 / 생성자함수는 일반함수로 호출
2. extends와 super로 상속을 자원한다.
3. 클래스는 호이스팅이 발생하지 않는것처럼 보인다.
4. 암묵적으로 strict모드가 지정되어 실행된다.
5. constructor,프로토타입 메서드 , 정적 메서드는 모두 프로퍼티 어트리뷰트[[Emurable]]의 값이 false이다. 열거 x

## 25.2 클래스 정의
### 클래스는 일급객체이다. 따라서 일급객체의 특징을 갖는다.
1. 런타임에 생성가능하다.
2. 변수나 자료구조(배열 등등)에 저장가능
3. 매개변수에 전달 가능
4. 함수의 반환값으로 사용가능

## 25.3 클래스 호이스팅
- 클래스 선언문 이전에 일시적 사각지대에 빠져 호이스팅이 발생하지 않는것처럼 보임

## 25.5 메서드
1. constructor
   - 인스턴스를 생성하고 초기화하기위한 메서드이다.
   - 클래스내에 최대 한개만 존재할수 있다.
   - 생략을 하면 암묵적으로 빈객체가 생성이 된다.
   - 별도의 반환문을 갖지 않아야한다.
   - 객체를 반환한다면 빈객체가 반환이 되어서 절대 사용하면 안된다.

2. 프로토타입 메서드 
   - 클래스가 생성한 인스턴스는 프로토타입체인에 포함된다.(프로토타입프로퍼티를 사용하지 않아도)
3. 정적 메서드

## 25.8 상속에 의한 확장
- 기존클래스를 상속받아 새로운 클래스를 확장한다는 뜻
- extends를 사용하여 기존의 superclass를 포함하여 새로운 객체를 만든다.
- extends는 class에서만 사용핤 있다.
- super를 호출하면 상위클래스의 매서드(constructor)를 호출한다.

### super함수를 호출할때 주의할 사항
1. 서브클래스에서 constructor를 생략하지 않는경우 내부에 super를 호출해야한다.
2. super를 호출하기 전에는 this를 참조할수 없다.
3. super는 서브클래스에서만 호출한다. 다른 데에 호출하면 오류발생한다.(메서드내에서 super를 참조하면 수퍼클래스의 메서드를 호출할수 있다.)
주의! es6의 메서드 축약표현만이 [[homeobject]]를 갖는다.

## 26장 es6함수의 추가기능
### 26.2 메서드
- 메서드의 정의: 메서드 축약표현으로 정의된 함수
- 화살표함수의 정의: 
  1. 화살표함수는 선언문이 아닌 함수표현식으로 정의해야한다.
  2. 매개변수가 없다면 소괄호를 생략할수 있다.
  3. 하나의 문으로 구성된다면 몸체를 감싸는 중괄호를 생각가능하다.
- 화살표함수와 일반함수의 차이
  1. 인스턴스를 생성할수 없는 non-constructor이다.(따라서 프로토타입 프로퍼티,타입도 생성하지 않는다.)
  2. 중복된 매개변수는 사용불가능 하다. 

#### es6에서는 메서드를 정의할 때 es6의 축약표현으로 정의한 메서드가 좋다.

### rest파라미터
- 함수에 전달된 인수들의 목록을 배열로 전달받는다.단 반드시 마지막에 위치해야한다.

## 33장 symbol
- es6의 새로운 원시타입으로 변경불가능한 원시타입의 값을 뜻한다.
   1. 다른값과 절대 중복되지 않는 유일무이한 값이다.
   2. symbol함수와 같이 사용해야하며 new연산자와 같이 호출하지 않아야 한다.
   3. 암묵적으로 string이나 number로 변환되지 않는다,

### 심벌과 프로퍼티키 
- 심벌을 프로퍼티키로 만들면 다른 프로퍼티키와 절대 충돌하지 않는다.
- 또 이러한 방법으로 외부에 노출할 필요가 없는 프로퍼티를 은닉할수 있다.
