# AWS 설정하기



## PUTTY

- 서버의 설정을 저장해주는 것



1. putty 를 설치해준다.

2. puttygen.exe 실행

3. Load or import - Save private Key

   ![image-20210807230947117](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807230947117.png)

4. PUTTY.exe 실행
   ![image-20210807231040576](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807231040576-16283454426031.png)

   ![image-20210807231055022](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807231055022.png)

- Auth 에서 Browse 클릭하고 생성된 파일 넣어준다.



![image-20210807232946730](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807232946730.png)

- ubuntu 로 해줘야한다.



## mobaXterm

![image-20210807233139592](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807233139592.png)

- Session



![image-20210807233212237](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807233212237.png)





![image-20210807234347294](AWS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.assets/image-20210807234347294.png)

- Remote host : 주소 (도메인 or ip)
- Specify username : 접속자 계정
  - ubuntu
- Port : SSH 접속 포트
  - 22 확인
- Use private key
  - .pem 파일



## DB 설정

```ubuntu
ubuntu@ip-10-10-10-221:~$ sudo apt install mysql-server
```

- 다른 것도 있지만 이걸로는 web server 에 db 를 넣어줄 수 있다.

```ubuntu
sudo apt-get update
```

- 이 과정을 거치면 외부에서 접속하기 위해서 별도의 과정이 필요하다.

```ubuntu
dpkg -l | grep mysql-server
```

- 설치 확인

```ubuntu
sudo apt install -y net-tools

netstat -tnlp
```

- 실행여부 확인

  - ```ubuntu
    (Not all processes could be identified, non-owned process info
     will not be shown, you would have to be root to see it all.)
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Progr               am name
    tcp        0      0 127.0.0.1:33060         0.0.0.0:*               LISTEN      -                       
    tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                       
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                       
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                       
    tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      -                       
    tcp6       0      0 :::22                   :::*                    LISTEN      -                       
    tcp6       0      0 ::1:6010                :::*                    LISTEN      -  
    ```

    - `127.0.0.1` 이라는 것이 localhost 에서만 가능하다는 것을 말한다.
      이것을 수정해줘야한다.

```ubuntu
ubuntu@ip-10-10-10-221:~$ cd /etc/mysql/mysql.conf.d
ubuntu@ip-10-10-10-221:/etc/mysql/mysql.conf.d$ sudo vi mysqld.cnf
```

```vi
# bind-address          = 127.0.0.1
bind-address            = 0.0.0.0
```

- mysql 외부접속 허용을 위한 것

```ubuntu
ubuntu@ip-10-10-10-221:/etc/mysql/mysql.conf.d$ sudo /etc/init.d/mysql restart

ubuntu@ip-10-10-10-221:/etc/mysql/mysql.conf.d$ netstat -tnlp
```

- 재시작을 해줘야 하고, sudo 를 붙여야한다.

- 이후에 모든 아이디를 허용할 계정을 만들어 줘야 한다.

```ubuntu
ubuntu@ip-10-10-10-221:/etc/mysql/mysql.conf.d$ sudo mysql -u root -p
Enter password:
```

- sudo 를 붙여줘서 mysql 을 실행해준다.

```mysql
mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | debian-sys-maint |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
5 rows in set (0.00 sec)
```

- 이걸 보면 DB 가 localhost 에만 있는 것을 알 수 있다.
- 모든 IP를 허용할 계정을 만들어주면 된다.

```mysql
mysql> create user 'ssafy_test'@'%' identified by 'password';
Query OK, 0 rows affected (0.15 sec)

mysql> grant all privileges on *.* to 'ssafy_test'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

- 만들고 권한부여까지

```mysql
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | ssafy_test       |
| localhost | debian-sys-maint |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
6 rows in set (0.00 sec)
```

- 여기에서 모든 사람이 접속할 수 있는 것이 나온다.
- 자 우리는 SSH 포트를 DB 에 연결하는 것으로 SSH Tunneling 이라는 것을 사용한다.





