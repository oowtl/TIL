# JPA 5. Relation Mapping



- 연관관계를 맺는 방법?



- 용어
  - 방향
    - 단방향
    - 양방향
  - 다중성
    - N:1
    - 1:N
    - N:M
  - 연관관계의 주인
    - JPA 계의 포인터?



## 연관관계는 왜 필요하냐

- 테이블 하나로 다 해결할 수는 없다.
  여러 개의 테이블을 가지고 연결시켜야 할 필요가 있다.

  - 객체지향 설계???

- 우리는 해당하는 테이블의 pk를 주는 것으로 연결시켜줄 수 있다.

  - 그런데 이거... 불편하다!

    ```java
    Member findMember = em.find(Member.class, member.getId());
    Long findTeamId = findMember.getTeamId();
    Team findTeam = em.find(Team.class, findTeamId);
    ```

    - 이걸 보게나!! member 를 통해서 team 을 찾으려면 너무나 불편하게 되어있다.
    - 테이블은 fk 로 연관된 테이블을 찾는데...
      객체는 참조를 사용해서 연관된 객체를 찾는다.
      이거... 뭔가 다르다!!



## 단방향 연관관계

- Code

  ```java
  @ManyToOne
  @JoinColumn(name = "TEAM_ID")
  private Team team;
  ```

  - `@ManyToOne`
    - N : 1 의 관계를 매핑하는 것
    - 지금 작성하는 entity 가 Many, 연결 대상으로 작성하는 것이 One
  - `@JoinColumn(name = "TEAM_ID")`
    - 해당하는 pk 를 연결시켜주기 위해서 적어주는 것이다.

- 수정하기

  - Code

    ```java
    Team newTeam = em.find(Team.class, 100L);
    findMember.setTeam(newTeam);
    ```

    - 찾아서 넣어주면 된다.



