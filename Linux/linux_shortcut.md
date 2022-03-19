# Shortcut



## Ubuntu



### find

- 파일 및 디렉토리를 검색할 때 사용하는 명령
- 접근할 수 있는 파일 시스템에서 파일 및 디렉토리를 찾는다 (권한에 영향을 받는다.)

```bash
find [OPTION...] [PATH] [EXPRESSION...]

find / -name nginx.conf
/etc/nginx/nginx.conf
```

- 다양한 검색 옵션이 존재한다.
  필요할 때 찾아서 정리할 필요 있음.
- 출처 : https://recipes4dev.tistory.com/156





## Amazon Linux ( AMI )



### echo

- 변수를 출력하는 프린트 함수

```bash
water = "삼다수" # 변수 등록

echo $water # 삼다수

unset water # water 변수 삭제
```

- 출처 : http://keepcalmswag.blogspot.com/2018/06/linux-unix-export-echo_49.html



### export

- 쉘 변수를 환경변수로 저장할 수 있다.

```bash
export water

env # 환경변수 확인
```

- 환경변수는 터미널이 꺼지면 사라지게 된다.
  매번 번거러우니까 자동으로 설정하고 싶다면??

  ```bash
  vi ~/.bashrc
  
  # 여기에서 설정하도록
  ```

- 출처 : http://keepcalmswag.blogspot.com/2018/06/linux-unix-export-echo_49.html



### rm

- 파일, 디렉토리를 삭제하는 명령어

```bash
# 파일 삭제
rm 파일명

# 파일 골라서 삭제 '.dat'으로 끝나는 파일 모두 삭제
rm *.dat 

# 모든 파일 삭제
rm *

# 디렉토리 삭제 (디렉토리를 삭제하기 위한 옵션 : -r)
rm -r 디렉토리명

# 강제로 삭제 ( 모든 것을 강제로 삭제 )
rm -rf 디렉토리명
```

