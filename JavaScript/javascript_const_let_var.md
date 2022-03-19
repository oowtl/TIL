# const, let and var



## const

> constant 에서 왔다. constant 의 뜻은상수로서 항상 같은 수를 말한다.
> const 키워드로 선언하면 변치 않는 값을 가지는 변수를 생성한다고 생각하면 된다.
>
> 블록 범위의 상수를 선언한다.
> 상수의 값은 재할당할 수 없으며 다시 선언할 수도 없다.

- 고정되어 있는 값이라고 생각하면 된다.

```javascript
// 재할당 불가능
const a = 5;
const b = 2;

console.log( a + b ); // 7

a = 3;
// error!!!
app.js:9 Uncaught TypeError: Assignment to constant variable.
    at app.js:9

// 재선언 불가능
const a = 5;
const b = 2;

console.log( a + b ); // 7

const a = 3;
// error!!!
Uncaught SyntaxError: Identifier 'a' has already been declared


// 값이 존재해야 한다
const c;
//error!!!
app.js:9 Uncaught SyntaxError: Missing initializer in const declaration
```



## let

> const 와 다르게 편하게 바꿀 수 있는 것이다.
>
> 블록 스코프의 범위를 가지는 지역 변수를 선언하며, 선언과 동시에 임의의 값으로 초기화할 수도 있다.

- 맘대로 바꿀 수 있는 것이라고 생각하면 된다.

```javascript
// 재할당 가능
let myName = "jun";
console.log(`Hi ${myName}`);  // Hi jun
myName = "young";
console.log(`Hi ${myName}`);  // Hi young

// 재선언 불가능
let say = "Hello!"
let say = "hi!"

// error!
// app.js:21 Uncaught SyntaxError: Identifier 'say' has already been declared

// 값이 없어도 된다.
let c;
console.log(c); // undefined
```



## var

> 마음대로 할 수 있는 것이다.

```javascript
// 다 된다는 것!!!
var age = 28;    // 28
age = 18;        // 18
var age = 'old'; // old
```





# why const and let??

- 원래는 var 만 있었는데, const와 let이 생겼다.
  왜 생긴 것일까???
  - 이것을 알기 위해서 우리는 변수의 선언 및 할당 과정, 호이스팅, 스코프를 알아야 한다.



## 변수

> 변수는 하나의 값을 저장하기 위해서 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해서 붙인 이름이다.

- 설명
  - 어떤 값에 접근하기 위해서 자바스크립트에서는 메모리 주소를 통해서 접근하는 것이 아니라 변수를 통해서 접근한다.
- 용어
  - 할당 : 변수에 값을 저장하는 것
  - 참조 : 변수에 저장된 값을 읽어들이는 것
  - 선언 : 변수명을 자바스크립트 엔진에 알리는 것



### 변수 선언

- 자바스크립에서 변수 선언 단계는 선언, 초기화 단계를 거쳐서 수행된다.

  - 선언단계 : 변수명을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다.
  - 초기화단계 : 값을 저장하기 위해서 메모리 공간을 확보하고 암묵적으로 undefined 를 할당해서 초기화 한다.

- var

  - var를 사용한 변수선언은 선언단계와 초기화 단계가 동시에 진행된다.

  - 자바스크립트 엔진은 변수 선언을 포함한 모든 선언을 찾아내서 먼저 실행한다.

    소스코드를 실행하기 전에 먼저 모든 선언을 찾아내고 시작한다고 생각하면 된다.
    변수 선언이 어디에 있던지 상관없이 다른 코드보다 먼저 실행되는 특징을 **호이스팅**이라고 한다.

    - var, let, const , function, class 모두가 호이스팅이 된다.



## 스코프

> 식별자(변수명, 함수명, 클래스명 등)의 유효범위를 뜻한다.
>
> 선언된 위치에 따라서 유효범위가 달라진다.
>
> 전역에 선언된 변수는 전역 스코프가, 지역에 선언된 변수는 지역 스코프를 가진다.

- 전역변수
  - 어디에서든지 참조가 가능한 값이다.
- 지역변수
  - 함수 몸체 내부를 말한다.
  - 지역변수는 자신의 지역 스코프와 그 하위 지역 스코프에서 유효하다.

- 주의할 점

  - var  로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.

    ```javascript
    var a = 1;
    if (true) {
      var a = 5;
    };
    
    console.log(a) // 5
    ```

    - 함수가 아닌 곳에서 a를 var로 선언을 했기 때문에 전역변수로 취급된다.
    - 코드가 몇백줄 있는 곳에서 이런식으로 작동하면 계속 값이 재할당되거나 재선언되는 현상이 벌어지면서 값이 자주 바뀔 것이다.

## var 의 문제점

1. 변수 중복 선언이 가능하기 때문에 예기치 못한 값을 반환할 수 있다.
2. 함수 레벨 스코프로 인해서 함수 외부에서 선언한 변수는 모두 전역 변수가 된다.
3. 변수 선언문 이전에 변수를 참조하면 언제나 Undefinde 를 반환한다.



## const 와 let

1. 변수 중복 선언 불가

2. 블록레벨 스코프

   ```javascript
   let a = 1;
   if ( true ) {
     let a = 5;
   };
   console.log(a); // 1
   ```

3. 변수 호이스팅

   1. let
      변수의 선언단계와 초기화 단계가 분리되어서 진행된다.
      선언단계가 먼저 실행되지만 초기화 단계가 실행되지 않았을 때 변수에 접근하려고 하면 참조 에러가 뜬다.

      ```javascript
      console.log(name)
      let name = "jun"
      
      // Uncaught ReferenceError: Cannot access 'name' before initialization at app.js:29
      ```

   2. const
      선언단계와 초기화 단계가 동시에 진행된다.

      ```javascript
      console.log(name)
      const name = "jun"
      
      //app.js:29 Uncaught ReferenceError: Cannot access 'name' before initialization at app.js:29
      ```

      let 으로 선언한 경우는 자바스크립트 엔진에 이미 존재하지만 초기화가 되지 않은 것이다.
      const 는 선언과 초기화가 동시에 이루어져야 하지만 런타임 이전에는 실행될 수 없는 것이다. 초기화가 진행되지 않은 경우이다.



## 결론

- var 를 사용하는 것은 지양하는 것이 좋다.
- 변경하지 않는 값이라면 let 보다는 const 를 사용하는 것이 좋다.





- 참고 : https://www.howdy-mj.me/javascript/var-let-const/