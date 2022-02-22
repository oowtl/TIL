# How_to_npm

> How to!
>
> Use!
>
> npm!



## install

```bash
# 패키지 설치
npm install (plugin)

# 시스템 전체에 설치, 프로젝트의 package.json 에는 기록되지 않는다.
npm install -g (plugin)

# 해당 옵션은 npm5 부터 default 가 되었다. npm install 과 동일
# 패키지를 node_modules 디렉토리에 설치하고 package.json 파일의 dependencies 항목에 정보가 저장된다.
# --production 빌드 시 해당 플러그인이 포함된다.
npm install -P (plugin)
npm install --save (plugin)
npm install --save-prod (plugin)

# 패키지를 node_modules 디렉토리에 설치하고 package.json 파일의 devDependencies 항목에 정보가 저장된다.
# --production 빌드 시 해당 플러그인이 포함되지 않는다.
npm install --save-dev (plugin)
npm install -D (plugin)
```



- 참고
  - https://ithub.tistory.com/165



