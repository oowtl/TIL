# Java 12. Variable



## Tip



### 값 한번에 바꾸는 방법 

#### 특정한 값 -> 특정한 값

- 바꾸고 싶은 값을 선택한다. (드래그)
- `Edit` - `Find / Replace` - `Replace with : ` 작성 - `Replace All`
  - `Replace All` 말고 다른 선택도 있다. 선택하기 나름!



#### 특정한 값 -> 변수

- 바꾸고 싶은 값을 선택한다.
- `우클릭` - `Refactor` - `Extract Local Variable` - `변수 이름` 작성 - `Finish`
- 주의할 점
  - 변수명이 이미 있으면 작동하지 않는다.
  - 변수의 데이터 타입은 선택한 값에 기초해서 만들어진다.



#### 주의할 점

- 일괄적으로 바꾸는 것이기 때문에, 관련은 없지만 값이 같은 것들이 자동으로 바뀔 수 있다.
  이러한 것은 매우 주의해야하며, 있으면 다른 방법을 추천한다.



### 변수선언을 하지 않고 사용할 때, 쉽게 변수를 추가하고 싶다면?

- 빨간줄 - 마우스 커서 - `create Local Variable` 
- 값까지는 추가해주지 않아서 입력해줘야 한다.