# JAVA 3. 변수



- 그 값이 변할 수 있는 문자



## 타입별 변수 선언해주기

### int

- ```java
  
  public class Variable {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		int a = 1; // 1 : Number -> integer (-4,-2, 0, 1, 4 ....)
  		System.out.println(a); // 1
  	}
  
  }
  
  ```

  - 정수



### double

- ```java
  
  public class Variable {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  //		int b = 1.1; // 선언한 것과 그 값이 다르면 안된다.
  		double b = 1.1; // real number -> double ... -2.0, -1.0, 0, 1.0, 2.0 ...
  		System.out.println(b);
  
  	}
  
  }
  
  ```

  - 실수



### String

- ```java
  
  public class Variable {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  
  //		int c = "Hello World" // 문자열도 안된다.
  		String c = "Hello World"; // 문자열을 String
  		System.out.println(c);
  		
  	}
  
  }
  
  ```





### Tip : 왜 데이터를 넣을 때 마다 그 타입을 선언해줘야 하는가?

- 변수에는 반드시 정수가 온다 문자열이 온다 라고 미리 말해준다면
  그 값이 아닌 다른 값이 들어가면 java가 컴파일되지 않는다.
  그 말은 그 값을 사용할 때마다 확인을 하지 않아도 된다. 거기에서 실행하는데 이점을 가진다.
  		



## 변수의 기능

- ```java
  
  public class Letter {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  
  		String name = "leezche";
  		System.out.println("Hello, " + name + " ... " + name + " ... egoing ... bye");
  		
  		double VAT = 10.0;
  		System.out.println(VAT);
  		
  		// 각각의 변수들은 하나의 값으로 기능을 하지만 동시에 의미를 표현하는 것이기도 하다.
  	}
  
  }
  
  ```

  - 출력문에서 name, VAT 가 아닌 leezche, 10.0 이 들어간다면, 다른사람이 봤을 때는 무슨 의미인지 모를 것이다.
    따라서 변수를 통해서 그 의미를 알도록 하는 것 또한 변수의 역할이다.



## CASTING

- 어떤 데이터 타입을 다른 데이터 타입으로 변환하는 것
- 손실이 없는 경우에 자동으로 casting 된다.



### int to double

- ```java
  
  public class Casting {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  
  		double a = 1.1;
  		double b = 1;
  		double b2 = (double) 1; 
  		System.out.println(b); // 1.0
  		// 자동으로 바꿔서 나온다.
  	}
  
  }
  
  ```

  - 1 의 경우에는 double 로 해도 1.0 으로 손실이 없다.



### double to int

- ```java
  
  public class Casting {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  //		int c = 1.1; // error
  		double d = 1.1; 
  		int e = (int) 1.1; // 강제로 int 로 바꿔주겠다는 뜻이다.
  		System.out.println(e); // 1 (내림), 손실
  		
  		// 자동으로 casting 되는 것은 손실이 없는 경우이다.			
  	}
  
  }
  
  ```

  - 1.1 의 경우에 int 가 되려면 1이 되는데, 그때 0.1 의 손실이 존재한다.
    따라서 자동으로 변환되지 않는다.

    - 강제로 하는 방법은

      ```java
      int e = (int) 1.1;
      ```

      `(데이터 타입) 변환값` 의 방식으로 해주는 것으로 바꾼다.



### 1 to String

- ```java
  public class Casting {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  
  		// 1 to String
  		String f = Integer.toString(1);
  		System.out.println(f); // 1
  		System.out.println(f.getClass()); // class java.lang.String
  	}
  
  }
  ```

  - `Integer.toString( 정수 )` : int -> String
  - `.getClass()` : 데이터 타입을 확인하는 메소드

