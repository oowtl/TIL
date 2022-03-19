# JAVA 0. Intro



- write once Run anywhere
  - 한번 쓰면 어디에서든지 작동된다.



## JAVA 설치

- window, mac 은 jdk 로 검색해서 나오는 대로 진행하면 된다.



### LInux에서 Java 설치하기

1. ubuntu 에서 java 설치여부 확인하기

   - ```terminal
     $ java -version
     
     Command 'java' not found, but can be installed with:
     
     sudo apt install openjdk-11-jre-headless  # version 11.0.11+9-0ubuntu2~20.04, or
     sudo apt install default-jre              # version 2:1.11-72
     sudo apt install openjdk-13-jre-headless  # version 13.0.7+5-0ubuntu1~20.04
     sudo apt install openjdk-16-jre-headless  # version 16.0.1+9-1~20.04
     sudo apt install openjdk-8-jre-headless   # version 8u292-b10-0ubuntu1~20.04
     sudo apt install openjdk-14-jre-headless  # version 14.0.2+12-1~20.04
     ```

     - ubuntu 에 java가 없을 때 나오는 것
       - 밑에 있는 것은 추천해주는 것이다.
     - 여기에서 `apt`는 unbuntu와 같은 것에서 사용하는 일종의 앱스토어

2. 지금 ubuntu에서 사용되는 앱들을 업데이트 한다.

   - ```terminal
     $ sudo apt update
     ```

3. jdk 설치

   - ```terminal
      sudo apt install default-jdk
     ```

   

## Eclipse 설치

- Eclipse 검색해서 설치하면 된다.





