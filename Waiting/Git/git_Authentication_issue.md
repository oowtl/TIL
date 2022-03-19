# Git 인증관련 이슈



```bash
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead
```



- 한마디로 비밀번호 입력하는걸로 인증하는 것은 이제 그만하겠다는 소리이다.
  그리고 유저 개인의 토큰으로 하겠다는 것이다.
  - 예전에 한 번 겪어서 셋팅을 했는데 까먹어서 다시 찾았다.



## 해결방법

1. Settings

2. Developer settings

3. Personal access tokens

4. Generate new token

   1. 여기에서 설정을 여러가 할 수 있도록 했는데 평범한 경우에는 repo 만 설정해도 된다.

5. Generate token

6. 생성된 토큰 값 복사하기

7. git 명령어

   ```bash
   $ git clone https://github.com/username/repo.git
   Username: your_username
   Password: 발급 받은 토큰
   ```

   - 나는 sourcetree 에다가 바로 넣어줌



## 주의점

- 만료일이 존재한다. 이건 알아서 설정하도록 한다.