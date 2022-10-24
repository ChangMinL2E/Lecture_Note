# 1024 JavaScript 심화  

---  
### INDEX  
> - DOM  
> - Event  
> - this  


---  
### DOM  
브라우저에서의 JS는 웹페이지에서 복잡한 기능을 구현하는 스크립트 언어  

#### Browser APIs  
: 웹 브라우저에 내장된 API, 현재 컴퓨터 환경에 관한 데이터를 제공하거나 여러가지 유용하고 복잡한 일을 수행  
&rightarrow; DOM이 종류중 하나이다.  

- DOM(Document Object Model)  
: 문서 객체 모델  
  문서의 구조화된 표현을 제공, 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공  
  HTML/CSS 조작 가능  
  문서가 구조화되어 있고, 각 요소는 객체(object)로 취급  
  프로그래밍 언어적 특성을 활용한 조작 가능  
  
  - DOM은 문서를 논리 트리로 표현  
    ![img_39.png](img_39.png)  
    DOM 메서드를 사용하면 프로그래밍적으로 트리에 접근할 수 있고 이를 통해 문서의 구조, 스타일, 컨텐츠를 변경할 수 있다.  
      
  - 웹 페이지는 일종의 문서(document)  
  - DOM은 동일한 문서를 표현하고, 저장하고, 조작하는 방법을 제공  
  - DOM은 웹 페이지의 객체 지향 표현, JS와 같은 스크립트 언어를 이용해 DOM 수정 가능  
      

- DOM의 주요 객체  
  - window  
  - document
  - navigator, location, history, screen 등  
    

`window` object  
: DOM을 표현하는 창  
가장 최상위 객체  
탭 기능이 있는 브라우저에서는 각각의 탭을 각각의 window 객체로 나타낸다.  







