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

## HTMLCollection과 NodeList

- HTMLCollection: 변화를 실시간으로 반영하는 살아있는 객체 -> 부작용 있으므로 가급적 사용X

- NodeList: 실시간으로 노드 객체 상태 변경 반영 X, 단 childNodes 프로퍼티가 반환하는 NodeList 객체는 live 객체 

-> 둘다 배열로 변환해서 사용하는 것을 권장 

### 


