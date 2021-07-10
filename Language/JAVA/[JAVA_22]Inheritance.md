# Java 22. Inheritance

```java
class Cal{
	public int sum(int v1, int v2) {
		return v1+v2;
	}
}
public class InheritanceApp {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Cal c = new Cal();
		System.out.println(c.sum(2, 1));
	}
}
```

- 상상해보자!

  - 만약 내가 만든 Cal 클래스가 너무 유명해져서 많은 사람들이 사용하고 있거나
    내가 만들지 않고 다른 사람이 만들었는데, 나는 조금 다른 기능을 원한다면??
    여기에 다른 기능을 추가하고 싶다면??

    - 클래스 자체를 복사해서 거기다 추가하는 것도 방법이 될 수 있다.

      ```java
      class Cal2{
      	public int sum(int v1, int v2) {
      		return v1+v2;
      	}
      	public int minus(int v1, int v2){
      		return v1 - v2;
      	}
      }
      ```

      - 이런 식으로 하나 새롭게 만들어서 할 수도 있다.
        좋은 방법이다.
        단지 sum 부분이 바뀌는 것을 모르며, 하나하나 다 수정해야해서 코드가 길어지면 거의 불가능에 가깝다는 단점이 있다.



## 사용

- ```java
  class Cal3 extends Cal {
  	
  }
  public class InheritanceApp {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub		
  		Cal3 c3 = new Cal3();
  		System.out.println(c3.sum(2, 1));
  	}
  }
  ```

  - 순서
    1. c3 에서 sum 메소드를 찾는다.
    2. c3 에 sum 메소드가 없는 것을 확인한 후에 extends Cal 을 본다.
    3. Cal 클래스에서 sum 메소드를 확인하고 실행한다.



## 정의

- 어떤 클래스가 있을 때, 그 클래스가 있는 변수와 메소드를 확장해서, 상속해서 다른 클래스가 가지고 있을 수 있도록 하는 것



## 상속받은 기능을 개선과 발전시키기

- ```java
  class Cal3 extends Cal {
  	public int minus(int v1, int v2){
  		return v1 - v2;
  	}
  }
  
  public class InheritanceApp {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		Cal3 c3 = new Cal3();
  		System.out.println(c3.sum(2, 1));
  		System.out.println(c3.minus(2, 1));
  	}
  }
  ```

  - 추가하고 싶은 기능은 그대로 추가하면 된다.

- 부모가 가지고 있는 메서드를 재정의할 수도 있다.
  부모가 가지고 있는 것을 덮어썼다.

  - Overridding

    - ```java
      class Cal{
      	public int sum(int v1, int v2) {
      		return v1+v2;
      	}
      }
      class Cal3 extends Cal {
      	public int minus(int v1, int v2){
      		return v1 - v2;
      	}
      	public int sum(int v1, int v2) {
      		System.out.println("Cal3");
      		return v1+v2;
      	}
      }
      public class InheritanceApp {
      	public static void main(String[] args) {
      		// TODO Auto-generated method stub
      		Cal3 c3 = new Cal3();
      		System.out.println(c3.sum(2, 1));
      		System.out.println(c3.minus(2, 1));
      	}
      }
      ```

      - Cal 클래스에 있는 sum 메소드를 Cal3 클래스에서 재정의 했다.
        그렇다면 어떻게 실행될까??

        - c3 인스턴스에서 sum 메소드를 찾으면 바로 있을 것이고, 그것을 실행할 것이다.

          ```console
          Cal3
          3
          1
          ```

          

## Overriding vs Overloading

### Overriding

- 부모 클래스의 메소드를 자식 클래스에서 재정의 하다.



### Overloading

- 과적하다?

- 기본적으로 상속과는 상관이 없다.

- ```java
  class Cal{
  	public int sum(int v1, int v2) {
  		return v1+v2;
  	}
  	//Overloading
  	public int sum(int v1, int v2, int v3) {
  		return v1+v2+v3;
  	}
  }
  public class InheritanceApp {
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		Cal c = new Cal();
  		System.out.println(c.sum(2, 1));
  		System.out.println(c.sum(2, 1, 1));
  	}
  }
  ```

  - 메소드의 이름을 얼마든지 같게해도 상관이 없다.
    단, 형태가 달라야 한다.
    위의 오버로딩은 파라미터, 매개변수의 갯수가 달라졌다. 

- Overloading 은 메소드이기 때문에 당연히 Overriding 도 가능하다.

  - ```java
    class Cal{
    	public int sum(int v1, int v2) {
    		return v1+v2;
    	}
    }
    class Cal3 extends Cal {
    	public int minus(int v1, int v2){
    		return v1 - v2;
    	}
    	//Overrring
    	public int sum(int v1, int v2) {
    		System.out.println("Cal3");
    		return v1+v2;
    	}
    	//Overloading
    	public int sum(int v1, int v2, int v3) {
    		return v1+v2+v3;
    	}
    }
    ```

    - 자식 클래스가 사용하능하다.



## this & super



### super

- 자식 클래스에서 부모 클래스를 지칭하는 말

- ```java
  class Cal{
  	public int sum(int v1, int v2) {
  		return v1+v2;
  	}
  	//Overloading
  	public int sum(int v1, int v2, int v3) {
  		return v1+v2+v3;
  	}
  }
  class Cal3 extends Cal {
  	public int minus(int v1, int v2){
  		return v1 - v2;
  	}
  	//Overrring
  	public int sum(int v1, int v2) {
  		System.out.println("Cal3");
  		return super.sum(v1, v2);
  	}
  }
  ```

  - Cal3 클래스의 sum 메소드의 return 을 보면 `return super.sum(v1, v2);` 이다.
    부모 클래스 Cal 의 sum 메소드를 실행시키는 것이다.



### this

- 자기 자신의 인스턴스를 나타낸다.

- ```java
  class Cal{
  	public int sum(int v1, int v2) {
  		return v1+v2;
  	}
  	//Overloading
  	public int sum(int v1, int v2, int v3) {
  		return this.sum(v1, v2)+v3;
  	}
  }
  ```

  - `return this.sum(v1, v2)+v3;`
    명시적으로 자기 자신을 나타내는 것을 통해서 좀 더 이해하기 쉽게 만든 것이 포인트이다.



## 상속과 생성자

- ```java
  class Cal{
  	int v1, v2;
  	Cal(int v1, int v2) {
  		this.v1 = v1;
  		this.v2 = v2;
  	};	
  }
  class Cal3 extends Cal {
  	
  }
  ```

  - Cal 클래스에서 생성자를 받았다. Cal3 클래스가 Cal  클래스를 계승하기 위해서는 생성자까지 받아와야한다.
    그래서 상속하는 부모 클래스가 생성자를 사용하면 자식 클래스도 사용하도록 강제한다고 생각하는 것이 편하다!

- ```java
  class Cal{
  	int v1, v2;
  	Cal(int v1, int v2) {
          System.out.println("Cal init!");
  		this.v1 = v1;
  		this.v2 = v2;
  	};	
  }
  class Cal3 extends Cal {
  
  	Cal3(int v1, int v2) {
  		super(v1, v2);
  		// TODO Auto-generated constructor stub
          System.out.println("Cal3 init!!!");
  	}
  }
  ```

  - 상속자를 받은 모습

  - `super(v1, v2);`

    - 부모 클래스의 생성자인 `Cal(int v1, int v2)` 을 실행시킨다.

  - 그 후에는 Cal3 에서 처리해야할 것을 처리하면 된다.

  - 결과

    - ```console
      Cal init!
      Cal init!
      Cal3 init!!!
      ```

- 약간의 예시

  - ```java
    class Cal{
    	int v1, v2;
    	Cal(int v1, int v2) {
    		System.out.println("Cal init!");
    		this.v1 = v1;
    		this.v2 = v2;
    	};
    	public int sum() {return this.v1+v2;}
    }
    class Cal3 extends Cal {
    
    	Cal3(int v1, int v2) {
    		super(v1, v2);
    		// TODO Auto-generated constructor stub
    		System.out.println("Cal3 init!!!");
    	}
    	public int minus() {
    		return this.v1 - v2;
    	}
    }
    public class InheritanceApp {
    
    	public static void main(String[] args) {
    		// TODO Auto-generated method stub
    		Cal c = new Cal(2, 1);		
    		Cal3 c3 = new Cal3(2, 1);	
            
    		System.out.println(c3.sum());
    		System.out.println(c3.minus());	
    	}
    }
    ```

    - ```console
      Cal init!
      Cal init!
      Cal3 init!!!
      3
      1
      ```





## 공부 키워드

1. 클래스간의 호환성 문제, 자식클래스와 부모클래스로서 동작하도록 하는 기능
   - polymorphism, 다형성
2. Access Modifiers
   - public, default, protected, private
3. Final
   - 상속불가, 오버라이드 불가
4. Abstract
   - 특정 메소드 구현 강제

