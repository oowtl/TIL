# Package.json

- 프로젝트에 관련한 메타데이터가 담기는 곳
- 설치한 패키지들을 관리하는 파일
- **가장 중요한 항목은 `name`, `version` 이다.**
- 보통 Node.js 프로젝트의 루트 디렉토리에 위치한다.

## 분석

```json
{
  "name": "zoom",
  "version": "1.0.0",
  "description": "Zoom Clone Coding WebRTC and WebSocket!",
  "scripts": {
    "dev": "nodemon"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/oowtl/studyWebSocket.git"
  },
  "keywords": [],
  "author": "",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/oowtl/studyWebSocket/issues"
  },
  "homepage": "https://github.com/oowtl/studyWebSocket#readme",
  "devDependencies": {
    "@babel/cli": "^7.17.0",
    "@babel/core": "^7.17.2",
    "@babel/node": "^7.16.8",
    "@babel/preset-env": "^7.16.11",
    "nodemon": "^2.0.15"
  },
  "dependencies": {
    "express": "^4.17.2",
    "pug": "^3.0.2",
    "ws": "^8.5.0"
  }
}
```

- package.json 파일
  - 22.02.16 진행 중인 프로젝트에서 들고왔다.





- [x] package.json 에 관한 모든 것을 정리하지는 않았다.

### name

- package.json 에서 가장 중요한 것은 `name`, `version` 필드 이다.
  - 완전히 고유한 것으로 간주되는 식별자를 형성한다.
- 패키지의 변경은 버전 변경과 함깨 제공되어야 한다.
- 만약 패키지를 게시하지 않는다면 이름과 버전필드는 선택사항이다.



- Rule!
  - 214자 이하
  - 소문자만 사용 가능하다.
  - 이름은 URL의 일부와 명령줄의 인수, 폴더의 이름이 된다.
    따라서 이름에는 URL에 안전하지 않은 문자가 포함될 수 없다.



### version

- 이름과 마찬가지로 필수적인 요소이다.
- 버전은 node-semver 에서 구문 분석을 할 수 있어야 한다.
  - 버전을 명시하는 방식이다. ( ~, ^  등등 을 통해서 호환되는 버전 혹은 일치하는 버전 등을 명시해주는 것이다. )



### description

- 말 그대로 설명을 넣는 곳이다.
- npm search 를 통해서 접근할 수 있도록 해주는 것



### keywords

- 키워드를 넣는 곳이다.
- npm search 를 통해서 접근할 수 있도록 해주는 것



### homepage

- 프로젝트 홈페이지가 있을 경우에 입력하는 곳
- url 항목과는 다르다.
  url 을 설정하면 외부에 설치된 패키지 소스로 리디렉션되고, 예상치 못한 움직임을 유발할 수 있다.
  - 20220216 기준으로 무슨 말인지는 모르겟지만,
    검색을 통해서 본 결과로 url 이 의도하지 않은 경로로 들어간다는 것을 볼 수 있었다.



### bugs

- 프로젝트의 이슈와 트래킹을 볼 수 있는 Url 과 이슈를 알릴 email 주소를 입력할 수 있다.
- bugs 에는 url 이나 Email 중 하나만 적용할 수 있다. 
- Url 을 지정하는 경우에는 `npm bugs` 명령을 사용할 수 있다.



### license

- 패키지 사용자들에게 해당 패키지를 사용하기 위해서 어떻게 권한을 얻는지, 어떤 금기사항이 있는지 알게하도록 라이센스를 명시해야한다.
- 라이센스를 사용하는 경우에 적는 것이다.
  - MIT, ISC ... 여러가지 라이센스가 존재한다.
  - 다수의 라이선스가 부여되었을 때의 표현식도 존재한다.
- 참고 
  - [SPDX 라이센스 ID 의 전체 목록](https://spdx.org/licenses/)
  - [OSI 승인 라이센스](https://opensource.org/licenses/alphabetical)
  - [SPDX 라이선스 표현식 구문](https://www.npmjs.com/package/spdx)



### author & contributors

- author : 한 사람만 지정한다.
- contributors : 여러사람을 배열로 지정한다.
- 각 사람들은 이름(필수) 선택적으로 url, email 을 넣을 수 있는 개체이다.



### scripts

- 패키지의 생명주기 중에 다양한 시점에서 실행되는 script 명령들을 포함하고 있는 사전이다.
- scripts 항목 객체에서 키는 이벤트이고, 값은 이때 실행될 커맨드 이다.
- 참고
  - https://docs.npmjs.com/cli/v8/using-npm/scripts



### dependencies

- 의존성을 규정하는 것은 패키지의 이름과 해당 패키지의 버전 범위를 지정한 객체를 통해서 이루어진다.
  (이름과 버전이라는 뜻)
- 테스트 관련 모듈이나 트랜스파일러 관련 모듈을 dependencies 개체에 추가하면 안된다.
  ( 개발에 사용한 것들을 추가하지 말라는 것이다. 관련한 것은 devDependencies 를 참고하면 된다. )
- 버전 범위를 지정하는 방법
  - [semver](https://github.com/npm/node-semver#versions)



### devDependencies

- 개발에 사용된 모듈들의 dependency 를 넣어주는 곳이다.





# 출처

- https://docs.npmjs.com/cli/v7/configuring-npm/package-json
- https://oneroomtable.tistory.com/entry/packagejson-%ED%8C%8C%EC%9D%BC%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%96%B4%EB%96%A4-%EC%97%AD%ED%95%A0%EC%9D%84-%ED%95%A0%EA%B9%8C%EC%9A%94
- https://programmingsummaries.tistory.com/385
- https://ooeunz.tistory.com/19
- https://webclub.tistory.com/472
- https://velog.io/@chancesu7/PUBLICURL-bug