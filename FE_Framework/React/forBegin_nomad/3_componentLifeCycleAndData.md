# component Life Cycle And Data



```react
class App extends React.Component{
  state = {
    isLoading: true,
    movies: []
  }

  componentDidMount() {
    setTimeout(() => {
      this.setState({ isLoading: false});
    }, 6000);
  }

  render() {
    const { isLoading } = this.state;
    return (
      <div>
        {isLoading ? "Loading..." : "We are ready"}
      </div>
    )
  }
}
```

- `{isLoading ? "Loading..." : "We are ready"}`
  - js의 삼항 연산자 이다.

- `componentDidMount(){}`
  - render 함수가 호출되고 가장 빨리 호출되는 함수



- 없어도 미리 적어놓는 것!

  ```react
  state = {
    isLoading: true,
    movies: []
  }
  
  componentDidMount() {
    setTimeout(() => {
      this.setState({ isLoading: false, book: true});
    }, 6000);
  }
  ```

  - `this.setState({ isLoading: false, book: true});`
    - book 은 없는데 넣어도 에러가 안난다.
    - 미래에 사용할 값들에 대해서 미리 넣어놓을 수 있으면 넣어두는 것이 좋다.