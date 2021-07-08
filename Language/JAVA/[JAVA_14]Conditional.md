# Java 14. Conditional



## 형태

- ```java
  if ( 조건 ) {
      실행문
  } else {
  	실행문
  }
  ```

  - if - else 를 셋트라고 생각하면 된다.
  - **조건을 통해서 얻어지는 값은 boolean 만 올 수 있다.**
  
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
  
- 중첩 사용

  - ```java
    if (false) {
        System.out.println(1);
    }else {
        if (true) {
            System.out.println(2);
        }else {				
            System.out.println(3);
        }
    }
    ```

- 중첩 사용 개선 버전 `else if`

  - ```java
    if (false) {
        System.out.println(1);
    }else if(true) {
        System.out.println(2);
    }else {
        System.out.println(3);
    }
    
    ```



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



### Dead code

- 이클립스가 실행되지 않을 코드에 대해서 알려주기도 한다.

  - ```java
    if (false) {
    			System.out.println(1);
    		}
    ```

    - 노란줄과 함께 Dead code 라고 알려준다.



### equals vs `==`

- 두 가지는 과연 같을 까??

  - ```java
    String o1 = "java";
    String o2 = "java";
    
    String o3 = new String("java");
    String o4 = new String("java");
    ```

    - `o1 == o2`  -> true
      `o3 == o4` -> false
      - `new` 를 통해서 새로운 메모리에 할당된 것이기 때문이다.
    - `o1.equals(o2)` -> true
      - 내부적으로 계산해서 내용이 같다면 true 리턴
      - 단, 원시 데이터는 `equals()` 를 가지고 있지 않기 때문에 실행되지 않을 것이다.

- `==`

  - 같은 메모리 주소를 가르키는지 확인한다.

- `equals()`
  - 내부적으로 같은 내용인지 확인하는 것
  - 비원시 데이터가 동등한지 비교하기 위해서 사용한다.



#### String 의 특혜

- 비 원시 데이터라면 equals 를 통해서 비교하는 것이 맞겠지만,
  String 은 많이 사용하고 특별하기 때문에 `==` 동등 비교 연산자를 사용할 수 있게 해놓았다.