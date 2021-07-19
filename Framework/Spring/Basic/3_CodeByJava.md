# Code By Java



- 염두에 둘 것!
  - 어떻게 인터페이스를 사용하는지 생각해보자.
  - 객체지향 설계가 어떻게 작동하는지 생각해보자.



## 회원 도메인

- 도메인 협력 관계
  - 기획자도 볼 수 있는 그림
- 클래스 다이어그램
  - 도메인 협력관계를 좀 더 자세하게 만든 것
  - 클래스 들만 분석해서 볼 수 있는 것
  - 각각 구현체 그림들은 동적으로, 실행환경에 따라서 다 달라진다.
- 객체 다이어그램
  - 따라서 각 상황에 맞는, 실제로 각 객체가 어떤 것을 바라보는 지에 대한 명시가 필요하다.



- 인터페이스가 아웃풋에 대한 명시니까 그것을 구현하는 과정은 어떤 방식이던지 상관이 없다는 것을 염두해두자.
- 결국 어떤 기능에 대해서 요구하는 것들을 명시적으로 대할 수 있다.



### 구조를 분석해보자

- 인터페이스는 어떤 결과가 필요하다는 것을 명시하는 것이다. 그러니까 이 기능에 대해서는 이러한 아웃풋이 나와야한다는 것이다.
  일단 그 결과가 제대로 나온다면 과정은 어떤 방식으로 되던지 간에 큰 상관이 없다는 것이 이것의 핵심!!

  - 인터페이스에 맞춰서 각 구현 클래스들을 만들어내는 것이다...

- 인터페이스 만으로는 사실 뭔가를 할 수는 없다. 인터페이스를 보면 그냥 결과에 대한 것들만 있으니까

  - 그래서 인터페이스를 받은 구현체가 필요해진다.

  - 인터페이스 자체도 필요하지만 그것에 추가해서 구현체까지 필요하다는 뜻이다.

  - ```java
    MemberService memberService = new MemberServiceImpl();
    ```

    - 이런 방식으로 인터페이스 자체도 필요하고, 구현체도 필요하다는 것이다.
    - 하나만 필요하지는 않다는 것이다.

- 추가적으로는 바라본다는 것이, 별다른 것이 없고 사용한다는 느낌도 있는 것 같다.
  위의 코드에서 멤버 서비스를 사용한다는 것이니까.
  어찌됫든 `public class { ... }` 에 속하는 것일테니 해당 class 에서 사용한다는 느낌인 것 같다.





### test

- 현대 프로그래밍은 test 코드가 필수적이다. 항상 출력으로만 알 수는 없기 때문이다.
- test 의 문법
  - given
    - 무엇이 주어지는 상황인지 체크한다.
  - when
    - 주어진 상황으로 어떻게 할지를 본다.
  - then
    - 무엇을 하고 난 다음의 상황을 체크한다.
- `DisplayName()`
  - test 에서는 한글로 확인할 수 있다.

 

## 좋은 설계

- 단일 체계 원칙이라는 것은 각각을 참조를 하는데 그 참조하는 값이 오로지 참조하는 곳에서만 변경이 되는 것이다.
  - 내가 가격을 산정하는데, 할인되는 가격을 알고 싶다면??
    - 총 가격을 산정하는 곳에서 이리저리 조정하는 것이 아니라 할인에 대한 항목들을 따로, 다른 공간에서 계산을 수행해야 한다는 것이다.
    - 그러면 할인에 대한 정책들이 수정될 때는 그곳에서만 수정하면 되니까 유지보수도 원활하게 할 수 있다.



## `assertThat`

- 이 메서드는 Junit 과 assertJ 라는 곳 두개에서 사용되는 것이다.

  - 이 설명은 assertJ 에 대한 내용이다.

- 두 값(객체)를 비교할 때 주로 사용하는 것이다.

- 구조

  - ```java
    assertThat(T actual).isEqualTo(expected)
    ```

    - 파라미터 : 비교 대상의 실제 값
    - 뒤 : 비교하는 방법과 비교하는 대상



## 의존관계 분석

- ```java
  private final MemberRepository memberRepository = new MemoryMemberRepository();
  ```

  - 이것이 객체지향적으로 잘 구성된 것일까??
  - 아니다!!
    - MemberRepository 인 interface 와 MemoryMemberRepository 인 구현체에 의존을 하고 있는 것이다.
    - 추상에만 의존해야하는데 추상과 구체 둘 다에 의존하는 결과를 초래하게 된다.
  - MemoryMemberRepository 를 바꾸는 순간 memberRepository를 바꿔야한다!

- 어떻게 인터페이스에만 의존할 수 있을까??



## 인터페이스 의존 설계

- 구체 클래스를 지워보자!
  - 지워놓고 보면 nullpointerException 이 날 것이다.
  - 그러면 어떻게 할까??
    - 누군가가 주입을 해줘야 한다!
- 누군가의 역할
  - 공연을 할 때 배우는 연기를 감독은 감독을 하는 것이 좋다. 즉, 목적에 맞는 일만 하도록 하는 것이 중요하다. 각자의 역할에 충실할 수 있도록 환경을 만들어주는 것이 필요하다.



### AppConfig (DI)

- AppConfig 는 실제 동작에 필요한 구현객체를 생성한다.
  생성한 객체 인스턴스의 레퍼런스를 생성자를 통해서 주입(연결) 해준다.
- 이걸 사용하면 모든 구현 객체가 주입되는 것은 AppConfig 에서 맡게 된다.
- 클라이언트인 구현체의 입장에서는 의존관계를 외부에서 주입해주는 것 같다고 해서 의존관계 주입, 의존성 주입이라고 한다. (Dependency Injection)



- ```java
  // AppConfig
  public MemberService memberService() {
    return new MemberServiceImpl(new MemoryMemberRepository());
  }
  ```

  - 여기에서 MemoryMemberRepository 가 있다는 것!

- ```java
  // 구현체
  private final MemberRepository memberRepository;
  
  public MemberServiceImpl(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
  }
  ```

  - 우리는 이제 구현체에 특정한 것을 넣지 않아도 그것을 사용할 수 있게 된다.
  - 이것을 우리는 생성자 주입이라고 한다.
    - 생성자를 통해서 객체에 들어간다고 해서!

- 다수의 구현체를 넣는 것은?

  - ```java
    public OrderService orderService() {
      return new OrderServiceImpl
        (new MemoryMemberRepository(), new FixDiscountPolicy());
    }
    ```

    - 상관없다. 맞춰서 넣어주면 된다.



#### App 실행하기

- ```java
  // APP
  
  AppConfig appConfig = new AppConfig();
  MemberService memberService = appConfig.memberService();
  ```

  - AppConfig 에서 나온 것이라면??

    `return new MemberServiceImpl(new MemoryMemberRepository());`

    - 전부 장착되어진 것이 올 것이다.



#### test 시 주의점

- `@BeforeEach` 사용하기

  - ```java
    MemberService memberService;
    
    @BeforeEach
    public void beforeEach() {
      AppConfig appConfig = new AppConfig();
      memberService = appConfig.memberService();
    }
    ```

    - `@BeforeEach` 로 인해서 각 테스트가 시작될 때마다 memberService 를 만들어서 진행한다.





## Meet Error



###  non-static variable

- `non-static variable cannot be referenced from a static context`

  - static 으로 선언되지 않은 변수는 static 컨텍스트 (함수) 로 부터 참조될 수 없다.
  - static 으로 선언된 함수 내에서 static 으로 선언되지 않은 변수를 사용했다는 오류메세지이다.

- ```java
  OrderService orderService = new OrderServiceImpl();
  
  // Error
  Order order = OrderService.createOrder(memberId, "itemA", 10000);
  
  // Run
  Order order = orderService.createOrder(memberId, "itemA", 10000);
  ```

  - 여기에서 나는 `createOrder`를 하기 위해서 인터페이스에다가 하고 있었다.
  - 선언해준 `orderService` 에 해주는 것이 맞았다.