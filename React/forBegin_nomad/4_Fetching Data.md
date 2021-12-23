# Fetching Data



## Axios

> fetch 위에 있는 작은 layer



- 설치하기

  - ```bash
    npm install axios
    ```



- 사용하기

  - ```react
    componentDidMount() {
      const movies = axios.get("https://yts-proxy.now.sh/list_movies.json");
    }
    ```

    - 이렇게 사용하면 된다
    - 여기에서 주의할 점은 axios 요청을 하고 data 를 받아오는데 시간이 걸린다.
      그래서 기다려줘야한다!



- axios 로 받아오기 까지 기다리기

  - 방법 1 async

    - ```react
      async componentDidMount() {
        const movies = axios.get("https://yts-proxy.now.sh/list_movies.json");
      }
      ```

      - 앞에 async를 달아준다.

  - 방법 2 function 만들기

    - ```react
      getMovies = async () => {
        const movies = await axios.get("https://yts-proxy.now.sh/list_movies.json");
      }
      componentDidMount() {
        this.getMovies();
      }
      ```

      - 중요한 점!!!

        ```react
        getMovies = async () => {
          const movies = await axios.get("https://yts-proxy.now.sh/list_movies.json");
        }
        ```

        - `async` 
          - 비동기라는 것을 말해주는 것이다.
          - 함수를 시작할 때, 이 녀석은 기다려줘야 해~ 라고 말하는 것
        - `await`
          - async 와 쌍으로 쓰인다.
          - 그럼 뭘 기다리는데? 아~ 이 녀석을 기다리라는 구나~ 라고 명시해주는 것

