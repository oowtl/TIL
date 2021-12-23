# react CSS



- ```react
  unction Movie({ year, title, summary, poster}) {
    return (
    <div class="movie">
      <img src={poster} alt={title} title={title}/>
      <div class="movie__data">
        <h3 class="movie__titile" style={{ backgroundColor: "red"}}>{title}</h3>
        <h5 class="movie__year">{year}</h5>
        <p class="movie__summary">{summary}</p>
      </div>
    </div>
    );
  }
  
  ```

  - `      <h3 class="movie__titile" style={{ backgroundColor: "red"}}>{title}</h3>`
    - `style` 을 통해서 할 수 있다.
      그런데 이거 좀 별로지?

- file 로 가자

  ```react
  // css
  import "./Movie.css";
  ```

  ```css
  body {
    background-color: #f2f2f2;
  }
  ```

  - file 을 만들어서 import 시키는 쪽으로 가자

