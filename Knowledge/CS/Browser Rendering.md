# Browser Rendering

[naver d2-브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)

[mozilla-crp](https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path)

[DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)

[javascript-info domTree](https://ko.javascript.info/dom-nodes)

[디자이너의 기록 - Rendering](https://designer-ej.tistory.com/entry/Web-Rendering-DOMCSSOMRender-Tree)

[velog/@ouo_yoonk - crp](https://velog.io/@ouo_yoonk/CRP)

[google developers - criticla rendering path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path?hl=ko)



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

> Dom 은 Tree 형식의 자료구조를 가지고 있다.
>
> Dom Tree = Dom 이라고 봐도 된다.

- 설명

  - HTML 응답은 토큰으로, 토큰은 노드로, 노드는 DOM 트리로 변환된다.
    1개의 DOM 노드는 시작태그 토큰으로 시작해서 끝태그 토큰으로 끝난다.
  - HTML/ XML 문서는 브라우저 안에서 DOM 트리로 표현된다.
    - 태그는 요소 노드가 되고 트리 구조를 형성한다.
    - 문자는 텍스트 노드가 된다.

  - DOM Tree 구성은 점진적으로 증가한다.
    - 페이지에 내용을 표시하기 위해서 문서전체를 로드할 필요가 없다.
  - 많은 수의 노드들은 CRP 에서 DOM Tree로 변환하는 이벤트를 더 오래 발생시킨다.
  - CSS 와 Javascript 로드 시에 페이지의 렌더링을 차단할 수 있다.



#### DOM

> DOM, Document Object Model

![문서 객체 모델 - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/1200px-DOM-model.svg.png)

[^위키피디아]: https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%84%9C_%EA%B0%9D%EC%B2%B4_%EB%AA%A8%EB%8D%B8

- 설명
  - DOM 은 HTML, XML 문서의 interface 이다.
  - DOM 은 문서의 구조화된 표현을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하는 것으로 문서 구조, 스타일 내용 등을 변경할 수 있도록 한다.



### CSSOM

> CSS Object Model
>
> Javascript 에서 CSS 를 조작할 수 있는 API 집합

특징

- DOM 을 스타일링 하기 위한 페이지의 모든 스타일 정보를 포함한다.
- CSS 는 렌더링을 막는다.
  - 브라우저는 모든 CSS 를 처리하고 수신할 때까지 페이지 렌더링을 막는다.
    -> CSS 는 규칙을 덮어쓸 수 있기 때문이다.
  - CSS 가 Cascade Style Sheet  이기 때문에, CSSOM 의 하위노드들은 스타일을 상속한다. 이를 통해서 이전의 규칙들에 덮여쓰여질 수 있기 때문에 CSSOM 을 전부 분석할 때까지 렌더트리 생성을 막는다.
- 선택자 성능 측면에서 덜 구체적일 수록 더 빠르다. `.foo {}` > `.foo .bar {}`
  - 2번 째에서 `.foo` 가 부모객체인 `.bar`를 가지고 있는지 확인하기 위해서 DOM 을 거슬러 올라간다.
    - 하지만 이것에 의한 차이는 작은 편이라서 최적화할 가치가 낮다.
      물론 필요하다면 최적화하는 것이 좋다.
    - [CSS 최적화를 하는 방법](https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS)이 따로 존재한다.
      - CSS non-blocking



### Render Tree

특징

- DOM 과 CSSOM 트리는 렌더 트리에 결합된다.
- 브라우저는 DOM 트리의 root  부터 시작해서 모든 노드를 확인하면서 어떤 CSS 규칙들을 적용할지 결정한다.
- 보여지는 콘텐츠만을 캡쳐한다.
  - `display:none;`이 적용된 요소는 포함되지 않는다.
  - `<head>`, `<script>` 태그도 포함하지 않는다.



### Layout

특징

- 요소들이 페이지에서 배치되는 위치와 방법, 각 요소의 너비와 높이 그리고 서로 관련된 위치를 결정한다.
  - 요소의 너비?
    - 부모 너비의 기본 너비값의 100% 이다.
      `body`태그는 뷰포트 너비의 100% 이다.
- 디바이스의 너비는 레이아웃에 영향을 미친다.
- 뷰포트 메타 태그는 레이아웃에 영향을 미치는 뷰포트 레이아웃의 너비로 정의한다.
  - `<meta name-"ViewPort" content="width=device-width">` 와 같이 정의할 수 있다.
- 디바이스 너비는 사용자가 디바이스를 가로, 세로 모드로 바꿀 때마다 바뀐다.
  - 레이아웃은 디바이스가 회전하거나 브라우저의 사이즈가 조정될 때마다 발생한다.
- 레이아웃의 성능은 DOM의 영향을 받는다.
  - 노드가 많을수록 레이아웃은 더 길어진다.
  - 스크롤링 또는 다른 애니메이션이 있으면 레이아웃에 끊김현상(Jank)이 생길 수 있다.
    - 모니터 주사율을 고려해야하며, 성능을 최적화하는 방법이 있다.
- 레이아웃의 반복과 형성시간을 줄이기 위해서는 일괄 업데이트 해야하고, 박스 모델 속성을 애니메이션화 하지 말아야 한다.