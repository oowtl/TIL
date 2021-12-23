# Node



## Windows

1. https://nodejs.org/ko/ 접속
2. `LTS` 버전 설치 (설치 시기에 따라 버전은 다를 수 있음)
3. 파일 다운로드

- 아래 화면까지 계속해서 next 클릭 (첫 next 버튼 활성화가 늦을 수 있음)

- 다음 화면에서 `체크박스 해제 유지`

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/086c33ba-efc1-4c28-b32d-c757cefdc529/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/086c33ba-efc1-4c28-b32d-c757cefdc529/.png)

- git bash에서 `node -v` 버전 확인

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d20a592f-972f-444d-9d1d-e5bd8ea48d4a/12344123123.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d20a592f-972f-444d-9d1d-e5bd8ea48d4a/12344123123.png)

------

## macOS

2가지 방법 중 하나만 선택하며 일반적으로 `nvm 방법을 권장`합니다.

### 1. via 공식 파일 다운로드

1. https://nodejs.org/ko/ 접속

2. `LTS` 버전 설치 (설치 시기에 따라 버전은 다를 수 있음)

3. 파일 다운로드

4. 설치

5. 설치 후 다음 명령어 실행

   ```bash
   $ sudo chown -R $USER /usr/local/lib/node_modules
   ```

### 2. via NVM

**nvm**

https://github.com/nvm-sh/nvm

```bash
# install

$ curl -o- <https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh> | zsh
# ~/.zshrc 에 작성

export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
$ source ~/.zshrc
$ nvm ls
# "node" is an alias for LTS version
$ nvm install node 

# or 10.10.0, 8.9.1, etc
$ nvm install 14.16.1 
$ nvm use 14.16.1
```

- terminal에서 `node -v` 버전 확인

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfff3afb-09ca-4824-83dd-9f1a229e94a2/Screen_Shot_2021-05-02_at_4.49.26_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfff3afb-09ca-4824-83dd-9f1a229e94a2/Screen_Shot_2021-05-02_at_4.49.26_PM.png)