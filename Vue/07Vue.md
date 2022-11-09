# 1109 Vue  

### INDEX  
> - UX & UI  
> - Vue Router  
> - Navidation Guard  
> - Articles app with Vue  

---  

### UX & UI  

#### UX(User Experience)  
: 유저의 경험  
유저와 가장 가까이에 있는 분야, 데이터를 기반으로 유저를 조사하고 분석해서 개발자, 디자이너가 이해할 수 있게 소통  
- 좋은 UX를 설계하기 위해서  
&Rightarrow; 사람들의 마음 생각을 이해, 정리해서 제품에 녹여내는 과정이 필요  
  유저 리서치, 데이터 설계 및 정제, 유저 시나리오, 프로토타입 설계 등이 필요하다.  
  
#### UI(User Interface)  
: 유저에게 보여지는 화면을 디자인  
UX를 고려한 디자인을 반영, 이 과정에서 기능 개선 혹은 추가가 필요한 경우 Front-end 개발자와 가장 많이 소통  

- [참고] Inter face  
: 서로의 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고 받는 경우의 접점   
    - 즉, 사용자가 기기를 쉽게 동작 시키는데 도움을 주는 시스템  
    ex) CLI(command-line interface), GUI(Graphic User Interface)  
      
- 좋은 UI를 설계하기 위해서?  
&Rightarrow; 협업  
  
- HCI(Human Computer Interaction)  
: 인간과 컴퓨터 사이의 상호작용에 대한 학문  
  
---  
### Prototyping  

- Software prototyping  
: application의 prototype을 만드는 것  
  
---  
### Vue Router  

#### Routing  
: 네트워크에서 경로를 선택하는 프로세스  
웹 서비스에서의 라우팅  
유저가 방문한 URL에 대해 적절한 결과를 응답하는 것  

#### Routing in SSR  
: Server가 모든 라우팅을 통제  
URL로 요청이 들어오면 응답으로 완성된 HTML 제공  
Django로 보낸 요청의 응답 HTML은 완성본인 상태였다.  
&Rightarrow; Routing(URL)에 대한 결정권을 서버가 가진다.  

#### Routing in SPA / CSR  
: 서버는 하나의 HTML(index.html) 만을 제공  
이후에 모든 동작은 하나의 HTML 문서 위에서 JavaScript 코드를 활용  
  DOM을 그리는데 필요한 추가적인 데이터가 있다면 axios와 같은 AJAX 요청을 보낼 수 있는 도구를 사용하여 데이터를 가져오고 처리  
&Rightarrow; 하나의 URL만 가질 수 있다.  

---  
### Vue Router  
: Vue의 공식 라우터  
SPA 상에서 라우팅을 쉽게 개발할 수 있는 기능을 제공  
라우트(routes)에 컴포넌트를 매핑한 후, 어떤 URL에서 렌더링 할지 알려줌  
SPA의 단점 중 하나인 "URL 변경되지 않는다" 해결  

- MPA(Multiple Page Application)  
: 여러 개의 페이지로 구성된 애플리케이션  
  SSR 방식으로 렌더링  
  
--- 
### Vue Router 시작하기  
`$ vue create vue-router-app`  
`$ cd vue-router-app`  
`$ vue add router`  

#### History mode  
: 브라우저의 History API를 활용한 방식  
  - 새로고침 없이 URL 이동 기록을 남길 수 있다.  
- 우리에게 익숙한 URL 구조로 사용 가능  

- App.vue  
: router-link 요소 및 router-view가 추가된다.  
  ![img_19.png](img_19.png)  
    
- router/index.js 생성  
- views 폴더 생성  

---  
- `router-link`  
: a 태그와 비슷한 기능 &rightarrow; URL을 이동시킨다.  
  - routes에 등록된 컴포넌트와 매핑된다.  
  - 히스토리 모드에서 router-link는 클릭 이벤트를 차단하여 a 태그와 달리 브라우저가 페이지를 다시 로드 하지 않도록 한다.  
    
- 목표 경로는 'to'속성으로 지정  
- 기능에 맞게 HTML에서 a 태그로 rendering 되지만, 필요에 따라 다른 태그로 바꿀 수 있다.  


- `router-view`  
  - 주어진 URL에 대해 일치하는 컴포넌트를 렌더링 하는 컴포넌트  
  - 실제 component가 DOM에 부착되어 보이는 자리를 의미  
  - router-link를 클릭하면 routes에 매핑된 컴포넌트를 렌더링  
    
  - Django에서 block tag와 비슷하다.  
    - App.vue는 base.html의 역할  
    - router-view는 block 태그로 감싼 부분  
    
- src/router/index.js  
  - 라우터에 관련된 정보 및 설정이 작성 되는 곳  
  - Django에서의 urls.py에 해당  
  - routes에 URL와 컴포넌트를 매핑  
    ![img_20.png](img_20.png)  
    ![img_21.png](img_21.png)  
    
- src/Views  
  - router-view에 들어갈 component 작성  
  - 기존에 컴포넌트를 작성하던 곳은 components 폴더 뿐이었지만 이제 두 폴더로 나뉘어진다.  
  - 각 폴더 안의 .vue 파일들이 기능적으로 다른 것은 아니다.  
    
  - views/  
    - routes에 매핑되는 컴포넌트, 즉 <router-view>의 위치에 렌더링 되는 컴포넌트를 모아두는 폴더  
    - 다른 컴포넌트와 구분하기 위해 View로 끝나도록 만드는 것을 권장  
    - ex) App 컴포넌트 내부의 AboutView & HomeView 컴포넌트  
    
  - components/  
    - routes에 매핑된 컴포넌트의 하위 컴포넌트를 모아두는 폴터  
    - ex) HomeView 컴포넌트 내부의 HelloWorld 컴포넌트  
    
---  
- 주소를 이동하는 2가지 방법  
> - 선언적 방식 네비게이션  
> - 프로그래밍 방식 네비게이션  

- 선언적 방식 네비게이션  
: router-link 의 `to`속성으로 주소 전달  
  - routes에 등록된 주소와 매핑된 컴포넌트로 이동  
    
  - Named Routes  
    : 이름을 가지는 routes  
    ![img_22.png](img_22.png)  
    
  - 동적인 값을 사용하기 떄문에 v-bind를 사용해야 정상적으로 작성  
    ![img_24.png](img_24.png)  
    
- 프로그래밍 방식 네비게이션  
  - Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 접근할 수 있다.  
  - 다른 URL로 이동하려면 `this.$router.push`를 사용  
    - history stack에 이동할 URL을 넣는(push) 방식  
    - history stack에 기록이 남기 때문에 사용자가 브라우저의 뒤로 가기 버튼을 클릭하면 이전 URL로 이동할 수 있다.  
    
  - 결국 <router-link :to="...">과 $router.push(...)을 호출하는 것은 같다.  
    ![img_23.png](img_23.png)  
    
- Dynamic Route Matching  
: 동적 인자 전달  
  - URL의 특정 값을 변수처럼 사용할 수 있다.  
    
![img_25.png](img_25.png)  
- `$route.params`로 변수에 접근 가능  
![img_26.png](img_26.png)  
  
data에 넣어서 사용하는 것을 권장  
![img_27.png](img_27.png)  

- Dynamic Route Matching - 선언적 방식 네비게이션 
  - App.vue에서 harry에게 인사하는 페이지로 이동해보기  
  - params를 이용하여 동적 인자 전달 가능  
  ![img_28.png](img_28.png)    
    
- Dynamic Route Matching - 프로그래밍 방식 네비게이션  
  ![img_29.png](img_29.png)  
  
- route에 컴포넌트를 등록하는 또다른 방법  
![img_30.png](img_30.png)  
  
- lazy-loading  
: 모든 파일을 한 번에 로드하려고 하면 모든 걸 다 읽는 시간이 매우 오래 걸린다.  
  미리 로드하지 않고 특정 라우트에 방문할 때, 매핑된 컴포넌트의 코드를 로드하는 방식을 활용할 수 있다.  
  - 모든 파일을 한 번에 로드하지 않아도 되기 때문에 최초에 로드하는 시간이 빨라진다.  
  - 당장 사용하지 않을 컴포넌트는 먼저 로드하지 않는것이 핵심  
    


  