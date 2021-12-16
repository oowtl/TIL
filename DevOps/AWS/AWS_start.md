# AWS 시작하기



## nvm 을 이용한 Node.js 설치하기

```bash
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

. ~/.nvm/nvm.sh

# 10.13.0 버전 다운로드
nvm install 10.13.0
```







## 소스코드 배포



### ubuntu

```bash
# ubuntu
sudo apt-get install git
```



### AMI

```bash

sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
```

- sudo 를 사용하는 이유 : 시스템 패키지를 설치할 때는 root 권한이 필요하다.
- git 을 설치하는 코드.
  - Git 을 설치하기 전에 Git 이 필요로 하는 라이브러리를 먼저 설치하는 것이 필요하다.
  - curl : client rㅏ 문서나 파일을 다운 받거나 전송하기 위한 명령어
  - zlib : 손실없는 데이터 압축을 수행해주는 라이브러리
  - openssl : 네트워크를 통한 테이터 통신에 쓰이는 프로토콜인 TLS 와 SSL의 오픈소스 구현판
  - expat : C 로 작성된 XML parser



```bash
cd /var
sudo mkdir www
```

- sudo 를 사용한 이유
  - /var 디렉토리의 소유권한이 root이기 때문에 해당 디렉터리에 파일이나 디렉토리를 추가하기 위해서는 root 권한이 필요하다.



```bash
sudo chown ec2-user www
```

- 코드 관리는 일반 유저인 ec2-user 로 처리할 것이기 때문에 /var/www 경로를 ec2-user의 소유로 변경한다.

- chown

  - 리눅스에서 파일은 어떤 Onwer, Group 에 속해있다.
    `chown`은 파일의 Onwer, Group 을 변경하는 것이다.

  - 사용법

    ```bash
    chown [OPTIONS] USER[:GROUP] FIEL(s)
    ```



