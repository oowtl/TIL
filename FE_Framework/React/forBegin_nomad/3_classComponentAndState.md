# class Component And State



- State 는 Dynamic Data 를 위해서 존재하고 기능한다.
  - 생겨나고 변경되는 데이터를 말한다.



## function Component VS class Component

| function Component                               | class Component                                              |
| ------------------------------------------------ | ------------------------------------------------------------ |
| function 이기 때문에 무엇인가를 return 해야한다. | class 인데 react Component로 부터 확장되고 screen에 표시된다.<br />그리고 그것을 render method 안에 넣어서 표현한다. |
|                                                  | react 는 자동적으로 모든 class component 의 render method 를 실행하려고 한다. |
|                                                  | class Component 가 state 라는 것을 가지고 있고 우리는 이것을 사용할 것이다. |



- ```react
  class App extends React.Component{
    state = {
      count: 0
    };
    render() {
      return (
        <div>
          <h1> The number is : {this.state.count}</h1>
        </div>
      )
    }
  }
  ```

  - ```react
    class App extends React.Component
    ```

    - extends 로 받아온다.

  - ```react
     render() { return }
    ```

    - class Component 는 이런 방식으로 한다.





## State

- Object
- 우리는 바꾸고 싶은 데이터를 State 에 넣어놓을 것이다.

- ```react
  class App extends React.Component{
    state = {
      count: 0
    };
    render() {
      return (
        <div>
          <h1> The number is : {this.state.count}</h1>
        </div>
      )
    }
  }
  ```

  - ```react
    state = {
      count: 0
    };
    ```

    - state에 저장할 것들을 넣어놓는다.

- 중요한 것은 data 가 변할 때는 어떻게 하는것일까???





### 함수 만들기

- ```react
  class App extends React.Component{
    state = {
      count: 0
    };
    add = () => {
      console.log("add")
    };
    minus = () => {
      console.log("minus")
    };
  
    render() {
      return (
        <div>
          <h1> The number is : {this.state.count}</h1>
          <button onClick={this.add}>Add</button>
          
          <button onClick={this.minus}>Minus</button>
        </div>
      )
    }
  }
  ```

  - ```react
    add = () => {
      console.log("add")
    };
    minus = () => {
      console.log("minus")
    };
    ```

    - component 안에다가 그냥 추가해주면 된다.

  - ```react
    <button onClick={this.add}>Add</button>
    <button onClick={this.minus}>Minus</button>
    ```

    - react 에서는 onClick 이라는 것이 있기 때문에 별다른 것을 하지 않아도 된다.
    - 그리고 `this.add` 를 하면 된다.
      여기에서 `this`를 사용해야한다는 것을 꼭 유념해두자.

  - 주의사항

    ```react
    <button onClick={this.add()}>Add</button>
    //이거는 아니다. this.add(), 괄호가 붙으면 즉시 실행하는 것이다.
    ```

    - `this.add()`를 하면 안된다.
      왜냐하면 괄호가 붙으면 즉시 실행하는 것을 의미하기 때문이다.
      그렇게 되면 `onClick`이 될 때마다 실행하는 것이 아니게 되므로 사용하면 안된다.
    - 실제로 사용해보니 그냥 실행된다.
      또한 버튼을 눌러도 실행되지 않는다. 아무래도 괄호가 붙으면 한번 실행되고 끝나는 것 같다.

