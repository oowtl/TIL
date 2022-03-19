# Java 13. Input Argument Variable



## Double.parseDouble()

- `args` 로 받아오기만 하면 되나요?!

  - 우리는 `Run Configurations` 라는 것을 통해서 `public static void main(Stirng[] args)` 라는 곳에서 `args` 를 정해서 받아올 수 있다.

  - 

    ```java
    double valueOfSupply = args[0];
    ```

    - 이 경우에는 가능할까????
    - String 과 double 은 다른 데이터 타입이라서 불가능하다.
      -> 따라서 형변환을 시켜줘야 한다. 

- `Double.parseDouble(String)`

  - 들어온 String 을 Double 로 바꿔주는 것



## Tips



### 이클립스 없이 java 실행하기

- 순서

  1. `Navigator` ( 이클립스 좌측) 창에서 해당 `Project` 우 클릭 

  2. `Properties` 

  3. `Location` (경로) 카피 

  4. `윈도우 터미널` 실행

  5. `cd 해당경로` (`ls` or `dir` 로 확인)

  6. java 실행

     1. ```cmd
        java 실행할파일명
        ```



### 터미널에서 java 실행 시 입력값 넣어주기

- ```cmd
  java 실행할파일명 입력값
  ```

  - ```cmd
    > java AccountingApp 33333.0
    
    Value of supply : 33333.0
    VAT : 3333.3
    Total : 36666.3
    Expense : 9999.9
    Income : 23333.1
    Dividend 1 : 11666.55
    Dividend 2 : 6999.929999999999
    Dividend 3 : 4666.62
    ```



### 다른 컴퓨터에서 java 파일을 실행시키고 싶다면??

- `.class`파일을 옮겨서 실행시키면 된다.
- 단, java 가 설치되어 있어야 한다.
  - java 가 없다면 Launch4j 라는 것을 통해서 실행시킬 수 있다.



## Errors

### Complie 하는 것과 Run 하는 것의 버전이 안맞을 때.

- 상황

  - cmd 에서 java project 폴더로 가서 java 를 실행하려고 했다.

  - ```cmd
    > java AccountingApp
    
    Error: LinkageError occurred while loading main class AccountingApp
            java.lang.UnsupportedClassVersionError: AccountingApp has been compiled by a more recent version of the Java Runtime (class file version 60.0), this version of the Java Runtime only recognizes class file versions up to 55.0
    ```

- 해결

  - 설치된 java 가 하나 뿐이라서 다른 설정이 필요없을 것이라고 추측
    `.class` 파일을 한번 더 만들어 보자.

  - 순서

    1. `.class` 파일 삭제하기

       - ```cmd
         del AccountingApp.class
         ```

    2. `.class` 파일 만들어주기

       - ```cmd
         java AccountingApp.java
         ```

