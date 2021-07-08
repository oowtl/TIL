# Java 9. Class



## 예시

```java
public class ClassApp {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		System.out.println(Math.PI); // 3.141592653589793
		// 내림
		System.out.println(Math.floor(1.6)); // 1.0
		// 올림
		System.out.println(Math.ceil(1.6)); // 2.0
		
		// class 는 그것과 연관된 변수와 메소드를 모아놓은 것!
	}
}

```

- Math 라는 Class 가 있다.
  - PI 는 변수
  - `floor()` , `ceil()` 은 메소드
  - **즉, Class 는 그것과 연관된 변수와 메소드를 모아놓은 것을 말한다.**



## 개념

- 서로 연관된 변수와 메소드를 그룹핑한 것이다.
  그리고 거기에 이름을 붙인 것이다.
- class 와 method 같은 것은 소프트웨어에서 구조를 구성하는 것이다.
- 코드들의 디렉토리 역할을 한다고 생각하면 된다.



## 형태

- ```java
  class Accounting {
  	public static double valueOfSupply;
  	public static double vatRate;
  	public static double expenseRate;
  	
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
  	public static double getDividend1() {
  		return getIncome() * 0.5;
  	}
  
  	public static double getDividend2() {
  		return getIncome() * 0.3;
  	}
  
  	public static double getDividend3() {
  		return getIncome() * 0.2;
  	}
  
  	public static double getIncome() {
  		return valueOfSupply - valueOfSupply * expenseRate;
  	}
  
  	public static double getExpense() {
  		return valueOfSupply * expenseRate;
  	}
  
  	public static double getTotal() {
  		return valueOfSupply + getVAT();
  	}
  
  	public static double getVAT() {
  		return valueOfSupply * vatRate;
  	}
  }
  
  public class AccountingClassApp {
  
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		
  		Accounting.valueOfSupply = 10000.0;
  		Accounting.vatRate = 0.1;
  		Accounting.expenseRate = 0.3;
  		Accounting.print();
  		
  	}
  	
  }
  
  ```

  - `class Accounting` 이 디렉토리처럼 기능하는 것 같다.
  - main 을 보면 `Account.valueOfSupply` 를 통해서 값을 지정해주고
    `Accounting.print()`를 통해서 실행시켜준다.
  - 하나의 기능의 집합체 같은 것으로 생각해도 될 것 같다.



## Tips



### Outline 보기

- class 안에 속해 멤버들 ( 변수, 메소드) 을 포괄적으로 보여주는 것
- 보기
  1. `Window`
  2. `Show View`
  3. `Outline`
