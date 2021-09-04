# 셀레니움 활용하기



## install



### 라이브러리 설치

```shell
$ pip install selenium
```



### 브라우저 드라이버 설치

> 셀레니움을 실행하기 위해서는 브라우저 드라이버가 필요하다.
>
> 크롬, 파이어폭스, 마이크로소프트 엣지 등이 있다.
>
> 해당 프로젝트는 mac 과 크롬으로 사용하고 있다.



1. 크롬버전 확인하기

   1. ![스크린샷 2021-09-04 오후 12.49.45](selenium.assets/스크린샷 2021-09-04 오후 12.49.45.png)
      - 크롬 우측 상단 `...` - 도움말 - 크롬정보

2. 크롬 드라이버 파일 다운로드 받기

   - 해당하는 OS 와 크롬 버전에 맞게 드라이버를 다운로드 받는다.
   - 어디에 해도 상관은 없는 것 같지만 경로가 다르면 여러가지 귀찮은 것들이 생길 수 있으니
     파이썬 파일과 같은 디렉토리에 넣는 것을 추천한다.
   - ![스크린샷 2021-09-04 오후 12.56.11](selenium.assets/스크린샷 2021-09-04 오후 12.56.11-0727795.png)
     - 일단 버전이 완전히 일치하는 것은 없지만 영어를 읽어봐서는 93 류를 사용하라는 것 같다.
   - OS 에 맞는 zip 을 다운받아서 chromedriver 이라는 파일을 얻는다.

3. 크롬 드라이버 파일 위치

   - 우리가 실행할 `.py` 파일과 동일한 디렉토리에 놓아준다.

     - 경로를 모른다면?

       ```python
       import os
       print(os.getcwd())
       ```

   - ```shell
     $ls
     chromedriver    hi_selenium.py  venv
     ```



## Hi! Selenium



1. Test

   - ```python
     import selenium
     from selenium import webdriver
     from selenium.webdriver.common.keys import Keys
     
     driver = webdriver.Chrome("/Users/new_2/Desktop/ssafy/crawling/hicrawl/src/chromedriver")
     # driver = webdriver.Chrome("./chromedriver")
     ```

     - `driver = webdriver.Chrome()` 이 부분에서 경로를 찾을 수 없다고 해서 당황했다.
       이것은 상대경로로 가지 않고, 절대경로로 간다고 한다.
       크롬드라이버의 절대경로를 넣어주면 된다.
       **아니다 상대경로도 먹는다!!!!!!!!!!!**

   - 결과

     - ![스크린샷 2021-09-04 오후 1.21.26](selenium.assets/스크린샷 2021-09-04 오후 1.21.26.png)
       - 빈 창이 나오면 성공이다.

2. 



























## 출처

- https://ddolcat.tistory.com/674
- https://chancoding.tistory.com/136
- https://greeksharifa.github.io/references/2020/10/30/python-selenium-usage/#install

