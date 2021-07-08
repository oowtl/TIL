# Java 17. Method

- 서로 연관된 코드를 그룹핑해서 이름을 붙인 정리정돈의 상자이다.
- 항상 모든 코드를 작성할 수 는 없다. 필요한 기능, 과정을 다른 파일에 넣어두고 필요할 때마다 불러서 사용하는 것이다.
- 다른 곳에서는 `Function` 이라고도 부른다.
  - 그렇다 `함수` 이다.



## 만들기

1. 드래그 ( 블럭 )
2. Refactor
3. Extract Method (`Alt + shift + M`)
4. Method name 작성
5. `Access modifier` 에서 해당하는 radio 버튼 선택 (`public` 으로 간다.)
6. 체크박스 `Replace 2 additional occurrences of statements with method` 체크 해제



## 형태

- Method 만드는 부분

  - ```java
    private static double getVAT(double valueOfSupply, double vatRate) {
    		return valueOfSupply * vatRate;
    	}
    ```
  
- Method 를 호출하는, 실행하는 부분

  - ```java
    double vat = getVAT(valueOfSupply, vatRate);
    ```

    - `getVAT(valueOfSupply, vatRate)`
      - 괄호 안의 `(valueOfSupply, vatRate)` : 입력값



## 매개변수, Parameter, 인자, Argument

- ```java
  public class WhyMethod {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  
  		printTwoTimes("A");
  		printTwoTimes("B");
  		printTwoTimes("C");
  		
  	}
  
  	public static void printTwoTimes(String text) {
  		System.out.println("_");
  		System.out.println(text);
  		System.out.println(text);
  	}
  }
  
  ```

  - `public static void printTwoTimes(String text)`

    - `printTwoTimes`라는 Method 는 String 형으로 `text` 라는 것을 받아온다.
      text 는 해당하는 Method 에서 변수로 활용된다.

    - 이런 변수들을(`String text`) method 밖에서 사용하는 쪽에서 주입하는 값을 method 안으로 흘러보내는 매개자라고 해서 **매개변수**라고 하고 영어로는 **parameter** 라고 한다.

  - `printTwoTimes("A");`

    - `("A")` : 인자, argument



### 다수의 값 받아오기

- ```java
  
  public class WhyMethod {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		printTwoTimes("A", "-");
  		printTwoTimes("B", "*");
  		printTwoTimes("C", "&");
  		
  	}
  
  	public static void printTwoTimes(String text, String delimiter) {
  		System.out.println(delimiter);
  		System.out.println(text);
  		System.out.println(text);
  	}
  }
  
  ```

  - `public static void printTwoTimes(String text, String delimiter)`
    - 두 개의 값을 받아와서 활용할 수 있다.



## 다수의 code 를 바꾸기


```java
	print(vat, total, expense, income, dividend1, dividend2, dividend3);	
}

public static void print(double vat, double total, double expense, double income, double dividend1,
		double dividend2, double dividend3) {
	System.out.println("Value of supply : " + valueOfSupply); 
	System.out.println("VAT : " + vat); 
	System.out.println("Total : " + total);
	System.out.println("Expense : " + expense);
	System.out.println("Income : " + income);
	System.out.println("Dividend 1 : " + dividend1);
	System.out.println("Dividend 2 : " + dividend2);
	System.out.println("Dividend 3 : " + dividend3);
}
```

- 여러 줄에도 상관없이 메소드로 바꿔줄 수 있다.

- 더 바꾸면

  ```java
  print();
  		
  	}
  	public static void print() {
  		System.out.println("Value of supply : " + valueOfSupply); 
  		System.out.println("VAT : " + getVAT()); 
  		System.out.println("Total : " + getTotal());
  		System.out.println("Expense : " + getExpense());
  		System.out.println("Income : " + getIncome());
  		System.out.println("Dividend 1 : " + getDividend1());
  		System.out.println("Dividend 2 : " + getDividend2());
  		System.out.println("Dividend 3 : " + getDividend3());
  	}
  ```

  

## 지역변수



### 상황

- ```java
  private static double getVAT() {
      return valueOfSupply * vatRate;
  }
  ```

  - method 를 하나 만들었다.
    이 method는 실행이 될까???

    - 실행되지 않는다!!!

  - `valueOfSupply`, `vatRate`  두 변수는 현재 메소드가 아닌 다른 곳에 존재한다.

    - ```java
      public static void main(String[] args) {
          
      	}
      ```

      - 다른 공간이라고 생각하면 된다.
        이거..뭔가 지역이 다른 것 같네???
        - 각각 존재하는 지역이 다르다고 해서 지역변수라고 한다.



## 전역변수

- 전역에서 사용할 수 있는 변수



### 만들기, 형태

- Code

  - ```java
    public static double valueOfSupply = 10000.0;
    public static void main(String[] args) {
    ```

- Refactor

  1. 드래그 ( 블록 ) - 우클릭
  2. `Refactor`
  3. `Convert Local Variable to Field`
  4. `Access modifier` - `public` radio 버튼
  5. `Finish`



### 결과

- ```java
  public static double valueOfSupply;
  	public static double vatRate;
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		valueOfSupply = 10000.0;
  		vatRate = 0.1;
  ```

  - 전역변수로 선언을 할 때는 변수만 선언해주고
    변수의 값은 main 이나 사용하는 method 안에서 정해주는 것이 좋다.



## Return

- 그 메소드의 return 뒤에 있는 것이 실행결과가 된다.
- return 값은 메소드를 종료시키는 역할을 하기도 한다.



### 형태

- ```java
  public static String a() {
      return "a";
  }
  ```

  - `public static String a() {`
    - `String a()` : 이것의 결과값의 형식은 String 이다.

- ```java
  public static int one() {
      return 1;
  }
  ```

  - `public static int one() {`
    - `int one()` : 결과 값의 형식이 Int 이다.

- 메소드는 그 메소드의 return 값이 어떤 데이터 타입을 가질지 정해줘야 한다.

- `void` : return 값이 없다는 것을 뜻하는 것이다.

- ```java
  public static String twoTimes(String text, String delimiter) {
      String out = "";
      out = out + delimiter + "\n";
      out = out + text + "\n";
      out = out + text + "\n";
      return out;
  }
  ```

  - 이런식으로 결과 값을 내야하는 것들을 계산하고 그걸 내는 것이 주로 사용되는 형태일 것이다.



## Tips

### main 도 Method!

- `main`이라는 특정한 이름을 가진 method 를 작성해왔다!



### Refactoring

- 코드의 결과는 똑같은데 내용을 훨씬 더 효율적으로 만드는 것
- 이클립스에서는 `Refactor`라는 이름으로 이러한 기능을 지원한다.
- 방법
  1. 드래그 ( 블럭 )
  2. Refactor
  3. Extract Method (`Alt + shift + M`)
  4. Method name 작성
  5. `Access modifier` 에서 해당하는 radio 버튼 선택 (`public` 으로 간다.)
  6. 체크박스 `Replace 2 additional occurrences of statements with method` 체크

- 

