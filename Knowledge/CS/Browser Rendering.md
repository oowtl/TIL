# Browser Rendering

[naver d2-브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)

[mozilla-crp](https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path)

[DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)

[javascript-info domTree](https://ko.javascript.info/dom-nodes)

[디자이너의 기록 - Rendering](https://designer-ej.tistory.com/entry/Web-Rendering-DOMCSSOMRender-Tree)

[velog/@ouo_yoonk - crp](https://velog.io/@ouo_yoonk/CRP)



## 중요 렌더링 경로 ( CRP )

> CRP, Critical Rendering Path

- 설명
  - 브라우저가 HTML, CSS, Javascript 를 화면에 픽셀로 변화하는 일련의 단계.
  - CRP 를 최적화하는 것은 렌더링 성능을 향상시킨다.
- 순서
  1. 사용자가 서버에 HTML 요청을 한다.
  2. 서버는 응답 헤더 또는 데이터로 HTML 을 반환한다.
  3. 브라우저는 반환받은 HTML 을 분석하고 DOM 트리로 변환한다.
  4. 브라우저는 스타일시트, 스크립트 또는 외부 자원(이미지 참조 등)에 대한 링크를 찾을 때마다 요청한다.
     ( 불러온 에셋을 다룰 때까지 나머지 HTML 을 분석하는 작업은 중단한다.)
  5. DOM 과 CSSOM이 완료되면 브라우저는 렌더 트리를 생성하고 보여지는 컨텐츠를 위해서 스타일을 계산한다.
  6. 렌더트리가 완료된 후 모든 렌더 트리 요소들에 대한 위치와 크기가 정의된 레이아웃이 만들어진다.
  7. 결정된 레이아웃에 따라 화면에 픽셀을 그린다.



### DOM Tree

- 설명

  - HTML/ XML 문서는 브라우저 안에서 DOM 트리로 표현된다.
    - 태그는 요소 노드가 되고 트리 구조를 형성한다.
    - 문자는 텍스트 노드가 된다.

  - DOM Tree 구성은 점진적으로 증가한다.
    - 페이지에 내용을 표시하기 위해서 문서전체를 로드할 필요가 없다.
  - CSS 와 Javascript 로드 시에 페이지의 렌더링을 차단할 수 있다.



#### DOM

> DOM, Document Object Model

![문서 객체 모델 - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/1200px-DOM-model.svg.png)

[^위키피디아]: https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%84%9C_%EA%B0%9D%EC%B2%B4_%EB%AA%A8%EB%8D%B8

- 설명
  - DOM 은 HTML, XML 문서의 interface 이다.
  - DOM 은 문서의 구조화된 표현을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하는 것으로 문서 구조, 스타일 내용 등을 변경할 수 있도록 한다.