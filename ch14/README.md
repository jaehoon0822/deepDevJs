# CH14 전역변수의 문제점 

<br />

### 암묵적 결합

<br />

> ***" 모든 코드가 전역변수를 참조하고 변경할 수 있는 암묵적 결합을 허용하는 것이다. 변수의 유효범위가 크면 클수록 코드의 가독성은 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다. "***
>

<br />

### 긴 생명주기

<br />

> ***" 전역 변수의 생명주기는 길다. 따라서 메모리 리소스도 오랜기간 소비한다. 또한 전역 변수의 상태를 변경할 수 있는 시간도 길고 기회도 많다. "***  
>

<br />


### 스코프 체인 상에서 종점이 존재

<br />

> ***" 변수를 검색할때 전역 변수가 가장 마지막에 검색된다. 즉, 검색속도가 가장 느리다. "***
>

<br />


### 네임스페이스 오염

<br />

> ***" 자바스크립트의 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다는 것이다. ... 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다. "***
>


### 모듈패턴

<br />

> ***" 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉( information hiding ) 이라 한다. "***
>

<br />

> ***" 모듈 패턴은 전역 스페이스의 오염을 막는 기능은 물론 한정적이기는 하지만 정보 은닉을 구현하기 위해 사용한다. "***
>

<br />

```javascript

const getStr = ( function () {
	let str = "init_string"; // information hiding( private member )
	return { // public members
		getStr() {
			return str;
		},	
		setStr( newStr ) {
			str = newStr	
		}
	}	
}() ); //--> module pattern

console.log( getStr.str ); // undefined
console.log( getStr.getStr() ); // "init_string"
console.log( getStr.setStr( "set_string" ) );
console.log( getStr.getStr() ); // "set_string"

```
<br />

## ES6 모듈

<br />

> ***" ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다. "***   
( Chrome 61, FF60, SF 10.1, Edge 16 이상 )
>

> ES6 모듈을 제공하는 브라우저에서는 script 태그의 type 을 통해 module attribute 를 제공한다. 이로써 모듈을 사용할 수 있게 된다.
>

```html

<script type="module" src="file.js"></script>

```
