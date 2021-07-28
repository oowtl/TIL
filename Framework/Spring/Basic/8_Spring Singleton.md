# Spring 8. singleton container

- 객체가 나의 jvm 안에 하나만 있어야 하는 패턴



## 왜 싱글톤일까?

- 과정

  - 대부분의 스프링 애플리케이션은 웹 애플리케이션이다.
  - 웹 애플리케이션은 보통 여러 고객이 동시에 요청을 한다.
    - 고객이 요청을 하면 DI에서 Service 를 사용해서 반환을 한다.
      -> 고객이 요청을 할 때마다 객체를 만들어야 한다!
      -> 이거... 문제가 있을 지도??

- 확인

  ```java
  @Test
  @DisplayName("스프링 없는 순수한 DI 컨테이너")
  void pureContainer() {
    AppConfig appConfig = new AppConfig();
  
    // 1. 조회 : 호출할 때 마다 객체를 생성한다.
    MemberService memberService1 = appConfig.memberService();
  
    // 2. 조회 : 호출할 때 마다 객체를 생성한다.
    MemberService memberService2 = appConfig.memberService();
  
    // 참조 값이 다른 것을 확인한다.
  
    System.out.println("memberService1 = " + memberService1);
    //memberService1 = hello.core.member.MemberServiceImpl@43bc63a3
    System.out.println("memberService2 = " + memberService2);
    //memberService2 = hello.core.member.MemberServiceImpl@702657cc
  }
  ```

  - 각자 생성되는 것이 다르다.

- 시사점

  - 기업 시점에서 이렇게 객체를 생성하는 것은 효율적이지 않을 것이다.
  - AppConfig 는 요청이 들어올 때마다 객체를 생성한다.
    - 그런데 이거! 안에는 훨씬 많은 객체가 있다... -> 비효율적

- 결론

  - 객체를 하나만 생성하고 이것을 공유하도록 설계하면 좀 더 효율적일 수 있을 것이다.
    - 물론 요즘은 가비지 컬렉트라는 것이 좋아서(컴퓨터가 좋아져서) 감당을 할 수 있지만, 효율적인 것이 좋다.