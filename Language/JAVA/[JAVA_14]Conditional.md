# Java 14. Conditional



### 형태

- ```java
  if ( 조건 ) {
      실행문
  } else {
  	실행문
  }
  ```

  - if - else 를 셋트라고 생각하면 된다.

- 

  ```java
  if ( 조건 ) {
      실행문
  } else if ( 조건 ) {
      실행문
  }
  else {
  	실행문
  }
  ```



## 사용

- ```java
  double dividend1;
  double dividend2;
  double dividend3;
  				
  if (income > 10000.0) {
      dividend1 = income * 0.5;
      dividend2 = income * 0.3;
      dividend3 = income * 0.2;
  } else {
      dividend1 = income * 1.0;
      dividend2 = income * 0;
      dividend3 = income * 0;
  }
  ```

  - `if ( 조건 ) { 실행 } else { 실행 }`
  - 변수들은 미리 선언해줘야 한다.



## 주의사항



### else 를 꼭 사용해주자.

- if 로 시작을 하면 else 까지 해야한다. 문장의 처음과 끝으로 생각하는 것 같다.

- ```java
  if (income > 10000.0) {
      dividend1 = income * 0.5;
      dividend2 = income * 0.3;
      dividend3 = income * 0.2;			
  } else if (income > 50000.0) {
      dividend1 = income * 0;
      dividend2 = income * 1;
      dividend3 = income * 0;
  }
  ```

  - 이렇게 할 떄 `dividend1, dividend2, dividend3` 변수를 사용하는 곳에서 인식을 하지 못했다.
  - else 문에서 추가를 하면

- ```java
  if (income > 10000.0) {
      dividend1 = income * 0.5;
      dividend2 = income * 0.3;
      dividend3 = income * 0.2;
  
  } else if (income > 50000.0) {
      dividend1 = income * 0;
      dividend2 = income * 1;
      dividend3 = income * 0;
  } else {
      dividend1 = income * 1;
      dividend2 = income * 0;
      dividend3 = income * 0;
  }
  ```

  - 오류가 나지 않는다.
  - 아무래도 else 를 통해서 변수들을 감지하는 것 같다.