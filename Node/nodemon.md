# nodemon

- 디렉토리의 파일 변경이 감지되면 노드 응용프로그램을 자동으로 다시 시작해서 Node.js 기반 응용 프로그램을 개발하는데 도움이 되는 도구
- nodemon은 코드나 개발 방법에 대한 추가 변경이 필요하지 않다.



- 용도
  - 애플리케이션 자동 다시 시작
  - 모니터링할 기본 파일 확장 감지
  - 특정 파일 또는 디렉토리 무시
  - 서버 애플리케이션과 함께 작동
  - 일회성 유틸리티 및 REPL



## install

```bash
# global
npm installl -g nodemon

# devDependencies
npm install --save-dev nodemon
```



## use

```json
// nodemon.json
{
	"watch" : ["./"],
	"exec" : "node server.js",
	"ext" : "js json yaml",
  "ignore" : ["./logs/"]
}
```

- `watch`
  - 서버 재시작을 위한 변경 감지 경로
- `exec`
  - nodemon 수행 명령
- `ext`
  - 변경 감지할 확장자
- `ignore`
  - 변경 감지 제외



- 참고
  - https://songjang.tistory.com/7



### package.json

```json
{
   "name" : " nodemon " ,
   "homepage" : " http://nodemon.io " ,
   "..." : " ... 기타 표준 package.json 값 " ,
   "nodemonConfig" : {
     "ignore" : [ " 테스트/* " , " 문서/* " ],
     "delay" : 2500 
  } 
}
```

- nodemon 은 package.json 에서도 실행된다.
  `nodemonConfig`를 통해서 사용할 수 있다.
- 주의점
  - nodemon.json 이나 --config 파일을 지정하는 경우에는 package.json 에서 설정한 것이 무시된다.



## 참고

- [공식](https://www.npmjs.com/package/nodemon)