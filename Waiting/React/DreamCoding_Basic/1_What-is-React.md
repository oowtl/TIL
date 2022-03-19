# What is React

- React 는 user interface 를 만들 수 있는 Library 이다.
- M **V** C
  - V = React ( View )
- React = components
  - reusual (재사용 가능)
  - independent(독립적)
    - testable(테스트 가능)
      - unit test
- component
  - navbar, button ....
- React Virtual DOM Tree
  - 리액트는 가상의 돔트리를 메모리에 저장한다.
    이를 통해서 render 함수가 호출되더라도 실질적으로 보여지는 데이터가 변동되어야만 업데이트한다.
    또한 하나의 컴포넌트가 업데이트될 때, 업데이트가 필요한 다른 컴포넌트들을 계산해서 필요한 곳만 업데이트 한다.
- 리액트는 데이터가 변경될 때마다 어플리케이션을 업데이트 한다.
  - 그래도 성능을 유지할 수 있는 것이 Virtual DOM Tree 를 통해서 필요한 곳을 할 수 있도록 한다.