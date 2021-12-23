# Create Component



## Component



- EX

  - ```react
    reactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );
    
    ```

    - `<App />` 가 우리가 앞으로 만들 컴포넌트 이다.

- 컴포넌트는 무엇일까??

  - 컴포넌트는 HTML 을 반환하는 함수이다.



### JSX

- react에서 사용하는 개념

  - 리액트에서만 사용된다.

- HTML 안의 Javascript

- EX

  - ```react
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );
    
    // 예시
    ReactDOM.render(<App />, document.getElementById("root"))
    ```

    - 밑의 예시를 보고 JSX 라고 하는 것 같다.



### Component 생성하기

1. `Potato.js`

   - ```react
     // 이게 있어야 JSX 를 사용할 수 있다.
     import React from 'react';
     
     function Potato() {
       return (
         <h3>I love Potato</h3>
       )
     }
     
     // 내보내주자!
     // function Potato 를 여기에서 사용한다.
     export default Potato;
     ```

2. 주의 사항
   `index.js`

   - ```react
     ReactDOM.render(<App />, <Potato />, document.getElementById("root"))
     ```

     - react의 application 에는 한번에 하나의 컴포넌트만 들어갈 수 있다.
       그 컴포넌트안에 수많은 컴포넌트가 들어간다.

3. `App.js`

   - ```react
     import React from 'react';
     import Potato from './Potato';
     
     function App() {
       return (
         <div>
           <h1>
             Hello!!
           </h1>
           <Potato />
         </div>
       );
     }
     
     export default App;
     ```

     - `<Potato />` 를 통해서 저 안에 html 을 넣어준다.



test