# Object
- 자바스크립트는 원시타입을 제외한 값은 모두 객체이다.
- 객체는 프로퍼티(Property)의 집합이다.

## 생성
- `{}` 객체 리터럴 : 내부적으로는 생성자 함수를 사용하여 객체 생성한다.
- `new Object()` : new 연산자와 Object 생성자 함수를 이용하여 객체 생성한다.
- `new function [name]([parameter]){[Running construction]}` : new 연산자와 선언 한 생성자 함수를 이용하여 객체 생성한다.
- 프로퍼티(Property)
  - 키(Key)와 값(Value)으로 구성, 키 및 값은 어떤 데이터로든 설정 가능하다. (원시타입 및 객체 모두 OK)
  - 객체 리터럴에서의 키는 원시타입만 가능하다.
  - 객체 리터럴에서는 `키: 값` 형태로 설정한다.
  - 생성자 함수에서는 `this.키 = 값` or `this[키] = 값` 형태로 설정한다.
  - `키: 값`나 `this.키 = 값`일 경우 키는 문자열로 자동변환되어 설정된다.
```JavaScript
//{} 객체 리터럴 사용
var obj1 = {
  1: 'numToStr',
  true: 'boolToStr',
  'string': 'strToStr',
  null: 'nullToStr',
  undefined: 'undefinedToStr',
  
  strToNum: 200,
  strToBool: true,
  strToStr: 'string',
  strToNull: null,
  strToUndefined: undefined,
  strToObj: new Object()
};

var obj1Array = [];
for(i in obj1) {
  obj1Array.push(obj1[i]);
}
console.log(obj1Array); //["numToStr", "boolToStr", "strToStr", "nullToStr", "undefinedToStr", 200, true, "string", null, undefined, {…}]

//new Object() 사용
//객체 생성과 동시에 프로퍼티 설정 불가
var obj2 = new Object();

//new function [name]([parameter]){[Running construction]} 사용
var obj3 = new function() {
  this[1] = 'numToStr',
  this[true] = 'boolToStr',
  this['string'] = 'strToStr',
  this[null] = 'nullToStr',
  this[undefined] = 'undefinedToStr',
  this[new Object()] = "objToStr";

  this.strToNum = 200;
  this.strToBool = true;
  this.strToStr = 'string';
  this.strToNull = null;
  this.strToUndefined = undefined;
  this.strToObj = new Object();
}

var obj3Array = [];
for(i in obj3) {
  obj3Array.push(obj3[i]);
}
console.log(obj3Array); //["numToStr", "boolToStr", "strToStr", "nullToStr", "undefinedToStr", "objToStr", 200, true, "string", null, undefined, {…}]
```

## 접근
- 객체에 저장된 프로퍼티는 `.`접근자와 `[]`접근자를 이용하여 접근 가능하다.
- 키는 일반적으로 문자열을 사용, 키가 문자열이면서 일반적인 변수 명명법을 따를경우 `.`접근자로 접근 가능하다. (`''`는 생략한다.)
- `[]`접근자로는 어떤 형태의 키도 접근 가능하다.
- 접근하려는 키가 존재하지 않을 경우 `undefined`가 반환된다.
- 값이 함수일 경우에는 그 프로퍼티를 메소드라고 한다.
```JavaScript
var obj = new function() {
  this[1] = 'numToStr',
  this.strToStr = 'string';
  this.objMethod = function() {
    console.log("This is objMethod.");
  }
}

console.log(obj.strToStr);  //string
console.log(obj['strToStr']); //string

// console.log(obj.1);  //Error발생
console.log(obj[1]);  //numToStr

console.log(obj.unknownKey);  //undefined

obj.objMethod();  //This is objMethod.
```

## 조작
- 추가
  - 접근자를 이용해 존재하지 않는 프로퍼티에 추가하려는 값을 설정하면 된다. 
- 갱신
  - 접근자를 이용해 존재하는 프로퍼티에 새로운 값을 설정하면 된다.
- 삭제
  - `delete 객체.프로퍼티 키;`연산자를 이용한다.
  - 피 연산자는 프로퍼티 키만 가능하다. (객체일 경우 구문이 무시 됨)
```JavaScript
var obj = {}

//추가
obj.key = "value";
console.log(obj.key);  //value

//갱신
obj.key = "not value";
console.log(obj.key);  //not value

//삭제
delete obj.key;
console.log(obj.key);  //undefined
```

## 속성 기술자(Property Descriptors)
- 객체를 구성하고 있는 속성의 구체적 내용을 기술 한 것이다. (속성 변경가능 유무, 삭제가능 유무 등)
- 데이터 기술자(Data Descriptors)와 접근자 기술자(Accessor Descriptors)의 두 가지 유형으로 나뉘어진다.
- 객체로 표현되며, 유형과 상관없이 가지고 있는 키와 유형에 따라 가질 수 있는 키가 존재한다.
- `Object.defineProperty()`메소드를 이용하여 속성 기술자를 정의 및 재정의 할 수 있으며, `Object.getOwnPropertyDescriptor()`메소드를 이용하여 속성의 속성 기술자를 얻을 수 있다.

### 유형에 상관없이 가지는 키
- `configurable`
  - `true`인 경우 기술자의 유형 변경, 기술자에 대응하는 속성의 삭제가 가능하다.
  - `false`인 경우 속성 기술자 재정의가 불가하다.
- `enumerable`
  - `true`인 경우 대응하는 속성이 객체 열거시 열거 대상이 된다. (for-in문, `Object.key()`를 이용한 반복문의 대상이 됨)

```javascript
var obj = {
  'a2': 1,
  'b': 2
};

Object.defineProperty(obj, 'a2', {
  configurable: true,
  enumerable: true
});

Object.defineProperty(obj, 'b', {
  configurable: false,
  enumerable: false
});

for(key in obj) {
  //enumerable이 true인 속성만 실행된다.
  console.log(key + ": " + obj[key]);
} //a2: 1

delete obj.a2;
delete obj.b;

//configurable가 false이면 delete명령에도 속성이 삭제되지 않는다.
console.log(obj.a2); //undefined
console.log(obj.b); //2
```

### 데이터 기술자만 가지는 키
- `value`
  - 기술자에 대응하는 속성의 값이다.
  - JavaScript의 유효한 데이터이여야 한다. (primitive type, object, function)
- `writable`
  - `true`인 경우 대입연산자(`=`)를 통해 `value` 값의 변경이 가능하다.

```javascript
var obj = {};

Object.defineProperty(obj, 'a2', {
  value: 1,
  writable: true
});

Object.defineProperty(obj, 'b', {
  value: 2,
  writable: false
});

obj.a2 = 11;
obj.b = 22;

//writable가 false이면 대입연산자로 새로운 값을 설정해도 수정되지 않는다.
console.log(obj.a2); //11
console.log(obj.b); //2
```

### 접근 기술자만 가지는 키
- `get`
  - 속성의 getter에 해당하는 함수이다.
  - 속성에 접근할 때 `get`에 설정되어 있는 함수가 인수 값이 없는 상태로 실행된다.
- `set`
  - 속성의 setter에 해당하는 함수이다.
  - 대입연산자(`=`)로 값을 할당 할 경우, 할당 된 값을 인수로하여 `set`에 설정되어 있는 함수가 실행된다.

```javascript
var obj = {};

Object.defineProperty(obj, 'a2', {
  get: function() {
    console.log('a에 저장된 값을 반환합니다.');
    return this.input;
  },
  set: function(input) {
    this.input = input;
    console.log(input + '가 a에 저장되었습니다.');
  }
});

obj.a2 = 1;  //1가 a에 저장되었습니다.
var aNum = obj.a2; //a에 저장된 값을 반환합니다.
console.log(aNum);  //1
```

### 속성 설정 방법에 따른 속성 기술자의 초기치

키 | 대입 연산자를 통한 속성 정의 | `Object.defineProperty()`를 통한 속성 정의
---|----------------------------|-------------------------------------------
`configurable` | `true` | `false`
`enumerable` | `true` | `false`
`value` | 초기화 값 | `undefined`
`writable` | `true` | `false`
`get` | - (데이터 기술자로 정의 됨) | `undefined`
`set` | - (데이터 기술자로 정의 됨) | `undefined`

```javascript
var obj = {};

//선언 후 초기화하지 않으면 속성이 정의되지 않는다.
obj.a1;
var pda1 = Object.getOwnPropertyDescriptor(obj, 'a1');
console.log(pda1); //undefined

obj.a2 = 1;
var pda2 = Object.getOwnPropertyDescriptor(obj, 'a2');
console.log(pda2); //{value: 1, writable: true, enumerable: true, configurable: true}


//서술 기술자의 특정 유형을 나타내는 키가 없으면 데이터 기술자로 간주한다.
Object.defineProperty(obj, 'b', {});
var pdb = Object.getOwnPropertyDescriptor(obj, 'b');
console.log(pdb); //{value: undefined, writable: false, enumerable: false, configurable: false}

Object.defineProperty(obj, 'c1', {
  get: function(){}
});
var pdc1 = Object.getOwnPropertyDescriptor(obj, 'c1');
console.log(pdc1); //{get: ƒ, set: undefined, enumerable: false, configurable: false}

Object.defineProperty(obj, 'c2', {
  set: function(){}
});
var pdc2 = Object.getOwnPropertyDescriptor(obj, 'c2');
console.log(pdc2); //{get: undefined, set: ƒ, enumerable: false, configurable: false}
```

## 객체분류
- Built-in Object
  - 브라우저가 기본적으로 제공하는 객체를 뜻한다.
  - 객체 생성코드 없이, 약속된 변수에 접근하면 이용 가능하다.
- Host Object
  - 사용자가 정의하고 생성한 객체를 뜻한다.
  - 객체 리터럴 또는 생성자 함수를 이용해 생성 후 이용 가능하다.
```JavaScript
//Host Object
//생성하지 않으면 사용 불가
function hostObj() {
  this.testStr = "test string"
}
console.log(hostObj.testStr); //undefined
console.log((new hostObj).testStr); //test string

//Built-in Object(ex.Math)
//생성하지 않아도 사용 가능
console.log(Math.PI); //3.141592653589793
```

## 참고
- [PoiemaWeb : JavaScript > 객체](https://poiemaweb.com/js-object)
- [[부스트코스] 웹 프로그래밍 : 웹 애플리케이션 개발 1/4 > 자바스크립트 객체](https://www.edwith.org/boostcourse-web)
- [MDN web docs : 開発者向けのウェブ技術 > JavaScript > JavaScript リファレンス > 標準ビルトインオブジェクト > Object > Object.defineProperty()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)