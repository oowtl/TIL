# JS Callback

## Callback 은 무엇인가요?

- JS에서 함수는 1급객체 이므로 인자 전달 시 함수를 전달할 수 있다.
- Callback 은 다른 함수에 인수로 전달되는 함수입니다.
  - 함수가 다른 함수를 호출할 수 있다.
- Callback 함수는 다른 코드가 실행을 완료할 때 까지 특정 코드가 실행되지 않도록 하는 방법이다.
- 콜백 함수는 코드를 통해서 명시적으로 호출하는 함수가 아니라, 어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수를 말한다.
- 콜백 함수는 특정한 문법적 특징을 가지고 있는 것이 아니라, 호출방식에 의한 구분이다.



## Callback 이 필요한 이유는?

- JavaScript 는 이벤트 중심 언어이다.
  - JavaScript는 계속 진행하기 위해서 응답을 기다리는 대신에 다른 이벤트를 수신대기하고 계속 실행시킨다.

- 비동기적 프로그래밍?
  - 싱글 스레드가 멈추는 것을 방지하기 위해서, 블록킹을 방지하는 것으로 싱글 스레드가 논 블록킹으로 동작하게 한다.




### 비동기적 프로그래밍이 필요한 이유는?

1. 사용자 이벤트 처리
   - 브라우저 화면에서 발생하는 사용자의 이벤트는 예측이 불가능하다.
   - 화면 이벤트를 관리하는 것에 특정 이벤트가 발생 시 호출하기를 원하는 내용을 callback 함수에 전달하게 된다.
2. 네트워크 응답 처리
   - 서버에 요청을 보냈을 때, 언제 응답이 올지 모른다.
3. 파일을 읽고 쓰는 등의 파일 시스템 작업
4. 의도적으로 시간 지연을 사용하는 기능(알람 등)
   - 이벤트를 기다리는데 싱글 스레드를 사용한다면, 서버의 응답을 기다리는 데 싱글 스레드를 사용한다면?
     사용자는 멈춰저 있는 화면을 보게된다.
     따라서, 스레드의 블록킹을 야기하는 작업은 필수적으로 비동기적 프로그래밍을 해야한다.



## Callback 은 어떻게 사용하는가?

```js
function fn_callback(callback) {
    callback();
}
console.log('before fn');
fn_callback(function() { // 콜백함수
    console.log('callback!!!!');
});
console.log('after fn');
// before fn
// callback!!!!
// after fn
```

- 콜백함수는 일반적인 함수이다.
- 비동기적으로 콜백함수를 호출하는 함수에 비동기적으로 호출되기를 원하는 코드를 콜백함수에 담아서 전달한다.

```js
function fn_callback(callback) {
    console.log("비동기적인 호출");
};
console.log('호출 합니다잉');
setTimeout(fn_callback, 0);
console.log('호출 끝입니다잉');
// 호출 합니다잉
// 호출 끝입니다잉
// 비동기적인 호출
```

- `setTimeout` 함수는 콜백함수의 실행을 지정된 밀리초만큼 지연하는 함수이다.
- 결과를 보면 이상한 점을 발견할 수 있다. `setTimeout` 부분과 마지막 부분의 순서가 바뀌었다는 것을 알 수 있다.
  - 왜냐하면 JavaScript가 응답을 기다리지 않았기 때문이다.
    실행은 순서대로 했지만, 응답을 기다리지 않고 실행했기 때문이다.
- 콜백은 다른 코드가 실행을 완료할 때까지 특정 코드가 실행되지 않도록하는 방법이다.



### Callback 속에 Callback 을 넣을 수 있나요?

```js
function fn_callback (talk, callback) {
    console.log(`let's ${talk}`);
    callback();
}
fn_callback('go', function (talk) {
    console.log('1차 callback');
    fn_callback('study!', function(talk) {
       console.log('2차 callback'); 
    });
});
// let's go
// 1차 callback
// let's study!
// 2차 callback
```

- 콜백함수 안에 콜백함수를 넣을 수 있다.
- 이러한 방식으로 코드를 전개하는 것은 가독성에 좋지않다.
  -> 다른 방법이 존재한다!!



### Callback은 에러를 어떻게 처리하나요?

```js
fn_callback('go', function (error, talk1, talk2, ...) {
    if (error) {
        console.log('err');
    } else {
        console.log('normal');
    }
});
```

- 오류 우선 콜백 이라고 불리는 패턴이다.
- callback 의 첫 번째 인수는 에러를 위해서 남겨둔다.
  - 에러가 발생하면 이 인수를 이용해서 `callback(err)`이  호출된다.
- 두 번째 인수 부터는 에러가 발생하지 않았을 때를 위해서 사용한다.
  - 원하는 동작이 성공하면 `callback(null, result1, result2 ...)` 가 호출된다.



### Callback 지옥은 쉽게 찾아온다.

```js
fn_callback('go1', function (error, talk) { 
  if (error) {
    console.log('err1');
  } else {
    fn_callback('go2', function (error, talk) {
      if (error) {
        console.log('err2');
      } else {
        fn_callback('go3', function (error, talk) {
          if (error) {
            console.log('err3');
          } else {
            console.log('stop3');
          }
        });
      }
    });
  }
});
```

- 호출이 계속되면서 코드가 깊어지는 지옥의 코드이다.
- 지금은 `console.log` 만 있지만 반복문과 조건문이 들어간다면 달라질 것이다.
- 개발자 모드에서 콘솔로 해당 코드를 작성하면, 매우 주의를 기울여야 할 것이다...



#### Callback 지옥을 피하는 방법

1. 각 동작을 독립적인 함수로 만들어서 사용한다.

```js
function step1 ( error, talk ) {
  if (error) {
    console.log('error1');
  } else {
    fn_callback('go1', step2);
  }
};

function step2 ( error, talk ) {
  if (error) {
    console.log('error2');
  } else {
    fn_callback('go2', step3);
  }
};

function step3 ( error, talk ) {
  if (error) {
    console.log('error3');
  } else {
    console.log('end')
  }
};
fn_callback('go0', step1);
        
// let's go0
// let's go1
// let's go2
// end
```

- 각 동작을 독립적으로 만들어서 관리하는 방법이 있다.
- 동작상의 문제는 없지만 다 찢어져서 한눈에 파악하는 것이 힘들다.
- 이런 방법보다 좀 더 나은 방법이 존재한다.
  **promise**를 사용하면 된다!

2. Promise 를 사용한다.
   - promise 참조





## 참조

- https://www.w3schools.com/js/js_callback.asp
- https://ko.javascript.info/callbacks
- https://bigtop.tistory.com/35
- https://velog.io/@minidoo/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98Callback-Function
- https://codeburst.io/javascript-what-the-heck-is-a-callback-aba4da2deced
- https://www.hanumoka.net/2018/10/24/javascript-20181024-javascript-callback/
- https://victorydntmd.tistory.com/48
- https://beomy.tistory.com/10