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



## 양방향 연관관계와 연관관계의 주인

- 객체는 참조를 사용하고 테이블은 외래키를 가지고 join 을 사용한다.



### 양방향 매핑

- 반대 방향과 연결한다는 것
- 두 테이블이 각각이 다른 테이블로 갈 수 있도록 하는 것
- 양방향 매핑을 한다고 해서 테이블이 바뀌지는 않는다.
  - 객체 연관관계는 List 를 통해서 받아야 하는데, 테이블은 그냥 외래키를 받으면 된다.



- List 로 넣어줄 때의 관례

  ```java
  private List<Member> members = new ArrayList<>();
  ```

  - null 이 안나서 이렇게 해준다고 한다. (처음 시작할 때)



#### `@OneToMany`

```java
@OneToMany(mappedBy = "team")
private List<Member> members = new ArrayList<>();
```



#### `mappedBy`

- 객체와 테이블이 관계를 맺는 차이가 있다.
  - 객체연관관계
    - 회원 -> 팀, 팀 -> 회원
    - 참조가 되는 것이 두개가 있어야 한다.
  - 테이블 연관관계
    - pk 와 fk 만으로도 바로 알 수 있다.
    - 연관관계가 하나만 있으면 다 알 수 있다는 것
- 양방향 관계 비교
  - 객체의 양방향 관계
    - 객체에서는 사실 양방향이 아니라 서로 다른 단방향 관계 2개로 양방향을 구현한 것이다.
  - 테이블의 양방향 관계
    - 테이블은 외래키 하나로 두 테이블을 관리한다.
      - 외래키 값 하나만 있으면 양방향 연관관계를 가진다. 양쪽으로 조인 가능
- 외래 키 관리
  - 객체 vs 테이블
  - 연관관계가 있는 두 테이블 사이에서 어떤 값이 수정될 때, 무엇을 기준으로 해야할지 문제가 생긴다.
    - 수정을 하는 것에 있어서 여러 관계로 엮여 있는 상황을 생각하면, 어디서 부터 어디까지 해야하는지 감이 안온다. 그래서 한 놈을 대표로 둬서 그 놈을 수정하기로 한다.



##### 연관관계의 주인!

- 설명
  - 객체의 두 관계 중에 하나가 그 관계의 주인이 된다.
  - 연관관계의 주인만이 외래키를 관리한다. (등록, 수정 가능!)
  - 주인이 아닌 쪽은 읽기만 가능하도록 한다.
  - 주인은 `mappedBy` 속성을 사용하지 않는다.
  - 주인이 아니라면 `mappedBy` 속성으로 주인을 설정한다. 
- 주의할 점!
  - 주인이 아닌 것이 아무리 추가해봐야 안된다!
  - 연관관계의 주인이라고 해서 비즈니스 적으로 중요한 것은 아니다.
    그냥 DB적으로 좀 중요한 것!
- 누구를 주인으로 할까??
  - 외래 키(FK)가 있는 곳을 주인으로 정하자!
    - 여러가지 방법이 있지만 이렇게 하는 것이 좋다.
    - DB 입장에서 외래 키가 있는 곳이 보통 다(Many) 의 입장이다.
      N 쪽이 연관관계의 주인이 되는 것



