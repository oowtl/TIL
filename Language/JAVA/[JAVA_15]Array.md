# Java 15. Array



## 형태

- ```java
  double[] dividendRates = new double[3];
  ```

  - ```java
    double[] dividendRates
    ```

    - `dividendRates` 라는 이름을 가진 `double` 형의 자료가 들어가는 수납상자

  - ```java
    new double[3];
    ```

    - `double[3]` : double 형 자료가 3개 들어간다.

- ```java
  double[] dividendRates = new double[3];
  dividendRates[0] = 0.5;
  dividendRates[1] = 0.3;
  dividendRates[2] = 0.2;
  
  double dividend1 = income * dividendRates[0];
  double dividend2 = income * dividendRates[1];
  double dividend3 = income * dividendRates[2];
  ```

  - 여러가지 값을 한 곳에서 관리할 수 있다는 점에서 메리트가 있다.

