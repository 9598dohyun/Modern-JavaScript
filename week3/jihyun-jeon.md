# 13장. 스코프

1. 모든 식별자(변수,함수,클래스 이름 등)은 스코프가 있다.
2. 스코프란?<br/>
: **변수와 함수 등의 유효 범위**를 뜻함.(정확히 말하자면 변수와 함수의 식별자(이름)의 유효범위)<br/>
: 어떤 변수나 함수가 참조 될 수 있는 유효 범위를 말하는 것.<br/>(누군가 스코프 내의 변수,함수 등의 값을 참조하고 있다면 스코프는 사라지지 않기 때문에 ←클로저)<br/>
3. 스코프 내에서 변수나 함수의 식별자(이름) 유일해야 함.<br/>
그러나 다른 스코프에서는 같은 식별자(이름)을 사용할 수 있다.<br/>
(그래도 같은 이름으로 변수를 만드는건 scope namespace가 오염되기 때문에 좋지 않음!)
 

### ✅ 스코프의 종류

1. 코드 가장 바깥 영역 : 전역 스코프 , 전역 변수
2. { } (block)내부 영역 : 지역 스코프 , 지역 변수
    
    ![https://velog.velcdn.com/images/jhplus13/post/b40f73be-c7e9-46f1-9454-15fabbe92eb3/image.png](https://velog.velcdn.com/images/jhplus13/post/b40f73be-c7e9-46f1-9454-15fabbe92eb3/image.png)
    

### ✅ 스코프 체인

- 변수를 참조할 때 스코프 체인을 통해 "변수를 참조하는 코드의 스코프"에서 시작해서 → "상위 스코프로" 이동하며 변수를 검색함.<br/>(예) 위사진 - 15, 16번째 코드 실행 예
- 상위 스코프에서 선언한 변수를 하위 스코프에서 참조할 수 있다.<br/>하지반 하위 스코프에서 선언한 변수를 상위 스코프에서 참조할 수는 없음<br/>(예) 위사진 - 24번째 코드 실행 예
- 함수도 스코프를 갖는다. 따라서 함수도 스코프 체인 가능함. (자세한 내용은 렉시컬 스코프 참조)
- 스코프와 스코프 체인은 “런타임 이전”에 결정된다.
    
    → 실행 컨텍스트의 평가 과정(런타임 이전)에 스코프에 식별자를 등록하고, 스코프 체인이 다 결정됨.
    (deep dive 361p 참조)
    

※ 스코프 체인은 “실행 컨텍스트의 렉시컬 환경”을 단방향으로 연결한 것이다.<br/>
※ 전역 렉시컬 환경 : “코드가 로드”되면 곧바로 생성<br/>
※ 햠수 렉시컬 환경 : “함수가 호출”되면 생성된다. (deep dive 195p) <br/>
    
<br/>

### ✅ 함수 레벨 스코프와 블록 레벨 스코프

- var는 함수 레벨 스코프, let const는 블록 레벨 스코프임<br/>
: var로 선언된 변수는 "함수 코드 블록"만 지역스코프로 인정하지만,let와 const는 "블록 레벨 스코프"를 지원함.<br/>
 

### ✅ 렉시컬 스코프

- 렉시컬 스코프란?<br/>
: 함수를 어디서 **호출**했는지에 따라 상위 스코프를 결정하는게 아니라,함수를 어디서 **"정의"** 했는지에 따라 함수의 상위 스코프를 결정한다.

 
- 이유<br/>
: 스코프는 <b>"런타임 이전"</b>에 결정되기 때문에, 함수 실행 이전에 이미 상위 스코프가 다 결정되 있음.<br/>
: 때문에 함수가 어디서 호출됐는지와 상관없이, 함수 자신이 이미 기억하고 있는 상위 스코프를 사용하게 되는 것임.<br/>
: (함수는 런타임 이전인 준비단계 때 메모리에 "함수 객체"가 등록되고, "스코프"에 다 준비되어 있음.)

<br/>
<br/>

 ---

 # 15장. let, const키워드와 블록 레벨 스코프
 ## ✅ let, var, const 차이점

var - 함수 레벨 스코프 , 중복 선언ㅇ, 재할당ㅇ

let - 블록 레벨 스코프 , 중복 선언X , 재할당ㅇ

const- 블록 레벨 스코프, 중복 선언X , 재할당x 

<br/>
< const >

- 새로운 값을 재할당 하는 것은 불가능, <br/>
그러나 객체나 배열일 경우 참조값은 변경되지 않으므로 객체나 배열의 값을 변경하는 것은 가능하다. <br/>
(그러나 객체,배열은 재할당 하는 경우가 드물다)
- 변경이 발생하지 않고, 읽기 전용으로 사용하는 경우 const 사용
- 상수 이름은 “스네이크 케이스”로. ex) `TAX_RATE`

<br/>

### ✅ 변수 호이스팅

호이스팅이란 ?
: 변수나 함수의 선언문이 스코프의 선두로 끌어 올려진 것 처럼 보이는 현상.

### 🔆var, let, const 키워드를 사용해 선언하는 변수는 모두 호이스팅이 된다.

- var는 호이스팅이 가능하지만, let,const는 호이스팅이 발생하지 않는 것처럼 동작한다.<br/> 하지만 모두 변수 호이스팅이 되는 것임!<br/>(사실 변수 선언 뿐만 아니라 함수,클래스 등 "모든 선언문"은 런타임 이전에서 먼저 실행되어 호이스팅 됨)

- 이유<br/>
: 변수 선언은 런타임 이전에 준비됨. <br/>런타임 이전에 변수가 들어갈 공간이 미리 준비가 되기 때문에 호이스팅 발생하는 것 처럼 보인다.<br/>
그러나 let,const는 tdz 때문에 참조에러가 발생하는 것뿐!

### 🔆var의 호이스팅

: var키워드로 선언한 변수는 런타임 이전에 암묵적으로 "변수 선언"과 "초기화"가 한번에 진행된다.<br/>때문에 런타임시 변수 선언문 이전에 변수를 참조할 수 있는 것임

```
console.log(score); // 2.런타임시 undefined 출력가능(호이스팅 가능)
var socore; // 1.런타임 이전에 "변수 선언"과 "초기화"가 이뤄짐(var는 선언과 동시에 undifined로 초기화됨.)
socore = 80;    // 3.런타임시 - "값 80이 할당"됨.
console.log(score);  // 4.80출력
```

### 🔆let의 호이스팅

- let키워드로 선언한 변수는 "변수 선언"과 "초기화" 단계가 분리되어 진행된다.<br/>
변수 선언은 런타임 이전에, 초기화 단계는 런타임 이후에 실행된다.<br/>
<br/>때문에 초기화 이전에 변수를 참조하려고 하면 참조 에러가 발생한다.<br/>결국 let으로 선언한 변수는 변수 호이스팅이 발생하지 않는 것 처럼 보이는 것이다.

- TDZ(Temporal Dead Zone): “스코프 시작 지점부터 ~ 초기화 시작” 지점까지는 변수를 참조할 수 없는 구간

```
console.log(foo);
// 2.런타임시 이전에 변수가 들어갈 공간이 준비되어 있기 때문에 호이스팅 발생함.
// 그러나 tdz 때문에 참조에러 발생하는 것임

let foo; // 1.런타임 이전에 변수 선언만 함
console.log(foo); // 3. undefined - 런타임시 변수 선언문에서 초기화가 실행됨.

foo = 1; // 4. 값 할당
console.log(foo); // 1 5.값이 참조됨.
```

### 🔆const의 호이스팅

- let과 비슷
- 변수 선언과 동시에 초기화를 같이 해야 함. (let과 마찬가지로 변수 선언은 런타임 이전에 되고, 초기화는 런타임때 됨.)

```
console.log(foo); // ReferrenceError // 2.호이스팅 됨. 하지만 참조에러 발생

const foo = 1; // 1.런타임 이전에 변수 선언만 됨 // 3.초기화 됨

console.log(foo); // 4.값 참조 가능
```

> < hoisting 요약정리 ><br/>1. var, let, const는 모두 호이스팅이 가능하다.<br/>2. 그러나 let, const는 호이스팅이 안되는 것 처럼 보인다.<br/>(사실 호이스팅이 가능하다. 때문에 "참조"에러가 나는 것임)<br/>3. 그 이유는 let, const의 변수 선언은 런타임 이전에 되지만, 초기화는 런타임시 되는데, <br/>TDZ라는 "스코프 시작~초기화" 단계 동안은 변수를 참조할 수 없는 구간이 있기 때문이다.
>
<br/>
<br/>

---

# 14장. 전역 변수의 문제점

<질문 전 내용 정리><br/>

0. 모든 선언문은 런타임 이전에 실행된다 (43p)
1. 함수 스코프 : 런타임 이전에 결정된다. (-> 렉시컬 스코프 가능)
2. 함수 선언 : 런타임 이전에 메모리에 함수객체가 등록된다. (-> 호이스팅 가능)  
3. 변수 선언 <br/> 
: 선언문은 런타임 이전에 된다. 그치만 이는 전역 변수에 한정된 말임.<br/>
: 지역 변수는 "함수를 실행"하면 코드를 순차적으로 실행하기 이전에 "선언이 된다."<br/>
(지역 함수 스코프 내에서 호이스팅이 되는 것이다. ←호이스팅은 스코프 단위로 동작함.)<br/>

4. 스코프 생성 시기
    
    ※ 참조1. 스코프 체인은 “실행 컨텍스트의 렉시컬 환경”을 단방향으로 연결한 것이다.
    
    ※ 참조2. 전역 렉시컬 환경 : “코드가 로드”되면 곧바로 생성
    
    ※ 참조3. 햠수 렉시컬 환경 : “함수가 호출”되면 생성된다. <- ??<br/>
    (deep dive 195p)



<질문 정리 - 지역 함수의 선언시점에 대해 >  
- 지역 함수의 함수 선언 시점도 함수 호출시 되는 것인지?
- 그렇다면 지역함수의 스코프도 함수가 호출되면 생성되는 것인지?
- 함수 렉시컬 환경은 함수가 호출되면 생성된다함, 렉시컬 환경을 연결한게 스코프 체인이고, 그럼 스코프가 이미 결정되있다는 렉시컬 스코프는 무엇인지?
 