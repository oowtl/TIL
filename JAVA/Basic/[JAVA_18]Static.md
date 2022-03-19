# Java 18. Static

- 참조
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



## 부가설명

- static : class method

- no static : instance method

  - method 가 instance 에 소속되면 static 을 빼야 한다.

  - ```java
    class Print{	
    	public String delimiter;	
    	public void a() {
    		System.out.println(this.delimiter);
    		System.out.println("a");
    		System.out.println("a");
    	}
    }
    
    public class staticMethod {
    
    	public static void main(String[] args) {
    	
    		Print t1 = new Print();
    		t1.delimiter = "-";
    		t1.a();
    		t1.b();
    	}
    }
    ```

    - `Print t1 = new Print();` 
      - Print 의 인스턴스를 사용한다.
        따라서 `public void a()` 이런 방식으로 사용하게 된다. (`static` 빠진다.)
    - `static` 이 없으면 `class` 의 소속이 아니다.
      - `Print.a()`  <= 이런 식으로 호출할 수 없게 된다.



### 정리

1. `static` 이 있으면 `class` 소속이다. class 를 그대로 가져다 사용하면 된다.
2. `static`이 없으면 `instance`소속이다. instance를 생성해서 가져다 사용하면 된다.



## 활용

```java
class Foo{
	public static String classVar = "I class var";
	public String instanceVar = "I instance var";
	public static void classMethod() {
		System.out.println(classVar); // OK
//		System.out.println(instanceVar); //Error
	}
	public void instanceMethod() {
		System.out.println(classVar); // OK
		System.out.println(instanceVar);  // OK
	}
}
public class StaticApp {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println(Foo.classVar); // OK
//		System.out.println(Foo.instanceVar); //Error
	}

}
```

- Static 변수
  - 해당 class 안의 static method 에서 : 사용가능 ( O )
  - 해당 class 안의 instance method 에서 :  사용가능 ( O )
  - main method 안에서 : 사용가능 ( O )
- Instance 변수
  - 해당 class 안의 static method 에서 : 사용불가 ( O )
  - 해당 class 안의 instance method 에서 :  사용가능 ( X )
  - main method 안에서 : 사용불가 ( O ) 



### static 과 instance 변수의 차이

- 인스턴스를 만들었을 때
  - class 에 속하는 (`static`) 변수는 class 의 값을 가르킨다.
    즉. 이 변수의 값이 바뀌면 나머지 인스턴스에서도 전부 바뀌게 된다.
  - instance 에 속하는 변수는 instance 안에서만 바뀐다.
    따라서 아무리 바꿔도 Instance 안에서만 논다.
    몇 개를 해도 똑같을 것이다.





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



## Tips

### 이클립스에서 자동으로 메서드 만들기

1. 메서드 작성

   - ```java
     public static void main(String[] args) {
     		// TODO Auto-generated method stub
     		b();
     	}
     ```

2. code 우측의 빨간, 전구 혹은 code 상에서 빨간 줄 클릭

3. create method 클릭

   - ```java
     private static void b() {
         // TODO Auto-generated method stub
     }
     ```

     - private 로 만들어진다.