# React Concept



## Component

- 컴포넌트를 만드는 방법
  - class component
    - dynamic
      - 데이터가 바뀌면서 바뀌어야 하는 부분에 사용한다.
    - state, render
      - state 를 통해서 데이터를 가지고, render 함수를 통해서 나온다.
    - lifecycle methods
      - 생성 순서에 따라서 작동할 수 있도록 한 것
  - function component
    - static
      - 정적인 부분에 사용한다.
        변하지 않는 부분에 사용한다.
    - state, render 를 가지지 않는다.
    - react hook (v 16.8)
      - state, render, lifecycle methods
        - react hook 을 사용해서 function component 에서도 사용할 수 있다.
      - class 에서 지원하는 것들을 그대로 function 에서 지원하는 이유
        1. class 라는 개념이 모든 사람에게 쉬운 것은 아니다.	
           - 디자이너
           - script 만 다루던 개발자
        2. class 로 개발하면 class 안에서 멤버변수에 접근하기 위해서는 this 를 사용해야한다.
        3. class 안의 함수에 접근할 때 binding 이슈 존재



| Component           | Function       |
| ------------------- | -------------- |
| React.Component     | function       |
| React.PureComponent | memo(function) |
|                     | React Hook     |

- 사용할 수 있는 것을 나타낸 표