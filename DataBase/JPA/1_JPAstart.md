# JPA 1. Start



- 데이터베이스 : H2

  - 다운로드 하고 압축 풀고! bin > h2.sh

  - 실행되지 않을 때

    - bin 폴더 경로 `chmod 755 h2.sh `

  - 첫 연결이 안된다면

    - ```text
      jdbc:h2:~/test
      ```

- 관리

  - 메이븐 사용



- 메이븐?
  - pom.xml
    - 여기는 설정 같은 것들이 들어간다.
    - 디펜던시도 여기에서 한다.
- 라이브러리 버전은 어떻게 선택하는 것이 좋을까?
  - spring boot 에서 검색을 하면 각 라이브러리 별 맞는 것이 다 있다.





## JPA 설정하기

- resources / META_INF(생성) / persistence.xml (생성)



### 데이터베이스 방언

- JPA는 특정 데이터베이스에 종속 되지않는다.
- 각각의 데이터베이스에서 표준적이지 않은 것들은 방언이라고 표현한다.
  - 특히 페이징 쿼리가 각 데이터베이스가 다르다.
- SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능!



- 설정하기

  - ```text
    <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
    ```

    - `H2Dialect`
    - `Oracle10gDialect`
    - MYSQLL5innoDBDialect

  - 하이버 네이트는 40가지 이상의 데이터 베이스 방언 지원



## JPA 개발하기



### JPA 구동 방식

1. Persistance 가 persistence.xml 에서 설정 정보를 조회한다.
2. EntityManagerFactory 를 생성한다.
3. EntityManagerFactory 에서 EntityManager 를 생성한다.





- 에러
  - language level 에 관한 에러는 intelliJ - file - projectstructure - language level 변경
  - persistence.xml 을 찾을 수 없다고 했는데, 알고보니 META-INF 라고 해야할 것을 META_INF 라고 이름지었음.







### 객체와 테이블을 생성하고 매핑하기

- `@Entity`
  - 이게 있어야 JPA가 관리를 한다.
- `@Id`
  - id 라는 것을 알려주는 어노테이션
  - 왠만한 것은 javax.persistence 로 간다.

- 트랜잭션
  - jpa 에서 모든 데이터 변경은 트랜잭션이라는 단위 안에서 해야한다.
    - DB는 내부적으로 전부다 트랜잭션이라는 개념을 가진다.
- JPA 를 통해서 엔티티를 가져오면 트랜잭션이 커밋하기 직전에 JPA가 변화를 감지한다.
  변화된 내용에 대해서 JPA 가 쿼리를 날리고 커밋을 하고 종료를 한다.
- EntityFactory는 웹서버가 하나 생기고 쭉 가는 것이다.
- EntityManager 는 쓰레드 간에 공유를 하면 안된다.
  - 사용하고 버리는 것이다.



### JPQL

- 결국 쿼리를 날리기는 해야하는데, 어떻게 해야하나??
- JPA는 JPQL이라는 것으로 보조를 해준다.
- JPA는 객체를 대상으로 쿼리를 날린다.
- 객체를 대상으로 하는 객체 지향 쿼리
- 이걸로 작성하면 알아서 데이터베이스 방언을 처리해준다.
- JPA를 사용하면 엔티티 객체를 중심으로 개발한다.
  - 문제는 검색쿼리 이다..!
- 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색한다면?
- 결국 어플리케이션을 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL dㅣ 필요해진다.
- 차이
  - JPQL : 엔티티 객체를 대상으로 
  - SQL : 데이터 베이스를 대상으로

