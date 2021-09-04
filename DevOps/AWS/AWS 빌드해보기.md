# AWS 빌드해보기



# 주의사항

- 21.08.08 기준으로 Docker compose 가 우분투 arm 으로는 작동시키기 힘들다고 한다.
  그러니까 AWS 에서 EC2 를 넣을 때 잘 넣자.





## Docker

```ubuntu
ubuntu@ip-10-10-10-221:/$ sudo apt-get update
```

```ubuntu
$ sudo apt-get install \
apt-transport-https \
ca-certificates \
curl \
gnupg \
lsb-release
```

```ubuntu
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```ubuntu
 echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```ubuntu
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

- 못찾겠다는데... 잘모르겠다.
- 에러가 나서... 
  - 아키텍쳐에 영향을 크게 받는다. EC2 설정에서 부터 잘 해야 할 것 같다.



### error 

```ubuntu
ubuntu@ip-10-10-10-221:/$ sudo apt-get update
Hit:1 http://ap-northeast-2a.clouds.ports.ubuntu.com/ubuntu-ports focal InRelease
Ign:2 https://download.docker.com/linux/debian focal InRelease
Hit:3 http://ap-northeast-2a.clouds.ports.ubuntu.com/ubuntu-ports focal-updates InRelease
Err:4 https://download.docker.com/linux/debian focal Release
  404  Not Found [IP: 54.192.175.107 443]
Hit:5 http://ports.ubuntu.com/ubuntu-ports focal-security InRelease
Hit:6 http://ap-northeast-2a.clouds.ports.ubuntu.com/ubuntu-ports focal-backports InRelease
Reading package lists... Done
E: The repository 'https://download.docker.com/linux/debian focal Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

- 이 부분에서 막혀서 `sudo apt-get install docker-ce docker-ce-cli containerd.io` 이 까지 안되는 것 같았다.
  그래서 찾아보니 `/etc/apt/sources.list.d/docker.list` 여기에서 파일 경로를 수정하라는 말이 있었다.

```ubuntu
ubuntu@ip-10-10-10-221:/etc/apt/sources.list.d$ sudo vi docker.list
```

```vi
#deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian   focal stable
deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu    focal stable
```

- `debian` -> `ubuntu`로 바꿔줬다.



- 문제점
  - repo 가 겹친다는 소리를 들었다.
  - 확인해보자.

```ubuntu
ubuntu@ip-10-10-10-221:/etc/apt$ sudo vi sources.list
```

- 여기에서 `arch=amd64` 이런식으로 되어있는 것을 발견했다.
  그런데 나는 우분투 arm 으로 설정했다. 그래서 과감하게 삭제하고
   `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
  여기부터 시작하니 된다.



- Docker Compose 설치하기
  https://docs.docker.com/compose/install/

```ubuntu
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```ubuntu
 sudo chmod +x /usr/local/bin/docker-compose
```

- 바이너리에 실행권한 적용
- **이거 안되는 이유가 ubuntu 아키텍쳐 문제인듯하다. arm -> x64 로 바꿈**





