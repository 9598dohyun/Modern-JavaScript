# 45장: 프로미스

- 전통적인 콜백 패턴이 가진 단점 보완

- 비동기 처리 시점 명확하게 표현



## 콜백 헬

- 비동기 함수는 비동기 처리를 외부에 반환할 수 없다

- 비동기 처리 결과에 대한 후속 처리를 전달하기 위해 콜백 함수를 전달 

- 콜백 함수를 반복해서 부르면, 복잡도가 높아지는 현상 '콜백 헬' 발생

- 문제점: 콜백 함수 내에서 발생한 에러는 밖에서 캐치 안됨

 

## 프로미스

- 비동기 처리 성공: resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
  -> 비동기 처리 결과값 1

- 비동기 처리 실패: reject 함수를 호출해 프로미스를 rejected 상태로 변경
  -> 비동기 처리 결과값 Error 객체

- c.f. settled 상태: fulfilled, rejected / pending 상태: 처리 중인 상태 



## 프로미스의 후속 처리 메서드

- then(callback1, callback2)
  
  - callback1: fulfilled 상태일 때 호출, 프로미스의 비동기 처리 결과 인수로 전달
  
  - callback2: rejected 상태일 때 호출, 프로미스의 에러를 인수로 전달
  
  - return: 프로미스 반환(콜백 함수가 프로미스가 아닌 걸 반환하면, 그 값을 암묵적으로 resolve/reject 해 프로미스를 생성해 반환)

- catch(callback) -> 내부적으로는 then(undefined, onRejected) 호출
  
  - callback: 프로미스가 rejected 상태인 경우에만 호출
  
  - return: 프로미스
  
  - then 호출 이후에 호출하면, then 메서드 내부에서 발생한 에러까지 모두 캐치 
  
  - 가독성을 위해 에러처리는 catch에서
  
  - ``` javascript
    promiseGet('https://naver.com')
    .then(res => console.xxx(res))
    .catch(err => console.error(err));
    ```

- finally(callback)
  
  - callback: 실패, 성공 상관 없이 무조건 한 번 호출됨
  
  - return: 프로미스 



## 프로미스의 정적 메서드

- Promise.resolve / Promise.rejcet: 이미 존재하는 값을 래핑해 프로미스 생성

- Promise.all
  
  - 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용 
  
  - 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음 / 인수가 프로미스가 아니더라도 Promise.resolve로 프로미스로 래핑
  
  - 전달받은 프로미스가 모두 fulfilled 상태가되면, 모든 처리 결과를 배열에 저장해 새로운 프로미스 반환 
  
  - 인수로 전달받은 프로미스 중 하나라도 rejected 상태되면, 나머지 상태 기다리지 않고 즉시 종료 

- Promise.race
  
  - 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음
  
  - 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스 반환
  
  - 인수로 전달받은 프로미스 중 하나라도 rejected 상태되면, 나머지 상태 기다리지 않고 즉시 종료, 에러를 reject하는 프로미스 반환

- Promise.allSettled
  
  - 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음
  
  - 전달받은 프로미스가 모두 settled(비동기 처리 수행)상태가 되면, 처리 결과를 배열로 반환 
    
    - fulfilled 인 경우: 비동기 처리 상태를 나타내는 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티 가짐
    
    - rejected인 경우: status 프로퍼티와 에러를 나타내는 reason 프로퍼티 가짐



## 마이크로태스크 큐

- 프로미스 후속처리 콜백함수는 마이크로태스크 큐에 저장

- 우선순위: 마이크로태스크 큐 > 태스크 큐



## fetch

- HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체 반환


