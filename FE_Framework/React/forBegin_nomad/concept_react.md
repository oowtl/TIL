# concept_react



## concept

- html 에 밀어넣는다!
- 보여줄 페이지에 대해서 div 안의 내용을 채워서 넣는다는 개념
- react 는 처음부터 html 을 만들지 않고 html 을 채우고 지우는 법을 안다!
  1. 빈 HTML 을 로드한다.
  2. react 가 HTML 을 밀어넣는다. (component 에 작성한 것)
- virtual DOM
  - 가상의 DOM을 만들어내서 거기에서 만들어진 것을 넣어둔다.



### 구성

1. `index.html`

   - ```html
     <div id="root"></div>
     ```

     - `root` 라고 되어있다!

2. `index.js`

   - ```js
     import React from 'react';
     import ReactDOM from 'react-dom';
     import App from './App';
     
     ReactDOM.render(
       <React.StrictMode>
         <App />
       </React.StrictMode>,
       document.getElementById('root')
     );
     ```

     - 여기에서 주목할 것은 `getElementById` 이다. `root`를 가리킨다.

3. `App.js`

   - ```js
     import React from 'react';
     
     function App() {
       return (
         <div className="App">
           Hello!!
         </div>
       );
     }
     
     export default App;
     ```

     - 여기에서 저걸 밀어넣어준다!

 