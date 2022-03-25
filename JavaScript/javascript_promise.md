# JavaScript Promise

> ES6 에서 부터 추가된 비동기 처리 방식
>
> 비동기 작업의 단위

## 설명

`Promise` 비동기 작업을 처리하는 방식으로 ES6부터 추가되었다.

제작코드 (producing code), 소비코드(consuming code), 프라미스(promise) 부분으로 나누어서 설명할 수 있다.

- 제작코드는 실행할 코드를 말하며,
  소비코드는 제작코드의 결과에 따라서 실행할 코드를 말한다.
  프라미스는 제작코드와 소비코드를 연결해주는 자바스크립트 객체를 말한다.
  프라미스는 시간이 얼마나 걸리던지 상관없이 제작코드의 결과에 따라서 모든 소비코드가 제작코드의 결과를 사용할 수 있도록 해준다.

`Promise`는 비동기 작업의 단위이다.

Callback 을 중첩시키는 것은 Callback 지옥을 만들어낸다. Callback 지옥을 해결할 수 있는 것이 Promise 이다.

```js
let promise = new Promise(function(resolve, reject){
  // executor ( 제작 코드 = producing code)
})
```

- `new Promise`에 전달되는 함수를 executor (실행자, 실행함수) 라고 부른다.
- executor 는 `new Promise`가 만들어질 때, 자동으로 실행된다.
- `resolve`, `reject`는 자바스크립에서 자체 제공하는 콜백이다.
  - 개발자는 해당하는 인수(`resolve`, `reject`)를 신경쓰지 않고 executor 부분을 개발하면 된다.
- **executor 에서는 결과를 언제 얻든지 상관없이 상황에 따라서 넘겨준 콜백 중 하나를 반드시 호출해야 한다.**
  - `resolve(value)` : 일이 성공적으로 끝난 경우에는 결과를 나타내는 `value`와 함께 호출한다.
  - `reject(error)` : 에러 발생 시에는 에러 객체를 나타내는 `error`와 함께 호출한다.

**executor는 자동으로 실행되며, 여기에서 원하는 일이 처리된다. 처리가 끝나면 executor는 처리 성공 여부에 따라서 `resolve`나 `reject`를 호출한다.**

### Promise states

`Promise` 객체는 state, result 라는 내부 프로퍼티를 가진다.

- `state`

  처음에는 `"pending"` ( 보류 ) 였다가 `resolve` 가 호출되면 `"fullfilled"` , `reject` 가 호출되면 `"rejected"`로 변한다.

- `result`

  처음에는 `undefined` 였다가 `resolve(value)`가 호출되면 `value`로 `reject(error)`가 호출되면 `error`로 변한다. 





`Promise` 객체에는 `"pending"`, `"fullfilled"`, `"rejected"` 총 3개의 상태가 존재한다.

- `"pending"` : 이행하지도 거부하지도 않은 초기상태
- `"fullfilled"` : 연산이 성공적으로 완료된 상태
- `"rejected"` : 연산이 실패한 상태

![promise_executor](javascript_promise.assets/promise_executor.png)

[^promise_executor]: https://velog.io/@tastestar/Promise%ED%94%84%EB%9D%BC%EB%AF%B8%EC%8A%A4

- executor 는 `Promise`의 상태를 둘 중 하나로 변화시킨다.

```js
let promise = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("done"), 1000);
});
```

- 처리과정
  1. executor는 `new Promise`에 의해서 자동으로, 즉각적으로 호출된다.
  2. executor에 의해서 처리가 시작된지 1초 후에 ( `setTimeout` 에 의하여 ) `resolve("done")`이 호출되고 결과가 만들어진다.
  3. `promise`객체의 상태가 변한다.











## 사용방법

```js
let promise = new Promise(function (resolve, reject ) {
    resolve();
});
promise.then(() => {
    console.log('then!')
});
// then!
// Promise {<fulfilled>: undefined}
promise.catch(() => {
    console.log('catch!')
});
// Promise {<fulfilled>: undefined}
```























## 참조

- https://ko.javascript.info/promise-basics
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://elvanov.com/2597

