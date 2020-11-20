# CH9 타입변환과 단축평가

## 타입변환이란?
<br />
> ***명시적 타입변환***   
	의도적으로 값의 타입을 변환한다.   
	javascript 는 다른 정적타입처럼 casting 하는 방법에 객체를 이용한다.
>

```javascript
	let num = 100;
	let str = String( num ); // or num.toString() 
	console.log( typeof str ); // "string"
```

<br />
> ***암묵적 타입변환***
	자바스크립트 엔진에 위해 암묵적으로 타입변환 된다. 이는 타입 강제 변환이라고도 한다.
>

```javascript
	let num = 100;
	let str = "이다!";
	let numStr = num + str;
	console.log( typeof str, ': ', numStr ); // "string: 100이다!"
```
   
<br />
* 숫자타입으로 변환  

| 타입 | 값 평가 |
| :---: | :---: |
| 0 + "" | "0" |
| -0 + "" | "0" |
| 1 + "" | "-1" |
| NaN + "" | "NaN" |
| Infinity + "" | "Infinity" |
| -Infinity + "" | "-Infinity" |
| true + "" | "true" |
| false + "" | "false" |
|	null + "" | "null" |
| undefined + "" | "undefined" |
| (Symbol()) + "" | TypeError |
| ({}) + "" | [object Object] |
| Math + "" | [object Object] |
| [] + "" | "" |
| [10, 20] + "" | "10,20" |
| (function(){})() | "function(){}" |
| Array + "" | "function Array() { [native code] }" |
<br />
   
* 숫자타입으로 변환   

| 타입 | 값 평가 |
| :---: | :---: |
| +"" | 0 |
| +'0' | 0 |
| +"1" | 1 |
| +"string" | NaN |
| +true | 1 |
| +false | 0 |
| +null | 0 |
| +undefined | NaN |
| +Symbol() | TypeError |
| +{} | NaN |
| +[] | 0 |
| +[10,20] | NaN |
| +(function() {}) | NaN |
<br />
   

* BOOLEAN 변환   

| 타입 | 값 평가 |
| :---: | :---: |
| if( 1 ) | if( true ) |
| if( -1 ) | if( true ) |
| if( 0 ) | if( false ) |
| if( -0 ) | if( false ) |
| if( 'string' ) | if( true ) |
| if( '' ) | if( false ) |
| if( null ) | if( false ) |
| if( NaN ) | if( false ) |
<br />
   
 > 위의 평가값은 truthy 와 falsy 로 나누어진다. 
 	***false Null NaN "" 0 -0 undefined*** 는 falsy 이며,   
	그외에는 truthy 이다.
 > 
<br />

## 단축 평가   
<br />

* ***객체를 가리키기를 기대하는 변수가 null 또는 undefined 가 아닌지 확인하고 프로퍼티를 확인할 때***
> 객체의 property 를 참조하는데 값이 null 이면 TypeError 로 인해 프로그램이 종료된다.   
	이러한 종료를 막기위해 논리곱을 사용한다.
>   
<br />

```javascript
let elem = document.querySelector( ".elem" ); // null 이라면
let result = elem && elem.getAttributeNode("src").value; // 값: null // elem 이 null 이면 false 로 뒤의 값이 평가되지 않는다.
````   
<br />

## 옵셔널 체이닝 연산자( ES11(2020) ) && 연산자의 단축 평가 
<br />

> l\_Operand 가 undefined 나 null 이면 undefined 를 반환하고 아니면 R\_Operand 를 반환   
	*** Falsy 값이라도 null 또는 undefined 가 아니면 R\_Operand 를 반환한다.***   	
>   
<br />

```javascript
let elem = document.querySelector(".elem"); // null 이라면
let result = elem?.elem.getAttributeNode("src").value; // undefined ( 위의 result 의 개선판이다. )
```   
<br />

## null 병합 연산자( ES11(2020) ) || 연산자의 단축 평가
<br />


> l\_Operand 가 undefined 나 null 이면 R\_Operand 를 반환 아니면 l\_Operand 반환   
	*** Falsy 값이라도 null 또는 undefined 이면 R\_Operand 를 반환한다.***   
	옵셔널 체이닝 연산자의 반대이다.
>    
<br />

```javascript
let elem = document.querySelector(".elem"); // null 이면 
let result = elem??document.querySelector(".main"); // .main 
```   
<br />

