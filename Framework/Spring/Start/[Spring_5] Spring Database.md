# Spring 5. spring database



## H2 database 설치하기

- [다운로드 링크](https://www.h2database.com/html/main.html)
- `h2 폴더` / `bin`
  - 윈도우 : `h2.bat`
  - mac : `h2.sh`
- uri
  - 반드시 sessionid 는 유지해야한다.
  - ip:8082 을 localhost:8082 로 바꿔줄 수 있다.



### DB 생성하기

- 데이터베이스 파일 생성
  - JDBC URL
    - `jdbc:h2:~/test `
      -  최초
    - `jdbc:h2:tcp://localhost/~/test`
      - 이후



### Table 생성하기

- ```h2
  create table member
  (
   id bigint generated by default as identity,
   name varchar(255),
   primary key (id)
  );
  ```

  - `generated by default as identity`
    - null 값이 들어오면 db 가 알아서 채워준다.





## 순수 JDBC



### 셋팅

- `build.gradle`

  - ```text
    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    runtimeOnly 'com.h2database:h2'
    ```

    - 추가해준다.

- db 서버 추가
  `application.properties`

  - ```text
    spring.datasource.url=jdbc:h2:tcp://localhost/~/test
    spring.datasource.driver-class-name=org.h2.Driver
    spring.datasource.username=sa
    ```

- 이렇게 하면 spring 이 알아서 다한다.
- `gradle refresh`를 반드시 해야한다.



### DB 붙이기

- 기존에 MemoryMemberRepository 를 바꿔 줘야한다.
- jdbc 는 엄청나게 많은 것들을 구현해야 한다.
- 그냥 편하게 넘어가고 필요하면 찾는다.





#### SpringConfig

- `SpringConfig`

  - ```java
    @Configuration
    public class SpringConfig {
    
        private DataSource dataSource;
        
        public SpringConfig(DataSource dataSource) {
            this.dataSource = dataSource;
        }
    
        @Bean
        public MemberService memberService() {
            return new MemberService(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository()
        {
            return new JdbcMemberRepository(dataSource);
        }
    }
    ```

    - 주의사항 : private 로 선언했으면 접근할 수 있도록 연결고리를 준다.

      - ```java
        public SpringConfig(DataSource dataSource) {
            this.dataSource = dataSource;
        }
        
        ```



### spring 에서의 repository 설정

- spring 컨테이너가 repository를 바꾸는 것을 쉽게 해준다.
- OCP - 개방 폐쇄원칙
  - 확장에는 열려있고, 수정, 변경에는 닫혀있다.
    - 객체의 다형성을 잘 이용하면 바꾸면서 수정할 필요는 없도록 할 수 있다.
  - repository를 바꾸는 것을 쉽도록 하는 것
- 스프링의 DI를 사용하면 기존 코드를 전혀 손대지 않고 설정만으로 구현 클래스를 변경할 수 있다.
  - injection 은 spring 이 쫙쫙 넣어준다.





## 스프링 Jdbc Template

- JdbcTemplate 와 MyBatis 같은 라이브러리는 JDBC API 에서 본 반복 코드를 대부분 제거한다.
- 단, SQL 은 직접 작성해야한다.





### code

- ```java
  package hello.hellospring.repository;
  
  import hello.hellospring.domain.Member;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.jdbc.core.JdbcTemplate;
  import org.springframework.jdbc.core.RowMapper;
  
  import javax.sql.DataSource;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.List;
  import java.util.Optional;
  
  public class JdbcTemplateMemberRepository implements MemberRepository {
  
      private final JdbcTemplate jdbcTemplate;
  
      @Autowired // 하나만 있으면 생략 가능
      public JdbcTemplateMemberRepository(DataSource dataSource) {
          this.jdbcTemplate = new JdbcTemplate(dataSource);
      }
  
  
      @Override
  
      public Member save(Member member) {
          return null;
      }
  
      @Override
      public Optional<Member> findById(Long id) {
          List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper());
          return result.stream().findAny();
      }
  
      @Override
      public Optional<Member> findByName(String name) {
          return Optional.empty();
      }
  
      @Override
      public List<Member> findAll() {
          return null;
      }
  
      private RowMapper<Member> memberRowMapper() {
          return (rs, rowNum) -> {
              Member member = new Member();
              member.setId(rs.getLong("id"));
              member.setName(rs.getString("name"));
              return member;
          };
      }
  }
  ```

  - ```java
    @Override
        public Optional<Member> findById(Long id) {
            List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper());
            return result.stream().findAny();
        }
    ```

    - jdbc 보다는 훨씬 짧고 간단하게 사용할 수 있다.



#### save 분석

- ```java
  @Override
   public Member save(Member member) {
       SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
       jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
       
       Map<String, Object> parameters = new HashMap<>();
       parameters.put("name", member.getName());
       
       Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));   
       member.setId(key.longValue());
       return member;
   }
  ```

  - ```java
    SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
    jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
    
    Map<String, Object> parameters = new HashMap<>();
    parameters.put("name", member.getName());
    ```

    - insert 구문을 만들어준다.

  - ```java
     Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));   
    ```

    - key 를 받는다.

  - ```java
    member.setId(key.longValue());
    ```

    - 넣어준다.



## JPA

- 반복코드 줄인다. JPA 가 SQL 도 직접 만들어서 실행해준다.
- SQL과 데이터 중심 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있다.



### 셋팅

- `build.gradle`

  - ```text
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // jpa 추가
    ```

    - grdle refresh!!

- `application.properties`

  - ```text
    spring.jpa.show-sql=true
    spring.jpa.hibernate.ddl-auto=none
    ```



### ORM

- object relation mapping
- mapping -> annotation



### Code



#### Member class

- `domain` / `Member`

  - ```java
    package hello.hellospring.domain;
    
    import javax.persistence.*;
    
    @Entity
    public class Member {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @Column(name = "username")
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

    - `@Entity`
      - jpa 가 관리하는 Entity이다.
    - `@id`
      - pk
    - `@GeneratedValue(strategy = GenerationType.IDENTITY)`
      - db 가 알아서 pk 값을 설정할 수 있도록 한다.
    - `@Column(name = "username")`
      - column 명이 username 으로 mapping 이 된다.



#### JpaMemberRepository

- `repository` / `JpaMemberRepository`

  - ```java
    package hello.hellospring.repository;
    
    import hello.hellospring.domain.Member;
    
    import javax.persistence.EntityManager;
    import java.util.List;
    import java.util.Optional;
    
    public class JpaMemberRepository implements MemberRepository{
    
        private final EntityManager em;
    
        public JpaMemberRepository(EntityManager em) {
            this.em = em;
        }
    
        @Override
        public Member save(Member member) {
            em.persist(member);
            return member;
        }
    
       @Override
        public Optional<Member> findById(Long id) {
            Member member = em.find(Member.class, id);
            return Optional.ofNullable(member);
        }
    
        @Override
        public Optional<Member> findByName(String name) {
            List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                    .setParameter("name", name)
                    .getResultList();
            return result.stream().findAny();
        }
    
        @Override
        public List<Member> findAll() {
            return em.createQuery("select m from Member m", Member.class)
                    .getResultList();
        }
    }
    ```

    - jpa 는 EntityManager 로 동작을 한다.
      - data-jpa 라이브러리를 받아서 사용하면 spring ㅠboot 가 자동으로  EntityManager 를 생성을 한다.
        우리는 그것을 injection 받으면 된다.
      - 결론 :  JPA 를 사용하기 위해서는 EntityManager 를 주입받아야 한다.

    - ```java
      @Override
      public Member save(Member member) {
          em.persist(member);
          return member;
      }
      ```

      - 저장하는 것...
      - 단, return 값이 없다는 것

    - ```java
      @Override
      public List<Member> findAll() {
          return em.createQuery("select m from Member m", Member.class)
              .getResultList();
      }
      ```

      - `"select m from Member m"` 
        - JPQL 이라는 것
        - 객체에다가 쿼리를 날린다.
        - ` Member m` Member 를 m 이라고 한다.
        - 
      - `select m`
        - member 자체, 그 객체를 대상으로 한다.
        - 이미 mapping 이 다되어이 싿.

    

### Transactional

- 데이터를 변경하거나 저장할 때는 항상 transaction 이 있어야 한다.

- `service` / `MemberService`

  - ```java
    import org.springframework.transaction.annotation.Transactional;
    
    @Transactional
    public class MemberService {
    
    ```

- 모든 변경은 Transaction 안에서 실행되어야 한다.





### Bean

- `SpringConfig`

  - ```java
    @Configuration
    public class SpringConfig {
    
        private EntityManager em;
    
        public SpringConfig(EntityManager em) {
            this.em = em;
        }
    
        @Bean
        public MemberService memberService() {
            return new MemberService(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository()
        {
            return new JpaMemberRepository(em);
        }
    ```
