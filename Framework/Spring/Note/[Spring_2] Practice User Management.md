# Spring 2. Practice User Management



[TOC]

------



## 가정상황

- 데이터 : 회원 ID, 이름
- 기능 : 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않은 상황



### 일반적인 웹 어플리케이션 계층 구조

| 컨트롤러  → | 서비스  ↓ → | 리포지토리 → |  DB  |
| :---------: | :---------: | :----------: | :--: |
|      ↘      | **도메인**  |      ↙       |      |

- 컨트롤러 : 웹 MVC 컨트롤러 역할
- 서비스 : 핵심 비즈니스 로직 구현
  - 비즈니스 도메인 객체를 가지고 핵심 비즈니스 로직을 구현한 계층
- 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비즈니스 도메인 객체
  - 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리된다.



### 클래스 의존관계

| MemberService → | MemberdRepository (interface) |
| :-------------: | :---------------------------: |
|                 | **↑ MemoryMemberRepository**  |

- `MemberService` : 회원 비즈니스 로직
- `MemberdRepository`
  - interface로 설계한다.
  - 데이터 저장소를 선택하면 그대로 할 수 있도록!
- `MemoryMemberRepository`
  - 일단 DB가 없으니까 메모리로 한다.
  - 그 후에 DB를 만들면 바꿔 끼울 것이다. 그러기 위해서 interface로 설계한다.



## 회원 도메인과 리포지토리 만들기



### 도메인 만들기

- `src` > `main` > `java` > `hello.hellospring` > `domain` package 생성

- `Member` class 생성

- ```java
  package hello.hellospring.domain;
  
  public class Member {
  
      private Long id;
      private String name;
  
      public Long getId() {
          return id;
      }
  
      public void setId(Long id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  }
  ```

  - ` private Long id;`
    - 컴퓨터가 지정을 하는 무작위 값을 id 로 한다.



### 리포지토리 만들기

- `src` > `main` > `java` > `hello.hellospring` > `repository` package 생성

- `MemberRepository` interface 생성

- ```java
  package hello.hellospring.repository;
  
  import hello.hellospring.domain.Member;
  
  public interface MemberRepository {
      Member save(Member member);
      Optional<Member> findById(Long id);
      Optional<Member> findByname(String name);
      List<Member> findAll();
  }
  ```

  - `Optional`

    - java8 에 들어간 기능

    - 값을 가져올 때, 값이 없다면 null 일 것이다.

      최근 null을 그대로 반환하는 것이 아니라, Optional 으로 감싸는 것을 좋아한다.

  - `findById()`, `findByname()`

    - 각각을 찾는 것

  - `findAll()`

    - 지금까지 저장한 member 전부를 반환한다.

- 구현체 만들기
  `src` > `main` > `java` > `hello.hellospring` > `repository`  >
  `MemoryMemberRepository` class 생성

- ```java
  package hello.hellospring.repository;
  
  public class MemoryMemberRepository implements MemberRepository{
  }
  ```

  - 여기에서 `alt` + `enter`

  - `implement method`

  - ```java
    package hello.hellospring.repository;
    
    import hello.hellospring.domain.Member;
    
    import java.util.List;
    import java.util.Optional;
    
    public class MemoryMemberRepository implements MemberRepository{
        @Override
        public Member save(Member member) {
            return null;
        }
    
        @Override
        public Optional<Member> findById(Long id) {
            return Optional.empty();
        }
    
        @Override
        public Optional<Member> findByName(String name) {
            return Optional.empty();
        }
    
        @Override
        public List<Member> findAll() {
            return null;
        }
    }
    
    ```

- save 를 하면 어디론가 저장을 해야겠지??

  - ```java
    private static Map<Long, Member> store = new HashMap<>();
    ```

    - 공유되는 변수에는 다른 것을 사용해야하는데, 일단은 `HashMap` 을 사용

  - ```java
    private static long sequence = 0L;
    ```

    - key 값 생성을 위한 것

- save

  - ```java
    @Override
        public Member save(Member member) {
            member.setId(++sequence);
            store.put(member.getId(), member);
            return member;
        }
    ```

    - `name`은 `member` 로 받아오면서 이미 있을 것이다.
    - `member.setId(++sequence);`
      - id 를 셋팅하는 것
    - `store.put(member.getId(), member);`
      - store 에 저장을 하는 것
    - `return member;`
      - 저장한 결과를 반환한다.

- `findById`

  - ```java
    @Override
        public Optional<Member> findById(Long id) {
            return store.get(id);
        }
    ```

    - 이런 식으로 찾으면 되는데, **만약에 없다면?**
      - `null`이 반환될 것이다.
    - 최근에는 `Optional` 으로 감싼다.

  - ```java
    @Override
        public Optional<Member> findById(Long id) {
            return Optional.ofNullable(store.get(id));
        }
    ```

    - 이것을 감싸주면 클라이언트에서 뭔가를 할 수가 있다.

- `findByName`

  - ```java
    @Override
        public Optional<Member> findByName(String name) {
            return store.values().stream()
                    .filter(member -> member.getName().equals(name))
                    .findAny();
        }
    ```

    - store 를 순회한다. (` store.values().stream()`)
    - member 의 이름과 (`member.getName()`) 입력값의 이름(`equals(name)`)이 동일한 것을 찾는다.
      - ` .filter(member -> member.getName().ㄴequals(name))`
    - 하나라도 찾는다면 반환한다. (`.findAny();`)
      - 만약에 끝까지 돌려서 못찾는다면 `Optional` 에 `null`이 감싸져서 반환된다.

- `findAll`

  - ```java
     @Override
        public List<Member> findAll() {
            return new ArrayList<>(store.values());
        }
    ```

    - java 실무에서 Array를 많이 사용한다. (배열 돌리기 편하다.)



## 회원 리포지토리 테스트케이스 만들기

- 테스트할 때는 자바의 main 메서드를 실행하거나, 웹 어플리케이션의 컨트롤러를 통해서 실행한다.
  - 이러한 방법은 준비하는 것이 어렵고, 실행하는 것이 오래걸린다.
  - 자바는 JUnit 이라는 프레임 워클를 가지고 테스트를 실행한다!



### 테스트 만들기

- `src` > `test` > `java` > `hello.hellospring`> `repository` 패키지 생성
- 테스트를 만들 때의 관례 `(테스트 대상 파일명)Test` 파일 생성



#### save test

- ```java
  package hello.hellospring.repository;
  
  import org.junit.jupiter.api.Test;
  
  public class MemoryMemberRepositoryTest {
  
      MemberRepository repository = new MemoryMemberRepository();
  
      @Test
      public void save() {
  
      }
  }
  ```

  - `@Test` : 이거 꼭 해줘야 정상적으로 받아온다.
    - 이 후에 코드 왼쪽 부분에 재생버튼을 해보자.
    - 알아서 테스트를 한다.
  - `MemberRepository repository` 
    - iterface에서 받아오는 것으로 한다.

- ```java
  @Test
      public void save() {
          Member member = new Member();
          member.setName("spring");
  
          repository.save(member);
  
          Member result = repository.findById(member.getId()).get();
      }
  ```

  - `Member result = repository.findById(member.getId()).get();`
    - `get` 으로 꺼내는 것이 알맞지는 않지만, 테스트라서 이렇게 한다!

- 검증하는 과정

  - 내가 넣은 것이 잘 들어갔는지 확인하는 과정

    - ```java
      @Test
          public void save() {
              Member member = new Member();
              member.setName("spring");
      
              repository.save(member);
      
              Member result = repository.findById(member.getId()).get();
      
              System.out.println("result = " + (result == member));
          }
      ```

      - 이런 식으로 검증해도 되긴 하다.

    - Assert 라는 기능이 있다! (`org.junit.jupiter` 가 제공하는 것)

      ```java
      import org.junit.jupiter.api.Assertions;
       
      @Test
          public void save() {
              Member member = new Member();
              member.setName("spring");
      
              repository.save(member);
      
              Member result = repository.findById(member.getId()).get();
      
              Assertions.assertEquals(member, result);
          }
      ```

      - `Assertions.assertEquals(member, result);`

        - 같은 것인지 비교해보는 것이다.

      - 만약에 기대한 값이 아닌 다른 것이 나온다면?

        - ```console
          org.opentest4j.AssertionFailedError: 
          Expected :hello.hellospring.domain.Member@74e52ef6
          Actual   :null
          ```

          - 노랑색 x 표시가 나오면서 error 를 출력해준다.

    - 최근에 주로 사용되는 것
      `Assertions` - `org.assertj` 에서 제공하는 것

      - ```java
        import org.assertj.core.api.Assertions;
        
        @Test
            public void save() {
                Member member = new Member();
                member.setName("spring");
        
                repository.save(member);
        
                Member result = repository.findById(member.getId()).get();
                
                Assertions.assertThat(member).isEqualTo(result);
            }
        ```

        - `Assertions.assertThat(member).isEqualTo(result);`

          - 비교를 하는 내용이고 비교적 잘 확인할 수 있다.

          - `assertThat` 바로 사용하기

            1. `Assertions`에서 `alt` + `enter`

            2. `Add on demand static import` 선택

            3. ```java
               import static org.assertj.core.api.Assertions.*;
               
                   @Test
                   public void save() {
                       Member member = new Member();
                       member.setName("spring");
               
                       repository.save(member);
               
                       Member result = repository.findById(member.getId()).get();
               
                       assertThat(member).isEqualTo(result);
               
                   }
               }
               ```

               - 이런식으로 추가가 되어서 바로 사용할 수 있다.



#### findByName test

- ```java
  @Test
      public void findByName() {
          Member member1 = new Member();
          member1.setName("spring1");
          repository.save(member1);
  
          Member member2 = new Member();
          member2.setName("spring2");
          repository.save(member2);
  
          Member result = repository.findByName("spring1").get();
  
          assertThat(result).isEqualTo(member1);
      }
  ```

  - `Member result = repository.findByName("spring1").get();`
    여기에서 `"spring2"` 와 `member1` 을 비교한다면??

    - ```console
      org.opentest4j.AssertionFailedError: 
      expected: hello.hellospring.domain.Member@4da4253
      but was : hello.hellospring.domain.Member@865dd6
      Expected :hello.hellospring.domain.Member@4da4253
      Actual   :hello.hellospring.domain.Member@865dd6
      ```

      - error 가 난다.



#### findByAll test

- ```java
  @Test
      public void findAll() {
          Member member1 = new Member();
          member1.setName("spring1");
          repository.save(member1);
  
          Member member2 = new Member();
          member2.setName("spring2");
          repository.save(member2);
  
          List<Member> result = repository.findAll();
  
          assertThat(result.size()).isEqualTo(3);
      }
  ```

  - 전체 클래스에서 실행을 하면 `findByName`에서 에러를 낼 것이다.

    - 왜냐하면 각 테스트 클래스들의 테스트 순서는 무작위로 잡히기 때문이다.
      따라서 각 테스트들을 순서와 상관없이 동작하도록 설계되어야 한다.

    - 에러 이유
      - `findAll` 에서 spring1 이 생성되었는데, 다시 생성하라고 해서
    - 해결 방법
      - test 가 끝나면 데이터를 클리어해주는 것을 통해서 깔끔하게 해준다.

- test를 하는 각 method가 실행되고 난 후에 (반환값이 나오던지 method 가 끝나던지) store를 비워준다.

  - `test` > `hello.hellospring` > `repository` > `MemoryMemberRepository`

    - ```java
      public class MemoryMemberRepositoryTest {
      
          MemoryMemberRepository repository = new MemoryMemberRepository();
      //    MemberRepository repository = new MemoryMemberRepository();
      
          @AfterEach
          public void afterEach() {
              repository.clearStore();
          }
      }
      ```

      - `MemoryMemberRepository repository`

        - 일단 test를 하고 싶은 것은  memory 에 있는 것이기 때문에, interface 가 아닌 repository에서 진행한다.

      - ```java
        @AfterEach
            public void afterEach() {
                repository.clearStore();
            }
        ```

        - `@AfterEach`는 각 함수가 실행되고 난 후에 실행하도록 하는 것이다.

  - `src` > `main` > `java` > `hello.hellospring` > `repository`  >`MemoryMemberRepository`

    - ```java
      public void clearStore() {
          store.clear();
      }
      ```

      - `store.clear()`
        - store 에 있는 것을 깔끔하게 지워주는 것



### TDD

- 테스트를 먼저 만들고 구현 클래스를 만드는 것
- 검증을 할 수 있는 도구를 먼저 만들고 그것에 대한 개발을 하는 것



### `test `의 장점

- 전체 class에서 실행할 수 있다.
  한방에 전체를 다 테스트 할 수 있다는 뜻
- 혼자하는 것은 test 없이 가능하지만, 코드가 많아질 수록 하나를 만들면 그것에 대한 error 가 엄청나게 많아진다.





## 회원 서비스 만들기



### 회원가입

- `src` > `main` > `java` > `hello.hellospring` > `service`  패키지 생성

- `MemberService` class 생성

- ```java
  package hello.hellospring.service;
  
  import hello.hellospring.domain.Member;
  import hello.hellospring.repository.MemberRepository;
  import hello.hellospring.repository.MemoryMemberRepository;
  
  public class MemberService {
  
      private MemberRepository memberRepository = new MemoryMemberRepository();
  
      // 회원가입
      public Long join(Member member) {
          memberRepository.save(member);
          return member.getId();
      }
  }
  ```

  - 여기에서 뭔가 이상하지 않나!
  - 같은 이름이 있는 중복회원 X 와 같은 조건을 찾아보자

- `memberRepository.findByName(member.getName());`

  - member 로 들어온 값이 같은 것인지 찾아본다.
  - `ctrl` + `alt` + `v`
    - `Optional<Member> result = memberRepository.findByName(member.getName());`
      - `result` 이것을 수정하면 된다.

- ```java
  // 회원가입
      public Long join(Member member) {
          // 같은 이름이 있는 중복 회원 X
          Optional<Member> result = memberRepository.findByName(member.getName());
          result.ifPresent(m -> {
              throw new IllegalStateException("이미 존재하는 회원입니다.");
          } );
  
          memberRepository.save(member);
          return member.getId();
      }
  ```

  - `result.ifPresent` 
    - 값이 있다면 (null 이 아닌, 값이 있는)
    - `Optional`이라는 것이 있어서 가능한 것
      - 예전에는 if not null 형식으로 간다.
  - `orElseGet()`
    - 이것도 사용할 수는 있다.
    - 있으면 꺼내고 아니면 ~한다.

- 코드를 조금 깔끔하게 다듬어 보자.

  ```java
  // 회원가입
      public Long join(Member member) {
          // 같은 이름이 있는 중복 회원 X
          memberRepository.findByName(member.getName())
                  .ifPresent(m -> {
                      throw new IllegalStateException("이미 존재하는 회원입니다.");
                  } );
  
          memberRepository.save(member);
          return member.getId();
      }
  ```

  - `memberRepository.findByName(member.getName())` 이것을 통해서 나오는 결과값이 이미 `Optional`이기 때문에, 앞에 선언을 할 필요가 없다.

  - 주의할점!

    - 이런 방식으로 findByName 에 관한 로직이 다 있다면 Method 로 추출할 필요가 있다.

    - `ctrl` + `T` or `alt` + `enter`

    - `Extract Method`

    - ```java
      public class MemberService {
      
          private MemberRepository memberRepository = new MemoryMemberRepository();
      
          // 회원가입
          public Long join(Member member) {
              ValidateDuplicatedMember(member); // 증복회원 검증
      
              memberRepository.save(member);
              return member.getId();
          }
      
          private void ValidateDuplicatedMember(Member member) {
              // 같은 이름이 있는 중복 회원 X
              memberRepository.findByName(member.getName())
                      .ifPresent(m -> {
                          throw new IllegalStateException("이미 존재하는 회원입니다.");
                      } );
          }
      }
      ```



### 전체 회원 조회

- 보통 service 에서는 비즈니스와 맞는 용어를 잡고, repository 에서는 개발에 맞는 용어를 사용한다.

- ```java
  // 전체 회원 조회
      public List<Member> findMembers() {
          return memberRepository.findAll();
      }
  ```



### 회원조회

- ```java
  // 회원 조회
  public Optional<Member> findOne(Long memberId) {
      return memberRepository.findById(memberId);
  }
  ```



## 회원 서비스 테스트



### 테스트 쉽게 만들기

- main 에서 작성한 것들을 test로 쉽게 옮기는 방법
- 해당하는 class 에 간다.
- `cmd` + `shift` + `T` or `ctrl` + `shift` + `T`
- `Create New Test`
- 각 테스트 툴 확인
- 만들 함수 체크 (하단에 존재)



### join test



#### test 의 문법?

- given
  - 이러한 상황이 주어져서
  - 이 데이터를 기반으로 하는구나
- when
  - 실행을 했을 때
  - 이것을 검증하는 구나
- then
  - 이러한 결과가 나와야한다.



#### join test code

- ```java
  @Test
      void join() {
          // given
          Member member = new Member();
          member.setName("hello");
  
          //when
          Long saveId = memberService.join(member);
  
          //then
          Member findMember = memberService.findOne(saveId).get();
          assertThat(member.getName()).isEqualTo(findMember.getName());
      }
  ```

  - 이 코드의 단점은 join 의 기능 중에 중복을 방지하는 기능이 있는데, 그것을 활용하지 않는 것이다.
  - `Member findMember = memberService.findOne(saveId).get();`
    - `. get()` :  Optional 을 벗겨주는 역활을 하는 것 같다.



####  duplicated test code

- ```java
  @Test
      public void duplicatedUserException() {
          //given
          Member member1 = new Member();
          member1.setName("spring");
  
          Member member2 = new Member();
          member2.setName("spring");
  
          //when
          memberService.join(member1);
          memberService.join(member2);
      }
  ```

  - member2 를 join 할 때 에러가 발생해야 한다.
    어떻게 해야할까??



##### try / catch

- ```java
  //when
  memberService.join(member1);
  try {
      memberService.join(member2);
  } catch (IllegalStateException e) {
  
  }
  ```

  - 이런 방식으로 try catch 를 통해서 error 를 잡을 수 있다.

- ```java
  //when
  memberService.join(member1);
  try {
      memberService.join(member2);
      // catch 실패
      fail();
  } catch (IllegalStateException e) {
      // 정상적으로 성공
      assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
  }
  ```

  - `memberService.join(member2);` 가 정상적으로 실행된다면?	
    - 코드가 제대로 작동한 것이 아니다.
    - `fail();` 를 통해서 알려준다.
  - `catch (IllegalStateException e){}`
    - 에러가 나와서 catch 가 되는 것이 정상적인 상황이다.
    - 만약 message 가 다른 것이 온다면 실패한다.



##### assertThrows

- ```java
  @Test
      public void duplicatedUserException() {
          //given
          Member member1 = new Member();
          member1.setName("spring");
  
          Member member2 = new Member();
          member2.setName("spring");
  
          //when
          memberService.join(member1);
          assertThrows(IllegalStateException.class, () -> memberService.join(member2));
  
      }
  ```

  - `assertThrows(IllegalStateException.class, () -> memberService.join(member2));`

    - `() -> memberService.join(member2)` 
      - 이 로직을 실행할 것인데!
    - `IllegalStateException`  
      - 이러한 exception 이 터지도록 기대하는 것
      - 기대하는 예외가 터진다면 성공이라고 판단한다.
        - 만약에 `NullPointerException.class` 를 넣으면 실패라고 판단한다.
          기대하는 예외에 대해서만 반응한다.

  - `assserThrows` 는 반환 값이 존재한다.

    - ```java
      @Test
      public void duplicatedUserException() {
          //given
          Member member1 = new Member();
          member1.setName("spring");
      
          Member member2 = new Member();
          member2.setName("spring");
      
          //when
          memberService.join(member1);
          IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
      
          assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
      }
      ```

      - ` assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");`
        - `e`를 통해서 검증을 할 수도 있다.

 

### clear store

- 여기에서도 마찬가지로 store 가 누적되는 것을 통해서 정확한 test 가 힘들어진다.

  - store 를 불러와서 clear store 진행

- ```java
  MemoryMemberRepository memberRepository = new MemoryMemberRepository();
  
  @AfterEach
  public void afterEach() {
      memberRepository.clearStore();
  }
  ```



### store instance

- memberService 에 있는 repository 도 new 로 새로운 instance 를 생성한다.
  memberServiceTest 에 있는 repository 도 new 를 사용한다.
  repository를 두개를 사용할 필요가 있을까??

  - `MemoryMemberRepsitory` 에서

    ```java
    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;
    ```

    `static`이라서 가리키는 것이 같아서 지금은 괜찮다.

    그런데..! `static`이 아니라면??

  - 해결책

    - 하나의 store 를 가르키도록 설정하면 된다.



#### 같은 store 사용하도록 하기 (의존성 주입 DI)

- `main` > `java`  `...` > `service` >`MemberService`

- code 바꾸기

  - ```java
    private final MemberRepository memberRepository = new MemoryMemberRepository();
    ```

  - ```java
    private final MemberRepository memberRepository;
    ```

    - `alt` + `enter` - `Contructor`

  - ```java
    private final MemberRepository memberRepository;
    
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    ```

    - 직접가는 것이 아니라 외부에서 넣어주도록 바꾸는 것이다.

- `test` > `java` > `...` > `service` > `MemberServiceTest`

- code 바꾸기

  - 현상황

    - ```java
      MemberService memberService = new MemberService(memberRepository);
      MemoryMemberRepository memberRepository = new MemoryMemberRepository();
      ```

  - ```java
    MemberService memberService = new MemberService();
    MemoryMemberRepository memberRepository = new MemoryMemberRepository();
    ```

    - 우리는 MemberService 를 생성할 때, MemoryMemberRepository를 직접 넣어줘야 한다.

  - ```java
    MemberService memberService;
    MemoryMemberRepository memberRepository;
    
    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }
    ```

    -  `MemoryMemberRepository` 가 생성된다.
    - `MemberService` 에 `memberRepository` 가 들어간다.

  - ```java
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    ```

    - `memberService`의 입장에서 `memberRepository`는 `new`로 인해서 생성하는 것이 아니라, 외부에서 받는다.
    - 이것을 **의존성 주입(DI)** 이라고 한다.



## Errors



### import 잘하기

- java 는 import 할 것이 많다.
- 수시로 `alt` + `enter` 를 해보면서 import 할 것을 숙련시키자.



### 변수명 한번에 바꾸기

- `shift` + `f6`
  - 한 단락에 있는 것들을 바꾸는 것 같다.



### gradle 실행하는 것 바꾸기 

- 설정 - `build` - `gradle` - `실행`하는 것에 대한 것 `intelliJ` 로 바꾸기