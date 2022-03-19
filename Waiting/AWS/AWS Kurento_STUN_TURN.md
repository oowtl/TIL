# AWS - Kurento STUN TURN





## Kurento

https://doc-kurento.readthedocs.io/en/latest/user/installation.html#installation-guide

```ubuntu
sudo docker pull kurento/kurento-media-server:latest

sudo docker run -d --name kms --network host \
    kurento/kurento-media-server:latestsudo 
```





## STUN / TURN

https://doc-kurento.readthedocs.io/en/latest/user/faq.html#faq-coturn-install

```ubuntu
sudo apt-get update && sudo apt-get install --no-install-recommends --yes \
    coturn
```



```ubuntu
ubuntu@ip-10-10-10-209:/$ sudo vi /etc/default/coturn
```

```vi
TURNSERVER_ENABLED=1
```





```ubuntu
ubuntu@ip-10-10-10-209:/$ sudo vi /etc/turnserver.conf
```

```vi
listening-port=3478
tls-listening-port=5349
listening-ip={프라이빗 IP}
relay-ip={프라이빗 IP}
external-ip={퍼블릭IP}/{프라이빗 IP}
fingerprint
lt-cred-meth
user=myuser:mypassword
realm=myrealm
log-file=/var/log/turn.log
simple-log
```



```ubuntu
sudo service coturn restart
```



## kurento - stun/turn connect

```ubuntu
ubuntu@ip-10-10-10-209:/$ sudo docker ps -a
CONTAINER ID   IMAGE                                 COMMAND            CREATED          STATUS                      PORTS     NAMES
ce82b04fafa5   kurento/kurento-media-server:latest   "/entrypoint.sh"   21 minutes ago   Up 21   minutes (healthy)             kms
```

- `CONTAINER ID` 알아두기

```ubuntu
ubuntu@ip-10-10-10-209:/$ sudo docker exec -it ce82b04fafa5 /bin/bash
```

- 결과

  ```ubuntu
  root@ip-10-10-10-209:/#
  ```



```docker
root@ip-10-10-10-209:/# vi /etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini
```

- 이거하니까 vi 를 못알아먹는다. (sudo 도 못알아먹음)

- 해결 - vim 설지하기

  ```docker
  apt-get update
  apt-get install vim
  ```



```ubuntu
root@ip-10-10-10-209:/# vim /etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini
```

```vim
stunServerAddress={퍼블릭 IP}
turnURL=myuser:mypassword@{퍼블릭 IP}?transport=udp
```



## 프론트에서 의존 라이브러리 참조

## HTTPS 활성화

### self-signed 인증서 생성

```bash
keytool -genkeypair -alias ssafy -keyalg RSA -keysize 2048 -storetype PKCS12 -keystore ssafy.p12 -validity 3650
```

- `PKC512` -> `PKCS12`
  - 오타 조심...

```terminal
키 저장소 비밀번호 입력:
키 저장소 비밀번호가 너무 짧음 - 6자 이상이어야 합니다.
키 저장소 비밀번호 입력:
새 비밀번호 다시 입력:
이름과 성을 입력하십시오.
  [Unknown]:  JunHongPark
조직 단위 이름을 입력하십시오.
  [Unknown]:  SSAFY
조직 이름을 입력하십시오.
  [Unknown]:  SSAFY
구/군/시 이름을 입력하십시오?
  [Unknown]:  Gumi
시/도 이름을 입력하십시오.
  [Unknown]:  Gumi
이 조직의 두 자리 국가 코드를 입력하십시오.
  [Unknown]:  KR
CN=JunHongPark, OU=SSAFY, O=SSAFY, L=Gumi, ST=Gumi, C=KR이(가) 맞습니까?
  [아니오]:  y
```

