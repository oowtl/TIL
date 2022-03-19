# Make Component



## class Component

```react
// src/component/habit.jsx
// .jsx 파일을 만든 후에 rcc 를 입력했더니 extension의 기능으로 자동으로 만들어졌다.

import React, { Component } from 'react';

// 여기에서 Habit 은 파일명을 따라간다.
// class 명은 앞에 대문자가 온다. ( 해당 예시는 파일 명을 수정한 상태 )
class Habit extends Component {
  render() {
    return (
      <div>
        Habit!
      </div>
    );
  }
}

export default Habit;
```

- 명명 규칙에 대해서 참고할만한 링크
  - [오라클 - 자바 관련 문서](https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html)



### connect Component

```react
// src/app.jsx

import React from 'react';
import './app.css';

// component 연걸하는 부분
import Habit from './components/habit';

function App() {
  // return 으로 html태그 형식이 들어간다.
  return <Habit></Habit>
  // return <Habit />
}

export default App;
```



## Component 작성하기



### className

- 리액트에서는 class 를 `className` 으로 사용한다.

- 약간의 팁! ( extension 사용 )

  - 태그를 쓰면서 `.` 을 붙이는 것으로 바로 className 을 붙여줄 수 있다.
    ```react
    // extension 이 필요하다~~~~
    // span.habit-name
    <span className="habit-name">Reading</span>
    ```



### synthetic event

- 참고
  - [react document - event](https://ko.reactjs.org/docs/handling-events.html)
  - [react document - syntheticEvent](https://ko.reactjs.org/docs/events.html)
- 리액트에서는 기본적으로 브라우저에서 사용하는 이벤트를 리액트에 맞게 감싸서 컴포넌트에 전달한다.
  - 각 상황에 맞는 이벤트가 있다. ex) keyboard, mouse, clipboard ...



### `setState()`

- 그냥 state 에 있는 값을 늘려주면 react 가 그 값을 모른다.
  따라서, 리액트가 값이 변경되었다는 것을 알기 위해서는 `setState` 가 필요하다.

```react
  handleIncrement = () => {
		// this.state.count += 1;
    // 이렇게 하면 리액트가 값이 변경되었다는 것을 모른다.
    
    this.setState({ count : this.state.count + 1}); 
  }
  
   handleDecreament = () => {
    const count = this.state.count - 1;
    // 깨알같은 삼항연산자!
    this.setState({ count : count < 0 ? 0 : count});
  }
```





## props

- 컴포넌트의 재사용성을 높이기 위해서 클래스 컴포넌트 밖에서 데이터를 받아올 때 사용하는 것
- 부모 컴포넌트에서 자식 컴포넌트로 값을 보내주는 형식이라고 생각하면 편하다.

```react
// 부모 컴포넌트
render() {
  return (
    // 깨알 JS map 함수의 내용을 {} 중괄호로 썻는데 render 가 안되는 현상이 있었다.
    // () 괄호로 바꾸니 render 됨... 이유는 모르겠지만 그렇다.
    <ul>
      {this.state.habits.map(habit => (
        // habit : 자식 컴포넌트에서 props 받아올 이름
        // {habit} : 부모 컴포넌트에서 자식 컴포넌트에 props 해줄 것
        <Habit habit={habit} />
      ))}
    </ul>
  );
}

// 자식 컴포넌트
render() {
  // JS 비구조화 할당
  const { name, count }= this.props.habit
  return (
    <>
    <li className="habit">
      <span className="habit-name">{ name }</span>
      <span className="habit-count">{ count }</span>
    </li>
    </>
  );
}
```

- 참고
  - [react document - component & props](https://ko.reactjs.org/docs/components-and-props.html)



### list key

- props 를 통해서 리스트를 만들 때, list의 요소들에 대해서 key를 요구하는 log 가 찍힌다.
- 리액트는 render 할 때 성능의 최적화를 위해서 render 를 할지 말지 계산하는데, 그 때 List id 를 가지고 계산한다.

```react
class Habits extends Component {
  state = {
    habits: [
      {
        id: 1, name: 'Reading', count: 0
      },
      {
        id: 2, name: 'Running', count: 0
      },
      {
        id: 3, name: 'Coding', count: 0
      },
    ]
  }
  
  render() {
    return (
      <ul>
        {this.state.habits.map(habit => (
          // key 를 통해서 넣어줄 수 있다.
          <Habit key={habit.id} habit={habit} />
        ))}
      </ul>
    );
  }
}
```

- id 는 고유한 것이기 때문에 겹치지 않도록 주의해서 사용해야 한다.

- 참고
  - [react document - key and list](https://ko.reactjs.org/docs/lists-and-keys.html)



### prop function!

- `onIncrement` 와 같이 `on~~ `를 사용하면 function 도 prop 해줄 수 있다.

```react
// parents component

<Habit
  key={habit.id}
  habit={habit}
  onIncrement={this.handleIncrement}
  onDecrement={this.handleDecrement}
  onDelete={this.handleDelete}
  />
```

- 받아주는 것은 `this.props` 를 통해서 받아준다.

```react
//children component

handleIncrement = () => {
  this.props.onIncrement(this.props.habit);
}
```



