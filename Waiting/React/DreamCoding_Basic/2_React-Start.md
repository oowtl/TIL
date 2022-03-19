# React Start



## create-react-app

- [Document](https://ko.reactjs.org/docs/create-a-new-react-app.html)

- 리액트 프로젝트를 만들 때 사용하도록 권고하는 것이다.
  하지만 현업에서는 환경에 맞는 Configuration을 설정해서 사용한다. (무조건 이걸로 하는 것은 아니다)



```bash
# npx 는 패키지를 실행할 수 있는 툴이다.
npx create-react-app my-app
cd my-app
npm start


# npm
npm init react-app my-app
# yarn
yarn create react-app my-app
```

- 좋은 점은 왠만한 것들은 전부 다 셋팅이 되어서 나온다는 것이다.
  만약에 모든 박스를 다 까보고 싶다면 `yarn eject` 를 해보면 까볼 수 있다.
  - 실행한 후에는 다시 봉합할 수는 없다.
    babel, webpack 등 설정을 볼 수 있다.



### app structure

![스크린샷 2022-02-25 오후 6.13.05](React-Start.assets/스크린샷 2022-02-25 오후 6.13.05.png)

- `public` : 정적인 것을 담아두는 곳
  - `manifest.json`
    - PWA(Progressive Web Application) 을 만들 때 필요한 파일
    - 모바일 환경에서 필요한 파일
  - `robot.txt`
    - 웹 크롤링에 필요한 파일
- `src` : 동적인 것을 담아두는 곳
  - `index.js`
    - 최상위에 있는 파일
    - 여기에서 `App.js`로 가도록 되어있다.
  - `App.js`
    - 여기에서 구현하면 된다.



### react hidden tool

- `yarn eject` 를 통해서 react에 어떤 것들이 사용되는지 알 수 있다.
- 무엇이 사용되는데?
  - Babel : JavaScript transcompiler
  - Webpack
    - 작성한 코드를 한번에 묶어서 번들단위로 전송해주는 것
    - 소스코드를 사용자에게 전달하기 용이하게 모듈을 번들링한다.
  - ESlint
    - 코드가 잘못된 것이 있으면 경고를 하는 것
  - Jest
    - Unit Test 를 할 때 사용하는 것
  - PostCSS
    - CSS 전처리기



## helpful Extension

- VS code
  - reactjs code snippets
  - auto import



## `public ` Directory

### `index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    
    ****PWA를 할 때, 사용하는 것************************************************
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    ************************************************************************
    
    <title>React App</title>
  </head>
  <body>
    
    ****사용자가 브라우저에서 JS 를 활성화하지 않았을 때 보여주는 메세지*******************
    <noscript>You need to enable JavaScript to run this app.</noscript>
    ************************************************************************
    
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>

```

- `<noscript>`
  - 사용자가 브라우저에서 JS 를 활성화하지 않았을 때 보여주는 메세지
  - react가 Js 로 돌아가니까 사용자가 JS 를 비활성화했을 때, 보여주는 경고 메세지



### ETC

- `robot.txt`
  - 웹크롤러가 사용하는 파일
  - 웹 크롤링에 대해서 허용, 비허용에 대한 내용을 작성하거나 어떤 부분을 허용하는지에 대해서 적을 수 있는 파일



## `src` Directory

- component는 JavaScript 파일과 구분하기 위해서 `.jsx` 를 붙여준다.
- 파일명에는 소문자가 앞이어야 한다.



### `reportWebVitals.js`

- 웹 퍼포먼스 측정도구로 web-vitals 라는 라이브러리를 사용한다.
- 사이트 성능 측정을 해서 그것을 객체로 보여주는 역할을 한다.



### `setupTests.js` , `App.test.js`

- 테스트용 파일
  Jest 를 사용하는 파일인 것 같다.



### `index.js`

- 엄밀히 말하면 JavaScript 이며, 연결해주는 역할을 한다.