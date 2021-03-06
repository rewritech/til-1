# 자바스크립트 호이스팅 (Hoisting)
- 자바스크립트가 선언부를 현재 스코프 (현재 스트립트 또는 현재 함수)의 최상단으로 끌어 올리는 것
- 즉, 자바스크립트는 스크립트 파일을 실행하기 전에 먼저 파일을 읽어서 변수 및 함수가 선언된 부분을 확인하고, 선언부를 스코프의 최상단에 위치시킨다.
- 자바스크립트의 변수는 선언과 초기화로 나뉘어지며, 변수 선언 후 초기화하지 않을 경우 초기값으로 `undefined`가 설정된다. 호이스팅은 선언부만 최상단으로 위치시킨 것일 뿐, 초기화는 실제 초기화 구문에서 행해지므로 주의가 필요하다.

```javascript
//Hoisting 적용
console.log(a); //undefined
var a = 1;
console.log(a); //1

//위의 구문은 실제로는 아래의 구문과 같이 동작한다.
var b;
console.log(b); //undefined
b = 2;
console.log(b); //2
```

- 이러한 호이스팅의 특징으로 인해 버그가 발생할 수 있기 때문에 함수 선언시에는 되도록 무명함수를 선언하고, 변수 선언 및 초기화는 스코프의 최상단에 위치 시키는 것이 바람직하다.

- ECMAScript2015(ES6)에서 추가된 `let`과 `const`는 호이스팅 대상이 아니다.

```javascript
console.log(varVar);
// console.log(letVar);  //ReferenceError: letVar is not defined
// console.log(constVar);  //ReferenceError: letVar is not defined

var varVar = 1;
let letVar = 2;
const constVar = 3;

console.log(varVar);  //1
console.log(letVar);  //2
console.log(constVar);  //3
```

## 참고
- [w3schools.com : JavaScript Hoisting](https://www.w3schools.com/js/js_hoisting.asp)