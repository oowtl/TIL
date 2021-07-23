# JPA 4. Entity Mapping



## 객체와 테이블 매핑

- 객체 : `@Entity`
- 테이블 : `@Table`
- 필드, 컬럼 : `@Column`
- 기본 키 : `@Id`
- 연관관계 : `@ManyToOne`, `@JoinColumn`



### `@Entity`

- 특징
  - JPA가 관리하는 것
- 주의
  - 기본 생성자가 있어야 한다.
  - final, enum, interface, inner 사용 금지
- 속성
  - name
    - 기본값으로 class 의 이름으로 간다.



###  `@Table`

- 엔티티와 매핑할 테이블을 지정하는 것
  - 데이터베이스의 명을 name 으로 설정하면 해당하는 이름을 지닌 데이터베이스와 맵핑해준다.



## 데이터 베이스 스키마 자동 생성

- 기능 

  - DDL 을 애플리케이션 로딩 시점에 자동으로 생성하는 기능
    - JPA 는 애플리케이션이 뜰 때, 필요한 것을 다 생성해준다.
  - 데이터베이스 방언을 활용해서 적절한 DDL을 생성시켜준다.
  - 물론 이렇게 생성된 DDL은 개발단계에서만 사용한다.
    - 절대 운영에서는 사용하지 말 것!

- 사용하기

  - ```properties
    <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
    ```

    - create
      - 기존 테이블 drop 후 create
      - 개발 초기 단계
    - create-drop
      - drop 후 create를 한다. 그리고 애플리케이션이 종료되면 다시 drop
      - 개발 초기 단계
    - update
      - 변경된 것만 반영하는 것
      - 지우는 것은 안된다! 추가하는 것만 된다.
      - 테스트 서버 (데이터가 추가만 된다!)
    - validate
      - 엔티티, 테이블이 정상 매핑되었는지 확인한다.
      - 테스트 서버, 스테이징, 운영서버
    - none
      - 이 기능 자체를 사용하지 않는 것
      - 매칭되는 것이 없으니 실행되지 않는다.
      - 테스트 서버, 스테이징, 운영서버

- 주의할 점
  - 사용하지 않는 것을 추천한다.
    - DB 자체를 아주 잘 아는 것으로 직접 하는 것도 좋지만, 이게 의도대로 잘 되지 않는다면 운영하는 서비스가 멈추는 큰 일이 벌어진다.
      그러므로 가급적이면 사용하지 않는 것을 추천한다.
      - alter table 같은 것도 잘못사용하면 테이블 락이 걸려서 잘 사용해야한다.



### DDL 생성 기능

- 제약조건 추가하기
  - `@Column(unique=true, length=10)`
    - `length=10` : 10글자 제한
- 특징
  - DDL 생성기능은 DDL을 자동생성할 때만 사용된다.
  - JPA 실행 로직에는 영향을 주지 않는다.





## 필드와 컬럼 매핑



### 매핑 어노테이션

- `@Column`
- `@Temporal`
- `@Enumerated`
- `@Lob`
- `@Transient`
  - 특정 컬럼을 DB 와 연결짓지 않는 것
  - 나는 이거 메모리에서만 사용할거야!



#### `@Column`

- name : 테이블의 컬럼 이름
- insertable, updatable
  - 등록하거나 업데이트를 할 때, 컬럼을 변경할 것인가!
- nullable
  - 기본값 true
  - `nullable=false` -> not null
- unique
  - 이 친구는 이름이 랜덤으로 나와서 사용하기 힘들다.
  - entity 에 직접 주는 방법이 있다.
- length
  - 문자열 길이 조건을 부여한다.
  - String 에만 가능하다.
- precision, scale
  - bigint, BigDecimal 타입에서 사용한다.
  - 자릿수를 정한다.



#### `@Enumerated`

- 설명
  - enum 타입은 DB 에 없지만, 설정하는 것으로 사용할 수 있다.
- value
  - `EnumType.ORDINAL` 
    - enum 의 순서를 데이터 베이스에 저장한다.
      - 이거는 순서가 바뀌면 바뀌는 대로 저장하기 때문에, 값을 수정하면 안된다.
      - 굉장히 위험한 것이다. 따라서 안쓰는 것이 좋다...
  - `EnumType.STRING`
    - enum 의 이름을 데이터 베이스에 저장한다. 



#### `@Temporal`

- 이거는 조금 구식일지도 몰라

- 우리에게는 

  ```java
  private LocalDate testLocalDate;
  private LocalDateTime testLocalDateTime;
  ```

  이것이 있지..!



#### `@Lob`

- varchar 보다 긴 것을 넣고 싶을 때 사용한다.
- 이거는 속성을 지정하는 것이 아니다.
- 매핑하는 필드 타입에 따라서 다르게 매핑된다.
  - 문자 : CLOB
  - 나머지 : BLOB



#### `@Transient`

- 이것은 아무것도 매핑하고 싶지 않을 때, 사용하는 것이다.



## 기본 키 매핑



### `@id`

- 직접 할당

- 자동 할당(`@GeneratedValue`)

  - 사용 

    - ```java
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      ```

  - 전략

    1. `GenerationType.AUTO`

       - DB 방언에 맞게 자동으로 설정한다.
       - 조금씩 다르게 나와서 확인해보고 사용해야 한다.

    2. `GenerationType.IDENTITY`

       - 기본키 생성을 DB에 위임하는 것
       - mysql auto_increment
       - 애매한 점!
         - null 로 들어오면 그 때 셋팅을 해준다.
           -> DB 에 들어가야 그 값을 알 수 있다.
           - 커밋 시점에 알 수 있다는 것이다.
             그래서 보통 회원가입 같은 생성을 하는 것은 값을 설정하고 persist 를 해주는 것으로 시점의 차이를 없앤다.

    3. `GenerationType.SEQUENCE`

       - 오라클에서 주로 사용

       - 과정

         - `call next value for hibernate_sequence`

           - 이게 기본적인 것이라서 이 친구를 통해서 값을 가져와서 한다.

         - ```java
           @Entity
           @SequenceGenerator(name = "member_seq_generator", sequenceName = "member_seq")
           public class Member {
           
             @Id
             @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "member_seq_generator")
             private Long id;
           
           ```

           - 이름을 지어줄 수도 있다.

       - code

         - ```java
           @Entity
           @SequenceGenerator(
                   name = "MEMBER_SEQ_GENERATOR",
                   sequenceName = "MEMBER_SEQ", //매핑할 DB 시퀀스 이름
                   initialValue = 1,
                   allocationSize = 1)
           public class Member {
               @Id
               @GeneratedValue(
                       strategy = GenerationType.SEQUENCE,
                       generator = "MEMBER_SEQ_GENERATOR")
               private Long id;
           ```

           - `allocationSize`
             - default = 50
             - 호출할 때마다 DB 를 많이 불러와서 사용한다.
             - 1번 을 호출하면 50까지 넣어준다. 그리고 51 을 요청하면 100까지 넣는다.

- 왠만하면 Long 을 사용하는 것이 좋다.

  - integer 와 long 의 성능의 차이는 거의 영향을 주지 않는다.
  - 10억이 넘어갈 때, 타입을 바꾸는 것이 더 힘드니까 그냥 처음부터 Long 으로 하자.



### TABLE 전략

- sequence 전략과 유사한 기능인데, 여러 DB에서 지원하는 것이다.
- 단, 이것은 DB를 건드리는 것이라서 잘 사용하지는 않는다.



### 권장하는 식별자 전략

- 기본 키에 대한 제약조건
  - null 이면 안된다.
  - 유일해야한다.
  - 변하면 안된다.
    - 먼 미래까지 변하면 안된다는 것이 포인트이다.
    - 어떻게 할 수 있을까?
      - 대리키, 대체키를 사용하자.
      - 심지어 주민등록번호도 기본 키로 적절하지 않다!
    - 권장 방법
      - Long 타입 + 대체 키 + 키 생성 전략 사용
        - Auto increment
        - sequence object
        - 랜덤!
    - 절대 비즈니스에 키가 들어가면 안된다.





## Code!



### 컬럼 매핑시 주의점

- 이름짓기
  - java 에서는 memberId, orderId 이런 식으로 지어주지만,
    DB에서는 member_id, MEMBER_ID 이런 식으로 지어준다.
    - spring boot 는 memberId 를 member_id 로 가져가는 옵션이 디폴트이다.
  - 회사에서는 보통 관례로 가는 편이다.
- 제약조건
  - 어노테이션으로 할 수 있는 것이면 가급적이면 달아주는 것이 좋다.
  - 바로 볼 수 있으니까?



### 데이터 중심 설계의 문제

- 객체 설계를 테이블 설계에 맞춘 것은 테이블의 외래키를 객체에 그대로 가져온다.
- 객체 그래프 탐색이 불가능하다.
- 자... 외래키가 아니라 객체를 그대로 가져와서 조회할 수 있다면 좋을 것이다.
