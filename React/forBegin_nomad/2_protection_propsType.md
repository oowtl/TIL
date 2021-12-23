# Protection Props Type

 

- 부모 component로 부터 받은 props 가 제대로 온 것인지 확인할 필요가 있다.
- 설치가 필요합니다.



## prop-types 설치

- ```bash
  npm i prop-types
  ```

  - 내가 전달받은 prop이 내가 원하는 것인지 체크해주는 것
  - 이거 하고 에러나면 `npm install` 하고 들어가자!



## 사용하기

- ```react
  import PropTypes from "prop-types";
  
  # 반드시 propTypes 로 해야 인식한다.
  Food.propTypes = {
    name: PropTypes.string.isRequired,
    picture: PropTypes.string.isRequired,
    rating: PropTypes.string.isRequired
  };
  ```

  - 형식

    ```react
    prop 하는 속성이름 : PropTypes.string(자료형).isRequired,
    ```
    - `isRequired`는 반드시 있을 필요는 없다.
    - 자료형을 주의해서 주는 것이 중요하다.

  - 주의할 점

    - rating은 숫자이기 때문에 에러가 나온다.

      ```javascript
      index.js:1 Warning: Failed prop type: Invalid prop `rating` of type `number` supplied to `Food`, expected `string`.
          at Food (http://localhost:3000/static/js/main.chunk.js:55:3)
          at App
      ```

      - 해결하는 방법은 ` rating: PropTypes.string.isRequired`에서 `string`-> `number`로 바꾸는 것이다.

    - ```react
      Food.propTypes = {
        name: PropTypes.string.isRequired,
        picture: PropTypes.string.isRequired,
        rating: PropTypes.number.isRequired
      };
      ```

      - 끝

