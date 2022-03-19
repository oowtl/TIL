# What is JSX?

- [공식문서 - JSX 소개 ](https://ko.reactjs.org/docs/introducing-jsx.html)
- [공식문서 - JSX 이해하기](https://ko.reactjs.org/docs/jsx-in-depth.html)



## Why JSX

- 초기 React
  ```react
  function App() {
    // 태그, CSS, 내용
    return React.cloneElement('h1', {}, 'Hello:)')
  }
  ```

  - 단점
    - 코드에 JavaScript 가 많아진다.
      - 디자이너들과 협업하는 것에 있어서 무리가 있음
    - HTML 으로 간단하게 할 수 있는 것을 JSX 를 사용하면 코드를 짜야한다.

- 현재 React, JSX
  ```react
  function App() {
    return <h1>Hello!</h1>
  }
  ```

  - 좀 더 직관적으로 바뀌었다.



## 활용하기



### 변수 사용하기

- JSX 안에서는 변수를 사용할 수 있다.

  ```react
  function App() {
    const name = 'cono';
    return (
      <h1>Hello! {name}</h1>
      );
  }
  
  // html
  Hello! cono
  ```

  - `{}` 중괄호를 사용해서 변수를 사용할 수 있다.

### 다수의 노드를 return 하기

- JSX는 다수의 노드를 return 하지 않는다.
  ```react
  function App() {
    return <h1>Hello1</h1>
    <h1>Hello2</h1>;
  }
  
  // Error
  // Adjacent JSX elements must be wrapped in an enclosing tag.
  // Did you want a JSX fragment <>...</>?
  ```

  - fragment 나 <> 빈태그 사용하라고 한다.
  - 다수의 노드를 return 하지 않는 JSX 를 가지고 다수의 노드를 return 시킬 때마다 div 태그를 사용하는 것은 그렇게 좋은 선택이 아닐 수도 있다. ( div 태그가 남발될 수도 있다 )

- `<React.Fragment>`

  ```react
  function App() {
    return (
      <React.Fragment>
        <h1>Hello1</h1>
        <h1>Hello2</h1>
      </React.Fragment>);
  }
  // return 에 괄호는 넣어야한다.
  ```

- `<>` ( 빈 태그 )

  ```react
  function App() {
    return (
      <>
        <h1>Hello1</h1>
        <h1>Hello2</h1>
      <>);
  }
  ```

### 코드 넣어주기

- JSX 에서 코드를 넣어줄 수 있다.
  ```react
  function App() {
    const name = 'cono'
    return (
      <>
        <h1>Hello1</h1>
        {name && <h1>Hello {name} </h1>}
  			{
          ['🙃', '🤗'].map(item => (
            <h1>{item}</h1>
          ))
        }
      </>
  
      );
  }
  ```

  - `{}` 중괄호를 통해서 비즈니스 로직을 짤 수 있다.
  - `{name && <h1>Hello {name} </h1>}`
    - 예시의 코드는 name 이 True 이면 `<h1>Hello {name} </h1>}` 를 실행한다는 코드이다.
  - 배열에 접근하는 변수 또한 `{}`중괄호로 묶어서 나와야 한다.