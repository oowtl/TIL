# JavaScript Hoisting

> 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미한다.

`var`로 선언한 변수의 경우 호이스틩 시 `undefined` 로 변수를 초기화 한다.
`let`, `const`로 선언한 변수의 경우 호이스팅 시 변수를 초기화하지 않는다.

"변수의 선언과 초기화를 분리하여 선언만 코드의 최상단으로 옮기는 것" 으로 말할 수 있다.
변수를 정의하는 코드보다 사용하는 코드가 먼저 등장할 수 있다.
주의할 점은 선언과 초기화를 함께 수행하는 경우에는 선언 코드까지 실행해야 변수가 초기화된 상태가 된다.

호이스팅은 ES5 이전에는 JavaScript 의 실행 맥락, 생생 및 실행 단계의 동작방식을 나타내는 방법이었다.

## 기본 예시

```js
function ready(name){
    console.log(`${name}, ready?`)
};
ready("jun");
// jun, ready?
```

```js
ready("jun");
function ready(name){
    console.log(`${name}, ready?`)
};
// jun, ready?
```

- 순서가 바뀌어도 동작을 한다.
  변수의 선언이 최상단에 가 있다는 것이다.
- 다른 자료형과 변수에도 작동한다.

## 변수의 선언 만이 호이스팅의 대상이 된다.

```js
console.log(num);
var num;
num=6;
// undefined
// 6
```

1. 호이스팅 된 `num`을 출력하려고 한다.
   `var`는 호이스팅 시에 `undefined` 로 변수를 초기화 한다.
   따라서, `undefined` 출력
2. `var num; ` 변수를 선언하는 부분이다.
3. `num=6` 변수를 초기화하는 부분이다.
   호이스팅은 변수의 선언만을 대상으로 하기 때문에 초기화한 값이 출력되지 않는다.

```js
console.log(num);
num=6;
// error
VM740:1 Uncaught ReferenceError: num is not defined
    at <anonymous>:1:13
```

- 변수에 대한 선언 없이 초기화만 한 코드이다.
  호이스팅은 변수의 선언을 대상으로 하기 때문에 `ReferenceError`가 발생한다.
- var를 붙이지 않고 초기화를 하는 것은 아예 전역변수로 만들어버리는 것이다.

```js
x = 1;
console.log(x + " " + y);
var y = 2;
// 1 undefined
```

- 기본적으로 위에서 부터 아래로 내려가면서 실행을 한다.
  거기에서 변수를 선언한 부분만 `undefined`로 올라가있다고 생각하면 된다.

## `let`, `const`

호이스팅은 ES5 까지 JavaScript 의 실행맥락, 생성 및 실행 단계의 동작방식을 나타내는 방법이었다.
그런데 왜 따로 `let`, `const` 를 만들어냈을까?

호이스팅은 변수의 선언을 최상단으로 올려버린다. 그 말은 변수명이 중복될 수 있다는 것을 뜻한다.
이로 인해서 큰 프로젝트를 개발할 때는 변수명을 작성하는데 비용이 발생한다.
이러한 현상을 막기 위해서 `var` 대신 `let`, `const`가 지원되기 시작했다.

`let`, `const` 도 호이스팅이 되기는 하지만 `var`와 같이 동작하지는 않는다.
`let`, `const` 도 선언한 부분이 호이스팅 되지만 초기화를 선언하기 전에 읽는 코드가 나타나면 에러를 발생시킨다. 

## 참고

https://developer.mozilla.org/ko/docs/Glossary/Hoisting

https://medium.com/korbit-engineering/let%EA%B3%BC-const%EB%8A%94-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-%EB%90%A0%EA%B9%8C-72fcf2fac365