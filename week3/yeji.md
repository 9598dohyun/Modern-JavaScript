# 13장: 스코프

스코프: 식별자가 유효한 범위
-> 스코프가 다르면 동일한 이름의 변수를 사용할 수 있다.

- var: 같은 스코프 내 중복 선언 허용 
- let: 같은 스코프 내 중복 선언 X

c.f. 렉시컬 환경: 코드가 어디서 실행되며 어떤 코드가 있는지. 코드의 문맥. 



## 스코프의 종류

- 전역: 코드의 가장 바깥 영역
- 지역: 함수 몸체 내부



## 스코프 체인 

스코프 체인: 스코프가 계층적으로 연결된 것(상속과 유사)

- 변수를 참조할 때 자바스크립트 엔진은 변수 참조 코드 스코프에서 상위 스코프 방향으로 선언된 변수를 검색 
- 상위 스코프에서 선언한 변수를 하위 스코프에서도 참조할 수 있다
- 하위 스코프에서 유효한 변수는 상위 스코프에서 참조할 수 없다
- 물리적인 실체 '렉시컬 환경'으로 존재



## 함수 레벨 스코프

- 블록 레벨 스코프: 코드 블록(if, for, while...)이 만드는 지역 스코프
  -> let, const는 블록 레벨 스코프를 지원
- 함수 레벨 스코프: var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정하는 특성
  -> for 문에서 var를 쓰면, 스코프가 for로 한정되지 않고 함수 단위로 결정된다



## 렉시컬 스코프

- 동적 스코프: 함수를 어디서 **호출** 했는지에 따라 상위 스코프를 결정
- 정적 스코프 / 렉시컬 스코프: 함수를 어디서 **정의**했는지에 따라 상위 스코프를 결정
  -> 자바스크립트를 비롯한 대부분의 언어는 렉시컬 스코프를 따른다.



# 14장: 전역 변수의 문제점

## 변수의 생명 주기 

### 지역 변수의 생명주기

메모리 공간이 확보(allocate)된 시점부터 메모리 공간이 해제(release)되어 가용 메모리 풀에 반환되는 시점까지.

- 지역변수: 함수가 호출되면 생성되고, 함수가 종료하면 소멸한다. = 함수와 일치

  - 자신이 등록된 스코프가 소멸(매모리 해제)될 때 까지 유효

  - 누군가가 스코프를 참조하고 있으면 소멸하지 않고 생존

- 전역변수: 런타임 이전에 엔진에 의해 먼저 실행 

호이스팅은 스코프를 단위로 동작

- 지역변수 호이스팅: 지역 변수의 선언이 지역 스코프의 선두로 끌어 올려진 것처럼 동작

### 전역 변수의 생명주기 

var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치

- 전역 객체: 브라우저에서는 window, 서버 사이드(Node.js)에서는 global 객체 
  - 브라우저에서 var 전역 객체, 전역 객체 window가 웹페이지 닫기 전까지 유효하므로 변수도 그러함



## 전역 변수의 문제점

- 암묵적 결합: 모든 코드가 전역 변수를 참조하고 변경할 수 있음
- 긴 생명 주기: 리소스 오랜 시간 소비
- 스코프 체인 상에서 종점에 존재: 전역 변수 검색 속도 가장 느림
- 네임스페이스 오염: 파일 분리되도 스코프가 같음



## 전역 변수의 사용을 억제하는 방법

- 즉시 실행 함수 
- 네임스페이스 객체 
  - ... 왜 억제하는 방법일까? 단지 객체가 되었을 뿐인데?
  - "식별자 충돌을 방지하는 효과는 있으나, 네임스페이스 객체 자체가 전역변수라 그다지 유용하지는 않음"
- 모듈 패턴: 클래스를 모방해 관련 변수와 함수를 모아 실행 함수로 감싸 하나의 모듈을 만든다
  - 캡슐화: 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 조작하는 메서드를 하나로 묶음 for 정보 은닉
  - 자바스크립트는 public, private, protected 등 접근 제한자를 사용하지 않으므로, 대신 모듈 패턴을 사용할 수 있다

- ES6 모듈: 파일 자체의 독자적인 모듈 스코프를 제공 (확장자 mjs 권장)

  - 트랜스파일링이나 번들링이 필요하므로, Webpack 등의 모듈 번들러를 사용하는 게 일반적

  ```html
  <script type="module" src="lib.mjs"></script>
  ```



# 15장: let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

- 변수의 중복 선언
- 함수 레벨 스코프 -> 함수 외부에서 선언하면 전역변수 되벌임,,
- 변수 호이스팅: 변수 호이스팅으로 선언문 이전에 참조 가능 
  - let과 const는 안그런단 말인가? -> 호이스팅이 발생하지만 발생하지 않는 것처럼 동작할 뿐



## let 키워드

- 변수 중복 선언 금지: 중복 선언하면 Syntax Error

- 블록 레벨 스코프: 함수 포함 모든 코드 블록(if, for, while...)을 지역 스코프로 인정

- **변수 호이스팅**: 선언 단계와 초기화 단계가 분리되어 진행 

  - 런타임 이전 엔진에 의해 암묵적으로 선언 단계 실행
  - 런타임에서 변수 선언문에 도달했을 때 초기화
    -> 초기화 단계 전 변수 접근하면 ReferenceError
  - 일시적 사각지대: 스코프 시작~초기화단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없는 구간 

  ```javascript
  let foo = 1;
  {
  	console.log(foo); // ReferenceError 지역변수 foo가 호이스팅돼 선언은 돼서 에러남. 호이스팅이 되지 않았더라면 전역변수 값인 1이 찍혔을 것이다.
  	let foo = 2;
  }
  ```

- let으로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님 ex) `window.x` 로 호출 불가



## const 키워드

- 선언과 동시에 초기화 해야 
- 블록 레벨 스코프, 호이스팅 발생하지 않는 것처럼 동작
- 재할당 금지
- 상수: 대문자와 언더스코어(\_)로 네이밍 ex)TAX\_RATE
- const 키워드로 선언된 객체는 재할당은 불가능하지만, 프로퍼티 변경은 가능하다



## 어떤 키워드를 사용할까

const > let(재할당이 필요한 경우) > var(사용하지 않는다)

