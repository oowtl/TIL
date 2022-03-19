# Web Server and Web Application Server



## 소개



### Web Server

- 개념
  - 클라이언트에서 HTTP 프로토콜로 요청을 받고 정적인 파일들을 응답으로 전달하는 것
    - ` 정적인 파일들을 응답으로 전달 ` : HTML, CSS, JS, 이미지 와 같은 정적인 파일의 내용을 그대로 응답으로 줄 수는 있지만 애플리케이션 코드를 실행해서 결과로 줄 수는 없다.
  - 주된 기능은 콘텐츠를 제공하는 것이지만 클라이언트로부터 콘텐츠를 전달 받는 것도 웹서버의 기능에 속한다.
    - 요청을 WAS 에 보내고 WAS 가 처리한 결과를 클라이언트에게 전달한다.
- 대표제품 
  - nginx, Apache, IIS



### WAS

> Web Application Server



- 개념

  - 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 프레임워크

  - 클라이언트의 요청에 대해서 코드 실행을 통해 동적으로 만들어진 컨텐츠를 반환한다.

- 기능
  대표적인 기능으로서 상황에 따라 달라질 수 있다.
  - 프로그램 실행 환경과 데이터베이스 접속 기능
  - 여러개의 트랜잭션 관리
  - 업무를 처리하는 비즈니스 로직 수행
- 대표제품
  - Phusion passenger, Apache Tomcat, JBoss

- 설명
  - WAS 의 의미는 무엇일까??
    - 여러가지 의미가 있지만 대부분 WAS = Web server + Web container 로 설명한다.



#### Web Container?

- CGI

  > Common Gateway Interface ( 공용 게이트웨이 인터페이스 )

  - Apache 에서 지원하는 개념
  - 두 개 이상의 컴퓨터 간의 자료들을 주고 받는 프로그램 또는 주고 받는 것 자체를 의미한다.
  - PHP, Perl, Python 등의 언어는 Apache 를 통해서 CGI를 적용시키는 것이 가능하지만 JAVA 는 안된다.
    그래서 컨테이너라는 것이 따로 있다.

- 개념

  - 웹 컨테이너는 Java 서블렛과 상호작용하는 WAS 의 구성요소이다.
    WAS 내부에서 개발자 대신 서블릿을 관리한다.
  - 통신을 지원한다.



## Question

### Q1. Web Server vs WAS

- 어떤 타입의 컨텐츠를 제공하는 것인가??
  - 정적인 컨텐츠 = Web server
    동적인 컨텐츠 = WAS
- Web server 와 WAS 는 각각 독립적으로 존재할 수 있다.
- 대부분의 WAS 는 정적인 컨텐츠를 제공해주기 때문에 Web server 없이 WAS 만 존재할 수 있다.
- WAS 는 웹 서버를 포함하는 개념이라고 할 수 있다.



### Q2. Web server! 사용하는 이유는?

1. 기능을 분리하여 서버 부하를 방지한다.
   - WAS 앞에 Web server 를 두는 것으로 Web server 는 정적인 문서만 처리하도록 하고 WAS 는 애플리케이션의 로직만 수행하도록 기능을 분배해서 서버의 부담을 줄인다.
2. WAS 의 환경설정 파일을 외부에 노출시키지 않도록 한다.
   - 클라이언트와 연결하는 포트가 직접 WAS 에 연결이 되어 있다면 중요한 설정파일들이 노출될 수 있다.
   - 웹서버와 WAS 에 접근하는 포트가 다르기 때문에 WAS 에 들어오는 포트에는 방화벽을 쳐서 보안을 강화할 수도 있다.
3. 여러 대의 WAS 를 연결해서 로드 밸런싱 용도로 사용할 수 있다.
   - 대용량 웹 어플리케이션의 경우 Web server 와 WAS를 분리하여 오류가 발생한 WAS 를 사용하지 않고, 다른 WAS fㅡㄹ 사용하게 만드는 것으로 무중단 운영을 가능하게 한다.



## 출처

- https://doozi316.github.io/web/2020/09/13/WEB26/
- https://binux.tistory.com/32
- https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%84%9C%EB%B2%84
- https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98_%EC%84%9C%EB%B2%84

