# nginx and Pushion Passenger





## 1. Phusion Passenger 설치하기

```bash
# Pushion Passenger 다운로드
wget http://s3.amazonaws.com/phusion-passenger/releases/passenger-5.3.6.tar.gz

# /var 디렉토리가 root 계정 소유이기 때문에 sudo
sudo mkdir /var/passenger
sudo chown ec2-user /var/passenger

# phusion-passenger 압축 해제
tar -xzvf passenger-5.3.6.tar.gz -C /var/passenger/
```



## 2. rvm 설치하기

> Ruby Version Manager

- 특정 버전의 루비 언어를 받게 해준다.
- Pushoin Passenger 가 C++, Ruby 로 만들어졋기 때문.



```bash
# 해당하는 코드는 공개키를 가져오는 것인데 이게 낡은 것이라서 다르게 해야한다는 소리가 있었다.
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
# 대체코드
gpg --keyserver keyserver.ubuntu.com --recv-key 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

# 공개키를 받는 이유는 내려받는 rvm 패키지가 유효한지 확인하기 위해서이다.


# stable 버전 다운로드
curl -sSL https://get.rvm.io | bash -s stable

# rvm 사용하기
source /home/ec2-user/.rvm/scripts/rvm
rvm reload

# ruby 설치하는데 필요한 라이브러리 등 설치
rvm requirments run
```



### gpg

- GPG, PGP 는 강력한 암호화 프로그램으로써 RSA 를 사용해서 암호화를 한다.
- 이메일에 주로 사용된다.





## 3. 루비 설치하기

```bash
# Ruby 2.4.3 install
rvm install 2.4.3
```



## 4. Passenger nginx module 설치하기



### 변수설정하기

```bash
# passener 의 경로를 $PATH 변수에 등록하는 것
# 전체 경로를 입력할 필요없이 passenger 명령어만 입력하면 된다.
echo export PATH=/var/passenger/passenger-5.3.6/bin:$PATH >> ~/.bash_profile
source ~/.bash_profile
```



### 설치하기

```bash
# start installer
passenger-install-nginx-module

# sample project only : Node.js
Which languages are you interested in?

Use <space> to select.
If the menu doesn't display correctly, press '!'

   ⬡  Ruby
   ⬡  Python
 ‣ ⬢  Node.js
   ⬡  Meteor
   
   
# virtual Memory? -> No
our system does not have a lot of virtual memory

Compiling Phusion Passenger works best when you have at least 1024 MB of virtual
memory. However your system only has 982 MB of total virtual memory (982 MB
RAM, 0 MB swap). It is recommended that you temporarily add more swap space
before proceeding. You can do it as follows:


# recommend! Ctrl-C
Press Ctrl-C to abort this installer (recommended).
Press Enter if you want to continue with installation anyway.

# 해당 명령어를 통해서 가상 메모리의 크기를 늘릴 후에 다시 진행한다.

# dd 명령을 이용해서 swap 파일을 만든다.
sudo dd if=/dev/zero of=/swap bs=1M count=1024
# mkswqp 명령을 이용해서 swap 공간을 쓰도록 만든다
sudo mkswap /swap
# swap 파일을 즉시 활성화 한다.
sudo swapon /swap


# nginx auto install? -> 1 enter
Automatically download and install Nginx?

Nginx doesn't support loadable modules such as some other web servers do,
so in order to install Nginx with Passenger support, it must be recompiled.

Do you want this installer to download, compile and install Nginx for you?

 1. Yes: download, compile and install Nginx for me. (recommended)
    The easiest way to get started. A stock Nginx 1.14.0 with Passenger
    support, but with no other additional third party modules, will be
    installed for you to a directory of your choice.

 2. No: I want to customize my Nginx installation. (for advanced users)
    Choose this if you want to compile Nginx with more third party modules
    besides Passenger, or if you need to pass additional options to Nginx's
    'configure' script. This installer will  1) ask you for the location of
    the Nginx source code,  2) run the 'configure' script according to your
    instructions, and  3) run 'make install'.
    
    
 # nginx install spot -> enter
 Where do you want to install Nginx to?

Please specify a prefix directory [/opt/nginx]: 

# Permisson problems!
Permission problems

This installer must be able to write to the following directory:

  /opt/nginx

But it can't do that, because you're running the installer as ec2-user.
Please give this installer root privileges, by re-running it with rvmsudo:

```

- Permission problems
  - `/opt/nginx` 에 설치하려고 했지만, 해당 폴더는 권한이 필요하다.
    해당 폴더에는 root 권한이 필요하지만 ec2-user로 작업을 진행하기 때문에 에러가 난다.
  - rvm 에서 관리하는 것은 ruby를 이용해서 설치하고 있어서 installer 에서는 rvmsudo 를 이용해서 처리하는 것을 권장한다.
    rvmsudo는 rvm 에서 제공하는 기능이다.
  - 일반적으로 sudo 명령어를 실행할 경우 ec2-user 에서 사용하던 환경변수가 보존되지 않는 새로운 터미널 세션을 생성한다.
    하지만 rvmsudo 는 sudo 권한을 실행하면서 ruby 에 필요한 환경변수 값까지 보존해주는 기능을 제공한다.

```bash
# start rvmsudo & restart installer
export ORIG_PATH="$PATH"
rvmsudo -E /bin/bash
export PATH="$ORIG_PATH"
export rvmsudo_secure_path=1
/home/ec2-user/.rvm/gems/ruby-2.4.3/wrappers/ruby /var/passenger/passenger-5.3.6/bin/passenger-install-nginx-module
```



- Error 발생

  ```bash
  src/agent/Core/SecurityUpdateChecker.h: In member function ‘void Passenger::SecurityUpdateChecker::logUpdateFailCurl(const Passenger::SecurityUpdateChecker::SessionState&, CURLcode)’:
  src/agent/Core/SecurityUpdateChecker.h:262:4: error: duplicate case value
      case CURLE_PEER_FAILED_VERIFICATION:
      ^~~~
  ```

  - `CURLE_PEER_FAILED_VERIFICATION` 에 해당하는 에러는 컴파일을 하면서 나는 에러인 것으로 추정된다.
    구글링 해본 결과 유사한 현상이 나온 것을 확인할 수 있었다.
  - 5.3.6버전에서 나오는 에러이고 5.3.7 버전에서 개선이 된 것으로 확인이 된다.
    따라서 해당하는 버전에 맞게 진행해볼 예정이다.
  - 참조 : https://github.com/phusion/passenger/issues/2139