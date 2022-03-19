# Java 20. OOP



- method programming = procedural programming
  - 절차 지향적 프로그래밍
    - 기존의 프로그래밍 방법
- method 만으로는 좀 부족한 것 같아!
  - 서로 연관된 것들을 모아서 그룹핑하고 이름 붙여서 사용할게!!
    - object oriented programming
      객체 지향적 프로그래밍



## 남의 클래스, 남의 인스턴스 사용하기



### Math

- ```java
  System.out.println(Math.PI);
  System.out.println(Math.floor(1.8));
  System.out.println(Math.ceil(1.3));
  ```

  - Math 라는 것은 서로 연관된 주제를 가진 메소드와 변수를 넣어 놓은 것이다.
  - 이 것들의 특징을 보면 한 번 사용하고 만다.
    그래서 그냥 class 에 두고 사용한다.



### FileWriter

- ```java
  FileWriter f1 =new FileWriter("data.txt");
  ```

  - ```java
    new FileWriter("data.txt");
    ```

    - `data.txt` 라는 파일에다가 저장을 하겠다는 상태를 지닌 `FileWriter`라는 class 의 복제본이 생긴다.

- ```java
  FileWriter f2 =new FileWriter("data2.txt");
  ```

  - f1 과 f2 는 서로 다른 것이다.

- 주의사항

  - `FileWriter`import 를 하기
  - 예외사항 체크하기
    - 모르면 `Add throw declaration`선택

- **정리**

  - f1, f2 를 보면 여러번 사용하게 되는 것이다.
    그래서 계속 클래스 하나로 하면 번거로운 작업이 된다.
    만약 클래스에서 필요한 값을 따로따로 유지할 수 있다면 편할 것이다.
    인스턴스로 만들어서 쭉~ 사용한다.



## 변수와 메서드 만들기

- method 안에서 정의 한 변수는 그 method 안에서만 사용가능하다.

  - ```java
    public class MyOOP {
    	public static void main(String[] args) {
    		// TODO Auto-generated method stub
    		
    		delimiter = "----";
    		printA();
    		printA();
    		printB();
    		printB();
    		
    		delimiter = "****";
    		printA();
    		printA();
    		printB();
    		printB();
    	}
        
        public static String delimiter = "";
    	public static void printA() {
    		System.out.println(delimiter);
    		System.out.println("A");
    		System.out.println("A");
    	}
    	public static void printB() {
    		System.out.println(delimiter);
    		System.out.println("B");
    		System.out.println("B");
    	}
    }
    ```

    - ```java
      public static String delimiter = "";
      
      public static void printA() {
          System.out.println(delimiter);
      }
      ```

      - public 으로 해줘야 사용할 수 있다.

    - ```java
      public static void main(String[] args) {
          // TODO Auto-generated method stub
      
          String delimiter = "----";
          printA();
          printA();
          printB();
          printB();
      }
      
      public static void printA() {
          System.out.println(delimiter);
          System.out.println("A");
          System.out.println("A");
      }
      ```

    - 이런 식으로 `main` method 에서 `String delimiter = "----";` 를 정의하고
      `printA` method 에서 `delimiter`를 사용하려고 하면 안될 것이다.



## 클래스 만들기

- ```java
  class Print {
  	public static String delimiter = "";
  	public static void A() {
  		System.out.println(delimiter);
  		System.out.println("A");
  		System.out.println("A");
  	}
  	public static void B() {
  		System.out.println(delimiter);
  		System.out.println("B");
  		System.out.println("B");
  	}
  }
  public class MyOOP {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		Print.delimiter = "----";		
  		Print.A();
  		Print.A();
  		Print.B();
  		Print.B();
  
  		Print.delimiter = "****";
  		Print.A();
  		Print.A();
  		Print.B();
  		Print.B();
  	}
  }
  ```

  - 연관된 것을 모아서 그룹핑을 하고 이름을 지은 것이 class 이다.



## 인스턴스 만들기

- 원형인 Class 를 복제한 Instance

- `new` : 인스턴스를 만드는, 클래스를 복제하는

- 예시

  - ```java
    class Print {
    	public String delimiter = "";
    	public void A() {
    		System.out.println(delimiter);
    		System.out.println("A");
    		System.out.println("A");
    	}
    	public void B() {
    		System.out.println(delimiter);
    		System.out.println("B");
    		System.out.println("B");
    	}
    }
    public class MyOOP {
    	public static void main(String[] args) {
    		// TODO Auto-generated method stub
    		
    		Print p1 = new Print();
    		p1.delimiter = "----";
    		p1.A();
    		p1.A();
    		p1.B();
    		p1.B();
    		
    		Print p2 = new Print();
    		p2.delimiter = "****";
    		p2.A();
    		p2.A();
    		p2.B();
    		p2.B();
    
    		p1.A();
    		p2.A();
    		p1.B();
    		p2.B();
    	}
    }
    
    ```

    - `public String delimiter = "";`
      `public void A()`
      `public void B()`
      - 인스턴스로 사용한다면 `static`을 빼야한다.

