# render

- 리액트는 데이터가 변동될때마다 render 함수를 통해서 모든 컴포넌트를 업데이트한다.
  규모가 큰 작업을 한다면?? -> 불필요한 작업을 계속하는 것이다.
  데이터가 변동될 때마다 변동된 컴포넌트만 다시 render 할 수는 없을까??
  그 방법이 바로 pureComponent 와 memo 이다.



## pureComponent

https://ko.reactjs.org/docs/react-api.html#reactpurecomponent

- Concept

  - React 컴포넌트의 `render()`함수가 동일한 props, state 에 대하여 동일한 결과를 렌더링 한다면 `React.PureComponent` 를 사용해서 성능 향상을 할 수 있다.
    - `<React.StrictMode>` 는 각 컴포넌트가 동일한 props, state 에서 동일한 결과를 내는지 확인하기 위해서 render 시에 한번 더 실행한다. ( `render()` 함수로 1번, `<React.StrictMode>` 로 1번 실행 )

  - `shouldComponentUpdate()`
    - `React.Component`에는 없는 것으로 `React.PureComponent`에 구현되어 있는 것이다.
    - props 와 state 를 shallow (얕은) 비교하는 것으로 변경을 감지한다.
  - 하위 트리에 대한 props 갱신 작업을 수행하지 않는다.
    PureComponent 를 적용한 컴포넌트의 하위 트리도 동시에 적용되는지 검토하고 사용해야한다.

```react
// 예시
// Component
import React, {Component} from 'react';
class addForm extends Component {
}
s
//PureComponent
import React, {PureComponent} from 'react';
class addForm extends PureComponent {
}
```

- addForm 컴포넌트의 부모 컴포넌트에서 props 된 것의 변경이 있거나 addForm 컴포넌트의 state 변경이 있을 시에만 addForm 컴포넌트를 변경한다.

- shallow comparison ( 얕은 비교 )

  - 오브젝트의 참조값? 주소값이 변경되지 않는다면 같다고 보는 것이다.

    - 근데 이게 너무 얕게는 아닌 것 같다. 어느정도의 깊이는 존재한다.
      - 정확하게는 1:1 로 값을 인지하는 것 같다.
        실험해본 결과 오브젝트안에 오브젝트를 넣어서 해봤는데 해당하는 값만 바뀌는 현상을 보았다.

  - ```javascript
    // 변경되지 않음!
    const a = {
      player : ['spring','angular']
    }
    a.player[0] = 'django'
    // 내용이 바뀌어도 주소값은 바뀌지 않았기 때문에 같은 것이라고 생각하는 것!
    ```

    - 내용은 변경안되도 주소값이 같으면 변경하지 않겠다는 것!

  - props 에 함수가 들어가 있으면??

    - 함수 자체의 내용이 바뀌어야 달라진 것으로 인식한다.

- 이슈

  - `<React.StrictMode>`는 render 함수를 한번 더 한다고 배웠고, 각 컴포넌트에 console.log 를 찍었는데, 두번 찍히지 않았다.

    - react 17 버전 이후에는 `<React.StrictMode>`으로 실행할 때 console.log 를 무시한다고 한다.
      이에 관련해서 pull 요청이 있다.

    - 해결방법

      - `let log` 를 사용한다.

      - ```jsx
        let log = console.log;
        
        function App () {
          log("render 1");
          return ( <div>App1</div>)
        };
        
        ReactDOM.render(
        	<React.StrictMode>
            <App />
          </React.StrictMode>,
          document.getElementById("root")
        );
        ```

      - https://dev.to/sebastianstamm/be-careful-with-console-log-when-using-react-strictmode-ah9
      - https://codesandbox.io/s/console-log-in-react-17-strictmode-9r5j5?file=/src/index.js:85-107

  - `<div>` 로 감싸야지 devtool 에서 인식하는 현상발생

    - Fragment 로 감싼 것은 Devtool 에서 PureComponent 로 인식하지 않는 버그가 있다.
      `<React.Fragment></React.Fragment>`, `<></>`를 인식하지 않는다.

      ```react
      // 예시
      
      render() {
        return (
          <React.Fragment>
            <div>
              ...
            </div>
          </React.Fragment>
        )
      }
      
      render() {
        return (
          <>
            <div>
              ...
            </div>
          </>
        )
      }
      ```

- 왜 리액트는 state 를 직접 수정하지 않는 것을 추천할까?
  - 리액트는 shallow comparison 을 사용하고 있기 때문이다.
    직접 수정하는 것은 리액트에서 값이 변경되었다고 인식하지 못하기 때문에 알 수 없다.