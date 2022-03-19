# props Array



```react
Movie.propTypes = {
  id: PropTypes.number.isRequired,
  year: PropTypes.number.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  poster: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired
}
```

- `genres: PropTypes.arrayOf(PropTypes.string).isRequired`
  - `arrayOf()`
    - array 가 들어올 것을 명시하고 그 안에 무엇이 들어있을지도 명시해준다.



## className

- ```react
   <section className="container">
    {isLoading
      ?
      <div className ="loader">
        <span className = "loader__text">Loading...</span>
      </div>
      :
      <div className = 'movies'>
      {movies.map(movie => (
        <Movie
          key={movie.id} 
          id={movie.id}
          year={movie.year} 
          title={movie.title} 
          summary ={movie.summary} 
          poster={movie.medium_cover_image}
          genres={movie.genres}/>
      ))}
    </div>
    }
  </section>
  ```

  - ` <section className="container">`

    - JSX 가 javascript 라서 html 의 class 와 javascript의 class 를 헷갈려 한다.
      그래서 명시적으로 나타내주기 위해서 사용한다.

  - 이와 비슷한 것으로

    ```html
    <label for ="" />
    <label htmlFor ="" />
    ```

    - label 의 for 를 사용할때는 `htmlFor`를 사용해야 한다.





## 에러를 잡아보자!

- ```react
  function Movie({ year, title, summary, poster, genres }) {
    return (
    <div className="movie">
      <img src={poster} alt={title} title={title}/>
      <div className="movie__data">
        <h3 className="movie__titile" style={{ backgroundColor: "red"}}>{title}</h3>
        <h5 className="movie__year">{year}</h5>
        <ul className="genres">
          {genres&&genres.map((genre, index) => ( 
            <li key={index} className="genres__genre">{genre}</li>
          ))}
        </ul>
        <p className="movie__summary">{summary}</p>
      </div>
    </div>
    );
  }
  
  Movie.propTypes = {
    id: PropTypes.number.isRequired,
    year: PropTypes.number.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    poster: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string)
  }
  
  export default Movie;
  ```

  - ```react
    <ul className="genres">
      {genres&&genres.map((genre, index) => ( 
        <li key={index} className="genres__genre">{genre}</li>
      ))}
    </ul>
    ```

    - `genres&&genres.map`
      - 하다보니까 genres 가 undefined 라서 안되는 map 을 할 수 없는 현상이 생겼다.
        그래서 genres 가 존재하면 실행하는 것으로 바꾸었다.
    - `(genre, index) `
      - map은 index 를 제공한다. 무조건 index 라고 할 필요없이 뒤에 이름을 넣어주면 알아서 작동한다.