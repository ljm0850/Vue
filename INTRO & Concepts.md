# INTRO & Concepts

## INTRO

### Vue

- JavaScript Framework
- SPA 지원

#### SPA

- Single Page Application
- 현재 페이지를 동적으로 렌더링하여 소통하는 웹 어플리케이션
- 단일 페이지로 구성하여 필요한 부분만 동적으로 수정하여 사용

#### CSR

- Client Side Rendering
- 클라이언트에서 화면을 구성(서버에서 화면을 구성하는 SSR)
- 최초 요청시 HTML, CSS, JS 를 제외한 리소스를 응답 받고 클라이언트에서 필요한 데이터만 요청해 JS로 DOM을 렌더링 하는 방식
- SPA가 사용하는 렌더링 방식

##### 장점

- 서버와 클라이언트 간 트래픽 감소
  - 정적 리소스는 한번에 다운받고, 필요할때 데이터만 갱신
- 사용자 경험 향상

##### 단점

- SSR에 비해 전체 페이지 최종 렌더링이 느림
- SEO(검색 엔진 최적화)가 어려움



#### SSR

- Server Side Rendering
- 서버에서 클라이언트에게 보여줄 페이지를 모두 구성하여 전달하는 방식
- 전통적인 렌더링 방식

##### 장점

- 초기 구동 속도가 빠름
- SEO(검색 엔진 최적화)에 적합

##### 단점

- 모든 요청마다 새로운 페이지를 구성하여 전달
  - 반복되는 새로고침
  - 트래픽이 많아 서버의 부담이 클 수 있음

#### SEO

- Search Engine Optimization
- 웹 페이지 검색엔진이 자료를 수집하고 순위를 매기는 방식에 따라 웹 페이지를 구성하여 검색 결과 상위에 노출될 수 있도록 하는 작업



## Concepts

### MVVM Pattern

- Model
  - JavaScript에서 Object 해당
    - Object === { key:value }
  - Vue Instance 내부에서 data 라는 이름
- View
  - DOM(HTML)에 해당
  - Data의 변화에 따라 바뀌는 대상
- View Model
  - 모든 Vue Instance
  - View 와 Model 사이에서 Data와 DOM의 모든 일 처리