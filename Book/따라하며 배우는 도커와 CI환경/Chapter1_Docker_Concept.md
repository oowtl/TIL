# Dokcer Concept

- 실행환경 : AWS, Ubuntu 20.04 focal, t1 

## Why Docker?
> 프로그램을 내려받는 과정을 간단하게 하기 위해서
> 
> 정확하게는 프로그램을 설치하는 서버, 패키지 버전, 운영체제 등에 따라서 다양한 에러가 생길 수 있는데 그것을 편하게 할 수 있도록 하는 것

<br>

### Docker 없이 Redis 설치하기

```bash
$ wget https://download.redis.io/releases/redis-6.2.6.tar.gz
$ tar xzf redis-6.2.6.tar.gz
$ cd redis-6.2.6
$ make
```

- `wget` : package, 무언가를 설치할 때 사용하는 패키지이다.

  이게 없으면 안되니까 이걸 다운로드할 필요성이 있다.
  여기서 말하고 싶은 것은 이게 복잡하다는 것이다.

<br>

### Docker 를 사용한 Redis 설치
```bash
docker run -it redis
```

- 여기에서 말하고 싶은 것은 쉽게 내려받아서 설치를 할 수 있다는 것이다.


## Docker and Container

- Docker?
  - 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼
  - 소프트웨어를 **컨테이너**라는 표준화된 유닛으로 패키징한다.
  - 환경에 구애받지 않고 애플리케이션을 신속하게 배포 및 확장할 수 있다.

- Container?
  - 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는데 필요한 모든 것이 포함되어 있다.

by. AWS site Docker...

<br>

### What is Container?

- 다양한 프로그램과 실행환경을 컨테이너에 담는다.

  동일한 인터페이스를 제공해서 프로그램의 배포 및 관리를 단순하게 한다.

  - 뭘 넣어도 인터페이스는 같으니까 관리하기 쉬워진다는 뜻인 것 같다.


## Docker Image and Container

> Docker Container 를 만들기 위해서는 Docker Image 가 필요하다.

- Docker Container
  - 코드와 모든 종속성을 패키지화하여 <Br>
    응용 프로그램이 한 컴퓨팅 환경에서 다른 컴퓨팅 환경으로 빠르고 안정적으로 실행되도록 하는 소프트웨어의 표준 단위

- Docker Image
  - 응용 프로그램을 실행하는 데 필요한 모든 것을 포함하는 가볍고 독립적이며 실행 가능한 소프트웨어 패키지

- 결론
  - 도커 이미지가 프로그램을 실행하는데 필요한 설정이나 종속성을 가지고 있다.

    따라서 도커 이미지를 이용해서 여러 개의 도커 컨테이너를 만들 수 있다.

  - 도커 컨테이너를 도커 이미지의 인스턴스라고 부른다.
  - 도커 컨테이너를 실행하면, 컨테이너 안에서 실행하고자 하는 프로그램을  실행할 수 있다.

  - 도커 이미지를 이용해서 컨테이너를 생성한다.

    생성한 컨테이너를 잉요해서 프로그램을 실행한다.


<Br>

## Docker Install

> https://www.docker.com/
> 
> https://docs.docker.com/engine/install/ubuntu/


도서에서는 window, mac OS 환경에서 설치하는 것이 나와있지만 ubuntu 를 사용하고 있기 때문에 공식문서를 참고해서 설치를 진행한다.

<br>

### 이전 버전 제거

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

<br>

### 설치
- 여러 방식으로 Docker 를 설치할 수 있다. ( 공식문서 참조 )

  대부분 사용하는 Docker 의 리포지토를 설정하고 그곳에 설치하는 방식을 따라가겠다.

#### 저장소를 사용해서 설치하기

<br>

##### 저장소 설정
1. HTTPS 를 통해서 리포지토를 사용할 수 있도록 한다.

```bash
sudo apt-get update
sudo apt-get install \
  ca-certificates \
  curl \
  gnupg \
  lsb-release
```
2. Docker 의 공식 GPG 키 추가

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3. 저장소 설정

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Docker Engine 설치

1. Docker Engine 설치

    특정 버전을 다운로드할 수 있는 방법도 있다. ( 공식문서 참조)

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

2. hello-world Image 실행해보기

```bash
# docker 버전확인하기
docker version
```


```bash
# image 실행
sudo docker run hello-world

# 해당 메시지가 출력되면 성공이다.
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:2498fce14358aa50ead0cc6c19990fc6ff866ce72aeb5546e1d59caac3d0d60f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
```

## Docker Use Flow

- 도커를 사용하는 흐름

  1. 도커 클라이언트 ( CLI ) 에 원하는 명령을 위한 명령어를 입력한다.
  2. 도커 서버 ( 도커 데몬 ) 가 도커 클라이언트에서 입력한 명령어를 전달받는다.
  3. 도커 서버는 명령어에 따른 이미지를 생성하고 컨테이너를 실행한다.
  4. 실행된 컨테이너에서 애플리케이션을 실행한다.


- docker run hello-world ( Docker engine install 부분 확인 )
  1. CLI 명령어 입력, 클라이언트에서 도커 서버롤 요청
  2. 서버에서 hello-world 이미지가 로컬에 다운로드 되어있는지 확인
  3. hello-world 이미지가 로컬에 없다는 것을 확인
  4. 도커 허브 ( Docker hub ) 에서 hello-world 이미지를 가져오고 로컬에 보관
       - 이미지 캐시 보관 장소에 hello-world 이미지가 없다면 도커허브에서 가져온다는 뜻 
  5. 이미지를 이용해서 컨테이너 생성
  6. 생성된 컨테이너는 이미지에서 받은 것에 맞게 프로그램 실행

<Br>

## Docker VS 기존의 Virtualization ( 가상화 )

- 가상화라는 키워드에 대한 공부도 해야하지만, 일단은 Go

<Br>

### 가상화 기술이 나오기 전에 서버를 사용하는 방식
- 한 대의 서버를 하나의 용도로만 사용한다.

    한 대의 서버에서 사용하고 남는 서버 공간은 방치하게 된다.

    하나의 서버, 하나의 운영체제, 하나의 프로그램

    안정적이나 비효율적이다.

<Br>

### 하이퍼바이저 기반의 가상화 기술
- 하이퍼바이저
  - 논리적으로 공간을 분할하여 가상머신 ( VM ) 이라는 독립적인 가상 환경에서 서버를 이용하는 기술
  - 호스트 시스템에서 다수의 게스트 운영체제 ( OS ) 를 구동할 수 있도록 하는 소프트웨어
  - 하드웨어를 가상화하면서 각각의 가상머신 ( VM ) 을 모니터링하는 중간 관리자
  - 한 대의 서버를 하나의 용도로 사용했던 비효율적인 부분 개선


