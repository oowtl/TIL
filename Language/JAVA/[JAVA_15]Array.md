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
  
- ```java
  String[] users = new String[3];
  ```

  - `String[]` : String 이 담길 배열 의 이름 `users`
  - `new` : 새로운 주소에 할당한다.
  - `String[3]` : String 이 담길 배열이며 그 길이는 3이다.



## 생성하는 방법

1. 빈 깡통을 만들고 넣는 방법

   - ```java
     String[] users = new String[3];
     users[0] = "junn";
     users[1] = "mcg";
     users[2] = "gary";
     ```

     - `users[0]`
       - 0 : index , 색인
     - `"junn"` : element

2. 만들면서 넣어주기

   - ```
     int[] scores = {10, 100, 1000};





## 이중 배열 만들기

- ```java
  String[][] users = {
      {"egoing", "1111"},
      {"jinhyuk", "2222"},
      {"junn", "3333"},
  };
  ```

  - users 의 원소는 배열이고
    그 배열들이 String 으로 이루어져 있다.

- ```java
  for (int i=0; i < users.length; i++) {
      String[] current = users[i];
      if (
          current[0].equals(inputId) && current[1].equals(inputPassword)
      ) {
          isLogined = true;
          break;
      }
  }
  ```

  - for 문을 통해서 배열을 하나 받아오고 여러가지를 활용한다.
