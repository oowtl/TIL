# State Work



## state 변경하기



- state는 바로 바꾸면 안된다.

  - ```react
    add = () => {
        console.log("add");
        this.state.count = 1;
      };
      minus = () => {
        console.log("minus");
        this.state.count = -1;
      };
    ```

    ```javascript
    src/App.js
      Line 14:5:  Do not mutate state directly. Use setState()  react/no-direct-mutation-state
      Line 18:5:  Do not mutate state directly. Use setState()  react/no-direct-mutation-state
    ```

    - 안바뀐다!!! state 값이 바뀌지 않는다.
    - 왜냐하면 값을 바꾸고 render method를 바꾸지 않았기 때문이다.

  - state 의 상태를 변경할 때 react 가 render function 을 호출해서 바꾸도록 해야한다.

    - `setState`라는 것을 통해서 이 과정을 한번에 다 할 수 있다.

- ```react
  state = {
    count: 0
  };
  add = () => {
    this.setState({ count: 1 });
  };
  minus = () => {
    this.setState({ count: -1 });
  };
  ```

  - `setState`를 호출하면 react 는 state를 refresh 하고 render function 을 호출할 것이다.
    - 새로운 state와 함께 render function 을 호출할 것이다.
  - html 의 변화가 있는 부분만 수정해서 표현해줄 것이다.

- ```react
  add = () => {
    this.setState({ count: this.state.count + 1 });
  };
  minus = () => {
    this.setState({ count: this.state.count - 1 });
  };
  ```

  - 이렇게 하는 것으로 값을 다 넣어줄 수 있다.
  - 단, 앞으로 state로 접근하는 것이 아닌 다른 방법으로 접근할 것이다.
    state 에 의존하면 몇가지 성능적인 문제가 있을 수 있다.

- ```react
  add = () => {
    this.setState(current => ({ count: current.count + 1 }));
  };
  minus = () => {
    this.setState(current => ({ count: current.count - 1 }));
  };
  ```

  - ` this.setState(current => ({ count: current.count + 1 }));`

    react 가 `this.state`로 접근해도 값을 주긴 하지만 그렇게 좋은 것은 아니다.
    `setState`에서 저런 형식으로 접근하면 `current`에 현재의 state가 들어가게 된다.
    저렇게 사용하자!



- vue 는 자동으로 따닥 해줬던 것 같은데 react 는 바꾸라고 말해줘야 하는 것 같다.