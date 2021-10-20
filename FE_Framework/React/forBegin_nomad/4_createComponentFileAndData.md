# create Component File And Data 



- ES6 문법에서 안에 있는 것을 지칭하기

  ```react
  getMovies = async () => {
    const {
      data: {
        data : { movies }
      }
    } = await axios.get("https://yts-proxy.now.sh/list_movies.json");
    console.log(movies);
  }
  ```

  ```react
  const {
      data: {
        data : { movies }
      }
    } = ...
  ```

  - 안에 있는 것을 선택할 때!



- setState

  ```react
  getMovies = async () => {
    const {
      data: {
        data : { movies }
      }
    } = await axios.get("https://yts-proxy.now.sh/list_movies.json");
    this.setState({ moives })
  }
  ```

  - `this.setState({ moives })`
    - `this.setState({ moives:movies })`
      이렇게 하는게 맞다. 그런데 js는 위의 것도 알아먹고 잘 동작한다.





## 다른 Component 연결하기



- ```react
  render() {
    const { isLoading, movies } = this.state;
    return (
      <div>
        {isLoading ? "Loading..." : movies.map(movie => (
          <Movie
            key={movie.id} 
            id={movie.id}
            year={movie.year} 
            title={movie.title} 
            summary ={movie.summary} 
            poster={movie.medium_cover_image}/>
        ))}
      </div>
    )
  }
  ```

  - `key`를 넣어줘야 인식한다.
    key는 표현되지 않지만 필수 props 이므로 꼭 작성해야한다.

- ```react
  import React from "react";
  import PropTypes from "prop-types";
  
  function Movie({id, year, title, summary, poster}) {
    return (
    <div>
      <h4>{title}</h4>
    </div>
    );
  }
  
  Movie.propTypes = {
    id: PropTypes.number.isRequired,
    year: PropTypes.number.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    poster: PropTypes.string.isRequired,
  }
  
  export default Movie;
  ```

  - state를 사용할 필요가 없다면 function 으로 해도 좋다.
  - `export default Movie;` 를 해줘야 다른 js에서 Movie 라는 component를 사용할 수 있다.



