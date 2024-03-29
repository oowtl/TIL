# Javascript 구조분해할당

>Destructuring Assignment

- 언제 사용하는가?
  - 함수에 객체나 배열을 전달해야하는 경우
  - 객체나 배열에 저장된 데이터 전체가 아닌 일부만 필요한 경우
  - 함수의 매개변수가 많거나 매개변수 기본값이 필요한 경우

-> 구조분해할당은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담아야할 때 사용하는 표현식이다.



## 배열 분해하기

```javascript
let arr = [ "Bora", "Lee"];
// 구조 분해 할당
let [first, last ] = arr;

console.log(first); // Bora
console.log(last); // Lee
```

- 인덱스를 이용해서 배열에 접근하지 않고 변수를 선언할 수 있다.

```javascript
// 구조 분해 할당
let [first, last] = "Bora Lee".split(' ');
console.log(first);// Bora
console.log(last);// Lee
```

- `split`과 같은 반환값이 배열인 메서드를 함깨 사용해도 좋다.



**분해(destructuring)의 의미**

- 구조 분해 할당이란 어떤 것을 복사한 이후에 변수로 '분해' 해준다는 의미때문에 붙여졌다.
- 분해 대상은 수정, 파괴되지 않는다.



**쉼표 사용을 통한 요소 무시하기**

```js
let [ firstName, ,title ] = ["이지은", "드라마", "나의아저씨", "주연"];
console.log(title); //나의아저씨
```

- 쉼표를 사용해서 필요하지 않은 요소를 생략할 수 있다.
- 두 번째 요소를 생략하고 세 번째 요소를 `title` 이라는 변수에 할당했다.
  할당할 변수가 없어서 네 번째 요소는 생략했다.



**우측에는 모든 이터러블이 올 수 있다.**

```js
let [a, b, c] = "abc";
console.log(a, b, c); // a b c

let [one, two, three] = new Set([1,2,3]);
console.log(one, two, three); // 1 2 3
```

- 배열 뿐만 아니라 모든 반복 가능한 객체에 구조 분해 할당을 적용할 수 있다.



**좌측에는 뭐든지 올 수 있다.**

```js
let user = {};
[user.name, user.surname] = "Bora Lee".split(' '); // (2) ['Bora', 'Lee']
console.log(user.name, user.surname); // Bora Lee
```

- 좌측에는 할당할 수 있는 것이라면 다 들어갈 수 있다.
- 객체 프로퍼티도 가능하다.



**변수 교환 트릭**

```js
let kicker = "Son";
let keeper = "Jo";
// 변수 교환 트릭
[kicker, keeper] = [keeper, kicker]; // (2) ['Jo', 'Son']
console.log(kicker, keeper); // Jo Son
```

<br>

### `...`으로 나머지 요소 가져오기

```js
let [ name, genre, ...rest] = ["이지은", "드라마", "나의아저씨", "주연"];
name // '이지은'
genre //'드라마'
rest[0] // '나의아저씨'
rest[1] // '주연'
```

- 배열에 앞쪽에 위치한 값 몇개만 할당하고 나머지 값들은 한 곳에 모아서 저장하고 싶을때 `...`를 사용해서 모아줄 수 있다.

- 묶은 것을 배열로 보고 접근하면 된다.

**할당할 때의 기본값**

```js
let [first, last] = [];
first // undefined
last // undefined
```

- 할당하고자 하는 변수의 개수가 분해하고자 하는 배열의 길이보다 크더라도 에러가 발생하지 않는다.
  -> `undefined`로 취급해버리기 때문이다.

```js
// 기본값 설정하기
let [ name = "son", position = "striker" ] = ["kane"];
name // 'kane'
position // 'striker'
```

- 왼쪽에서 구조 분해 할당을 할 때, `=` 를 사용해서 기본값을 설정해줄 수 있다.

```js
function A (x) {
    return `function A : ${x}`;
}
function B (x) {
    return `function B : ${x}`;
}
let [first = A('Hi'), last = A('Hello')] = [B('Welcome')];
first // 'function B : Welcome'
last // 'function A : Hello'
```

- 더 복잡한 표현식이나 함수 호출도 기본값이 될 수 있다.

<br>

## 객체 분해하기

```js
let {var1, var2} = {var1: 'first', var2:'last'};
var1 // 'first'
var2 // 'last'
```

- 우측에는 분해하고자 하는 객체를 넣는다.
- 좌측에는 상응하는 객체 프로퍼티의 패턴을 넣는다.

```js
let options = {
    title: "nav",
    width: 100,
    height: 200
};
let { title, width, height } = options;
title // 'nav'
width // 100
height // 200
```

- 이런 식으로 사용할 수 있다.

```js
let options = {
    title: "nav",
    width: 100,
    height: 200
};
let { height, title, width } = options;
title // 'nav'
height // 200
width // 100
```

- 순서를 바꿔도 그대로 동작한다.

```js
let options = {
    title : "Menu",
    width: 150,
    height: 100
};
let { title } = options;
title // 'Menu'
```

- 원하는 정보만 선택해서 가져오는 것도 가능하다.

**변수 조정하기 (`:`)**

```js
let options = {
    title: "button",
    width: 50,
    height: 30
};
let { width: w, height: h, title } = options;
title // 'button'
width // 100
height // 200
```

- 분해할 때 `:` 콜론을 사용해서 다른 이름을 가진 변수로 저장해줄 수 있다.

**기본값 설정하기(`=`)**

```js
let options = {
    title: "Article",
};
let { width = 100, height = 60, title } = options;
width // 100
title // 'Article'
height // 60
```

- 객체 안에 해당하는 값이 존재하지 않으면, 기본값으로 사용할 수 있다.

```js
function A(x) {
    return `function A : ${x}`;
};
let options = {
    title : "Article"
};
let { width = A("width?"), title = A("title")} = options;
width // 'function A : width?'
title // 'Article'
```

- 배열분해와 마찬가지로 객체 안에 해당 값이 존재하지 않으면 함수를 실행시킬 수 있다.

```js
let options = {
    title : "Article"
};
let { width : w = 100, height: h = 200, title } = options;
title // 'Article'
w // 100
h // 200
```

- 콜론가 할당자를 동시에 사용하는 것도 가능하다.

<br>

### `...` 으로 나머지 요소 가져오기

배열과 마찬가지로 객체도 `...` 를 통해서 나머지 요소를 가져올 수 있다.

모던 브라우저는 나머지 패턴을 지원하지만 IE 를 비롯한 구식 브라우저는 나머지 패턴을 지원하지 않는다.
물론 Babel 을 이용하면 사용가능하다.

```js
let game = {
    title : "fifa22",
    genre : "sports",
    produciton : "EA"
}
let { title, ...rest} = game;
rest.genre // 'sports'
rest.produciton // 'EA'
```

- 묶은 것을 객체로 보고 접근하면 된다.

<br>

**`let` 없이 사용할 때 주의할 점**

```js
let title, width, height;
{title, width, heigth} = {title: "Menu", width : 200, heigth: 100};
VM6340:1 Uncaught SyntaxError: Unexpected token '='
```

- 자바스크립트는 표현식 안에 있지 않으면서 코드흐름 상에 있는 `{title, width, height}`부분을 코드블록으로 인식한다.
  - 코드블록에 `=` 을 가져다 붙이는 것이 문법오류라는 것
- 따라서 해당 부분을 표현식으로 인식하도록 할 필요가 있다.

```js
({title, width, heigth} = {title: "Menu", width : 200, heigth: 100});
 // {title: 'Menu', width: 200, heigth: 100}
title // 'Menu'
```

- `()` 괄호로 감싸는 것을 통해서 표현식으로 해석하도록 만들어주면 된다.

<br>

## 중첩 구조 분해

객체 안에 배열이나 객체가 있는 경우, 배열 안에 배열이나 객체가 있는 경우에 그 객체나 배열의 정보를 추출하는 것을 중첩 구조 분해 (nested destructuring) 이라고 한다.

```js
let options = {
    size: {
        width: 100,
        height: 200
    },
    items: ["Cake", "Donut"],
    extra: true};

let {
    size: {
        width,
        heigth
    },
    items: [item1, item2], // 배열 구조분해
    title = "Menu" // 기본값 할당
} = options;
title // 'Menu'
item1 // 'Cake'
item2 // 'Donut'
width // 100
height // 200
```

- 주의해야할 점은 `size`, `items`에 대한 변수는 없다는 것이다.
  해당하는 변수 대신에 그 안의 정보들에 변수를 할당했다.

<br>

##  함수 매개변수에 구조 분해 할당 적용하기

함수의 매개변수를 넣어줄 때, 구조 분해 할당을 적용할 수 있다.

```js
function showMenu ( title = "Untitled", width = 200, height = 100, items=[]) {
    return `title: ${title}, width: ${width}, height: ${height}, items: ${items}`;
};
showMenu("myMenu", undefined, undefined, ['item1', 'item2']); 
// 'title: myMenu, width: 200, height: 100, items: item1,item2'
```

- 적용하기 전에 사용할 수 있는 함수이다.
  기본값을 사용할 때는 `undefined`를 사용해야 하고 변수의 순서를 맞춰서 넣어줘야 한다는 불편함이 존재한다.

```js
let options = {
    title: "mymenu",
    items: ['item1','item2']
};
function showMenu ({title = "untitled", width = 200, height = 100, items = []}) {
    return `title: ${title}, width: ${width}, height: ${height}, items: ${items}`;
};
showMenu(options); // 'title: mymenu, width: 200, height: 100, items: item1,item2'
```

- 함수를 선언할 때, 매개변수 모두를 객체에 모아서 함수에 전달하는 것을 통해서 함수가 전달받은 객체를 분해하여 변수에 할당하는 작업을 수행할 수 있다.

```js
function showItem ({
    title = "Untitled",
    weight: w = 77,
    height: h = 183,
    items: [item1, item2] // item1, item2 변수에 할당한다.
}) {
    return `${title} player / weight : ${w} / height : ${h} / item : ${item1} , ${item2}`;
};
showItem(options);
// 'fifa22 player / weight : 77 / height : 183 / item : item1 , item2'
```

- 구조 분해 할당의 문법 사용과 같다.

### 주의할 점 : 구조 분해할 때는 반드시 인수가 전달되어야 한다.

```js
showItem();
VM4196:2 Uncaught TypeError: Cannot read properties of undefined (reading 'title')
    at showItem (<anonymous>:2:5)
    at <anonymous>:1:1
```

- 함수 매개변수를 구조 분해할때는 반드시 인수가 전달된다고 가정되고 사용된다.

```js
function showMenu ({title = "untitled", width = 200, height = 100, items = []} = {} ) {
  // function showMenu({} = {}) 의 형식을 지닌다.
    return `title: ${title}, width: ${width}, height: ${height}, items: ${items}`;
};
showMenu();
// 'title: untitled, width: 200, height: 100, items: '
```

- 따라서 모든 인수에 기본값을 할당하기 위해서는 빈 객체를 명시적으로 전달해야한다.

<br>

## 참고

[mozilla - 구조분해할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

[모던 자바스크립트 튜토리얼 - 구조분해할당](https://ko.javascript.info/destructuring-assignment)