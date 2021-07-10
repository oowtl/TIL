# Spring 0. Settings

[TOC]





## 준비하기

- 준비물
  1. Java 11
  2. IDE
     - Eclipse
     - Intelli J



## Spring 프로젝트 생성하기

- [스프링 부트 스타터](https://start.spring.io/)
  - Project
    - 필요한 라이브러리를 들고오고 라이프사이클을 관리하는 툴
    - `Gradle Project` 선택하기
      - 과거에는 `Maven`을 사용했지만, 최근에는 `Gradle`을 사용한다.
      - spring 라이브러리 자체도 이걸로 넘어오는 추세이다.
  - Language
    - `Java` 선택
  - Spring Boot
    - 버전 선택
    - `SNAPSHOT`, `M1` 이 달려있는 것은 정식으로 릴리즈 된 버전이 아니다.
  - Project Metadata
    - Group
      - 기업의 정보
    - Artifact
      - 빌드되고 난 다음의 결과물
  - Dependencies
    - 프로젝트에서 어떤 라이브러리를 사용할 것인지 정하는 것
    - `ADD DEPENDENCIES` 클릭해서 한다.
    - 웹 프로젝트
      - Spring Web
      - Thymeleaf
        - html 을 만들어주는 템플릿 엔진
  - 다 했다면!  `GENERATE` 클릭한다!
    - `.zip`으로 다운로드 받을 수 있다.

- Import 해주기 (Intelli J)
  - `Open or Import`  
  - `.zip` 파일을 압축 해제한 폴더로 간다.
  - `bulid.gradle` 을 연다.
  - `Open as Project`
  - 처음 열면 외부에서 라이브러리를 90MB 정도 다운을 받아애 해서 시간이 걸린다.



### 프로젝트 구조

- 프로젝트 폴더 구조 보이는 방식 바꾸는 방법
  - 왼쪽 상단에 cog icon 클릭
  - `compact middle packages`



- gradle

  - wrapper

- src

  - main

    - java

      - > java 와 관련된 코드

    - resources

      - > java 코드를 제외한 html 이나 설정파일

  - test

  - > 최근에는 main 과 test 로 구조가 나뉘어지는 것이 트렌드 (중요하다!)

- build.gradle

  - 설정파일
    예전에는 이런 것들을 하나하나 설정해줘야 하는데 spring boot 가 나오면서 개발자 친화적으로 바뀌면서 자동으로 해준다.

  - dependencies

    - 의존성에서 추가한 것들이 있다.

    - ```gradle
      dependencies {
      	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
      	implementation 'org.springframework.boot:spring-boot-starter-web'
      	testImplementation 'org.springframework.boot:spring-boot-starter-test'
      }
      ```
      - test 라이브러리는 자동으로 들어간다.

  - repositories

    - 의존성에 있는 것들을 추가해야할 때, 어디에서 다운로드 받을 지 설정하는 것

    - ```gradle
      repositories {
      	mavenCentral()
      }
      ```

      - `mavenCentral()` 이 곳에서 받으라는 뜻

## Spring 프로젝트 실행하기



### 실행하기

- `src` > `main` > `java` > `spring boot 에서 기입한 이름` > `(기입한이름)Application`
- main method 실행
  - 실행을 하면 tomcat이라는 웹서버를 자체적으로 가지고 있으면서 서버를 시작하면서 같이 한다.
- console 창 확인
- `localhost: 8080` 접속
  - `Whitelabel Error Page` 라는 내용을 지닌 html 이 뜨면 성공!



### 실행환경 설정

- Gradle 을 통해서 실행하면 좀 느릴 때가 있어서 자바에서 직접 사용하도록 한다.
- `intelli J` > `preference` or `setting` > `Build, Execution, Deployment` > `Build Tools` > `Gradle` > `Build and run` > `Build and run using` - `IntelliJ IDEA`  & `Run test using` - `IntelliJ IDEA`
  - `Build, Execution, Deployment`  이 단계에서 `gradle`을 검색하면 된다.



### console 창 컬러 폰트 사용하기

- `main` > `resources` > `application.properties`

- ```code
  spring.output.ansi.enabled=always
  ```

- 저장하기!



## 라이브러리 둘러보기

- gradle, maven 은 의존관계를 관리해준다.
- 각각의 것들을 사용하기 위해서 하나만 필요한 것이 아니라 여러가지 필요하다.
  따라서 이런 것들을 사용하기 위해서 필요한 것을 다 땡겨온다.
- 확인해보기
  - Intelli J 에서 왼쪽 최하단 창 두개 겹친 아이콘 클릭
  - 우측 상단 `Gradle` 클릭
  - `Dependencies` 클릭
  - 이 곳에서 의존성을 확인할 수 있다.
    - (*) 달려있는 것은 이미 설치된 의존성이라는 것을 표시하는 것

- 예전에는 서버를 설치하고 해야했는데, 요즘은 자체적으로 내장이 되어있어서, 설치하고 실행시키면 알아서 다한다.



### 로그 남기기

- 현업에서는 `System.out.println`을 사용하면 안된다.
- 실무에서는 로그를 사용해야 한다.
- 라이브러리 (궁금하면 사용해보기 )
  - `slf4j` : log 인터페이스
  - `logback` : log 검색 (성능 :+1: )



### 라이브러리 내용 (최근 표준)

- 스프링 부트
  - spring boot-starter-web
    - spring-boot-tomcat : 톰캣 (웹 서버)
    - spring-webmvc : 스프링 웹 MVC
  - spring-boot-starter-thymeleaf : 템플릿 엔진
  - spring-boot-starter(공통)
    - spring-boot
      - sprint-core
    - spring-boot-starter-logging
      - logback, slf4j
- 테스트
  - spring-boot-starter-test
    - j unit : 테스트 프레임워크
      - 5 버전을 사용하는 추세이다.
    - mockito : 목 라이브러리
    - assertj : 테스트 코드 편하게 작성하게 하는 것
    - sprint-test : 스프링 테스트 지원
      - j unit 테스트를 할 때, spring 과 통합해서 하도록 하는 것



## View 환경설정



### Welcome Page 만들기

- 정적 페이지 
  - `src` > `main` > `resources` > `static` > `index.html` 생성
  - spring documentation 확인하기
    - static 에서 찾고 없으면 template 에서 찾는다고 한다.
- 템플릿 엔진
  - 내가 뭔가 값을 바꾸면 그것대로 움직이도록 하는 것
  - thymeleaf 라는 템플릿 엔진을 사용한다.



### controller

- 웹 어플리케이션의 진입점

- 만들기

  - `src` > `main` > `java` > `hello.hellospring`  > `controller` package 생성 > `helloController` java class 생성

  - class 위에 `@Controller` annotation 을 적어 줘야 한다.
    `Model` 을 적으면 하나 import 하나 들어오고
    `GetMapping` 을 적으면 import 하나 들어온다.

    - ```java
      package hello.hellospring.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.web.bind.annotation.GetMapping;
      
      @Controller
      public class helloController {
      
          @GetMapping("hello")
          public String hello(Model model){
              model.addAttribute("data", "hello");
              return "hello";
          }
      }
      
      ```

      - 웹 어플리케이션에서 `/hello` 로 들어오면 여기로 들어온다.
      - `model.addAttribute("data", "hello");`
        - model 에 뭔가를 추가한다.

    - `GetMapping` : HTTP method 일 때의 GET

  - `src` > `main` > `resources` > `templates` > `hello.html` 생성

    - ```html
      <!DOCTYPE HTML>
      <html xmlns:th="http://www.thymeleaf.org">
      <head>
          <title>Hello</title>
          <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
      </head>
      <body>
      <p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
      </body>
      </html>
      ```

      - `<html xmlns:th="http://www.thymeleaf.org">` 
        - `th` : 타임리프의 th 이다.
        - 엔진을 선언하는 문장
      - `<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>`
        - `${data}` 
          - java class 에서 선언한 ` model.addAttribute("data", "hello");` 의 data

    - 결과

      - ```html
        <p>안녕하세요. hello!!</p>
        ```



### 동작과정

1. 웹 브라우저가 `localhost:8080/hello` 를 입력받음
2. tomcat 서버가 받고서 스프링에게 물어본다.
   '너희 hello 있니???'
3. 스프링에서 찾아보고 있는 것을 확인한다. (helloController)
   - `GetMapping("hello")` 가 있구나!!
     해당 메서드를 실행시킨다!
   - `public String hello(Model model)`
     - spring 이 model 이라는 것을 만들어서 넘겨준다.
   - `return "hello";`
     - `resources` > `templates` > `hello.html`  로 가라!
4. Thymeleaf 템플릿 엔진이 처리한다. (viewResolver)
   - `templates/hello.html` 를 처리한다.



- viewResolver
  - 컨트롤러에서 리턴값으로 문자열이 나온다.
  - 그것을 가지고 viewResolver 가 찾아서 처리를 한다.
  - 기본적으로 viewName 을 매핑해놓고 있다.
    - `resources/templates/ {ViewName(return String)} . html`



### spring-boot-devtools

- `.html` 파일만 컴파일하면 서버 재시작 없이 View 파일을 변경시키는 것이 가능한 라이브러리
- 컴파일 방법
  - 메뉴 `build ` - `Recompile`



## Build

- 막간의 개념
  - 컴파일 : 사용자가 작성한 코드를 컴퓨터가 이해할 수 있는 언어로 번역하는 일
  - 빌드 : 컴파일된 코드를 실제 실행할 수 있는 상태로 만드는 일
    - 컴파일을 포함해서 war, jar 등의 실행 가능한 파일을 뽑아내기까지의 과정을 빌드한다라고도 한다.
  - 배포 : 빌드가 완성된 실행 가능한 파일을 사용자가 접근할 수 있는 환경에 배치시키는 일



- 터미널에서 spring 이 있는 폴더로 간다.

  - spring project 폴더이며, 구성하는 파일들이 있는 곳이다.

- `gradlew build` 를 해준다.

  - ```terminal
    ./gradlew build
    ```

    - 받아야할 것들은 받고 한다.

- build 폴더로 이동한다.

  - ```terminal
    cd build
    ```

- libs 폴더로 이동한다.

  - ```terminal
    cd libs
    ```

- jar 파일 실행

  - ```terminal
    java -jar hello-spring-0.0.1-SNAPSHOT.jar
    ```

  - 서버를 실행할 떄는 jar 파일을 복사해서 넣은 다음에 실행시킨다.
    - 이것만 있으면 다 됩니다.



### build error? - clean!

- 에러가 나서 잘 안되는 경우에는 다 지우고 새로 설치하는 것도 좋은 방법이다.

- ```terminal
  ./gradlew clean
  ```

  - build 폴더가 삭제된다.

- ```terminal
  ./gradle clean build
  ```

  - 삭제하고 다시 build

