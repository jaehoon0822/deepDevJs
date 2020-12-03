# CH26 ES6 함수의추가 기능

<br />

>
ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함술서 호출할 수 있다. 다시말해 ES6 이전의 모든 함수는 callable 이며 constructor 이다.
>

<br />

>
cllable 은[[call]] 내부 메서드가 있는 함수객체를 말하며,   
constructor 는[[construct]] 내부 메서드가 있는 함수객체를 말한다.   
non-contructor 는 [[construct]] 내부메서드가 존재하지 않은 함수 객체를 말한다. 즉 생성자 함수가 아니라는 것이다. 
>

<br />

```javascript

let obj = {
	JEDAI: "Anakin Skyworker",
	changeDasvader: function() {
		return this.JEDAI;	
	}	
}


// 메소드 호출
console.log( obj.chageDasvader ); "Anakin Skyworker"

// 메소드 함수 할당
let copyChangeDasvader = obj.changeDasvader;

// 함수호출 this 값이 window
console.log( copyChangeDasvader() ) // undefined

// new 연산자로 메소드 인스턴스 생성
let DasvaderObj = new obj.changeDasvader(); 

// 인스턴스 객체 
console.log( DasvaderObj ); // {}

```

<br />

>
ES6 에서 함수를 정확하게 구분한다.   
이유는, prototype 을 사용하지 않거나, 생성자 함수를 사용하는데 생성되는 [[construct]] 를 사용하지 않는 함수들을 구분하기 위해서이다. 이 책에서는 생성자 함수로 사용하지 않는데, 만들어지는 prototype 객체가 성능상 좋지 않다고 말한다. 쓸데없이 객체를 생성할 필요가 없다는 것이다. 그리고 object 의 메소드로써 구분되는 뚜렷한 정의가 기존의 자바스크립트에서는 존재하지 않으므로( 객체의 메소드도 그냥 함수이다. ) 이를 정확히 구별할 필요가 있다.
>

<br />

| 함수 | constructor | prototype | super | arguments |
| :---: | :---: | :---: | :---: | :---: |
| function() {} | O | O | X | O |
| Method() {} | X | X | O | O |
| () => {} | X | X | X | X |

<br />

## 메소드 

<br />

>
ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부슬롯 [[HomeObject]] 를 갖는다.
>

<br />

[[HomeObject]] 는 super 키워드를 사용할 때 참조하는 내부슬롯이다. class 에서 보았듯이 super 는 해당 메서드를 바인딩한 객체의 prototype 을 가리킨다.

<br />

## 화살표함수

<br />

>
***" 화살표 함수 내부에서 this, arguemnts, super, new.target 을 참조하면 스코프 체인을 통헤 상위 스코프의 this, arguments, super, new.target 을 참조한다. "***
>

<br />

### this

<br />

this 는 동적으로 할당된다. 자신이 호출되는 시점의 둘러싼 환경에 따라 바인딩이 결정된다( 둘러싼 환경이 객체일경우 바인딩 ).   
<br />

하지만, 화살표함수는 다르다.   
화살표함수의 this는 스코프의 this 가 된다.

<br />


```javascript

class pedigree {
	constructor( lastName ) {
		this.lastName = lastName;	
	}
	sayNames( names  ) {
		names.forEach( function( v ) {
				console.log( v + this.lastName );
		} );
	}
}

const skyWorkerPedigree = pedigree( 'SkyWorker' );

// this 가 undefined 된다. class 는 자동으로 strick 모드로 작동되어 전역 객체를 향하지 않는다는점을 알아두자.

skyWorkerPedigree( ['Anakin', 'Luke' ] );
// Uncaught TypeError: Cannot read property 'lastName' of undefined


````

<br />

반면, 화살표 함수는 this 가 원하는 대로 바인드된다. 

<br />

```javascript

class pedigree {
	constructor( lastName ) {
		this.lastName = lastName;	
	}
	sayNames( names  ) {
		names.forEach( ( v ) => {
				console.log( v + this.lastName );
		} );
	}
}

const skyWorkerPedigree = pedigree( 'SkyWorker' );

skyWorkerPedigree( ['Anakin', 'Luke' ] );

````


<br />
