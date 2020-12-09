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
				console.log( `${v} ${this.lastName}` );
		} );
	}
}

const skyWorkerPedigree = new pedigree( 'SkyWorker' );

// this 가 undefined 된다. class 는 자동으로 strick 모드로 작동되어 전역 객체를 향하지 않는다는점을 알아두자.

skyWorkerPedigree.sayNames( ['Anakin', 'Luke' ] );
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
				console.log( `${v} ${this.lastName}` );
		} );
	}
}

const skyWorkerPedigree = new pedigree( 'SkyWorker' );

skyWorkerPedigree.sayNames( ['Anakin', 'Luke' ] );

// Anakin Skyworker
// Luke Skyworker

````

<br />

>
***" 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조함ㄴ 상위 스코프틔 this를 그대로 참조한다. 이를 lexical this 라 한다. "***
>

<br />

```javascript

// 책에서 화살표 함수에 대하해 bind 를 사용해 표현했다. 매우 눈에 확 들어오는 예시이다.

// 화살표 함수
() => this.x;

// bind native method로 this 바인드
( function() { return this.x; } ).bind( this );

// 화살표 함수는 마치 bind method 를 사용하여 상위스코프의 this를 바인드 한것처럼 움직인다.

```

<br />

>
화살표 함수는 사용시 매우 조심해야 한다.    
화살표 함수는 자신을 둘러싼 가장 가까운 함수의 this 를 바인드 한다. 그러므로, 
자신을 둘러싼 환경이 함수가 아니라면 전역객체를 가리킨다. 이는 객체의 메소드러 화살표 함수를 할당할때도 생기는 문제이다.문제이다.
>

<br />

```javascript

const JEDAI = {
	name: 'Luke',
	sayName: () => `My name is ${this.name}.`;
};

console.log( JEDAI.sayName() );
// My name is undefined.

/* 화살표함수는 자신의 this 를 갖고 있지 않다.    
자신의 부모 스코프에서 this 를바인딩 한다. 여기서는 상위 스코프인 Global 을 가진다.    
그러므로 메소드로 할당하는건 좋지 않다.*/

```

<br />

클래스에서 정의 할때는 this 바인딩에 원하는대로 바인딩 된다. 왜 그럴까?

<br />

```javascript

class Pedigree {
	lastName = 'SkyWorker';
	sayNames = ( names ) => names.forEach( v => console.log( v ); ); 
};

const SkyWorkerPedigree = new Pedigree();

/* 위는 다음과 같다.

class Pedigree {
	constructor() {
		this.lastName = 'SkyWorker';	
		this.sayNames =( names ) => names.forEach( v => console.log( v ); ); 
	}
}

const SkyWorkerPedigree = new Pedigree(); // { lastName, sayNames }

*/

SkyWorkerPedigree.sayNames( ['Anakin', 'Luke' ] );

```

좋지않다. 만약 해당 class 를 연속해서 만들게 되면 중복되는 sayNames 가 계속 생겨날 것이다. 이러한 메소드는 prototype에 할당하는 것이 좋다.   

그러므로, class 에서 메소드 등록시 화살표함수 보다는 메소드함수를 사용할 것을 권장한다.

<br />

this 외의 super 와 arguments 역시 상위 스코프의 값을 참조한다. 

<br />

## Rest 파라미터

<br />

arrow function 에서 arguments 를 사용하지 않는다면, 다른 대체 방법이 필요할 것이다. 그것이 바로 Rest parameter 이다.

<br />

```javascript

const plusNum = ( one, ...rest ) => { 
	// one = 1;
	// rest = [2, 3, 4 ];

  const arr = rest.map( v => v + 1 );
	arr.unshift( one );
	return arr;
};

const result = plusNum( 1, 2, 3, 4 );

console.log( result ); // [1, 3, 4, 5 ]

```

<br />

이는 매우 편리한것이 유사배열이 아닌 배열이기에 배열의 native method 를 쓸수 있다. 기존의 arguments 객체는 유사배열로써 배열 객체로 변환해서 사용했다. arguments 보다 더 편리한 기능이다.

<br />

이책에서는 몇가지 주의사항을 말해준다.   

<br />

>
1.Rest parameter 는 마지막에 있어야 한다.
>

<br />

>
2.Rest parameter 는 하나만 선언가능하다.
>

<br />

>
3.Rest parameter 는함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.
>

<br />

이 주의사항만 지키면 별 다른 에러는 없다. 매우 편리하게 사용가능하다.

<br />

## 매개변수 기본값

<br />

ES6 에서는 매개변수를 초기화 할수 있다. 이는 매우 유용하며, 기존 ES5 에서는 매개변수가 없으면 undefined 였다. 귀찮게 함수 매개변수를 일일히 입력할 필요가 없어진 셈이다.

<br />


