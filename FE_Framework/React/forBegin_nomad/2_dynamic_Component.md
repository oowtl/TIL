# Dynamic Rendering



## 먼저 알아둘 것

### map

- array 의 각 요소에 접근해서 함수를 실행시키고 map 안에서 return 한 것을 그대로 배열로 가지고 있는다.

- ```javascript
  # 선언
  const friends = ["jj", "ey", "kim", "bt", "hs"];
  undefined
  
  # map 사용
  friends.map(current => {
      console.log(current + " 🤞");
      return current + " 😄";
  })
    VM558:2 jj 🤞
    VM558:2 ey 🤞
    VM558:2 kim 🤞
    VM558:2 bt 🤞
    VM558:2 hs 🤞
  	(5) ['jj 😄', 'ey 😄', 'kim 😄', 'bt 😄', 'hs 😄']
  ```

  - 이렇게 내어준다!



## Dynamic Rendering

- ```react
  import React from 'react';
  
  function Food({ name }) {
    return (
      <h3>I like { name }</h3>
    );
  }
  
  const foodLike  = [ 
    {
      name: "Kimchi",
      image:
        "http://aeriskitchen.com/wp-content/uploads/2008/09/kimchi_bokkeumbap_02-.jpg"
    }, ...
  ];
  
  function App() {
    return (
      <div>
        {foodLike.map(dish => (
          <Food name={dish.name} />
        ))}
      </div>
    );
  }
  
  export default App;
  ```

  - ```react
    {foodLike.map(dish => (
      <Food name={dish.name} />
    ))}
    ```

    - 우리가 봐야할 것은 이것이다. map 이라는 반복문을 통해서 Component 를 쉽게 만들어낼 수 있다.
    - `{}` 괄호를 사용하는 것을 통해서 javascript 의 문법을 사용할 수 있다.

- ```react
  function Food({ name, picture }) {
    return (
      <div>
        <h2>I like { name }</h2>
        <img src={ picture } />
      </div>
    );
  }
  
  const foodLike  = [ 
    {
      name: "Kimchi",
      image:
        "http://aeriskitchen.com/wp-content/uploads/2008/09/kimchi_bokkeumbap_02-.jpg"
    }, ...
  ];
  
  function App() {
    return (
      <div>
        {foodLike.map(dish => (
          <Food name={dish.name} picture={dish.image} />
        ))}
      </div>
    );
  }
  ```

  - ```react
    function Food({ name, picture }) {
      return (
        <div>
          <h2>I like { name }</h2>
          <img src={ picture } />
        </div>
      );
    }
    ```

    - 다시 한번 주의하자! component 에서 리턴되는 것은 하나여야 한다.

      ```react
      return (
      	<h2>I like { name }</h2>
        <img src={ picture } />
      );
      ```

      이렇게 했다가 에러나고 왜인지 몰랐다...



