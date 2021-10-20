# gh-pages

> git hub 에서 static 웹사이트, html, css javascript 를 올릴 수 있도록 해준다.



- 설치하기

  - ```bash
    npm install gh-pages
    ```

- 명시하기

  - ```json
    # package.json
    
    "homepage" : "https://oowtl.github.io/reactjs_nomad/"
    ```

    - 추가해준다.
    - 여기에서 주의할 점은 유저아이디와 repository 이름 모두가 소문자이어야만 한다는 것이다. 무조건!!

- package.json

  - ```json
    "scripts": {
      "start": "react-scripts start",
      "build": "react-scripts build",
      "deploy": "gh-pages -d build",
      "predeploy" : "npm run build"
    }
    ```

    - npm run deploy를 하면 predeploy 가 실행되면서 build 폴더에 build 결과물을 놓는다.
    - `-d build` 는 build directory 에 있는 것을 붙인다는 것이다.
    - 여기에서 pre 가 붙는 것은 뭐든 하기전에 실행하는 것이다.

- build

  - 하기 전에 먼저 build 를 해준다.

    ```bash
    npm run build
    ```

- deploy

  - ```bash
    npm run deploy
    ```

    - 이걸하는 것으로 올린다.

- 주의할 점

  - 나는 브랜치를 develop에다가 하는데, main 으로 바꾸니까 된다.