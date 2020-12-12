# ch28 Number

<br />

## Number 생성자 함수[

<br />

>
***" Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면  [[NumberData]]내부슬롯에 0 을 할당한 Number 래퍼 객체를 생성한다.  "***
>

<br />

더 정확한 정보는 p552 에서 살펴보자.

<br />

## Number 프로퍼티

<br />

### Number.EPSILON

<br />

>
***" EPSILON 은 부동소수점으로 발생하는 오차를 해결하기 위해 사용한다. 어떤 수들의 연산에서 Number.EPSILON 보다 작은수면 같은 수로 인정한다.  "***
>

<br />

이 부분은 부동소수점에 대한 이해가 있어야 한다. C 언어의 정석책을 보면서 익히기는 했는데, 완벽하지 않다. 여전히 헷갈리는 부분이 있어서 다시한번 책을 보고 이해할 수 있도록 해야겠다. 일단 이 Number.EPSILON 은 부동소수점으로 만들어지는 오차가 Number.EPSILON ( 2.2204460492503130808472633361816 x 10^-16 ) 의 값보다 작다면, 같은 같으로 인정한다는 것이다.

<br />

```javascript

/* 책에 나오는 예시다. */

function isEqual( a, b ) {
	return Math.abs( a - b ) < Number.EPSILON;
}

isEqual( .1 + .2, .3 ); // true

```

<br />

나머지 프로퍼티는 책을 살펴보자.

<br />

## Number 메서드

<br />

### Number.isFinite

<br />

Infinity 인지 아닌지 확인하는 메서드. Infinity 가 아니면 true 맞으면 false 이다.

<br />

>
***" 빌트인 전역함수인 isFinite 와 Number.isFinity 는 차이가 있다. 빌트인 전역함수 isFinite 는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만, Number.isFinite 는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.  "***
>

<br />

```javascript

Number.isFinite( null ) // false
isFinite( null ) // true -> null == 0 으로 타입변환 수행  

```

<br />

### Number.isInteger

<br />

>
***"ES6 에서 도입된 Number.isInterger 정적 메서더는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 변환한다.  "***
>

<br />

### Number.isNaN

<br />

>
***" ES6 에서 도입된 Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN 인지 검사하여 그 결과를 불리언 값으로 변환한다. "***
>

<br />

>
***" isNaN 과 차이가 있다. 빌트인 전역함수 isNaN 은 전달받은 인수를 숫자로 암묵적 타입변환하여 검사를 수행하지만 Number.isNaN 메서드는 전달받은 인수를 숫자로 암묵적 타입변환하지 않는다.  "***
>

<br />

```javascript

Number.isNaN( undefined ); // false
isNaN( undefined ); // true -> 인수를 숫자로 암묵적 타입변환한다. undefined 는NaN 으로 암묵적 타입변환된다. 
isNaN( null ); // false -> null 은 0 으로 타입변환한다.

```

<br />

### Number.isSafeInteger

<br />

>
***" 인수로전달된 숫자값이 안전한 정수인지 검사하여 그결과를 불리언 값으로 반환한다. 안전한 정수값은 -( 2^53 - 1 ) 과 2^53 - 1 사이의 정수값이다. "***
>

<br />

### Number.prototype.toExponential

<br />

>
***" 숫자를 지수 표기법으로 변환하여 문자열로 반환한다. "***
>

<br />

```javascript

	77.toExponential(); 
//^^

//Uncaught SyntaxError: Invalid or unexpected token

	(77).toExponenetial(); // "7.7e+1"

```
<br />

>
***" 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다. "***
>

<br />

>
***" 숫자 뒤의 . 은 의미가 모호하다. 부동 소수점 숫자의 소수 구분 기호일 수도 있고, 객체 프로퍼티에 접근하기 위한 프로퍼티 접근 연산자일 수도 있다. 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석한다. 77 은 Number 래퍼 개체다. 따라서 77 뒤의 .을 소수구분 기호로 해석하면 뒤에 이어지는 toExponential 을 프로퍼티로 해석할 수 없으므로 에러가 발생한다. ( 2번째 . 은 프로퍼티 접근 연산자로 해석한다. )"***
>

<br />

>
***" 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기 위해 그룹 연산자를 사용할 것을 권장한다.  "***
>

<br />

>
***" 정수 뒤에 공백이후 . 이 오면 이것역시 프로퍼티 연산자로 해석한다. "***
>

<br />

알아두자. 이런게 피와 뼈가된다. 디테일의 차이를 만든다.

<br />

### Number.prototype.toFixed 

<br />

>
***" 숫자를 반올림하여 문자열로 반환한다. "***
>

<br />

```javascript

( 123.67 ).toFixed(); // "124" 
( 123.67 ).toFixed(1); // "124.7" 
/* 인자로 소수점 이하 유효 자리수를 반올림하여 나타낸다.*/

```

<br />

### Number.prototype.toPrecision 

<br />

>
***" 인수로전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다. 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다. "***
>

<br />

```javascript

(12345.6789).toPrecision(); // 12345.6789
(12345.6789).toPrecision(1); // 1e+4

```

<br />

>
***" 전체자릿수를 나타내는 0 ~ 21 사이의 정수값을 인수로 전달할 수 있다. 생략하면 0 이다. "***
>

<br />

### Number.prototype.toString

<br />

>
***" 숫자를 문자여ㅛㄹ로 변환하여 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. "***
>


