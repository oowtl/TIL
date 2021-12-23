# Java 21. Constructor and this



## Constructor

- class 가 처음 만들어질 때 필요한 값을 받아오거나
  반드시 계산해야할 초기 값 등을 해결하기 위해서
  **Constructor**를 사용한다.

- 예시

  - ```java
    Print p2 = new Print();
    p2.delimiter = "****";
    ```

    - `p2.delimiter = "****";`
      - 사용자가 이 부분을 항상 입력하기 힘들다!
      - 어떻게 해볼까?

  - ```java
    Print p2 = new Print("****");
    ```

    - 처음부터 입력하는 것을 통해서 편하게 하자 (실수를 차단해보자)
    - 이렇게 받아오려면 생성자 (Constructor) 라는 것을 정의해야한다.



### 정의하기

```java
class Print {
	public String delimiter = "";
	public Print() {	
	}
	
}
public class MyOOP {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Print p1 = new Print("----");
		Print p2 = new Print("****");
	}
}

```

- java 에서 class 는 생성자라고 하는 특수한 method 를 구현할 수 있는 기능을 제공한다.
- class 와 똑같은 이름의 method 를 정의하면 생성자이다.
  - 인스턴스를 생성할 때, 이 클래스와 동일한 이름의 메소드가 있다면
    그 메소드를 호출하도록 되어있다.
  - 이것을 통해서 클래스가 인스턴스화 될 때 실행되어야 할 코드를 constructor method 안에 정의하는 것으로 초기화를 할 수가 있다.

```java
class Print {
	public String delimiter = "";
	public Print(String _delimiter) {
		delimiter = _delimiter;
	}
}
```

- 이런 방식으로 활용하면 된다.



### 주의사항

- ```java
  class Print {
  	public String delimiter = "";
  	public Print(String delimiter) {
  		delimiter = delimiter;
  	}
  }
  ```

  - `delimiter = delimiter;` 
    - 무엇이 무엇을 가르킬까???
      - 이 코드 상으로는 `public String delimiter ="";`의 `delimiter`를 지칭한다.
      - 이럴 때 우리는 어떻게 하는 것으로 지칭하는 것을 구분할 수 있을까??
        - `this`사용



## this

- 생성한 instance를 가리키는 이름이다.

- ```java
  class Print {
  	public String delimiter = "";
  	public Print(String delimiter) {
  		this.delimiter = delimiter;
  	}
  	public void A() {
  		System.out.println(this.delimiter);
  		System.out.println("A");
  		System.out.println("A");
  	}
  	public void B() {
  		System.out.println(this.delimiter);
  		System.out.println("B");
  		System.out.println("B");
  	}
  }
  
  public class MyOOP {
  	public static void main(String[] args) {
          // TODO Auto-generated method stub
  
          Print p1 = new Print("----");
          p1.A();
          p1.A();
          p1.B();
          p1.B();
      }
  }
  ```

  - 이 상황에서 `this` 는 `p1` 이라는 생성된 인스턴스를 지칭한다. 

