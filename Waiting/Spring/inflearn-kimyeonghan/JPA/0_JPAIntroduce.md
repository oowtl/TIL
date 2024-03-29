# 0. Introduce JPA



- 쿼리를 사용하는 것없이 JPA를 사용하는 것으로 데이터베이스를 사용한다.
- 개발속도와 유지보수 측면에서 확실하게 좋다.



- 그런데 이거... 왜 어려워???
  - 예제들은 보통 테이블이 한 두개로 단순하다.
  - 실무는 수십 개 이상의 복잡한 객체과 테이블을 사용한다.
- 객체와 테이블을 올바르게 맵핑하는 방법이 존재한다.
  - 여기에서 좀 힘들어진다.
- 어떠한 복잡한 시스템도 JPA로 설계가 가능하다.



- JPA의 내부 동작 방식을 이해하는 것이 중요하다.
  - 코드 한줄로 불러오는 것이 쉬워보이지만, 동작방식을 이해하는 것이 중요하다.
  - 이걸 이해해야만 에러와 각종 대체가 가능해야한다.
  - **JPA가 어떤 SQL을 만들어 내는지 이해하는 것이 필요하며**
    **JPA가 언제 SQL을 실행하는지 이해하는 것이 필요하다.**





## Why JPA??

- 우리의 앱은 거의다 SQL 기반으로 돌아간다.
  - 문제는 뭘 만들던지 무한 CRUD를 돌려야한다는 것이다.
- 자자 생각해보자
  - SQL 을 일일이 다하면 뭘 하나 추가하면 그것에 대한 쿼리문을 전부 다 수정해야하는 불상사가 생긴다.
    이거... 보통 일이 아니다!
    - 그런데 방법이 없었다...
- 패러다임의 불일치?
  - 관계형 데이터베이스 = 데이터를 잘 정규화해서 보관하는 것이 목적
  - 객체 = 한번에 잘 감싸서, 캡슐화 해서 쓰는 것이 목적
- 그런데 이거...대안이 없다! 관계형 데이터베이스가 너무나 좋구나...
  - 결국 객체를 SQL로 변환하는 것이 필요한데, 그것을 누가하느냐...... 개발자가 한다.



- 객체 상속 관계를 어떻게 테이블로 구현해야할까??
  - Table 의 슈퍼타입 서브타입 관계로 좀 유사하게 할 수 있다.



- 테이블에서 1개의 테이블을 3개의 테이블이 상속하는 경우에 각각을 조회해야하는 상황을 상상해보자!
  - 쿼리문을 엄청나게 사용해야한다.
  - 3개의 테이블 각각을 참조할 수 있도록 1개의 테이블에 작성해줘야한다.
  - 그런데 자바 컬렉션 같은 것을 생각하면?? 객체를 생각하면 그냥 `get` 같은 것을 사용하면 된다.



- 연관관계 표현
  - 객체는 참조하는 것을 사용한다.
    - 물론 객체는 단방향성 연결이라서 연결된 방향으로만 갈 수 있다.
  - 테이블은 외래 키를 사용한다.
    - 들고와서 join 하면 된다.
    - 테이블은 연결이 되면, 양방향에서 연결될 수 있다.



- 여러 테이블이 연결되면 상당히 복잡해진다.
  차라리 큰 테이블 하나를 생성해서 거기에 다 때려넣는 것이 더 좋을지도 모른다.



- SQL의 단점
  - SQL 은 여러 곳을 탐색할 수 없다.
    - SQL은 처음 실행할 때부터 탐색의 범위가 결정된다.
      객체처럼 여러곳을 다닐 수 없다.
  - 엔티티 신뢰에 대한 문제가 있다.
    - 타인이 작성한 코드가 어떤 쿼리문으로 작동되는 지 알 수 없기 때문에, 일일이 다 봐야한다.
    - 신뢰성의 문제가 생긴다.
  - 모든 객체를 만들 수가 없다.
    - 필요한 것을 조회하기 위해서 사용할 때마다 다른 메서드를 작성하는 것은 비효율적이다.



- 객체스럽게 데이터베이스를 꾸려갈수록 할 일만 많아진다는 단점이 있다.
- 그러한 것을 계속 연구해온 결과로 JPA 가 탄생한다.





## What JPA?

- java 진영의 orm 기술 표준

### ORM

- 객체 관계 매핑
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스 대로 설계한다.
- 나머지는 ORM 프레임 워크가 중간에서 매핑한다.



- 어플리케이션과 JDBC 의 사이에서 동작한다.



### 저장 동작

- 어떤 Entity 객체를 넘겨 받으면..

  - Entity 를 분석해서 적절한 insert sql 을 생성하고 jdbc api 를 사용한다.

  - 가장 중요한 것은 패러다임의 불일치를 해결해준다는 것이다.



### 조회 동작

- pk 를 넘겨받고 해당 객체를 본다.
  - select 쿼리를 만들고 jdbc api 를 사용해서 통신한다.
  - ResultSet을 매핑한다.
  - 패러다임의 불일치를 해결해준다.



- JPA 자체는 인터페이스의 모음이다.
  - 별다른 것 없다. 전부 인터페이스다!



## How JPA?



### CRUD

- 저장

  - ```java
    jpa.persist(member)
    ```

- 조회

  - ```java
    Member member = jpa.find(memberId)
    ```

- 수정

  - ```java
    member.setName("변경할 이름")
    ```

    - jpa 는 마치 자바 컬렉션에 데이터를 넣고 빼는 것과 같도록 설게된 것이다.

- 삭제

  - ```java
    jpa.remove(member)
    ```





### 유지보수가 용이하다!

- 필드를 선언하기만 하면 된다.
- 쿼리문을 안써도 된다.



### 패러다임의 불일치를 해결해준다.

- 상속
  - 저장의 경우에서 jpa 저장을 하라고 하면 알아서 연결된 데이터베이스에 저장을 해준다.
  - 조회의 경우에서도 알아서 찾아서 처리해준다.
- 객체가 연결되는 것처럼 jpa가 연결해준다.
  - 지연로딩
    - 해당하는 데이터가 실제로 사용되는 시점에 sql를 가져오는 기능
- jpa은 같은 트랜잭션에서 조회한 데이터는 같음을 보장해준다.



### JPA의 성능 최적화 기능

- 계층 사이에 중간에 어떤 것이 있다면, 모아서 쏘는 버퍼링과 읽을 때 캐싱하는 것을 할 수 있다..!
  - 단순한 것 SQL 보다 성능을 더 끌어올릴 수 있다.



#### 1차 캐시와 동일성보장

- 같은 트랜잭션 안에서 같은 엔티티 반환
- 동일한 DB 에서 동일한 pk로 접근할 때 캐싱을 하는 것으로 sql 까지 않가고 바로 간다.



#### 버퍼링

- 비슷한 쿼리문이 있으면 모아서 한번에 쏜다.
- 데이터베이스에서 트랜잭션 커밋을 하기 전까지만 쏘면 된다.
  그래서 모았다가 커밋이 되기 전에 한번에 보내는 것이다.



#### 즉시 로딩과 지연 로딩

- JPA에서 중요한 부분
- 지연로딩
  - 다른 데이터베이스의 값을 처음부터 가져오지 않고, 실제로 참조해야할 때 가져온다.
  - 쿼리가 두 번 나간다는 단점이 있다.
- 즉시로딩
  - JPA의 옵션이다.
  - 해당하는 데이터베이스를 참조할 때 항상 쓰이는 다른 데이터베이스가 있다면, 그것을 지정하는 것,
    이 데이터 베이스를 사용하면 이건 무조건 같이 불러와줘
- JPA는 옵션으로 이것을 껏다가 켯다가 바로바로 할 수 있다.
  - 너무나 손쉽게 DB를 운영할 수 있다.
- 방법
  - 전부 지연로딩으로 하고 필요한 것에 따라서 최적화를 하는 것도 하나의 방법





- 물론 중요한 것은 RDB이다.
  - 이것에 대한 공부가 중요하다.