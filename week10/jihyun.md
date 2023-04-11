# 45장. 프로미스 
## 콜백 함수를 통한 비동기 처리

- 단점 : 콜백 헬 발생, 가독성 나쁨, 비동기 처리 중 발생한 에러 처리 곤란, 여러개 비동기 처리 한번에 처리하기 어려움
- js는 싱글스레드 방식이기 때문에 동기 코드가 다 실행된 후 콜스택에 아무것도 없게되면 태스크 큐에 저장되있던 비동기 코드가 콜스택으로 푸시되어 실행된다.<br/>
따라서 함수 내부의 비동기 코드에서 처리 결과를 외부로 반환하거나, 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
    
    ```jsx
    let num = 0;
    setTimeout(() => {num += 1;}, 1000);
    console.log(num); // 1이 아닌, 0이다
    ```
    
- 따라서 비동기 코드의 처리 결과에 대한 후속 처리는 비동기 함수 “내부”에서 수행해야 한다.
    
    ```jsx
    let num = 0;
    
    setTimeout(() => {
      num += 1;
      console.log(num); // 1
    }, 1000);
    ```
    
- 그러나 이러한 방식은 콜백 함수 호출이 중첩되어 콜백헬 발생한다.
→ 가독성 나쁨, 실수 유발, 에러 처리 곤란
    
    ```jsx
    get('stpe/1',(a)=>{
    	get('stpe/2/${a}',(b)=>{
    		get('stpe/1/${b}',(c)=>{
         console.log(c);
    		})
    	})
    })
    ```
    
- 콜백 패턴을 이용한 비동기 처리의 단점 : 에러 처리 곤란
    
    ```jsx
    // setTimeout 함수가 실행 되어 try~catch문 다 읽힌 후, 콜 스택이 비게되면
    // setTimeout함수 내부의 콜백함수가 태스크 큐에서 콜 스택으로 푸시되어 실행된다.
    // 따라서 콜백함수가 실행될 때는 하위 실행 컨텍스트가 setTimeout함수가 아니기 때문에 에러가 전파되지 않는다.
    // ※ 에러는 콜스택 아래 방향으로 전파된다.
    try {
      setTimeout(() => { 
        throw new Error("err");
      }, 1000);
    } catch {
      console.error("에러 캐치"); // 에러 캐치 못한다.
    }
    ```
    
<br/>

## 프로미스 생성

- 프로미스 생성자 함수

```jsx
new Promise(()=>{}) // 프로미스 생성자 함수는 비동기 처리를 수행할 "콜백함수"를 인수로 받음.

// 그 콜백함수의 첫번재 인자는 resolve함수, 두번째 인자는 reject함수
// 비동기 처리 성공시 resolve 함수 호출, 실패시 reject 함수 호출 한다.

new Promise((resolve,reject)=>{
	if(/*비동기 처리 성공*/){resolve('result');}
	else{reject('fail');}
}) 
```

- 프로미스 : 비동기 “처리 상태”와 “처리결과”를 관리하는 객체
  1. 상태 
      - pending(비동기 처리 수행되지 않은 상태)
      - settled(비동기 처리 수행 완료) - fulfilled(비동기 처리 성공), rejected(실패)<br/>
  2. 결과 - 콜백 함수 내부에서 비동기 처리 수행 결과
      - 성공시
      : resolve함수 호출해 → 프로미스를 fulfilled 상태로 변경
      : 비동기 처리 결과 값을 갖는다.
      - 실패시
          
          : rejected 함수 호출해 → 프로미스를 rejected 상태로 변경
          
          : Error객체를 값으로 갖는다.
          
          ```jsx
          const fulfilled = new Promise(resolve => resolve(1));
          const rejected = new Promise(_, rejected => rejected(new Error("err!")));
          ```
            
<br/>  

## 프로미스 후속처리 메서드- then(), catch(), finally()

: 콜백 패턴은 에러 처리가 곤란하지만, 프로미스는 후속처리 메서드를 통해 에러 처리를 문제없이 처리 할 수 있다.<br/>
: 콜백 패턴은 비동기 로직이 중첩되어 콜백헬이 발생하지만, 프로미스는 후속처리 메서드를 통해 콜백헬을 해결ㅇ

- then(), catch(), finally() 셋다 언제나 프로미스를 반환, 비동기로 작동됨.<br/>
- 프로미스 체이닝 : then, catch, finally는 프로미스를 반환하기 때문에 연속적으로 호출할 수 있다.<br/>

<b>1. then</b> <br/>
: 프로미스가 fulfilled상태 일때 - 첫번째 콜백함수 호출, 비동기 처리 결과를 인수로 전달받음<br/>
: 프로미스가 rejected상태 일때 - 두번째 콜백함수 호출, 에러를 인수로 전달받음

```jsx
get("https://url").then(
	res => console.log(res), 
	err => console.log(err)
); 
```

<b>2. catch</b><br/>
: then 메서드의 두번째 콜백함수로 에러 처리하기 보단, catch를 활용하는게 가독성이 더 좋다.
    

```jsx
get("https://wrong-url").catch(
	e => console.log(e)
);
```

<b>3. finally</b><br/>
: fulfilled든, rejected든 상관없이 무조건 한 번 호출된다.
    

```jsx
get("https://url")
.then(res => console.log(res))
.finally(()=> console.log("finally!"));
```

## 프로미스의 정적 메서드

1. Promise.resolve() , Promise.reject()<br/>
: 인수로 전달받은 값을 resolve, rejected 하는 프로미스를 생성한다.

```jsx
new Promise((resolve) => resolve("a"));
Promise.resolve("a"); // 처음부터 바로 프로미스 객체가 fulfilled인 상태인 객체를 만듦

new Promise((_,reject) => reject(new Error("err!)));
Promise.reject(new Error("err!")); // 처음부터 바로 프로미스 객체가 rejected인 상태인 객체를 만듦
```

1. Promise.all()<br/>
: 여러개 비동기 처리를 모두 병렬 처리할 때 사용.<br/>
: 여러 비동기 처리는 서로 의존하지 않고, 개별적으로 수행된다.<br/>
: 모든 비동기 처리 결과가 fulfilled 되면 모든 비동기 처리 결과를 배열에 저장하여 새로운 프로미스 반환.<br/>
: 하나라도 rejected가 되면 나머지 프로미스가 fulfilled되는걸 기다리지 않고 즉시 종료한다.
    <br/>
    ```jsx
    // 1번 직원 정보
    const p1 = fetch('https://learn.codeit.kr/api/members/1')
    .then((res) => res.json());
    
    // 2번 직원 정보
    const p2 = fetch('https://learn.codeit.kr/api/members/2')
    .then((res) => res.json());
    
    // 3번 직원 정보
    const p3 = fetch('https://learnnnnn.codeit.kr/api/members/3')
    .then((res) => res.json());
    
    // 전달받은 인수가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스로 래핑됨.
    const p4 = 4; // Promise.resolve(4);
    
    Promise.all([p1, p2, p3, p4])   
      .then((results) => {
        console.log(results);   
      })
      .catch((error) => {  //하나의 Promise 객체라도 rejected 상태가 되면, 전체 작업이 실패한 것으로 간주
        console.log(error);
      });
    // 결과: [{1번직원 정보},{2번직원 정보},{3번직원 정보}]
    ```
    
<br/>
2. Promise.allSettled()<br/>
:  프로미스 모두가 fulfilled든, rejected든 settled 상태가 되면 처리 결과를 배열로 반환<br/>
: 모든 프로미스들의 처리 상태와 결과가 담겨있다.
    
  ```jsx
  const p1 = fetch('https://learn.codeit.kr/api/members/1').then((res) => res.json());
  const p2 = fetch('https://learn.codeit.kr/api/members/2').then((res) => res.json());
  const p3 = fetch('https://learndd.codeit.kr/api/members/3').then((res) => res.json());
  
  Promise
    .allSettled([p1, p2, p3])
    .then((results) => {
      console.log(results); 
    })
    .catch((error) => {
      console.log(error);
    });
  
  // 결과  (3) [{…}, {…}, {…}]
  // 0: {status: 'fulfilled', value: {…}},
  // 1: {status: 'fulfilled', value: {…}},
  // 2: {status: 'rejected', reason: 'TypeError: Failed to fetch at http://localhost:3000/%EC%97%B0%EC%8A%B5.js:28:12'}
  ```
    

1. Promise.race()<br/>
:  프로미스가 fulfilled든, rejected든 가장 먼저 settled 상태가 되면 resolve나 reject하는 새로운 프로미스 반환
    
    ```jsx
    const p1 = new Promise((resolve, reject) => {
        setTimeout(() => resolve('Success'), 6000);
      });
      const p2 = new Promise((resolve, reject) => {
        setTimeout(() => reject(new Error('fail')), 2000); //제일먼저 pending상태가 끝남.
      });
      const p3 = new Promise((resolve, reject) => {
        setTimeout(() => reject(new Error('fail2')), 4000);
      });
      
      Promise
        .race([p1, p2, p3])
        .then((result) => {
          console.log(result); 
        })
        .catch((value) => {
          console.log(value);
        });
        //결과 Error: fail
    ```
    

## 마이크로 태스트 큐

태스크 큐 <br/>
- 마이크로태스크 큐 : 프로미스 후속처리 메서드의 콜백 함수가 일시 저장
- 매크로태스크 큐 : 그 외의 비동기 함수의 콜백함수, 이벤트 핸들러 함수 저장
- 마이크로태스크 큐가 매크로태스크 큐보다 우선순위가 높음<br/>
즉, 태스크 큐 → 마이크로태스크 큐 → 매크로태스크 큐 순으로 콜스택 작동

## fetach

http응답을 나타내는 응답 객체를 래핑한 Promise 객체를 반환.