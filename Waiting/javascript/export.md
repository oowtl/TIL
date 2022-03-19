# export

> 함수, 객체, 원시 값을 내보낼 때 사용한다.
>
> 내보낸 값은 다른 프로그램에서 import 를 통해서 가져와서 사용할 수 있다.
>
> 추가 키워드 "Use strict", "엄격모드" (https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)





## 구문예시

```javascript
// 하나씩 내보내기
export let name1, name2 ...
export var name3, name4 ...
export function functionName1(){}
export class className {}

// 목록으로
export { name1, name2, ...} 
export { var1 as name1, var2 as name2, ... }

// 비구조화
export const {name1, name2: bar } = o;

// 기본 내보내기
export default expression
export default function () {}
export default function name1 () {}
export {}

// 모듈 조합
export * from ...;
export * as name1 from ...
export { name1, name2, ...} from ...
export { import1 as name1, import2 as name2, ...} from ...
export { default } from ...
```



## 설명

> 내보내기에는 유명 ( named ), 기본 ( default ) 내보내기가 있다.
>
> 유명 내보내기는 여러 개 존재할 수 있다.
>
> 기본 내보내기는 하나만 가능하다.



### named export

```javascript
// 먼저 선언한 식별자 내보내기
export { myFunction, myVariable }

// 각각의 식별자 내보내기
export let myVariable = Math.sqrt(2)
export function myFunction() {}
```

- 여러 값을 내보낼 때 유용하다.
- export 시에 선언한 이름과 동일한 이름을 사용해야 한다.



### Default export

```javascript
// 먼저 선언한 식별자 내보내기
export { myFunction as default }

// 각각의 식별자 내보내기
export default function () {}
export default class {}
```

- 어떤 이름으로도 가져올 수 있다.





- 출처
  - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export