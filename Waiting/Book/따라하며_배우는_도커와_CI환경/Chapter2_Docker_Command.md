# Docker Command



## docker run ls : 도커 이미지의 내부 파일 구조 보기

- 도커 이미지의 내부 파일 구조를 보려면 해당 이미지로 컨테이너를 실행한 다음 그 컨테이너에 어떤 파일이 있는지 확인 하면 된다.

```bash
docker run <이미지 이름> ls

# docker 
# run : 컨테이너 생성 및 실행
# < image name > : 컨테이너를 위한 이미지
# ls : 이미지를 시작하는 명령어 대신 실행할 명령어
```

- 도커 이미지 이름 뒤에 명령어를 추가하면 컨테이너를 실행하면서 작동될 명령어를 무시하고 추가한 명령어를 실행한다.



```bash
sudo docker run alpine ls
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
59bf1c3509f3: Pull complete 
Digest: sha256:21a3deaa0d32a8057914f36584b5288d2e5ecc984380bc0118285c70fa8c9300
Status: Downloaded newer image for alpine:latest
bin
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

- 순서
  1. `docker run alpine ls` 명령어를 도커 서버에 전달한다.
  2. 도커서버에서 도커 이미지를 이용해서 도커 컨테이너를 실행한다.
  3. `ls` 명령어를 실행해서 `alpine` 이미지 안에 들어 있는 디렉터리와 파일 목록을 조회한다.

- 모든 도커이미지에서 `ls` 명령어를 실행할 수 있는 것은 아니다.
  `ls`가 실행되는 이유는 `alpine` 이미지의 파일 스냅숏 안에 `ls` 명령어를 사용할 수 있는 파일이 들어있기 때문이다.
  - `docker run hello-world ls` 를 하면 에러가 발생한다.
