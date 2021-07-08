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

  

