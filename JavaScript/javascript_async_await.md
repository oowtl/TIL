# Javascript_async_await

> 프라미스를 좀 더 편하게 사용할 수 있도록 하는 함수
>
> ES 7 부터 추가되었다.



## async 함수

```js
async function f() {
    return 1;
}
f().then(alert);
// Promise {<fulfilled>: undefined}
```

- function 앞에 `async`를 붙이면 해당 함수는 항상 `promise`를 반환한다.
- 프라미스가 아닌 값을 반환하도록 해도 이행 상태의 프라미스로 값을 감싸서 반환한다.

```js
async function f() {
    return Promise.resolve(1);
};
f().then(alert);
// Promise {<fulfilled>: undefined}
```

- 반환 값으로 프라미스를 반환하는 것도 할 수 있다.

## await

**`await`는 `async` 함수 안에서만 동작한다.**

```js
async function f() {
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve("완료!"), 1000)
    });
    let result = await promise;
    alert(result);
};
f();
// Promise {<pending>}
```

- 함수를 호출한다.
  `let result = await promise;` 부분에서 실행이 잠시 중단된다.
  프라미스가 처리되면 실행이 재개되고 `result` 부분에 프라미스 객체의 `resolve`값이 할당된다.
  `alert` 를 실행한다.
- `await` 는 프라미스가 처리될 때까지 함수 실행을 기다리게 한다.
  프라미스가 처리되면 그 결과와 함께 실행이 재개된다.
  프라미스가 처리되길 기다리는 동안 엔진이 다른 일( 다른 스크립트 실행, 이벤트 처리 )을 할 수 있기 때문에 CPU 리소스가 낭비되지 않는다.

## 주의할 점

1. 일반 함수에는 `await`를 사용할 수 없다.

```js
function f() {
    let promise = Promise.resolve(1);
    let result = await promise;
};
// Uncaught SyntaxError: await is only valid in async functions and the top level bodies of modules
```

- `function` 앞에 `async` 를 붙이는 것으로 `await` 를 사용해야 한다.

2. `await` 는 최상위 레벨에서는 작동하지 않는다.

```js
// 최상위 레벨 코드에선 문법 에러가 발생함
let response = await fetch('/article/promise-chaining/user.json');
let user = await response.json();
```

- 다른 예시를 만들어보려고 했는데, 실행이 되어서 당황했다.
  그래서 검색을 좀 해보니 `top level await` 라는 키워드를 알게 되었다.
  - `await`가 최상위 수준에서 사용되면 모듈이 큰 비동기 기능으로 작동한다.
    - 뭔가를 `Import` 할 때, 주로 사용되는 것으로 보인다.

```js
(async() => {
  let response = await fetch('/article/promise-chaining/user.json');
	let user = await response.json();
  ...
});
```

- `await` 를 최상위 레벨에서 작동시키기 위해서는 익명의 `async`를 함수로 감싸면 된다.

3. `await`는 `thenable`객체를 받는다.

```js
class Thenable {
    constructor(num) {
        this.num = num;
    }
    then (resolve, reject) {
      alert(resolve);
        setTimeout(() => resolve(this.num * 2), 1000);
    };
}
async function f() {
    let result = await new Thenable(1);
    alert(result);
}
f();
// Promise {<pending>}
```

- `promise.then`처럼 `await`에도 thenable 객체 (`then` 메서드가 있고 호출이 가능한 객체)를 사용할 수 있다.
- 프라미스와 호환 가능한 객체를 제공할 수 있다.
- `await`는 `.then`이 구현되어 있으면서 프라미스가 아닌 객체를 받으면 내장함수 `resolve`와 `reject`를 인수로 제공하는 메서드인 `.then`을 호출한다.
   `resolve` 와 `reject` 중 하나가 호출되는 것을 기다렸다가 호출 결과를 가지고 다음 작업을 진행한다.

4. 클래스안의 async 메서드

```js
class Move {
    async go() {
        return await Promise.resolve("go!!");
    }
}
new Move()
.go()
.then(res => console.log(res));
// go!!
// Promise {<fulfilled>: undefined}
```

- 클래스 안의 메서드 앞에 `async`를 붙여줄 수 있다.

## 에러 핸들링

`promise`가 정상적으로 이행되면 `await promise`는 프라미스 객체의 `result`에 저장된 값을 반환한다.
`promise`가 거부되면 (`reject`) `throw` 문을 작성한 것처럼 에러가 던져진다.

```js
async function f() {
    await Promise.reject(new Error("Error!!!"));
};
f();
Promise {<rejected>: Error: Error!!!
    at f (<anonymous>:2:26)
    at <anonymous>:1:1}
```

```js
async function f() {
    throw new Error("throw Error");
};
f();
Promise {<rejected>: Error: throw Error
    at f (<anonymous>:2:11)
    at <anonymous>:1:1}
```

- 두 개의 구문은 동일한 내용을 가진다.
- `promise` 가 거부되기 전에 약간 시간이 지체가 되는 경우가 생긴다.
  그런 경우에는 `await`가 에러를 던지기 전에 지연(`pending`)이 발생한다.

```js
async function f() {
    try {
        let response = await fetch("idontknow");
        let user = await response.json();
    } catch (err) {
        console.log(err);
    }
};
f();
// Promise {<pending>}
// GET https://ko.javascript.info/idontknow 404
// SyntaxError: Unexpected token < in JSON at position 0
```

- 여러 줄의 코드를 try 로 감싸는 것도 가능하다.
- `catch`부분에서 `fetch`, `response.json` 부분에서 발생한 에러를 전부 처리한다.

```js
async function f() {
    let response = await fetch("howAreYou");
};
f().catch(alert); // f()는 거부 상태의 프라미스이다.
// Promise {<pending>}
// GET https://ko.javascript.info/howAreYou 404
```

- `async` 함수 밖에서 `.catch`를 추가하는 것으로 거부된(`reject`) 프라미스를 처리할 수 있다.



## `Promise.all` & `async`, `await`

 여러 개의 프라미스가 모두 처리되길 기다려야 하는 상황이라면?
`Promise.all`로 감싸고 여기에 `await`를 붙여서 사용할 수 있다.

```js
let reuslts = await Promise.all([
	fetch(url1),
	fetch(url2),
  fetch(url3),
  ...
]);
```

- 실패한 프라미스는 `Promise.all`로 들어간다.
  에러 때문에 생긴 예외는 `try...catch` 로 감싸서 잡을 수 있다.

## 요약

function 앞의 `async` 키워드의 효과

1. 이 함수는 언제나 `Promise`를 반환한다.
2. 함수 안에서 `await`를 사용할 수 있다.

`Promise` 앞에 `await` 키워드를 붙이면 자바스크립트는 프라미스가 처리될 때까지 대기한다.
처리가 완료되면 2가지 동작으로 나뉜다.

1. 에러 발생 : 예외가 생성된다. ( 에러가 발생한 장소에서 `throw error`를 호출한 것과 동일하다. )
2. 에러 미발생 : `promise` 객체의 result 값을 반환한다.



## 참고

https://ko.javascript.info/async-await

https://blog.saeloun.com/2021/11/25/ecmascript-top-level-await