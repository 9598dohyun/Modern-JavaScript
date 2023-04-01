# 41장: 타이머

## 타이머 함수

#### setTimeout/clearTimeout

- 단 한번 동작하는 타이머

- 타이머를 식별할 수 있는 고유한 타이머 id 반환

- clearTimeout의 인수로 타이머 id 전달해, 타이머 취소 
  
  
  
  

### setInterval / clearInterval

- 반복 동작하는 타이머 

- 타이머를 식별할 수 있는 고유한 타이머 id 반환

- clearInterval의 인수로 타이머 id 전달해, 타이머 취소



## 디바운스와 스로틀

- 짧은 시간 간격으로 연속적으로 발생하는 이벤트(scroll, resize, input, mousemove 등)는 과도하게 호출돼 성능에 문제 일으킬 수 있음

- 디바운스와 스로틀: 이러한 이벤트를 그룹화 해, 과도한 호출을 방지하는 프로그래밍 기법



```javascript
<!DOCTYPE html>
<html>
<body>
  <button>click me</button>
  <pre>일반 클릭 이벤트 카운터    <span class="normal-msg">0</span></pre>
  <pre>디바운스 클릭 이벤트 카운터 <span class="debounce-msg">0</span></pre>
  <pre>스로틀 클릭 이벤트 카운터   <span class="throttle-msg">0</span></pre>
  <script>
    const $button = document.querySelector('button');
    const $normalMsg = document.querySelector('.normal-msg');
    const $debounceMsg = document.querySelector('.debounce-msg');
    const $throttleMsg = document.querySelector('.throttle-msg');

    const debounce = (callback, delay) => {
      let timerId;
      return event => {
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
    };

    const throttle = (callback, delay) => {
      let timerId;
      return event => {
        if (timerId) return;
        timerId = setTimeout(() => {
          callback(event);
          timerId = null;
        }, delay, event);
      };
    };

    $button.addEventListener('click', () => {
      $normalMsg.textContent = +$normalMsg.textContent + 1;
    });

    $button.addEventListener('click', debounce(() => {
      $debounceMsg.textContent = +$debounceMsg.textContent + 1;
    }, 500));

    $button.addEventListener('click', throttle(() => {
      $throttleMsg.textContent = +$throttleMsg.textContent + 1;
    }, 500));
  </script>
</body>
</html>
```

### 디바운스

- 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가, 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.
- resize 이벤트 처리, input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 ui 구현, 버튼 중복 클릭 방지 처리 등에 유용
- 실무에서는 Underscore 또는 Lodash의 debounce 함수 사용 권장



### 스로틀

- 짧은 시간 간격으로 이벤트가 연속해서 발행하더라도, 일정시간 간격으로 이벤트 핸들러가 최대 1번만 호출하도록 한다. (일정 시간마다 이벤트 핸들러 실행 보장)

- scroll 이벤트 처리나 무한 스크롤 UI 구현에 유용 

- 실무에서는 Underscore 또는 Lodash의 throttle 함수 사용 권장



? 차이가 뭘까 - 실행 보장

? 왜 디바운스 예제에서는 clear 해주지



## 42장: 비동기 프로그래밍

- 자바스크립트 엔진은 싱글 스레드로 동작 -> 동기 처리 방식으로 동작하면 블로킹됨
- 비동기 처리 방식으로 동작 -> 현재 실행 중인 태스크가 종료되지 않아도 다음 태스크를 곧바로 실행하므로 블로킹이 발행하지 않음
- setTimeout, setInterval, HTTP요청, 이벤트 핸들러가 비동기 처리 방식



### 자바스크립트 엔진과 브라우저/Node.js의 역할 분담

- 자바스크립트 엔진 (싱글스레드)
  
  - 콜스택: 실행 컨텍스트 스택(함수의 평가와 실행)
  
  - 힙: 객체가 저장되는 메모리 공간

- 브라우저/Node.js (멀티스레드)
  
  - 태스크 큐: 타이머 함수와 같은 비동기 함수의 콜백 함수, 이벤트 핸들러가 일시적으로 보관되는 영역
  
  - 이벤트 루프: 콜 스택이 비어있고 태스크 큐에 대기중인 함수가 있다면, 이벤트 루프는 순차적(FIFO)으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킨다.



```javascript
function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

setTimeout(foo, 0); // 0초(실제는 4ms) 후에 foo 함수가 호출된다.
bar();
```

1. 전역 코드가 평가되어 전역 실행 컨텍스트가 생성되고 콜 스택에 푸시된다.

2. 전역 코드가 실행되기 시작하여 setTimeout 함수가 호출된다. 이때 setTimeout 함수의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 중인 실행 컨텍스트가 된다. 브라우저의 Web API(호스트 객체)인 타이머 함수도 함수이므로 함수 실행 컨텍스트를 생성한다.

3. setTimeout 함수가 실행되면 콜백 함수를 호출 스케줄링하고 종료되어 콜 스택에서 팝된다. 이때 호출 스케줄링, 즉 타이머 설정과 타이머가 만료되면 콜백 함수를 태스크 큐에 푸시하는 것은 브라우저의 역할이다.

4. 브라우저가 수행하는 4-1과 자바스크립트 엔진이 수행하는 4-2는 병행 처리된다.
   
   4-1. 브라우저는 타이머를 설정하고 타이머의 만료를 기다린다. 이후 타이머가 만료되면 콜백 함수 foo가 태스크 큐에 푸시된다. 위 예제의 경우 지연 시간(delay)이 0이지만 지연 시간이 4ms 이하인 경우 최소 지연 시간 4ms가 지정된다. 따라서 **4ms 후에 콜백 함수 foo가 태스크 큐에 푸시되어 대기하게 된다.** 이 처리 또한 자바스크립트 엔진이 아니라 브라우저가 수행한다. 이처럼 setTimeout 함수로 호출 스케줄링한 콜백 함수는 정확히 지연 시간 후에 호출된다는 보장은 없다. 지연 시간 이후에 콜백 함수가 태스크 큐에 푸시되어 대기하게 되지만 콜 스택이 비어야 호출되므로 약간의 시간차가 발생할 수 있기 때문이다.
   
   4-2. bar 함수가 호출되어 bar 함수의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 중인 실행 컨텍스트가 된다. 이후 bar 함수가 종료되어 콜 스택에서 팝된다. 이때 브라우저가 타이머를 설정한 후 4ms가 경과했다면 **foo 함수는 아직 태스크 큐에서 대기 중이다.**

5. 전역 코드 실행이 종료되고 전역 실행 컨텍스트가 콜 스택에서 팝된다. 이로서 콜 스택에는 아무런 실행 컨텍스트도 존재하지 않게 된다.

6. 이벤트 루프에 의해 콜 스택이 비어 있음이 감지되고 태스크 큐에서 대기 중인 콜백 함수 foo가 이벤트 루프에 의해 콜 스택에 푸시된다. 다시 말해, 콜백 함수 foo의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 중인 실행 컨텍스트가 된다. 이후 foo 함수가 종료되어 콜 스택에서 팝된다.



# 43장: Ajax

Ajax(Asynchronous JavaScript and XML) 

- 자바스크립트를 사용해 브라우저가 서버에게 비동기 방시긍로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식

- Web API인 XMLHttpRequest 객체를 기반으로 동작 

- 변경할 부분 데이터만 받고 렌더링, 서버-클라이언트통신이 비동기라 블로킹 없음 



JSON(JavaScript Object Notation)

- JSON.stringify

- JSON.parse



XMLHttpRequest

- 자바스크립트를 사용해 HTTP 요청을 전송하기 위해 사용하는 객체

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();


// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');


// HTTP 요청 전송
xhr.send();


// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```



# 44장: REST API

구성 요소

- 자원: URI

- 행위: HTTP 요청 메서드

- 표현: 자원에 대한 행위의 구체적인 내용, 페이로드



설계 원칙

- URI 는 리소스를 표현해야 한다 -> 명사

- 리소스에 대한 행위는 HTTP 욫어 메서드로 표현 




