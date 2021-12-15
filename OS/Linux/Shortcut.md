# Shortcut





## echo

- 변수를 출력하는 프린트 함수

```bash
water = "삼다수" # 변수 등록

echo $water # 삼다수

unset water # water 변수 삭제
```

- 출처 : http://keepcalmswag.blogspot.com/2018/06/linux-unix-export-echo_49.html



## export

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