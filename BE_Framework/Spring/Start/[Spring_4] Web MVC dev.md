# Spring 4. Web MVC 개발



## 홈 화면 추가

- 순서
  1. `controller` - `HomeController` 생성
  2. `resources` - `templates` - `home.html` 생성



###  static 과 mapping 의 우선순위

- 순서
  1. 요청을 받으면 Controller 를 먼저 찾는다.
  2. Controller 가 있으면 그것에 맞는 것을 `templates` 에서 찾아서 보내준다.
  3. 만약 Controller 가 없다면 `static`에서 찾는다.



## 회원가입, 등록



### `PostMapping`

- ```java
  @PostMapping("/members/new")
  public String create(MemberForm form) {
      Member member = new Member();
      member.setName((form.getName()));
  
      memberService.join(member);
  
      return "redirect:/";
  }
  ```

  - `@PostMapping`
    - post 요청일 때, mapping
  - `(MemberForm form)`
    - 들어오는 값이 MemberForm 에 들어간다.
    - input 태그에서 `name = name` 으로 인해서 MemberForm 에 name 이 들어간다.



## 회원 조회



- 코드

  - ```html
    <tr th:each="member : ${members}">
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
    </tr>
    ```

- 설명

  - `${members}`

    - 모델 안에 있는 값을 꺼낸다.

    - 과정

      1. members 의 위치

         - ```java
           @GetMapping("/members")
           public String list(Model model) {
               List<Member> members = memberService.findMembers();
               model.addAttribute("members", members);
               return "members/memberList";
           }
           ```

           - members 에는 모든 member 를 list 로 담아놓은 것이다.

  - ` th:each`

    - 루프를 돈다
    - 타임리프 문법!

  - `${member.id}` , `${member.name}`

    - 각각 `getId`, `getName` 을 해주는 것이다.



## 주의할점

- DB 가 메모리에 있기 때문에, 서버가 종료되면 같이 DB 가 날아간다.