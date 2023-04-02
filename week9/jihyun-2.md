# 41장. 타이머
js 엔진은 “싱글 스레드”로 동작한다. <br/>
때문에 setTimeout, setInterval은 “비동기 처리” 방식으로 동작한다.

## setTimeout / clearTimeout , setInterval / clearInterval

- setTimeout, setInterval 함수는 타이머를 식별할 수 있는 고유한 타이머id를 반환함
- clearTimeout, clearInterval은 그 고유한 타이머id를 이용해 타이머를 취소할 수 있다.

```jsx
const timerId = setTimeout(()=>console.log('Hi'),1000);
clearImteout(timerId);
```

## 디바운스

- 마지막에 한번만 이벤트 핸들러가 호출되어, 이벤트 핸들러가 과도하게 호출되어 성능 악화 방지
- 사용 : resize, input, scroll, mousemove 이벤트 사용시 활용됨.

```jsx
// 처음 input 이벤트 발생시 등록된 setTimeout만 작동되고, 그 이후 input시는 이벤트가 다 clear된다.
// 때문에 1000ms 이후에 처음 등록된 이벤트핸들러만 딱 한번 실행되는 것이다.

const inputEl = document.querySelector(".input");

function debounce(eventHandler, time) {
  let timerId;
  return (e) => {
    if (timerId) {
      clearTimeout(timerId);
    }
    timerId = setTimeout(eventHandler, time, e);
  };
}

inputEl.addEventListener(
  "input",
  debounce((e) => {
    console.log(e.target.value);
  }, 1000)
);
```

## 스로틀

- 일정 시간 간격으로 이벤트 핸들러가 한번만  호출되어, 이벤트 핸들러가 과도하게 호출되어 성능 악화 방지.
- 사용 : scroll 이벤트 처리, 무한 스크롤

```jsx
// 처음 scroll시만 이벤트 핸들러 등록되고, 1초 이내 다시 scroll 이벤트 실행시는 이벤트 핸들러가 그냥 return됨.
const throttle = (callback) => {
  let timer;

  return (event) => {
		if(timer){
			return
		}
    timer = setTimeout(() => {
      timer = null; // 1초 뒤에 null로 되서 다시 이벤트 핸들러 등록됨.
      callback(event);
    }, 1000);
  };
};

window.addEventListener(
  "scroll",
  throttle((e) => console.log("scroll!"))
);
```

# 42장. 비동기 프로그래밍
### js엔진은 싱글 스레드

: js엔진은 한 번에 하나의 태스크만 실행할 수 있다. js엔진은 단 하나의 실행 컨텍스트 스택을 갖는다 <br/>
: 동시에 2개 이상의 함수를 실행할 수 없다는 의미

### 동기, 비동기

동기  <br/>
- api 요청을 보내고, 응답이 와야만 다음 동작을 수행할 수 있다.
- 장) 실행 순서가 보장됨 / 단) 이후 태스크들이 블로킹 된다.

비동기 

- api 요청을 보내고, 응답상태와 상관없이 다음 동작을 수행할 수 있다.
- 장) 이후 태스크가 블로킹 되지 않는다. / 단) 실행 순서가 보장되지 않는다.
- setTimeout, HTTP요청, 이벤트 핸들러는 비동기 처리 방식으로 동작됨.

## 이벤트 루프와 태스크 큐

자바스크립트 엔진 만으로는 싱글스레드 방식이기 때문에 비동기 처리가 불가능하나, <br/>
브라우저에서 실행되는 “이벤트 루프”와 “태스크 큐” 를 통해 비동기 처리가 가능하게 됨.

→ 이렇게 js엔진과 브라우저가 협력하여 비동기 처리를 하는 것이다. <br/>

※ 자바스크립트 엔진 : 실행컨텍스트 스택(콜 스택)  <br/>
※ 브라우저 : 이벤트 루프 , 태스크 큐( 매크로 태스크 큐, 마이크로 태스크 큐 ), Timer function(타이머 설정, 타이머 끝나면 콜백함수 등록), HTTP request, DOM API
 <br/> <br/>

**<비동기 처리가 일어나는 과정>**

1. (자바스크립트 엔진에 있는) <b>"콜 스택”</b>에 태스크가 <b>후입 선출 순</b>으로 실행됨.
2. "비동기 함수의 콜백함수"는 바로 스택에 등록되는게 아니라, <b>“태스크 큐”</b>에 일시적으로 저장되게 됨.
    1. 마이크로 태스크 큐 : 프로미스의 후속 처리 메서드(<u>then메서드)에 쓰인 “콜백함수”</u>가 일시적으로 저장됨
    2. 매크로 태스크 큐 : <u>그 외 setTimeout의 “콜백함수</u>”나 <u>이벤트 핸들러</u>가 일시적으로 저장됨
    
3. 이벤트 루프: 콜 스택에 아무것도 없고, 큐에 대기중인 함수가 있는지를 반복해서 확인함

4. 이후 그런 상태가 되면 > 이벤트 루프는 태스크 큐에 “젤 앞에 있는” 함수를 <u>콜 스택으로 이동</u>시키고 > 그제서야 <u>그 콜백함수가 실행되는데</u>  <br/>
⇒ 이런식으로 태스크 큐에 보관된 함수가 "비동기 처리 방식" 으로 동작되는 것임.

# 43장. Ajax
읽기만

# 44장. REST API

### REST API란 <br/>
: HTTP프로토콜을 의도에 맞게 활용할 수 있는 아키텍처

###  구성
- 자원(RESOURCE) : URI (엔드포인트)
- 행위(Verb)  : 자원에 대한 행위(CRUD)
- 표현(Representations) : 자원에 대한 행위의 구체적 내용 (페이로드: 요청 몸체에 담아 서버로 전송할 데이터)

<설계 원칙>

1. 엔드포인트 이름은 동사보다 명사로<br/>
`/getTodos/1 (x)`,  `/todos/1 (o)`


※ 면접 예상 질문
1. REST API가 뭔가요?
2. REST API의 구성은 어떤 것이 있나요?
3. REST API를 설계하는데 중요한 것이 있을까요?    

※ 참고<br/>
 https://dev.to/anwar_nairi/design-an-easy-to-use-and-flexible-rest-endpoints-3fia
 https://meetup.nhncloud.com/posts/92
