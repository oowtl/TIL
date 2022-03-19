# Java 16. Loop

- 반복문
- 어떤 동작을 반복적으로 수행하도록 하는 것



## While



### 형태

- ```java
  while ( 반복가능조건 ) {
  	반복동작할 실행문    
  } 
  ```

  - 반복가능조건 : 이 조건이라면 반복을 더 해야한다.
  - 반복동작할 실행문 : 조건을 통과했다면 이 행동을 해야한다.

- ```java
  double[] dividendRates = new double[3];
  dividendRates[0] = 0.5;
  dividendRates[1] = 0.3;
  dividendRates[2] = 0.2;
  
  int i = 0;
  while(i < dividendRates.length) {
      System.out.println("Dividend : " + (income * dividendRates[i]));
      i = i + 1;
  }
  ```

  - `int i = 0;`
    - 반복가능조건을 만들어주기 위해서 선언한 것

  

## for

- 몇 번 반복해라 를 수행하는 반복문
- 한번 실행이 되고 끝이다.



### 형태

- ```java
  for (int j=0; j < 3; j++) {
      System.out.println(2);
      System.out.println(3);
  }
  ```

  - ```java
    for ( 사용할 변수 초기화; boolean 조건문; 변수 증가량 ) {
        실행문
    }
    ```





### 반복문 활용하기

- ```java
  String[] users = new String[3];
  users[0] = "junn";
  users[1] = "mcg";
  users[2] = "gary";
  
  for ( int i=0; i < users.length; i++) {
      System.out.println("<li>" + users[i] + "</li>");
  }
  
  // 결과
  <li>junn</li>
  <li>mcg</li>
  <li>gary</li>
  ```



### 주의할 점

- 인덱스 에러
  - 배열의 숫자를 정해놓고 그것보다 덜 하던지 더 하면 안된다!!!



## break

- 반복문을 종료한다.

- ```java
  for (int i=0; i < users.length; i++) {
  			String currentId = users[i];
  			if (currentId.equals(inputId)) {			
  				break;
  			}
  		}
  ```

