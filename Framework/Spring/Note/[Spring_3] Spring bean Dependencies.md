# Spring 3. Spring bean & Dependencies

- 화면을 붙이고 싶은데??
  - View 템플릿이랑 Controller 가 필요하다.
- 회원가입에 관련해서 html 을 뿌려주고 싶다.
  - 그러면 member Controller 가 필요하고, memberController가 memberService 를 통해서 회원가입을 하고, 데이터를 조회할 수 있어야 한다.
    **이것을 memberController 가 memberService 에 의존한다** 라고 표현한다.



## Controller

- `@Controller`
  - 이 annotation 을  달아주면 spring 이 실행되면서 컨테이너가 생기는데, 거기에서 `controller` 객체가 생긴다. 그리고 spring 이 관리를 한다.
    - 이것을 spring container 에서 spring bean 이 관리된다. 라고 표현한다.



## new 로 생성하는 것에 대해서

- ```java
  @Controller
  public class MemberController {
      private final MemberService memberService = new MemberService();
  }
  ```

  - 이것을 new 로 받는 것이 좋은 것일까??
    - MemberService 는 많은 곳에서 사용하고 있다.
    - 이것을 굳이 다 만들 필요는 없다. 그냥 하나를 공유하는 것이 좋다.
  - 해답
    - 하나를 등록시키는 것으로 공유를 하자.



### 의존성 주입 (Dependency Injection)

- ```java
  @Controller
  public class MemberController {
  
      private final MemberService memberService;
  
      @Autowired
      public MemberController(MemberService memberService) {
          this.memberService = memberService;
      }
  }
  ```

  - `Add contructor`
  - `@Autowired`
    1. `MemberController` 는 spring container 가 뜰 때, 생성된다.
    2. 이 생성자를 호출한다.
    3. `@Autowired`가 있다면, spring 이 `memberService`  과 연결을 시켜준다.



#### @Autowired

- 순서

  1. `MemberController` 는 spring container 가 뜰 때, 생성된다.
  2. 이 생성자를 호출한다.
  3. `@Autowired`가 있다면, spring 이 `memberService`  과 연결을 시켜준다.

- 주의사항 ( 찾을 수 없음!)

  - error

    ```console
    'hello.hellospring.service.MemberService' that could not be found.
    ```

    - 찾을 수가 없다고 한다.

  - 이유

    - 본래 `@Autowired`가 되면 spring 이 서비스가 되면서 controller 에 넣어준다.
    - 그런데 `memberService`는 순수한 java class 이다.
      spring 이 이것의 정체를 알 수가 없다.

  - 해결방법

    - spring 이 알 수 있도록 annotation 을 달아주면 된다!

    - ```java
      // service / MemberService
      
      import org.springframework.stereotype.Service;
      
      @Service
      public class MemberService {
      ```

- 하나의 정형화된 패턴

  - code

    - ```java
      // service / MemberService
      
      import org.springframework.stereotype.Service;
      
      @Service
      public class MemberService {
      ```

    - ```java
      // service / MemoryMemberRepository
      
      import org.springframework.stereotype.Repository;
      
      @Repository
      public class MemoryMemberRepository implements MemberRepository{
      ```

  - 패턴
    - 각 annotation 과 class 의 이름이 동일한 부분이 있다.
    - controller, service, repository 가 정형화된 패턴이다.



### 활용

- `MemberController` - `MemberService` - `MemberRepository`

- 과정

  1. ```java
     @Controller
     public class MemberController {
     
         private final MemberService memberService;
     
         @Autowired
         public MemberController(MemberService memberService) {
             this.memberService = memberService;
         }
     }
     ```

  2. ```java
     @Service
     public class MemberService {
     
         private final MemberRepository memberRepository;
     
         @Autowired
         public MemberService(MemberRepository memberRepository) {
             this.memberRepository = memberRepository;
         }
     ```

  3. ```java
     @Repository
     public class MemoryMemberRepository implements MemberRepository{
     ```

