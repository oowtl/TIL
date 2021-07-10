# Spring 1. Web dev Basic



- 웹 개발

  - 정적 컨텐츠
    - 파일을 그대로 내려주는 것
  - MVC, 템플릿 엔진
    - 가장 많이하는 방식
    - 서버에서 프로그래밍을 해서, html 동적으로 바꿔서 내려주는 것

  - API
    -  json 이라는 데이터 포멧으로 운영하는 것
    - data를 내려주면 화면은 클라이언트가 만드는 것
    - 서버끼리 통신을 할 떄





## 정적 컨텐츠

- 순서
  - 웹브라우저가 `localhost:8080/hello-static.html` 요청
  - 내장 톰켓 서버가 요청을 받는다.
  - 요청을 spring 에게 넘긴다.
  - spring 은 `hello-static` 이름을 지닌 컨트롤러를 찾는다.
    - 없다면?
  - `static/hello-static.html` 파일을 찾는다.
    - 찾으면?
  - `hello-static.html`을 반환한다.
- 스프링 부트는 정적 컨텐츠에 관련한 기능을 자동으로 제공한다.
  - 이 파일을 그대로 반환해서 보여주기 때문에 프로그래밍을 할 수는 없다.
  - 활용
    - `src` / `resources` / `static` / `hello-static.html`생성
    - `http://localhost:8080/hello-static.html` 접속



## MVC  와 템플릿 엔진

- MVC

  - Model
    - 비즈니스 로직 관련
  - View
    - 화면을 그리는 것
  - Controller
    - 비즈니스 로직 관련

  - 설명
    - 각각의 역할이 나뉜다.
    - 만약에 MVC 가 하나의 파일에 있다면?? 엄청나게 많은 것들을 관리하는 것이 힘들다!!
      - 그래서 나누어서 관리한다.



### 연습하기

- 컨트롤러 만들기

  - ```java
    package hello.hellospring.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    
    @Controller
    public class helloController {
        @GetMapping("hello-mvc")
        public String helloMvc(@RequestParam("name") String name, Model model) {
            model.addAttribute("name", name);
            return "hello-template";
        }
    }
    ```

    - `@RequestParam("name")`
      - name 이라는 이름으로 파라미터, 매개변수를 받는다.

  - 템플릿 만들기
    `resources` - `templates` - `hello-template.html` 생성

    - ```html
      <!DOCTYPE html>
      <html lang="en" xmlns:th="http://www.thymeleaf.org">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
        <p th:text="'hello ' + ${name}">hello! empty</p>
      </body>
      </html>
      ```

      - `<p th:text="'hello ' + ${name}">hello! empty</p>`
        - 타임리프의 장점
          - 서버가 작동되지 않을 때는 `hello! empty` 가 html 에 출력되고
          - 서버가 작동하면 `th:text="'hello ' + ${name}"`가 출력된다.

#### RequetsParam 사용하기

- 웹 브라우저에 `http://localhost:8080/hello-mvc`입력

  - 에러 발생!!!

  - ```console
    2021-07-10 18:03:15.970  WARN 7244 --- [nio-8080-exec-2] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.bind.MissingServletRequestParameterException: Required request parameter 'name' for method parameter type String is not present]
    
    ```

    - 파라미터가 없다...!
      RequestParam 이라는 것을 통해서 파라미터를 받으려고 하는데, 입력이 안되니까 에러를 발생시킨다.

- 옵션 설정하기

  - ```java
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
    ```

    - `name` 에서 파라미터 정보 보기 옵션
      - `commad + p` , `ctrl + p`
      - 여기에서 확인을 하는 것이다.
      - `required` 가 `true` 라면 반드시 있어야 하는 파라미터이다.
        없으면 안되는 것!

- 주소 입력하기

  - `http://localhost:8080/hello-mvc?name=spring!`
    - 파라미터 옵션에서 확인한 것에 따르면 name 이라는 것은 `required = true` 라서 반드시 있어야 한다.
      따라서 `?name=spring!`으로 해주는 것을 통해서 파라미터를 넣어준다.



### MVC, 템플릿 엔진 처리방식

1. 웹 브라우저에서 `http://localhost:8080/hello-mvc?name=spring!` 을 요청한다.
2. 내장 톰켓 서버에서 감지하고 스프링에게 물어본다.
   '야 이거 있냐??'
3. 스프링 컨테이너에서 `hello Controller`를 찾는다.
4. `helloController` 에서 
   return 값으로 hello-template 를 내놓고,
   파라미터로 받은 spring! 을 넘겨준다.
   - 어디에?? viewResolver 에!
5. viewResolver는 받은 것을 토대로 hello-template.html 을 찾는다.
   찾은 것을 토대로 Thymeleaf 템플릿 엔진에 처리를 하도록 한다.
   - viewResolver 는 뷰를 찾아주고 템플릿 엔진을 연결시켜준다.
6. 처리한, 변환한 HTML 을 웹 브라우저에 날려준다.





## API

- html 을 내리냐, 아니면 데이터를 내리냐
- View 가 없다는 것이 특징 데이터 그대로 내려간다.



### 연습하기



#### 문자

- ```java
  @GetMapping("hello-string")
  @ResponseBody
  public String helloString(@RequestParam("name") Stinrg name) {
      return "hello " + name;
  ```
  - `@ResponseBody`
    - HTTP 규약 내에서의 Body 부분에 ` return "hello " + name;` 이 데이터를 직접 넣어주겠다는 뜻



#### 데이터

- ```java
  @GetMapping("hello-api")
      @ResponseBody
      public Hello helloApi(@RequestParam("name") String name) {
          Hello hello = new Hello();
          hello.setName(name);
          return hello;
      }
  
      static class Hello {
          private String name;
  
          public String getName() {
              return name;
          }
  
          public void setName(String name) {
              this.name = name;
          }
      }
  ```

  - 먼저 class 를 생성해준다.
  - class 에서 getter setter 를 생성해준다.
    - alt + insert 를 눌러보면 생성할 수 있는 것이 나와있다.
      - `cmd` + `n`
    - `setName` : 들어올 때 사용하는 것
    - `getName` : 내보낼 때 사용하는 것
  - class 를 활용해서 name 을 받아서 사용한다.
  
- `http://localhost:8080/hello-api?name=spring!!!`

  - `{"name":"spring!!!"}`
  - JSON 형태로 출력된다.



### ResponseBody의 동작과정

1. `http://localhost:8080/hello-api?name=spring!!!` 웹브라우저가 요청
2. 톰켓이 감지하고 spring 으로 보냄
3. `helloController` 가 `hello-api`발견함
4. `hello-api`에서 `@ResponseBody` 를 발견하게 된다.
   - 데이터를 그대로 넘겨야한다고 판단한다.
5. HttpMessageConverter 가 동작한다. (viewResolver 대신에 동작한다.)
   - 문자인지 객체인지 판단한다.
     - JsonConverter 
       - `MappingJackson2HttpMessageConverter`
         - `jackson`  : 객체를 json 으로 바꿔주는 라이브러리 (spring 기본탑재)
     - StringConverter
       - `StringHttpMessageConverter`
   - 만약에 객체라면 JSON으로 만들어서 반환하겠다
     (기본정책)
6. JSON 으로 요청한 곳에 응답을 한다.



### Getter & Setter

- 자바 빈 규약

  - ```java
     static class Hello {
            private String name;
    ```

    - private 이라서 바로 접근할 수 없다. 따라서 method 를 통해서 접근해야 한다.

  - 이러한 규약에 의해서 값에 접근하기 위해서는 getter 와 setter 를 마련해야할 필요성이 있다.

### JSON

- html 방식은 태그를 여닫는 것도 있고 여러 이유로 인해서 조금 무겁다.
- 최근에는 json 방식으로 한다.
- `@ResponseBody`를 설정한다면 JSON 으로 가는 것이 기본으로 설정되어있다.



## Tips

### 자동완성기능

- `cmd` + `shift` + `enter`
- `ctrl` + `shift` + `enter`

