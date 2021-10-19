# map Recap



## 더 깔끔하게 관리해보자?

- ```react
  function renderFood(dish) {
    return <Food name = {dish.name} picture={dish.image} />
  }
  
  function App() {
    return (
      <div>
        {foodLike.map(renderFood)}
      </div>
    );
  }
  ```



## child unique "key" prop

- ```javascript
  index.js:1 Warning: Each child in a list should have a unique "key" prop.
  
  Check the render method of `App`. See https://reactjs.org/link/warning-keys for more information.
      at Food (http://localhost:3000/static/js/main.chunk.js:26:3)
      at App
  ```

  - 자식에게 data 를 넘겨줄 때 list 에 있는 것들은 유일성을 잃어버린다.
    따라서 id 를 지정해주는 것을 통해서 유일성을 가지도록 해준다.
  - react 의 모든 element 들은 다 다르게 보일 필요가 있다.



## data id

- ```react
  
  function Food({ name, picture }) {
    return (
      <div>
        <h2>I like { name }</h2>
        <img src={ picture } alt={name} />
      </div>
    );
  }
  
  function renderFood(dish) {
    return <Food key={dish.id} name = {dish.name} picture={dish.image} />
  }
  
  function App() {
    return (
      <div>
        {foodLike.map(renderFood)}
      </div>
    );
  }
  ```

  - ```react
    function renderFood(dish) {
      return <Food key={dish.id} name = {dish.name} picture={dish.image} />
    }
    ```

    - `key` 를 설정해주면 더이상 에러가 나지 않는다.
    - `key` prop은 Food Component 로 전달되지 않고, react 에서 사용되기 위해서 나온 것이다.



