

# AWS 인스턴스 만들기



1. 리전 설정해주기
   ![image-20210807173446683](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807173446683.png)



2. VPC 생성하기
   ![image-20210807173526621](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807173526621.png)

3. VPC 생성하기 - 입력
   ![image-20210807173654508](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807173654508.png)

   - IPv4 CIDR 이 뭔지는 모르겟지만, 일단 이렇게 넣어봄

4. 서브넷 생성하기
   ![image-20210807173934327](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807173934327.png)

   - 서브넷이라는 것을 설정해준다.

5. 서브넷 생성하기 - 1, 2 (데이터 센터 구축 - public)
   ![image-20210807174101718](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807174101718.png)

   - 이 데이터 센터는 물리적으로 떨어져있기 때문에, 하나가 떨어져도 괜찮도록 두 개를 유지한다.

   ![image-20210807174353462](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807174353462.png)

   - 퍼블릭을 두개로 한다.
   - 또한 10번대는 퍼블릭 네트워크 영역으로 만든다.
     - ip에도 의미를 부여하는 것도 좋음

6. 서브넷 생성하기 ( private 구축)
   ![image-20210807174646129](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807174646129.png)

   - 100번대로 하고 가용영역을 주의해서 사용

   ![image-20210807174754013](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807174754013.png)

7. 퍼블릭 서브넷에서 인스턴스를 생성할 때 퍼블릭 IP가 자동 생성되도록 설정
   ![image-20210807175150419](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175150419.png)

   - 서브넷 ID 에서 우클릭하기 - 자동 할당 IP 설정 수정

   ![image-20210807175234297](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175234297.png)

   - 자동할당 체크



## 보안 그룹 생성

1. 보안 그룹
   ![image-20210807175314030](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175314030.png)

2. 보안 그룹 생성하기
   ![image-20210807175334260](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175334260.png)

   - 입력
     ![image-20210807175527661](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175527661.png)

     - 용도를 식별할 수 있는 그룹 이름 설정
     - 세부적인 설정은 하지 않고 나중에 하도록

     ![image-20210807175652554](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175652554.png)

     - 사용할 VPC 를 정확하게 고르도록

     ![image-20210807175845166](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175845166.png)

     - 총 4개 생성

     ![image-20210807175930391](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807175930391.png)

     - 이런 식으로 네임태그를 달아서 filter 를 먹이는 것으로 원하는 것만 볼 수 있도록 한다.



## EC2 인스턴스 생성

- 시스템에 필요한 EC2 생성하기



### Bastion 인스턴스 생성하기

1. 인스턴스 시작하기
   ![image-20210807180101902](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180101902.png)

2. AMI 선택하기
   ![image-20210807180205644](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180205644.png)

   - 20.04 LTS - 64비트(Arm) - 선택 **(그냥 속편하게 x86사용하자 / 안되는 것 많음)**

3. 인스턴스 유형 선택
   ![image-20210807180302345](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180302345.png)

   - 무료 평가판 선택
     - 여기에서는 t4g 라는 것인데, x86이면 다를 것이다.
     - 그리고 저 유형에 따라서 서브넷을 다르게 해야한다.
       ap-north-a, ap-north-b, ap-north-c .. 이렇게 있는데 유형마다 허용하는 것이 다르다.
   - 그 후 `인스턴스 세부 정보 구성` 선택
   
4. 인스턴스 세부 정보 구성

   ![image-20210807180457838](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180457838.png)

   - 서브넷을 pub-a-sub 로 해준다.
   - `스토리지 추가` 선택

5. 스토리지 추가
   ![image-20210807180714505](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180714505.png)

   - 여기에서는 딱히 선택할 것이 없어 보인다.
   - 볼륨 유형을 가장 저렴한 마그네틱으로 설정
   - `태그 추가` 선택

6. 태그 추가

   ![image-20210807180804274](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180804274.png)

   ![image-20210807180850851](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807180850851.png)

   - `보안 그룹 구성` 선택

7. 보안 그룹 구성
   ![image-20210807181354225](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807181354225.png)
   - 보안 그룹 할당 - 기존 보안 그룹 선택 (미리 생성했으니까)
   - bastion-sg 선택
   - 검토 및 시작
8. 포트 에러 나오는데 계속 하기
9. 범용 SSD 부팅
   ![image-20210807181939851](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807181939851.png)
   
   - 싸고 좋은 마그네틱으로 간다.
10. 인스턴스 시작 검토
    - 이것은 이때까지 설정한 것들이 나오는 것 같다.
    - 시작하기
11. 기존 키 페어 선택 또는 새 키 페어 생성
    - 키 페어도 관리용과 서비스용을 분리해서 생성하는 것이 좋다.
      ![image-20210807182147267](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807182147267.png)
    - 인스턴스 시작

### Service 인스턴스 생성하기



#### private a 서버

1. 인스턴스 시작
2. 인스턴스 유형 - 동일
3. 인스턴스 세부 정보 구성
   ![image-20210807182608249](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807182608249.png)
   - vpc 에서 자동할당을 설정하지 않아서 `서브넷 사용 설정(비활성화)`
4. 스토리지 추가
   - 볼륨유형 - 마그네틱
5. 태그 추가
   ![image-20210807182747227](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807182747227.png)
6. 보안 그룹 구성
   ![image-20210807182807662](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807182807662.png)
7. 범용 SSD 구성
   - 마그네틱으로 구성
8. 인스턴스 시작 검토
   - 시작하기
9. 기존 키 페어 선택 또는 새 키 페어
   ![image-20210807182918128](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807182918128.png)
   - 키페어 만들기 후 다운로드



#### private b

1. 인스턴스 세부 정보
   ![image-20210807183356459](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183356459.png)
2. 나머지는 동일하게



### NAT 인스턴스 생성하기

1. AMI 선택
   ![image-20210807183626961](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183626961.png)

   - 커뮤니티 AMI - nat 검색

   ![image-20210807183706180](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183706180.png)

   - 원래는 설치를 해줘야 하지만, 이미 AMI 로 제공을 해준다.

   - 선택

2. 인스턴스 세부 정보 구성
   ![image-20210807183833414](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183833414.png)

   - pub-a-sub 로 선택한다.

3. 스토리지 추가
   ![image-20210807183915333](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183915333.png)

   - 마그네틱

4. 태그 추가
   ![image-20210807183942963](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807183942963.png)

5. 보안 그룹 구성
   ![image-20210807184004383](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807184004383.png)

   - nat-sg 선택

6. 기존 키 페어 선택 또는 새 키 페어
   ![image-20210807184113910](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807184113910.png)

   - 기존 키 페어에서 들어간다.
   - 밑에 체크

#### NAT 인스턴스 설정

1. 소스/대상 확인
   ![image-20210807185010901](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185010901.png)

   - 해당 인스턴스 우클릭

   ![image-20210807185037093](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185037093.png)

   - `중지` 체크 후 `저장`



## IGW 생성

VPC 와 인터넷 게이트웨어 생성 및 연결하기



![image-20210807185211054](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185211054.png)

- vpc - 인터넷 게이트 웨이 - `인터넷 게이트 웨이 생성`



![image-20210807185256391](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185256391.png)

- 이름 넣고 생성
- 외부에서 연결하기 위해서는 필수적으로 존재해야한다.
  인터넷 공유기를 설치하는 것과 비슷한 개념



![image-20210807185406436](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185406436.png)

- 우클릭- `VPC 에서 연결`

![image-20210807185432848](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185432848.png)



## Routing Table 생성

- 퍼블릭용, 프라이빗용 라우팅 테이블을 각각 생성 설정
- 실제 라우터에 적용해서 라우팅 규칙으로 작동



### 퍼블릭 라우팅 테이블



![image-20210807185548563](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185548563.png)



![image-20210807185631337](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185631337.png)

- 이름을 입력하고 vpc 맞게 해준다.



#### 라우팅 편집

![image-20210807185727973](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185727973.png)

- 생성된 것을 클릭 - 밑의 세부항목 라우팅 - 라우팅 편집

![image-20210807185841028](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185841028.png)

- 대상 - `0.0.0.0/0`
  대상 - `인터넷 게이트 웨이`

![image-20210807185935545](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807185935545.png)

- 생성했던 게이트 웨이 선택



#### 서브넷 연결 편집

![image-20210807190024372](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190024372.png)

- 명시적 서브넷 연결에서 `서브넷 연결 편집`

![image-20210807190137997](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190137997.png)

- 퍼블릭에서 가는 거니까



### 프라이빗 라우팅 테이블

![image-20210807190314565](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190314565.png)



![image-20210807190447041](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190447041.png)

- 프라이빗 서버는 한번에 나갈 수 없도록 nat 를 걸어준다.
- `네트워크 인터페이스` 

![image-20210807190530014](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190530014.png)

- nat 걸어주기
- 경유해서 가도록 한다.

![image-20210807190616439](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807190616439.png)

- 프라이빗 용도



## 보안 그룹 설정

- 방화벽 설정

![image-20210807191520170](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807191520170.png)

- EC2 - 네트워크 및 보안 - 보안 그룹 - 해당 인스턴스 - 인바운드 규칙 - 인바운드 규칙 편집



### bastion-sg 인스턴스 설정

- 유형
  ![image-20210807191642532](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807191642532.png)
  - ssh
- 소스
  ![image-20210807191800365](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807191800365.png)
  - 접근하는 것을 제한해주는 것 같다.
  - 지금은 아무나 다 하는 것으로 설정하지만, 원래는 제한한다.



### alb-sg 인스턴스 설정

![image-20210807191949766](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807191949766.png)

- 80port 로는 아무나 들어올 수 있도록 해야지 유저 접속 가능



### web-sg 인스턴스 설정

![image-20210807192206028](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807192206028.png)

- 어플리케이션 로드밸런서도 들어올 수 있어야한다.



### nat-sg 인스턴스 설정

![image-20210807192349212](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807192349212.png)

- 웹 서버가 사용하기 때문에 모든 트래픽
- web-sg



## 웹서버 설치

- Bastion 호스트에 접속하고 웹서버로 접속해서 NGINX 설치하기



### Bastion 웹서버 접속하기

```bash
multicampus@DESKTOP-KVCQHCD MINGW64 ~/Desktop/1st_project/아몰랑 한번해봐/AWS/practice
$ chmod 400 ssafy-bastion.pem
```

- 권한을 낮춰서 웹서버 접속 가능

```bash
$ssh -i ssafy-bastion.pem ubuntu@13.125.49.18
```

- ip 는 bastion 인스턴스의 퍼블릭 ip
- 중지하고 끌때마다 퍼블릭 ip 가 달라진다... 유념할 것

```bash
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

- yes

```bash
ubuntu@ip-10-10-10-221:~$
```

- 성공화면

```bash
ubuntu@ip-10-10-10-221:~$ cd .ssh/
ubuntu@ip-10-10-10-221:~/.ssh$ ls
authorized_keys
ubuntu@ip-10-10-10-221:~/.ssh$ vi ssafy-private.pem
```

- bastion 에서 웹 서버로 넘어가야하는데, 거기에서도 개인키가 필요하다.
  그래서 bastion 서버 안에서 개인키가 필요하다.
  물론, 관리 상 이렇게 하지만, 필요할 때마다 만들어서 사용하고 폐기하는 것이 최선이다.
- vi 로 수정을 하는데 `ssafy-private.pem` 으로 생성했던 것을 복사 붙여넣기 하고 `:wq` 해준다.

```bash
ubuntu@ip-10-10-10-221:~/.ssh$ chmod 400 ssafy-private.pem
```

- 권한을 낮춰준다.



### private 웹서버 접속하기



#### priv-a 서버 접속

```bash
ubuntu@ip-10-10-10-221:~/.ssh$ ssh -i ssafy-private.pem 10.10.100.45
```

- 프라이빗 웹서버는 바로 접속할 수가 없다.
- ip만 봐도 내가 프라이빗에 있는지 퍼블릭에 있는지 알 수 있다.
  잘 설정하기!

```bash
ubuntu@ip-10-10-100-45:~$ sudo apt update
```

- 이 프라이빗에 있는 인스턴스가 인터넷이 되는 이유는 라우팅 테이블에서 nat 로 돌려줫고, nat 에서 인터넷 게이트웨이를 통해서 인터넷이 된다.

```bash
ubuntu@ip-10-10-100-45:~$ sudo apt install nginx
Do you want to continue? [Y/n] Y
```

- nginx 설치하기

- 에러

  ```bash
  ubuntu@ip-10-10-100-45:~$ apt install nginx
  E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
  E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
  ```

  - sudo 를 하지 않으면 나오는 듯하다.

```bash
ubuntu@ip-10-10-100-45:~$ sudo vi /var/www/html/index.nginx-debian.html
```

- 웹 서버가 두 대인데 로드밸런싱이 잘 작동하는지 확인하기 위해서 html 을 수정하는 작업

  ```bash
  <h1>Welcome to Web 01 server nginx!</h1>
  ```

  - 이렇게 수정!

```bash
ubuntu@ip-10-10-100-45:~$ exit
logout
Connection to 10.10.100.45 closed.
```

- 셋팅했으니 나가준다.



#### priv-b 서버 접속

```bash
ubuntu@ip-10-10-10-221:~/.ssh$ ssh -i ssafy-private.pem 10.10.200.249

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

ubuntu@ip-10-10-200-249:~$ sudo apt update

ubuntu@ip-10-10-200-18:~$ sudo apt install nginx

확인용으로 수정하기
ubuntu@ip-10-10-200-249:~$ sudo vi /var/www/html/index.nginx-debian.html
```



```bash
ubuntu@ip-10-10-200-249:~$ curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to web server 02 nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

- 확인하기



### 로드밸런서 생성

- 로드 밸런서의 그룹에 web01 web02 포함해서 한꺼번에 생성하기
- 웹서버로 바로 접근할 수 있는 것이 아니라, 로드밸런서를 통해서 접근할 수 있다.

![image-20210807205804827](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807205804827.png)

- 로드밸런서 생성

![image-20210807205835065](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807205835065.png)

- Application Load Balancer 생성



#### Load Balancer 구성

1. 로드 밸런서 구성
   ![image-20210807210002568](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210002568.png)

   - 이름은 설정해준다.

   ![image-20210807210038909](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210038909.png)

   - 어디에 로드밸런서를 배치할지?

2. 보안 설정 구성

   - 빠르게 넘어간다. `보안 그룹 구성 ` 으로 넘어간다.

3. 보안 그룹 구성
   ![image-20210807210345486](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210345486.png)

   - alb-sg 로 설정한다.

4. 라우팅 구성
   ![image-20210807210441436](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210441436.png)

   - 그룹으로 묶어서 장애가 생기면 대처하도록 대상을 등록해야한다.

5. 대상 등록
   ![image-20210807210535395](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210535395.png)

   - 우리가 2 개의 것을 타겟으로 하겠다.

6. 생성하기

7. 확인
   ![image-20210807210633557](AWS%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.assets/image-20210807210633557.png)

   - 프로비저닝 
   - 퍼블릭 존에 두개의 웹 서버로 EC2 가 동작해서 서비스를 셋팅하는 것
   - 상태가 Active 가 되기 까지 시간이 많이 걸리는데, 기다린다.

- 했는데 안되서 삭제
  과금해야해서 삭제 조진다.



## 인스턴스 중지하기

- 불필요한 것은 중지하는 것이 좋다.
- 약간의 과금이 되긴하지만...?
  아예 필요없으면 종료로 삭제한다.