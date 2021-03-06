# 스타일 속성(Style Property)
- DOM HTML API에서는 각 태그의 스타일 속성이 `CSSStyleDeclaration`객체로 관리되며 일반적으로 `style`키로 접근 가능하다.

## `CSSStyleDeclaration`객체의 속성 접근 방법

접근 방법 | 설명 | 특징
---------|------|-----
`CSSStyleDeclaration.propertyKey` | `CSSStyleDeclaration`객체의 `propertyKey`와 같은 키를 가진 값에 접근한다. | 속성의 키에 `-`가 포함되어있는 경우 카멜표기법으로 바꿔서 접근 가능하다.
`CSSStyleDeclaration[propertyKey]` | `CSSStyleDeclaration`객체의 `propertyKey`와 같은 키를 가진 값에 접근한다. | `propertyKey`는 문자열이어야 한다.
`CSSStyleDeclaration.getPropertyValue(prop)` | `prop`와 같은 키를 가진 값을 반환 | `prop`는 문자열이어야 한다. `CSSStyleDeclaration.setPropertyValue(prop, value)`로 추가 및 수정이 가능하다.
`CSSStyleDeclaration.cssText` | CSS문이 문자열로 저장된 값에 접근한다. | -

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Example of accessing to CSSStyleDeclaration properties</title>
</head>
<body>
  <p style="font-size: 24px"></p>
</body>
<script>
  var pTag = document.getElementsByTagName('p')[0];
  var pTagStyle = pTag.style;
  console.log(pTagStyle.toString());  //[object CSSStyleDeclaration]

  //CSSStyleDeclaration.propertyKey
  console.log(pTagStyle.fontSize);  //24px
  
  //CSSStyleDeclaration[propertyKey]
  console.log(pTagStyle['font-size']);  //24px
  
  //CSSStyleDeclaration.getPropertyValue(prop)
  console.log(pTagStyle.getPropertyValue('font-size')); //24px
  
  //CSSStyleDeclaration.cssText
  console.log(pTagStyle.cssText); //font-size: 24px;
</script>
</html>
```

## 객체 습득 방법

- 태그에 적용된 스타일 속성을 가진 `CSSStyleDeclaration`객체 습득 (읽기 전용)
  - `window.getComputedStyle(element)`를 사용하여 습득한다.
  - inline style, style 태그, 외부 CSS 파일로 인해 적용된 스타일 속성 전부를 계산하여 새로운 `CSSStyleDeclaration`객체를 만들어 반환하는 것이므로 읽기 전용객체이다.
  - 태그에 적용된 스타일의 정보가 필요할 경우 유용하다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Example of how to get CSSStyleDeclaration object</title>
  <style>
  p {
    font-style: italic;
  }
  </style>
</head>
<body>
  <p style="font-size: 24px"></p>
</body>
<script>
  var pTag = document.getElementsByTagName('p')[0];
  
  //window.getComputedStyle(element)
  var pTagStyle = window.getComputedStyle(pTag);
  console.log(pTagStyle.toString());  //[object CSSStyleDeclaration]

  console.log(pTagStyle.fontStyle); //italic
  console.log(pTagStyle.fontSize);  //24px
  
  //Uncaught DOMException 발생!
  // pTagStyle.fontSize = '12px';
</script>
</html>
```

- inline style 적용된 스타일 속성을 가진 `CSSStyleDeclaration` 객체 습득
  - `element.style`으로 습득한다.
  - `element.setAttribute('style', cssText)`를 이용해도 inline style 선언이 가능하나, 기존에 선언되어있던 inline style이 덮어쓰기 되어 초기화되므로 비추천한다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Example of how to get CSSStyleDeclaration object</title>
  <style>
  p {
    font-style: italic;
  }
  </style>
</head>
<body>
  <p style="font-size: 24px; font-weight: bolder"></p>
</body>
<script>
  var pTag = document.getElementsByTagName('p')[0];
  
  //element.style
  var pTagStyle = pTag.style;
  console.log(pTagStyle.toString());  //[object CSSStyleDeclaration]

  console.log(pTagStyle.fontStyle); //(빈 문자열)
  console.log(pTagStyle.fontSize);  //24px
  
  pTagStyle.fontSize = '36px';
  console.log(pTagStyle.fontSize); //36px

  //element.setAttribute('style', cssText)
  console.log(pTagStyle.fontWeight);  //bolder
  pTag.setAttribute('style', 'font-weight:300');
  console.log(pTagStyle.fontWeight);  //300
  console.log(pTagStyle.fontSize);  //(빈 문자열)
</script>
</html>
```

- style 태그나 외부 CSS파일로 적용된 스타일 속성을 가진 `CSSStyleDeclaration` 객체 습득
  - `document.styleSheets[index].cssRules[index].style`으로 습득한다.
  - 스타일 속성 정보 습득은 `window.getComputedStyle(element)`, 수정 및 추가는 `element.style`이 더 편하기 때문에 잘 사용되지는 않는 듯하다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Example of how to get CSSStyleDeclaration object</title>
  <style>
  p {
    font-style: italic;
  }
  </style>
</head>
<body>
  <p style="font-size: 24px"></p>
</body>
<script>
  var pTag = document.getElementsByTagName('p')[0];
  
  //document.styleSheets[index].cssRules[index].style
  var pTagStyle = document.styleSheets[0].cssRules[0].style;
  console.log(pTagStyle.toString());  //[object CSSStyleDeclaration]

  console.log(pTagStyle.fontStyle); //italic
  console.log(pTagStyle.fontSize);  //(빈 문자열)
  
  pTagStyle.fontStyle = 'normal';
  console.log(pTagStyle.fontStyle); //normal
</script>
</html>
```

## 참고
- [PoiemaWeb : JavaScript > 문서 객체 모델(Document Object Model)](https://poiemaweb.com/js-dom)
- [MDN web docs : Web technology for developers > Web APIs > CSSStyleDeclaration](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration)