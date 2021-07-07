## Instance

### concept

- 어떤 내부적인 상태를 말하는 것을 통해서 지칭하는 것을 다르게 유지할 수 있다.
- 하나의 클래스를 사용하는 것보다는 하나의 클래스를 `new`를 통해서 복제하는 것으로 변수에 담아서 사용하는 것이 더 효율적일 수 있다.
  - 복제한 클래스가 장기적, 반복적으로 사용될 수록 더 효율적으로 사용될 수 있다.
- Math vs PrintWriter
  - Constructors : Instance 로 사용하기위해서 받는 것
    - Math : Constructors X
    - PrintWriter : Constructors O
      - Constructor를 사용해서 Instance 를 만드는 것이 허용되어있다.
      - ![image-20210707233150704]([JAVA_10]Instance.assets/image-20210707233150704.png)
        - 이런 식으로 다 나와있다!
        - 어떤 값이 들어와야하는지, 어떤 에러가 날 수 있는지 등

### Constructor

- 생성자

- new 연산자와 같이 사용되어 클래스로부터 객체를 생성할 때 호출되어 객체의 초기화를 담당한다.

- 인스턴스 변수의 초기화 작업에 주로 사용되며, 인스턴스 생성 시에 실행되어야 할 작업을 위해서도 사용된다.

  

### 실습

- ```java
  public class InstanceApp {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		PrintWriter p1 = new PrintWriter("result1.txt");
  		
  	}
  
  }
  
  ```

  - `new PrintWriter` : PrintWriter 의 복제본, 아바타를 만든다.
  - `p1` : 복제본, 아바타를 변수에 담다. 이 변수에 담겨져 있는 어떠한 무엇인가를 PrintWriter 라는 class 의 인스턴스라고 한다.
  - `PrintWriter p1` : 이 변수에는 PrintWriter 라는 형식만 들어가도록 규제하기 위해서.

- 에러 없애기

  1. 불러오기

     - 	import java.io.PrintWriter;
       	
       	public class InstanceApp {
       	
       		public static void main(String[] args) {
       			// TODO Auto-generated method stub
       			
       			PrintWriter p1 = new PrintWriter("result1.txt");
       			
       		}
       	
       	}

       - Math 와 다르게 PrintWriter 는 패키지를 가져오는 작업을 해야한다.

  2. 예외상황

     - 파일을 읽으려고 하는데 파일이 없으면 문제가 생길 수가 있다. => 예외상황
     - 만약 이 상황에서 파일이 없어서 빨간 줄이 나온다면?
       - ![image-20210707231655683]([JAVA_10]Instance.assets/image-20210707231655683.png)
         - 마우스 대고 `Add throws declaration` 선택