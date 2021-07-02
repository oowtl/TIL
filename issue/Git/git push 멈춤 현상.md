# Git push 멈춤현상



## 상황

- Git Bash 사용

- GitHub 이 아닌 다른 곳에서 git 을 사용하고 있었는데,
  GitHub로 repository를 사용하려고 했다.
  `pull`, `add`, `commit` 까지 성공했으나
  `git push` 를 할 때, 실행은 되고 있으나 결과물이 없는 상태가 지속이 된다.

- ```bash
  /d/TIL (master)
  $ git push
  
  
  ```

  - 이 상태로 지속이 된다.

-  `Ctrl + C`를 통해서 취소하면 `open SSH` 설정이 나온다.



## 해결 방법

1. cmd 나 Windows Terminal 을 실행한다. (Git Bash 가 아닌 것)
2. `add`, `commit`, `push` 를 해본다.
3. 설정 창이 나올 것이다. -> `(1)` 선택한다.
4. 정상적인 push 결과물이 나올 것이다.
   이제부터는 Git bash 에서도 가능하다.