# 40장. 이벤트

이벤트 발생을 브라우저가 감지하고 있음.

이벤트가 발생했을 때 이벤트 핸들러 호출을 브라우저에게 위임하여 작동되는 것임 (이벤트 핸들러 등록)

---
## 이벤트 타입

- dbclick : 마우스 더블 클릭시
- mouseenter : 마우스 커서를 html 요소 안으로 이동했을 때 (버블링x)
- mouseleave : 마우스 커서를 html 요소 밖으로 이동했을 때 (버블링x)<br/>
(※ mouseover, mouseout : 버블링 됨)

- blur : 포커스를 읽었을 때 (focus의 반대)
- change : 사용자의 입력이 종료되어 값이 변경되면 이벤트 발생

- input : 사용자가 입력하고 있을 때 이벤트 발생

- DOMContentLoaded : HTML 문서 로드와 파싱이 완료되어 DOM 생성이 완료 됬을 때

- load : DOMContentLoaded 이벤트 발생한 후, 모든 리소스(이미지,폰드 등) 로딩이 완료욌을 때 

- unload : 리소스가 언로드 될 때

--- 

## 이벤트 핸들러 등록 방식 3가지

<방법1 - 이벤트 핸들러 어트리뷰트 방식>

- 방법. 이벤트 핸들러 어브리뷰트 값으로 : 문(함수 호출 문)을 할당한다.<br/>
- 함수 호출문을 할당하면 함수가 호출되서 함수의 반환값이 이벤트 핸들러로 등록될 것 같지만<br/>
그러나 사실 암묵적으로 onClick과 같은 이름의 이벤트 핸들러 함수가 생성되고 그 함수가 할당되는 것이다.

```html
<body>
    <div>
      <button onclick="sayHi('kim')">click me</button>
<!-- <button onclick="function onclick(e){sayHi('kim')}">click me</button> // 사실 onClick 함수가 등록되는 것! -->
    </div>
</body>

<script>
function sayHi (name){
    console.log(`Hi! ${name}`)
  }
</script>

```

<방법2 - 이벤트 핸들러 프로퍼티 방식>

- 방법. 이벤트 타깃 요소에 이벤트 핸들러 함수를 바인딩 한다.
- 단점 : 하나의 이벤트에 하나의 이벤트 핸들러만 바인딩할 수 있다.

```jsx
<body>
    <div>
      <button onclick="sayHi('kim')">click me</button>
    </div>

  <script>
    const buttonEl = document.querySelector("button")

		// 여긴 무시됨.
    buttonEl.onclick = function (e){
      console.log("click111",e.target.innerHTML)  
    }

    buttonEl.onclick = function (e){
      console.log("click222",e.target.innerHTML)
    }
  </script>`
  </body>
```

<방법3 - addEventListener 메서드>

- `addEventListener(type, listener, useCapture);`
- 3번째 인자 - false(기본값, 버블링에서 이벤트 캐치) , true(캡처링 단계에서 이벤트 캐치함)
- 하나의 이벤트에 하나 이상의 이벤트 핸들러를 바인딩할 수 있다.

```jsx
<body>
    <div>
      <button  >click me</button>
    </div>

  <script>
    const buttonEl = document.querySelector("button")
    buttonEl.addEventListener("click",()=>console.log("111")) // 둥록ㅇ
    buttonEl.addEventListener("click",()=>console.log("222"))
  </script>
</body>
```

---
## 이벤트 핸들러 제거

`removeEventListener(type, listener, useCapture);`

- addEventListener 메서드에 전달한 인수와 removeEventListener 의 인수가 같아야 함.
- removeEventListener 예제 - 여러번 클릭해도 단 한번만 이벤트 핸들러 호출하게 됨.
    
    ```jsx
    <body>
        <div>
          <button>click me</button>
        </div>
    
      <script>
        const buttonEl = document.querySelector("button")
    
        buttonEl.addEventListener("click",function foo(){
          console.log("111")
    
          buttonEl.removeEventListener("click",foo)
         // buttonEl.removeEventListener("click",foo,true) // 인수가 다르면 이벤트 핸들러 제거되지 않음
        }
        )
    
      </script>
      </body>
    ```

---    
## 이벤트 객체

- 이벤트 객체를 이벤트 핸들러의 첫 번째 인수로 전달된다.
- 이벤트 핸들러 “어트리뷰트 방식”으로 이벤트 핸들러 등록시 : 매개변수 이름이 반드시 “event” 이여만 한다.
    
    ```html
    <body>
        <div>
          <button onclick="Hello(event)">click me</button> // 반드시 event 라는 이름으로!
    
      <-- <button onclick="function onclick(event){Hello(event)}">click me</button> -->
    	// 사실 onclick 함수가 암묵적으로 생성되서 할당되는 것인데, onclick의 매개변수가 event로 지정되있기 때문에 반드시 event 이름을 써야한다. 
        </div>
    
      <script>
       function Hello(e){
        console.log(e)
       }
      </script>
      </body>
    ```
    

- 체크박스의 체크 상태 : e.target.checked // true, false
- 키보드 이벤트에서 : keycode는 폐지됨. → key 프로퍼티 사용하기

---
## 이벤트 전파

- 이벤트 발생시 이벤트 객체가 생성되는데 이 이벤트 객체가 전파되는 순서는 다음과 같다.
    1. 캡처링 : window → 타깃 요소로 전파
    2. 타깃 단계 : 이벤트를 발생시킨 dom요소에 도달
    3. 버블링 : 타깃 요소 → window로 전파

- 이벤트를 바인딩한 요소와 발생시킨 요소가 다를 수 있다.
    
    : target - 이벤트를 발생시킨 Dom요소  
    : currentTarget - 이벤트가 현재 발생하고 있는 Dom요소  
    

- addEventListener의 3번째 인자<br/>
: false(기본값) : 버블링 단계에서 이벤트 캐치함 / true: 캡처링 단계에서 이벤트 캐치함

- 버블링, 캡처링 코드예제
```jsx
<body>
    <p>버블링과 캠처링 이벤트 <button>버튼</button></p>

  <script>
   const bodyEl = document.querySelector("body")
   const pEl = document.querySelector("p")
   const buttonEl = document.querySelector("button")

   bodyEl.addEventListener("click",()=>console.log("body"))
   pEl.addEventListener("click",()=>console.log("p") , true) // 캡처링 단계때 이벤트 캐치됨.
   buttonEl.addEventListener("click",()=>console.log("button"))

  // p 클릭시 : "p"(캡처링, 타깃단계), "body"(버블링)
  // button 클릭시  : "p"(캡처링), "button"(타깃단계), "body"(버블링)
  </script>
  </body>
```

※ 버블링 되지 않는 이벤트

1) focus, blur (버블링ㅇ - focusin, focusout)

2) load, unload, abort, error

3) mouseenter, mouseleave (버블링ㅇ - mouseover, mouseout)

---
## 이벤트 위임

- 하위요소 각각에 동일한 이벤트를 바인딩 하는게 아니라, 상위요소 하나에만 이벤트 바인딩을 하여 이벤트 위임 활용<br/>
- 장점:  성능을 지켜낼 수 있고, 유지보수 하기도 더 좋음

```jsx
<body>
   <ul>
    <li>1</li>
    <li>2</li>
    <li>100</li>
   </ul>

  <script>
   const ulEL = document.querySelector("ul")
   ulEL.addEventListener("click",()=>console.log("이벤트 위임됨"))
  </script>
  </body>
```
---
## 이벤트 기본동작 막기

- `e.preventDefault()` : dom요소의 기본 동작을 중단시킨다.

```jsx
<body>
  <a href="https://www.google.com/" >goto link</a>

  <script>
   const linkEL = document.querySelector("a")
   linkEL.addEventListener("click",(e)=>{
    e.preventDefault(); // a태그의 링크 이동하는 기능을 막게된다.
    console.log("click!")
   })
  </script>
  </body>
```

- `e.stopPropagation()` : 이벤트 버블링을 막는다. 이벤트 전파를 중지시킨다.

```jsx
<body>
   <ul>
    <li>1</li>
    <li id="two">2</li>
   </ul>

  <script>
   const ulEL = document.querySelector("ul")
   const twoEL = document.querySelector("#two")

   ulEL.addEventListener("click",()=>console.log("click ul"))
   twoEL.addEventListener("click",(e)=>{
    e.stopPropagation(); // 버블링이 되지 않아, 자신에게 바인딩된 이벤트 핸들러만 실행됨.
    console.log("click two")
   })
  </script>
  </body>

//  e.stopPropagation()이 없다면 출력 결과 : "click two" "click ul"
```

---
## 이벤트 핸들러 내부의 this

- 이벤트 핸들러 함수 내부의 this는
1) 이벤트를 바인딩한 DOM요소가 된다.
2) 그러나 이벤트 핸들러로 “화살표 함수” 사용시 : “상위 스코프의 this”를 가리키게 된다.

- 이벤트 핸들러 어트리뷰트 방식 에서의 this <br/>
: window를 가리킴<br/>
: 암묵적으로 생성되는 onclick함수 내부의 this는 “이벤트를 바인딩한 DOM요소”로 되고,<br/>
  그 안의 handleClick함수는 일반함수로 호출됐기 때문에 window를 가리키는 것임<br/>

```jsx
<body>
   <button onclick="handleClick()">버튼</button>
  <!-- <button onclick="function onclick(){handleClick()}">버튼</button> -->

<script>
  function handleClick(){
    console.log(this) // window
   }
</script>
</body>
```

- 원하는 객체로 this 바인딩 하는 법 : 1) bind 메서드 활용, 2) 화살표 함수 사용하여 한단계 위의 this로 지정