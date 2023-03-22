# 39장: DOM

## 노드

- HTML 요소는 렌더링 엔진에 의해 파싱돼 DOM을 구성하는 요소 노드 객체로 변환

- 노드 객체는 트리 자료구조로 구성 = DOM

- 노드 타입(총 12개)
  
  - 문서 노드: 최상위 유일한 document 객체, 노드에 접근하기 위한 진입점 역할 담당
  
  - 요소 노드: HTML 요소를 가리키는 객체, 중첩에 의해 부자 관계, 문서의 구조 표현
  
  - 어트리뷰트 노드: HTML 요소의 어트리뷰트를 가리키는 객체, 요소 노드와 연력되어 있음 
  
  - 텍스트 노드: HTML의 텍스트를 가리키는 객체. 요소 노드의 자식 노드, DOM 트리의 최종단 

- 노드 객체는 브라우저 환경에서 제공하는 호스트 객체 (O), 빌트인 객체(X)
  
  - 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다
  
  - 상속 구조는 개발자 도구 > Elements 패널 우측 > Properties 패널 에서 확인
  
  - 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받음 
  
  - 문서 노드 - Document, HTMLDocument 인터페이스 상속
  
  - 어트리뷰트 노드 - Attr 인터페이스 상속
  
  - 텍스트 노드 - CharacterData 인터페이스 상속
  
  - 요소 노드 - Element 인터페이스 + 태그 종류에 따라 추가 인터페이스 상속 
  
  - ex) input 요소 노드 객체 프로토타입에 따른 특성
    
    - Object 객체
    
    - EventTarget 이벤트 발생시키는 객체
    
    - Node 트리 자료구조의 노드 객체
    
    - Element 브라우저가 렌더링할 수 있는 웹 문서 요소 표현 객체
    
    - ...

- DOM API: 노드 타입에 따라 필요한 기능인 프로퍼티와 메서드의 집합

## 요소 노드 취득

- getElementById
  
  - HTML 요소에 id 어트리뷰트를 부여할 때 부수 효과: id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당. 단, id 값으로 전역 변수가 이미 있으면, 이 변수에 노드 객체가 재할당되지는 X.

- getElementsByTagName -> DOM 컬렉션 객체인 HTMLCollection 객체 반환(유사배열, 이터러블, 변화를 실시간으로 반영하는 '살아있는 객체')
  
  - Document의 메서드 -> DOM 전체에서 요소 노드 탐색
  
  - Element 의 메서드 -> 특정 요소 노드의 자손 노드 중에서 탐색

- getElementsByClassName -> HTMLCollection 반환
  
  - Document 메서드  / Element 메서드(차이는 getElementsByTagName과 동일)

- querySelector: 인수로 전달한 CSS 선택자를 만족시키는 **하나**의 요소 노드 반환

- querySelectorAll: 인수로 전달한 CSS 선택자를 만족시키는 **모든** 요소 노드 반환(NodeList)
  
  - Document 메서드 / Element 메서드
  
  - getElementsBy\*\* 메서드 보다 다소 느림
  
  - 구체적인 조건으로 요소 노드를 취득하는 건 장점 

- matches: CSS 선택자를 통해 특정 노드 취득할 수 있는지 확인(boolean 반환), 이벤트 위임에 유용

### HTMLCollection과 NodeList

- HTMLCollection: 변화를 실시간으로 반영하는 살아있는 객체 -> 부작용 있으므로 가급적 사용X

- NodeList: 실시간으로 노드 객체 상태 변경 반영 X, 단 childNodes 프로퍼티가 반환하는 NodeList 객체는 live 객체 

-> 둘다 배열로 변환해서 사용하는 것을 권장 

### 

## 노드탐색

- Element.prototype, Node.prototype이 제공하는 프로퍼티로 부모/형제 노드 탐색 가능
  
  - 부모노드: Node.prototype.parentNode
  
  - 형제노드: Node/Element.prototype.previousSibling, Node/Element.prototype.nextSibling 

- 스페이스, 탭, 개행 등의 공백 문자는 텍스트 노드를 생성

- 자식 노드 탐색을 위한 프로퍼티 
  
  - Node.prototype.childeNodes: 반환한 NodeList에는 요소 노드 + 텍스트 노드 포함
  
  - Element.prototype.children: 반환한 HTMLCollectioon에는 텍스트 노드 X, 오로지 요소 노드 O

- Node.prototype.hasChildNodes 메서드로 자식 노드 존재 여부 확인 가능(텍스트 노드 포함)

- Node.children.length, Element.childElementCount로 텍스트 노드 아닌 요소 노드 존재 확인 가능  



## 노드 정보 취득

- Node.prototype.nodeType: 노드 객체의 종류, 즉 노드 타입을 나타나는 상수를 반환
  
  - 예) 요소노드 1, 텍스트 노드 3, 문서 노드 9

- Node.prototype.nodeName: 노드 이름을 문자열로 반환
  
  - 요소노드: 대문자 문자열로 태그 이름 반환
  
  - 텍스트 노드: "#text"
  
  - 문서 노드: "#document"



## 요소 노드의 텍스트 조작

- Node.prototype.nodeValue: 텍스트 반환, Node.prototype.firstChild.nodeValue로 요소 노드의 텍스트 변경 가능 

- Node.prototype.textContent
  
  - nodeValue보다 간단한 코드로 텍스트 반환/변경 가능(텍스트 노드 찾아야할 필요 없으니까)
  
  - 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가
  
  - 할당한 문자열에 HTML 마크업이 포함되어 있더라도, 파싱하지 않고 문자열 그대로 인식된다.

- Element.prototype.innerText (권장X)
  
  - CSS에 순종적이어서 비표시 지정된 요소 노드의 텍스트 반환 X
  
  - CSS를 고려해야 하므로 textContent 프로퍼티보다 느림



## DOM 조작

- Element.prototype.innerHTML 
  
  - getter: 요소 노드 콘텐츠 영역 내에 모든 HTML 마크업을 문자열로 반환
  
  - setter: HTML 마크업 문자열을 할당하면 렌더링 엔진에 의해 파싱돼 요소 노드의 자식으로 DOM에 반영 -> 크로스 사이트 스크립팅 공격에 취약(예제 39-44, 39-45)
    
    > DOMPurify.sanitize 메서드로 HTML 마크업의 잠재적 위험을 제거할 수 있다
    
    자식 요소를 모두 지우고 HTML 마크업 문자열을 파싱해 DOM을 변경

- Element.prototype.insertAdjacentHTML(position, DOMString)
  
  - 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입, innerHTML 보다 효율적이고 빠름
  
  - 첫번째 인수: 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
  
  - HTML 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약

- Document.prototype.createElement(tagName)
  
  - 요소 노드를 생성해 반환, 인수는 태그 이름 문자열 예) 'li'
  
  - DOM에 추가하는 처리는 별도로 필요함

- Document.prototype.createTextNode(text)
  
  - 텍스트 노드 반환, 인수는 텍스트 노드의 값인 문자열
  
  - DOM에 추가하는 처리 별도로 필요함

- Node.prototype.appendChild(childNode)
  
  - 인수로 전달한 노드를 마지막 자식 노드로 추가
  
  - 이때 리플로우와 리페인트 실행
  
  - 이미 존재하는 노드를 인수로 받으면, 노드는 이동됨

- Node.prototype.insertBefore(newNode, childNode)
  
  - 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입
  
  - 두 번째 인수 노드는 호출한 자식의 노드여야. 아니라면 DOMException 에러 발생.
  
  - 두 번째 인수가 null이면 마지막 자식 노드로 추가
  
  - 이미 존재하는 노드를 첫 번째 인수로 받으면, 노드는 이동됨

- Document.prototype.createDocumentFragment
  
  - 기존 DOM과 별도로 존재하므로, 여러 노드를 DOM에 한꺼번에 추가할 때 활용할 수 있다.

- Node.prototype.cloneNode([deep: true | false])
  
  - 노드의 사본을 생성하여 반환
    
  
  - 인수가 true면 깊은 복사(모든 자식 노드 포함), false면 얕은 복사(노드 자신만, 자식 노드인 텍스트 노드 당연히 없음)

- Node.prototype.replaceChild(newChild, oldCHild)
  
  - 자식 노드를 다른 노드로 교체
  
  - 두 번째 인수는 자식 노드여야 

- Node.prototype.removeChild(child)
  
  - 인수로 전달한 노드를 DOM에서 삭제
  
  - 인수는 자식 노드 여야
