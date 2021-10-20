# Component Life Cycle



- react Component 가 가지고 있는 기능

- component 가 render 되기 전에 호출 되는 function 이 있고, render 된 후에 호출되는 function이 있다.



## Mounting 

> born



- constructor()

  - javascript에서 class 를 생성할 때 호출되는 것

    - 이건 react 가 아니라 javascript 의 문법이다. 

  - ```react
    class App extends React.Component{
    
      constructor(props) {
        super(props);
        console.log("hello");
      }
    
      render() {
        console.log("render");
        return (
        )
      }
    }
    ```

    - hello, render 순서로 console.log 될 것이다.

  - class 가 만들어질때 바로 나오는 것 같다.

  - component 가 mount 될 때, component 가 screen 에 표시될 때, component 가 website로 갈때 constructor 를 호출한다.

- render()

- componentDidMount()

  - ```javascript
    componentDidMount() {
      console.log("component redered");
    }
    ```

    처음으로 render() 가 되었을 때, 실행되는 것



## Updating

> state 를 변경하거나 할 때 호출되는 것
>
> setState() 를 사용했을 때 호출되는 것이라고 생각하면 된다.

- componentDidUpdate

  - ```react
    componentDidUpdate() {
      console.log("I just updated");
    }
    ```

    마찬가지로 render() 가 되고 나서 호출되는 것



## Unmounting

> 페이지 이동 등의 상황에서 나올 수 있는 것



- componentWillUnmount

  - ```react
    componentWillUnmount() {
      console.log("Good Bye!")
    }
    ```

    지금은 안되는데 페이지를 이동하거나 하면 작동한다.



