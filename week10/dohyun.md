## promise.then의 사용법
1. 비동기 작업을 처리하는 위한 기능중 하나
2. promise객체의 상태가 변할 때 실행될 콜백함수를 등록하는 메서드
3. Promise.then은 여러개의 Promise를 연결해서 처리하여 Promise체이닝을 가능하게 해준다.

### Promise.then 메서드 
```jsx
//resolve,reject 는 각각 promise가 이행될때와 거부될대 호출되는 콜백함수
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const randomNumber = Math.floor(Math.random() * 10);
        if (randomNumber % 2 === 0) {
            resolve(randomNumber);
        } else {
            reject(new Error('Odd number'));
        }
    }, 1000);
});
// 짝수일때 첫번째 매개변수 호출  
// 결과값이 result 매개변수로 전달
// 홀수일때 두번째 매개변수 호출 
// error 매개변수로 전달
promise.then(
    (result) => {
        console.log(`Resolved: ${result}`);
    },
    (error) => {
        console.log(`Rejected: ${error}`);
    }
);

```
### 유의할 점
1. 상태변화 이후 한번이상 처리될수 없다.
2. Promise체이닝을 할때 반환값이 반드시 Promise 객체여야하낟.
3. promise.then보다는 async/await 구문을 더 사용한다.

### 비동기코드를 동기적인 코드처럼 작성한 예시
```js
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    const anotherResponse = await fetch('https://api.example.com/another-data');
    const anotherData = await anotherResponse.json();
    console.log(anotherData);
  } catch (error) {
    console.error(error);
  }
}

```
- await 키워드를 사용하여 'fetch'메서드와 'response.json()'메서드를 동기적으로 실행하고 있다.
- await 는 위 두개의 메서드가 promise객체를 반환할때까지 함수의 실행을 일시정지 한다.
- 따라서 위 두 메서드가 완료된 이후에 두번째 (another)메서드를 실행합니다.

### axios와 async/awit 상호보완
```js
async function fetchData() {
  try {
    const response = await axios.get('https://api.example.com/data');
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}
```
- axios.get메서드가 Promise객체를 반환하므로 'await'키워드를 사용하여 response변수가
할당될때까지 함수의 실행 일시중지
- 코드의 간결성을 도와주는것 같다. 