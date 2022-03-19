# Bable



## What is Babel?

> Babel 은 현재 및 이전 브라우저 또는 환경에서 ECMA script 2015+ 코드를 이전 버전의 JavaScript 로 변환하는 데 주로 사용되는 도구 체인이다.

- 자바스크립트 컴파일러
- 소스코드를 웹브라우저가 처리할 수 있는 JS 버전으로 변환하는 것으로 새로운 JS(ECMA script 2015+) 의 기능을 사용할 수 있다.
- JS 환경에서 누락된 기능을 위해서 core-js 에서 제공하는 폴리필을 자동으로 주입할 수 있다.
  - [core-js Git](https://github.com/zloirock/core-js)

- chrome, firefox 와 같은 브라우저들은 ES6 의 문법을 지원하지만 ie 와 같이 지원하지 않는 브라우저도 존재한다.
  그래서 babel을 사용해서 여러 브라우저에 호환될 수 있도록 해야한다.



### polyfill?

> polyfill : 충전솜

- 브라우저에서 지원하지 않는 코드를 사용 가능한 코드 조각이나 플러그인으로 변환한 코드를 의미한다.
  최신 자바스크립트의 기능을 구식 자바스크립트 코드로 똑같이 구현한 코드를 의미한다.
- ES6+ 의 문법에서 새로운 것이 나오면 그것을 쓸 수 있도록 하는 것
- 주의점 
  - @babel/polyfill 은 Babel 7.4.0 부터 deprecated 되었다.
    따라서 @babel/plugin-transform-runtime 을 사용하는 것으로 추가한다.
    물론 이것을 설치할 필요는 없다. babel 사용하면 다 추가된다.



### Babel 의 동작

- 3단계

  1. 파싱 (Parsing)
     - 소스코드를 읽고 추상 구문 트리(AST)로 변환하는 단계

  2. 변환, Transforming
     - 1단계에서 파싱된 추상 구문 트리를 각 브라우저의 환경에 맞는 결과로 변환하는 단계
     - 바벨의 플러그인 중 preset/plugin 에 의해서 처리되는 곳
       - preset 은 plugin 들을 모아놓은 배열이다.
       - 플러그인 들은 babel-traverse 를 이용해서 원분 AST 가로질러가는 과정 속에서 각 부분들이 어떻게 바뀌고 정의되어야하는지를 기록한다.

  - 출력, Printing
    - 2단계에서 생성된 새로운 AST를 바탕으로 실제 브라우저 환경에 맞는 소스코드로 변환한다.

- 전체 동작과정

  - Source Code -> babylon parser -> AST -> Plugin + babel-traverse -> modified AST -> babel-generator -> compiled code



#### 추상 구문 트리 ( AST : Abstract Syntax Tree )

- 추상화된 코드의 표현체
- 각각의 노드는 소스코드의 구조를 의미한다.



- 소스코드가 바이트코드가 되는 과정
  - 소스코드 -> scanner/parser -> AST -> interpreter -> byte code
  - scanner, 어휘 분석기, Lexical analyzer
    - 문자들을 하나씩 다 읽어서 단어단위로 끊어서 다 분리시킨다.
  - parser, 구문 분석기, Syntax analyzer
    - scanner 를 거친 뒤 만들어진 내용을 가져와서 구문을 확인해보고 AST 로 변환한다.

- 참고
  - https://blog.naver.com/PostView.nhn?blogId=dlaxodud2388&logNo=222260114774



## babel 의 구성요소

- @babel/core
  - [Document : @babel/core](https://babeljs.io/docs/en/babel-core)
  - babel 을 사용하기 위해서 설치해야하는 것
  - 공식문서를 봐서는 source code 에서 modified AST 로 바꿔주는 역할을 맡고 있는 것 같다.
    babel 그 자체랄까
    - 하지만?? babel 은 플러그인이 동작하는 것이다. ( plugin 을 설치하지 않는다면 아무것도 하지 않는다.)
- @babel/cli
  - [Document : @babel/cli](https://babeljs.io/docs/en/babel-cli)
  - 커맨드 인터페이스로 바벨을 실행할 수 있도록 도와준다.
  - 프로젝트 별로 로컬로 설치하는 것을 권장한다.
    - Babel 의 다른 버전에 의존할 수 있다.
    - 작업 중인 환경에 대한 종속성이 없어야 프로젝트를 좀 더 이식성있고 쉽게 설정할 수 있다.
- @babel/preset
  - [Document : @babel/preset-env](https://babeljs.io/docs/en/babel-preset-env)
  - 대상 환경에 필요한 구문 변환( 특정 브라우저 폴리필 )을 세세하게 관리할 필요 없이 최신 JavaScript를 사용할 수 있도록 하는 스마트 사전 설정
  - 여러가지 옵션을 통해서 필요한 기능을 선택해서 제공할 수 있다.
    - @babel/preset-env 는 모든것을 이전 단계의 JS 로 바꾸기 때문에 코드의 길이가 상당히 길어질 수 있다.
      오래된 브라우저에서는 이 길이를 무시할 수 없다.
- @babel/node
  - babel/cli 에서 독립된 모듈로 node cli 처럼 동작한다.
    - `node index.js` 를 `babel-node index.js`로 쓸 수 있다.
  - node.js 실핸 전 Babel의 설정과 플러그인을 고려하여 컴파일하는 이점이 있다.
  - 주의점
    - 배포용에서 사용해서는 안된다.
      메모리에 캐시가 저장되기 때문에 메모리 사용량이 많아서 불필요하게 무겁다.
      전체 랩을 즉석에서 컴파일 해야하므로 시작 성능 저하가 있을 것이다.
      -> 개발용으로 사용하기에 적합하다.
    - `@babel/node`를  `@babel/core`먼저 설치해야한다.
      아니면 `babel-node 6.x` 가 설치될 것이다.
  - 참고
    - [공식문서](https://babeljs.io/docs/en/babel-node)







## 출처

- https://babeljs.io/docs/en/
- https://lihano.tistory.com/20
- https://moonformeli.tistory.com/28
- https://defineall.tistory.com/646
- https://pathas.tistory.com/217
- https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-%EC%99%9C-babel-preset%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80-yhk03drm7q