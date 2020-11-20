# CH6 _데이터타입 정리

## undefined

> undefined 는 해당 변수 선언후 초기화 되지 않았을 때, 자바스크립트 엔진이 할당하는 값이다.   
>

## null

> 변수 선언후 값을 없는 값으로 초기화 하기 위해 사용되는 값이다.   
	이 값은 undefined 와 다르다.

***undefined는 값을 초기화 하지 않은것을 말한다.***   
***null 은 값이 초기화 되었지만, 빈 값으로 정의하기 위해 사용된다.***
>

> 이는 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는 것을 의미한다.
>


> 함수가 유효한 값을 반환할 수 없는 경우 명시적으로 null 을 반환하기도 한다.
>

```javascript
	let elem = document.querySelector( '.undefinedElem' ); // 존재하지 않은 undefinedElem 
	console.log( elem ); // null
```

## 심벌타입

> 심벌은 immutable 한 값이며, 유일하다. 중복은 불가하다. 즉 유일한 값을 만들어낼 용도로 추가된 primitive value 이다.
	ES5 까지 primitive value 는 객체를 제외한 리터럴 값( number, string, boolean, undefined, null ) 이었다. 하지만, ES6 에서 부터 추가된 SYMBOL 은 함수이다.   
	그러므로 SYMBOL 은 객체이다. ( 함수역시 객체이다. ) 
>

```javascript
	let sym = Symbol( "key" );
	const obj = {};
	obj[sym] = "symbol";
	console.log( obj.sym ); //undefined .sym === ["sym"] -> typeof "sym" === string
	console.log( obj[sym] ); // symbol -> typeof symbol === 'symbol'
	for( const i in obj ) console.log( i ); // undefined
	obj.first = 1;
	for( const i in obj ) console.log( i ); // "first"
```

> 코드를 생성해 보면 알겠지만,  SYMBOL 은 for...in 문으로 나열되지 않는다.   
>

> 책에서는 33장에서 SYMBOL 을 다룬다고 한다.
>

## 데이터 타입의 필요성

> 이 책은 마음에 드는것이 추상적으로 설명하기 보다는 메모리를 통해 설명한다.   
	그리고 그 과정에대해 설명해준다. 
>

> 보통의 다른 언어들는 각각의 값 type 을 구분한다.   
	하지만, javascript 는 data type 을 구분하는 keyword 가 없는데, 그렇다고 타입이 존재하지 않지는 않다. 원시타입이 존재한다는 것이 그것이다.   
	단지 사용자 편하도록 원시타입을 자동 형변환 해주는것 뿐이다. 컴퓨터가 해석할 수 있는 코드는 2진수로 이는 기계어로 표현된다.   
	string 같은 경우는 unicode 에 명시된 숫자로, number 는 배정밀도 64비트 부동소수점을 사용한다.
>

> 숫자값을 저장하면 자바스크립트 엔진은 숫자값인지 data type 확인후, 8byte( 64bit ) 를 할당. 저장한다. 이때 저장값은 당연하게도 2진수로 저장된다.
>

> 부동소수점에 대해서 c 언어 책을 다시한번 책을 살펴보자.. 헷갈린다.
>

> 2진수로 저장된 값을 어떻게 의도한 값으로 변환시키느냐이다. string 은 string 에 맞는 2진수로 저장된다.   
	number 역시 그에 맞는 2진수로 저장된다. 같은 2진수인데 이를 구분하기 위해서는 data type 이 존재해야한다.
	data type 으로 인해 같은 2진수도 다른 값으로 해석된다.   
	ASCII 에서 0100 0001 는 'a' 이다. number type 은 65 이다. 이렇게 각 type 에 맞는 값으로 변환한다.   
	이는 프로그래머가 의도한 값을 사용할 수 있도록 해준다.
>

## 동적타이핑

> 정적 타이핑을 지원하는 언어는 명시적 타입 선언을 통해 타입을 지정한다. 하지만 javascript 는 동적 타이핑을 지원한다.
	이는 할당될때 해당 할당된 data 의 type 을 추론하며( type inference ), 재할당시에 type 은 언제든지 변환된다. ( 동적 타이핑 ) 
>

## 동적 타입 언어와 변수

> 필요한 문구를 옮겨본다. 주의해야 할 것들이다.
>

---

> 변수는 꼭 필요한 경우에 한해 제한적으로 사용한다.
>

> 변수의 유효 범위(스코프) 는 최대한 좁게 만들어 변수의 부작용을 억제해야 한다.
>

> 전역 변수는 최대한 사용하지 않도록 한다.
>

> 변수보다는 상수를 사용해 값의 변경을 억제한다.
>

> 변수 이름은 변수의 목적이나 의미를 파악할 수 있도록 네이밍 한다.
>

---

