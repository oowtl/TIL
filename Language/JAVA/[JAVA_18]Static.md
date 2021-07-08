# Java 18. Static

- [망나니개발자 블로그](https://mangkyu.tistory.com/47)



## 개념

- 메모리에 한번 할당되어서 프로그램이 종료될 때 해제되는 것을 의미한다.
- 메모리 상에서의 동작
  - 일반적으로 Class 는 Static영역에 생성되고
    new 연산을 통해서 생성한 객체는 Heap 영역에 생성된다.
    Heap 영역의 메모리는 Garbage Collector 를 통해서 수시로 관리를 받는다.
  - 장점
    -  Static 키워드를 통해서 Static 영역에 할당된 메모리는 **모든 객체가 공유하는 메모리**라는 장점을 지닌다.
  - 주의할점
    - Garbage Collector의 관리 영역 밖에 존재하므로 Static을 자주 사용하면 프로그램의 종료시까지 메모리가 할당된 채로 존재하므로 퍼포먼스에 악영향을 준다.



## Static 변수

- 클래스 변수이다.
- 객체를 생성하지 않고도 Static 자원에 접근이 가능하다.



### 예시

- ```java
  public class Person { 
      private String name = "MangKyu";
      public void printName() {
          System.out.println(this.name);
      }
  }
  ```

  - 이 class 를 1000번 돌리면 저 name 1000개가 메모리를 차지하게 될 것이다.
  - 같은 값을 가진 것이 1000개나 중복될 필요가 없다.

- 해결

  - ```java
    public class Person {     
        public static final String name = "MangKyu";
        public static void printName() {
            System.out.println(this.name);     
        } 
    }
    ```

    - `static` : 여러 객체가 참조 가능하도록 한다.
    - `final` : 결코 변하지 않는 값이라서 붙여준다.



### 활용

- 일반적으로 상수들만 모아서 사용한다.
- 변수명은 대문자와 `_` 를 조합해서 짓는다.
- 상속방지를 위해서 `final class`로 선언한다.





## Static 메소드

- 정적 메소드
- 객체의 생성없이 호출이 가능하다.
- 객체에서는 호출이 가능은 하지만 지양하고 있다.



### 활용

- 상속방지를 위해서 `final class`로 선언한다.
- 유틸 관련된 함수들을 모아둔다.