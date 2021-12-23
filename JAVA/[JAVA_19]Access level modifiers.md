# Java 19. Access level modifiers

- `public`, `protected`, `default`, `private`
- 외부에서 접근할 수 있는 레벨을 말한다.



## public

- 어느 곳에서든 공동으로 사용할 수 있도록 하는 것

- ```java
  class Greeting{
  	//public, protected, default, private
  	public static void hi() {
  		System.out.println("hi");
  	}
  		
  }
  public class AccessLevelModifiersMethod {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		Greeting.hi();
  	}
  }
  ```

  - `class Greeting` 안의 `public static void hi()`

    - `Greeting.hi();` 

    - ```java
      class이름.method이름();
      ```



## private

- 같은 클래스 안에서만 사용할 수 있는 것

- ```java
  public class AccessLevelModifiersMethod {
  
  	private static void hi() {
  		System.out.println("hi");
  	}
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		hi();
  	}
  
  }
  ```

  - `private static void hi()` 이 `public class AccessLevelModifiersMethod` 안에 있다.

    - ```java
      public static void main(String[] args) {
          // TODO Auto-generated method stub
          hi();
      }
      ```

      - `hi()` 를 사용할 수 있다.

- ```java
  class Greeting{
  	//public, protected, default, private
  	private static void hi() {
  		System.out.println("hi");
  	}
  }
  public class AccessLevelModifiersMethod {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		hi();
  	}
  
  }
  ```

  - `class Greeting` 안에 `private static void hi()`이 있다.
    - `public class AccessLevelModifiersMethod ` 안에서 `hi()`는 실행되지 않는다.

`