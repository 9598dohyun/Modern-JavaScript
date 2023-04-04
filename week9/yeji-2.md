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



## 프로미스의 에러 처리




