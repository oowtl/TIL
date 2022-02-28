# react-dom

- 컴퓨터가 이해하는 것은 결국 html, css, javascript 이다.
  그 말은 우리가 작성한 component들을 html 과 연결해줘야할 필요가 있다는 것이다.
  그걸 연결해주는 것이 `react-dom`이다.

```html
<!-- public / index.html-->
<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
</body>
```

- `pulic / index.html` 은 사용자가 보는 곳이다.
  그곳의 body 태그에 있는 것은 `<div id="root"></div>` 이다.
  우리가 react 를 통해서 작업하는 것은 root 라는 id 를 가진 div 태그에 들어간다는 것이다.

```javascript
// src / index.js

import ReactDOM from 'react-dom';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

- root 라는 아이디를 가진 div 태그를 들고와서 App 컴포넌트를 연결시켜준다.

- `  <React.StrictMode>`
  - 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구
  - 개발모드에서만 활성화되어서 프로덕션 빌드에는 영향을 미치지 않는다.
  - 기능
    - 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
    - 레거시 문자열 ref 사용에 대한 경고
    - 권장되지 않는 findDOMNode 사용에 대한 경고
    - 예상치 못한 부작용 검사
    - 레거시 context API 검사
  - [공식문서](https://ko.reactjs.org/docs/strict-mode.html)