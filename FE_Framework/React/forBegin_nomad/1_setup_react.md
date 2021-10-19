# SetUP



## 1. create-react-app

> webpack, babel 등을 설치해야하는 과정을 한번으로 끝내는 것



```bash
npx create-react-app 'folder name'
```

- facebook 에서 관리하는 git 으로 들어가면 자세한 것을 알 수 있다.





### 설치에러

```bash
npm ERR! 404  'creat-react-app@latest' is not in the npm registry.
```

- 이런 에러가 발생했다.

- 해결

  - https://wiki.jjagu.com/?p=107

  - ```bash
    npm config set registry http://registry.npmjs.org
    ```





### package.json

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

- 이 부분에서 우리는 'start', 'build' 를 사용할 것이다.





## 3. npm start

```bash
npm start
```

- 해당하는 폴더에 npm start 눌러보자

