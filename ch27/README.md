# CH27 배열

<br />

# 자바스크립트 배열은 배열이 아니다.

>
***" 자바스크립트의 배열은 일반적인 배열의 동작을 융내 낸 특수한 객체다.  "***
>

<br />

C 언어에서 배열은 메모리의 나열이다.   
   
8 byte 원소를 3개 가진 배열이라면 다음과 같다.   
   

```c

	int i;
	long long arr[] = { 1, 2, 3 }; // 8byte 원소
	const int len = sizeof( arr ) / sizeof( arr[0] );
	/*
		( index * type ) \ type; -> 갯수 
	 */

	for ( i = 0; i < len; i++ )	
		printf( "%p, ", arr[i] );
	printf( "\n" );
	/* 
		 메모리 시작이 px100 일때,
		 ox100, ox108, ox110 
	 */

```

<br />

>
이러한 배열의 특성을 dense array( 밀집 배열 ) 이라 한다.
>

<br />

>
***" 일반 객체와 배열을 구분하는 가장 명확한 차이는 '값의 순서' 와 'length 프로퍼티' 다. 인덱스로 표현되는 값의 순서와 length 프로퍼티를 갖는 배열은 반복문을 통해 순차적으로 값에 접근하기 적합한 자료구조다.  "***
>

<br />

>
***" 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다. 배열의 요소가 연속적으로 이어져 있지 않는 배열을 희소 배열( sparse array ) 이라 한다. "***
>

<br />

```javascript

console.log( Object.getOwnPropertyDescriptors( [1, 2, 3] ) );
/*

{
	'0': { value: 1, writable: true, enumerable: true, configurable: true },
	'1': { value: 1, writable: true, enumerable: true, configurable: true },
	'2': { value: 1, writable: true, enumerable: true, configurable: true },
length: { value: 3, writable: true, enumerable: false, configurable: false }
}

*/
 
```

<br />

>
***" 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다 . "***
>

<br />

더 자세히 알고 싶다면, p497 을 보자.

<br />

## length 프로퍼티와 희소 배열

<br />

```javascript

const parase = [ , , 1, 2, ];

console.log( Object.getOwnPropertyDescriptors( parase ) );
/*
{
	'2': { value: 1, wratable: true, enumerable: true, configurable: true },
	'3': { value: 2, writable: true, enumerable: true, configurable: true },
	length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/

```

<br />

>
***" 희소 배열은 length 와 배열 요소의 개수가 일치하지 않는다. 희소 배열의 length 는 희소 배열의 실제 요소 개수보다 언제나 크다.  "***
>

<br />

그러므로, 희소 배열을 사용하는것은 좋지 않다. 존재하지 않은 값이기 때문이다. 또한, 이러한 희소 배열은 배열의 나열에 영향을 주어 성능상 좋지 않다고 한다.

<br />

모던 자바스크립트에서는 최적화를 위해 최대한 연속적으로 배열을 나열한다. 책에서는 일반적인 객체와 배열간의 속도를 보여주는 예시가 있다.

<br />

## Array.of

<br />

```javascript

Array.of( 1 ); // -> [1]
Array.of( 1, 2, 3 ); // -> [1, 2, 3]
Array.of( 'string' ); // -> [ 'string' ]

```

<br />

Array.of 는 Array 생성자로 배열을 생성할때, ( 특히 단일숫자로 ) 발생하는 length 적용은 혼란을 줄 여지가 있다고 여긴다. 예전부터 이야기가 많았다. 그런 이유 때문인지 ES6 에서는 Array.of 를 지원한다. 위의 예시를 보면 알겠지만, Array.of 는 단일숫자를 넣어도 단일숫자를 가진 배열을 반환한다.

<br />

## Array.from

<br />

```javascript

let arr_like = {
	'0' : 1,
	'1' : 2,
	'2' : 3,
	length: 3,
};

let fromArr = Array.from( arr_like );
/*
	[1, 2, 3]
*/

let fromArr2 = Array.from( { length: 5 }, ( v, i ) => i + 1 );

/*
	 [ 1, 2, 3, 4, 5 ]
*/

```

<br />

>
***" Array.from 메서드는 유사 배열 객체또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다. "***
>

<br />

첫번째 인자는 array like 객체 혹은 iterable object 를 받는다. 그리고 두번째 인자는 callback 함수를 받는다.   
   
callback 함수는 value, index, object 의 인자 순서대로 받는다.

<br />

## 배열 요소의 참조

<br />

>
***" 희소 배열의 존재하지 않는 요소를 참조해도 undefined 가 반환된다.  "***
>

<br />

이는 너무나 당연하다. 희소 배열을 console.log 해보면 정의 되지 않은 원소는 undefined 가 아닌 empty 가 나온다. 희소 배열을 참조하면 마치 empty 가 나올것 같지만 undefined 가 나온다는 것은 변수와 같이 '정의되지 않은' 이라는 뜻을 갖고 있다. empty 는 값이 정의 되지않은 값을 말하니 없는 변수를 참조하듯 undefined 를 반환한다.

<br />

## 배열 메서드

<br />

>
***" 배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다. "***
>

<br />

### Array.isArray

<br />

전달된 인수가 배열이면 true, 아니면 false 를 반환한다.

<br />

```javascript

let arr = [ 1, 2, 3 ]; 
let arr_like = { '0': 1, '1': 2, '2': 3, length: 3, };

console.log( Array.isArray( arr ) ); // true
console.log( Array.isArray( arr_like ) ); // false

```

<br />

### Array.prototype.indexOf

<br />

>
***" 원본 배열에 인수로 전달한 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스를 반환한다.  "***
>

<br />

>
***" 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1 을 반환한다. "***
>

<br />

### Array.prototype.includes (ES7)

<br />

>
***" Array 에 인자로 준 요소가 포함되어 있는지 확인하는 메소드  "***
>

```javascript

let starWarsCharacters = [
	'C-3PO', 'R2D2', 'Luck', 'Anakin', 'Leia', 'HanSolo', 'Chupaka',
];

if( !starWarsCharacters.includes( 'Darth vader' ) ) {
	starWarsCharacters.push( 'Darth vader' );	
};

console.log( starWarsCharacters.indexOf( 'Darth vader' ) ); // 7;

```

<br />

### stack 생성자 함수 및 클래스

<br />

이 책에서는 pop 과 push 메서드를 사용하여 stack 를 구현해본다. 이는 자료구조로써 자바스크립트에서 stack 을 구현하는 방법이니 알아두는 것이 좋다.

<br />

```javascript

const stack = ( function() {
	function Stack( arr = [] ) {
		if( !Array.isArray( arr ) ) {
			throw new TypeError( `${arr} is not an array.` );
		}
		this.array = arr;
	}
	stack.prototype = {
		constructor: Stack,	
		push( val ) {
			return this.array.push( val );
		},
		pop() {
			return this.array.pop();	
		},
		entries() {
			return [ ...this.array ];	
		}
	};

	return Stack;
} )()

```

<br />

```javascript

class Stack {
	#array;
	constructor( arr = [] ) {
		if( !Array.isArray( arr ) ) {
			throw new TypeError( `${array} is not an array.` );	
		}
		this.#array = arr;
	} 
	push( val ) {
		return this.#array.push( val );	
	}
	pop() {
		return this.#array.pop();	
	}
	entires() {
		retunr [ ...this.#array ];	
	}
}

```

<br />

### pop 생성자 함수 및 클래스

<br />

```javascript

const Queue = ( function() {
	function Queue( arr = [] ) {
		this.array = arr;	
	}
	Queue.prototpye = {
		constructor = Queue,	
		enqueue( val ) {
			this.array.unshift( val );	
		},
		dequeue() {
			this.array.shift();	
		},
		entries() {
			return [ ...this.array ];	
		}
	};

	return Queue;
} )()

```

<br />

```javascript

class Queue {
	#array;

	constructor( arr = [] ) {
		this.#array = arr;	
	}

	enqueue( val ) {
		this.#array.unshift( val );	
	}

	dequeue() {
		this.#array.shift();	
	}

	entries() {
		return [ ...this.#array ];	
	}
}

```

<br />

### Array.prototype.concat

<br />

```javascript

let arr = [ 1, 2 ];
let cocnatArr = arr.concate( [ 3, 4 ], 5 ); // 새로운 배열을 반환
																						// 배열을 해체해서 새로운 																							 배열과 연결
console.log( arr ); // [1, 2 ]
console.log( concatArr ); // [ 1, 2, 3, 4, 5 ];

```

<br />

```javascript

lat arr1 = [ 1, 2 ];
lat arr2 = [ 3, 4 ];

const concatArr = arr1.concat( arr2 );
const spreadArr = [ ...arr1, ...ar2 ];

console.log( concatArr ); // [1, 2, 3, 4]
console.log( spreadArr ); // [1, 2, 3, 4]

// 결과 값이 동일하다.

```

<br />

### Array.prototype.splice

<br />

>
***" splice 메서드는 3개의 매개변수가 있으며 원본 배열을 직접 변경한다. "***
>

<br />

* start: 원본 배열의 요소를 제거하기 시작할 인덱스, start 만 입력하면 start 지점의 인덱스부터 모든 요소를 제거, 음수 지정시 인덱스 끝에서부터 start 지점을 지정

<br />

* deleteCound: 삭제할 갯수를 지정, 0 이면 제거 안함

<br />

* items: 제거한 위치에 삽입할 요소, 입력 안하면 삭제만 한다.

<br />

```javascript

let arr = [ 100, 90, 98, 99 ];
let spliceArr = arr.splice( 1, 2. 100. 100 );
// splice 는 제거한 요소를 배열로 반환한다.
// splice 는 start, deleteCound, items 를 넣어 원본 배열을 수정한다.

console.log( arr ); // [ 100, 100, 100, 99 ]
console.log( spliceArr ); // [ 90, 98 ]

```

<br />

```javascript

let arr = [ 100, 90. 98. 99 ];
let spliceArr = arr.splice( 1 );
// start 만 지정하면 start 지점의 index 부터 끝까지 모든 요소를 제거한다.

console.log( arr ); // [ 100 ]
console.log( spliceArr ); // [ 90, 98, 99 ]

```

<br />

```javascript

const remove = ( arr, idx ) => {
	let index = arr.indexOf( idx );
	if( index === -1 ) arr.splice( idx, 1 );
	return arr;	
};

```

<br />

```javascript

const OverlapRemoveAll = ( arr, item ) => {
	return arr.filter( v => v !== item );
}; 

```

<br />

### Array.prototype.slice

<br />

>
***" slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열을 반환한다.  "***
>

<br />

* start: 복사를 시작할 인덱스다. 음수로 지정하면 배열의 끝에서 인덱스를 나타낸다. 

<br />

* end: 복사를 종료할 인덱스다.  이 인덱스에 해당하는 요소는 복사되지 않는다. 생략시 기본값은 length 값이다.

<br />

```javascript

const arr = [ 100 , 90, 98, 99 ];
const sliceArr = arr.slice( 1 );

console.log( arr ); // [ 100, 90, 98, 99 ];
// arr 의 값은 변경되지 않는다.
console.log( sliceArr ); // [ 90, 98, 99 ];
// 단지 arr 에서 slice 한 값을 반환한다.
 
// 여기서 중요한것은 end 값이다. 해석할때 start 부터 end 지점 전까지 복사하여 배열로 반환한다는 것이다. end 값을 지정 index - 1 로 생각하는게 덜 헷갈릴 듯 싶다.

```

<br />

```javascript

const arr = [ 
	{ id: 1, score: 100 }, 
	{ id: 2, score: 90 }, 
	{ id: 3, score: 98 }, 
	{ id: 4, score: 99 }, 
];

const sliceArr = arr.slice(); /*  [ 
																		{ id: 1, score: 100 }, 
																		{ id: 2, score: 90 }, 
																		{ id: 3, score: 98 }, 
																		{ id: 4, score: 99 }, 
																	];
*/

console.log( arr == sliceArr ); // false
console.log( arr[0] == sliceArr[0] ) // true
// shallow copy 를 통한 복사이다.

```

<br />

```javascript

const sum = function() {
	/ * const arr = Array.prototype.slice.call( arguments ); 
	const arr = Array.from( arguments ); */ 
	const arr = [ ...arguments ] // 유사배열을 배열로 반환
	// spread operator 를 사용하여 쉽게 배열로 변환.
	// ES6 부터 유사배열객체에 Symbol.iterator 가 내부에 존재하며, 이는  iterable Object 이라 부른다.
	return arr.reduce( ( pre, cur ) => pre + cur, 0 );
};

console.log( sum( 1, 2, 3 ) ); // 6

```

<br />

### Array.prototype.join

<br />

>
***" join 메서드는 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다. "***
>

<br />

```javascript

const ar = [ 1, 2, 3, 4 ];

arr.join(); // 1,2,3,4
arr.join( '/' ) // 1/2/3/4
arr.join( '' ) // 1234

```
<br />

### Array.prototype.reverse

<br />

```javascript

const arr [1, 2, 3, 4];
const result = arr.revers();

console.log( result ); // [4, 3, 2, 1];
console.log( arr ); // [4, 3, 2, 1];

// 주의점은 원본배열을 변경하며, 반환값도 변경된 배열을 반환한다는 것이다.
// 부수효과가 있다는것을 알아두자.

```

<br />

### Array.prototype.fill

<br />

>
***" ES6 에서 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. "***
>

<br />

```javascript

const arr = [1, 2, 3, 4];

arr.fill( '1' );
console.log( arr ); [ '1', '1', '1', '1' ]

arr.fill( '2', 1 ); // 두번째 인자는 첫번째 인자로 채울 시작 index 를 지정한다.
console.log( arr ); [ '1', '2', '2', '2' ]

arr.fill( '3', 2, 3 ); // 마지막 인자는 멈출 index 를 지정한다.
console.log( arr );

/* fill 은 원본배열을 변환한다. 부수효과가 있으니 조심할 것. */

```

<br />

```javascript

/* 이 책에서 Array.from 을통해 각 배열을 순차적을 채우는 예시를 보여준다. */

const sequence = ( length = 0 ) => Array.form( { length }, ( v, i ) => i );
console.log( sequence( 3 ) ); //  [ 0, 1, 2 ]

/* 이런식의 활용법을 계속 작성해 보는건 좋은 공부인거 같다. */

```

<br />

### Array.protoype.includes

<br />

```javascript

const arr = [1, 2, 3, 4];

/* arr 에 인자값이 포함되어 있는지 확인 */

arr.includes( 2 ); // true
arr.includes( 5 ); // false

/* 
	 2 번째 인자값은 해당 index 부터 값이 포함되어 있는지 확인 
	 값지정 안할시 시작 index 는 0,
	 음수 지정시 length + 음수 의 index
*/

arr.includes( 2, 1 );  // true
arr.includes( 2, 2 );  // false

```

<br />

### Array.prototype.flat 

<br />

>
***" ES10(ECMAScript 2019) 에 도입된 flat 메서드는 인수로 전달한 깊이만큼 제귀적으로 배열을 평탄화한다. "***
>

<br />

```javascript

[ 1, [ 2, [ 3, [ 4 ] ] ] ].falt(); // [ 1 ,2 [ 3 ,[ 4 ] ] ]
[ 1, [ 2, [ 3, [ 4 ] ] ] ].falt(1); // [ 1 ,2 [ 3 ,[ 4 ] ] ]
[ 1, [ 2, [ 3, [ 4 ] ] ] ].falt(2); // [ 1 ,2, 3 ,[ 4 ] ]
[ 1, [ 2, [ 3, [ 4 ] ] ] ].falt().flat(); // [ 1 ,2, 3 ,[ 4 ] ]
[ 1, [ 2, [ 3, [ 4 ] ] ] ].falt(infinity); // [ 1 ,2, 3, 4 ]

```

<br />

## 배열 고차 함수

<br />

>
***" 고차함수는 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다. "***
>

<br />

### Array.prototype.sort

<br />

```javascript

const alp = [ 'Z', 'D', 'A', 'B', 'C' ];

alp.sort(); // ['A', 'B', 'C', 'D', 'Z']
console.log( alp ); // ['A', 'B', 'C', 'D', 'Z']

```

<br />

```javascript

const pts = [ 100, 20, 300, 4 ];

pts.sort();
console.log( pts ); // [ 100, 20, 300, 4 ] 

```

<br />

>
***" sort 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따른다. 배열의 요소가 숫자 타입이라 할지라도 배열의 요소를 일시적으로 문자열로 변환한 후 유니코드 포인트의 순서를 기준으로 정렬한다.  "***
>

<br />


>
***" sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다. "***
>

<br />

```javascript

const pts = [ 100, 20, 300, 4 ];

pts.sort( ( a, b ) => a - b ); // Ascending [ 4, 20, 100, 300 ]
pts.sort( ( a, b ) => b - a ); // Decending [ 300, 100, 20, 4 ]

```

<br />


```javascript

const brands = [
	{ key: 3, brand: ""FILA" }
	{ key: 1, brand: "ADIDAS" }
	{ key: 2, brand: "NIKE"}
];

const objAcending = ( key ) => {
	return ( a, b ) => a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0;
};

const objDecending = ( key ) => {
	return ( b, a ) => a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0;
};

brands.sort( objCompare( 'key' ) );

console.log( brands );

```

<br />

a 와 b 값을 비교후 1 이면, a 를 오른쪽으로( Acending ) 이동한다.    
-1 이면 b 값은 a 보다 더큰 값이므로 그 다음부터 b 를 a 로 두고 다음 값을 b 로비교한다.   
0 이면 a 의 값을 그대로둔다.   
   
이 값을 반대로 지정하면 Decending 이된다.

<br />

아직 알고리즘에 대한 기초가 없다. 공부해야할 부분이다. 그러므로 위의 생각이 정확히 맞지는 않을 것이다. 위의 경우는 bubble sort 를 기준으로 생각했다. 아마 이러한 토대에서 만들어지지 않았을까 생각한다. 아닐수도 있고,,, 이 책에서는 quick sort 에서 timesort 알고리즘으로 변경되었다고 한다. 

<br />

### Array.prototype.forEach

<br />

```javascript

const arr = [ 100, 200, 300 ];
const pows = [];
const _pows = [];

arr.forEach( ( v, i, o ) => pows[i] = v**2 );
/*
	v = arr[index]
	i = index
	o = arr

	forEach 는 this의 value, index, this 를 callback 으로 전달한다.
*/

// 로직은 다음과 비슷할 것이다.

Array.prototype._forEach = function( callback, thisArg ) {
	const len = this.length;
		for( let i= 0; i < len; i++ ) {
			callback( this[i], i, this );	
		}

};

arr._forEach( ( v, i, o ) => _pows[i] = v**2 );

console.log( pows ); // [10000, 40000, 90000 ]
console.log( _pows ); // [10000, 40000, 90000 ]

// 미처 생각하지 않았던 것인데 forEach 의 this 를 통해서 원본배열역시 변경 가능하다.

arr.forEach( ( i, v, o ) => o[i] = v**2 );

console.log( arr ) // [10000, 40000, 90000 ]

```
<br />

```javascript

// class 를 많이 사용해보지 않아 아직 어색하다. 이 책에서는 class 를 어떻게 활용하는지 보여준다.

class Numbers {
	numArray= [];

	multiply( arr ) {
		arr.forEach( v => this.numArray.push( v * v ) );
	}
}

 /*
class Numbers {
	numArray = [];

	multiply( arr ) {
		arr.forEach( function( v ) {
			this.numArray.push( v * v );	
		}, this ); // forEach 는 마지막 인자값으로 this 를 받는다.	
	}
}
*/

const num = new Numbers();
num.mulitply( [2, 3, 4 ] ); 
console.log( num.numArray ); // [ 4, 9, 16 ]

```

<br />

책의 마지막 부분에 forEach 에 대한 polyfill 을 제공한다.

<br />

### Array.prototype.map

<br />

```javascript

const arr = [ 100, 200, 300 ];

const pows = arr.map( ( v, i, o ) => v**2 );

console.log( pows ); // [ 10000, 40000, 90000 ] 

```

<br />

```javascript

class Prefixer {
	constructor( prefixer ) {
		this.prefixer = prefixer;
	}
	add( arr ) {
		return arr.map( v => {
			return this.prefixer + v;		
		} );
	}
}

const webkit = new Prefixer( '-webkit-' );
let webkit_prefixer = webkit.add( [ 'flex-item', 'flex', 'grid', 'grid-columns' ] );

console.log( webkit_prefixer );
/*
	[
		'-webkit-flex-item',
		'-webkit-flex',
		'-webkit-grid',
		'-webkit-grid-columns',
	] 
 */

// 로직은 다음과 비슷할 것이다.


Array.prototype._map = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function' );
	thisArg = thisArg || window;
	const len = this.length;
	const arr = [];

	for( let i = 0; i < len; i++ ) {
		arr[i] = callback.call( thisArg, this[i], i, this );	
	}

	return arr;
}

```

<br />

### Array.prototype.filler

<br />

```javascript

// 로직은 다음과 비슷할 것이다.

Array.prototype._filter = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'no function' );
	thisArg = thisArg || global;
	var len = this.lenght;
	var arr = [];

	for( var i = 0; i < len; i++ ) {
		if( callback.call( thisArg, this[i], i, this ) ) arr.push( this[i] );	
	}

	return arr;
}

// 책에 명시되어 있는code 이다.

```javascript

class User {
	users = [
		{ id: 1, name: 'Lee' },	
		{ id: 1, name: 'Choi' },	
		{ id: 1, name: 'Han' },	
		{ id: 2, name: 'Kim' },	
	];	

	findById( id ) {
		this.users.filter( v => v.id == id );	
	}

	removeAll( id ) {
		this.users = this.users.filter( v => v.id !== id );	
	}

	remove( id ) {
		const idx = this.users.indexOf( id );	
		this.users.splice( idx, 1 );
	}
}

user = new User();

user.remove( 1 );
console.log( user.users );

user.removeAll( 1 );
console.log( user.users );

```

<br />

<br />

```javascript

Array.prototype._findIndex = function( callback ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'no function' );
	thisArg = thisArg || global;
	var len = this.lenght;

	for( var i = 0; i < len; i++ ) {
		if( callback.call( thisArg, this[i], i, this ) ) return i;	
	}
}


class User {
	users = [
		{ id: 1, name: 'Lee' },	
		{ id: 1, name: 'Choi' },	
		{ id: 1, name: 'Han' },	
		{ id: 2, name: 'Kim' },	
	];	

	findById( id ) {
		this.users.filter( v => v.id == id );	
	}

	removeAll( id ) {
		this.users = this.users.filter( v => v.id !== id );	
	}

	remove( id ) {
		this.users.splice( this.users._findIndex(
			v => v.id == id;					
		), 1 );
	}
}

users = new User();

console.log( users.users );
/*
 	[
		{ id: 1, name: 'Lee' },	
		{ id: 1, name: 'Choi' },	
		{ id: 1, name: 'Han' },	
		{ id: 2, name: 'Kim' },	
	]
 */

let user = users.findByID( 1 );
/*
 	[
		{ id: 1, name: 'Lee' },	
		{ id: 1, name: 'Choi' },	
		{ id: 1, name: 'Han' },	
	]
 */

users.remove( 1 );
console.log( users.users );
/*
 	[
		{ id: 1, name: 'Choi' },	
		{ id: 1, name: 'Han' },	
		{ id: 2, name: 'Kim' },	
	]
 */

users.removeAll( 1 );
console.log( users.users );
/*
 	[
		{ id: 2, name: 'Kim' },	
	]
 */

```

<br />
 
### Array.prototype.reduce

<br />

```javascript

/* 로직은 다음과 비슷할것 같다. */

Array.prototype._reduce = function( callback, init, thisArg ) {
	if( typeof callback !== 'function' ) new TypeError( 'not function' );	
	thisArg = thisArg || global;
	var len = this.length;

	for( var i = 0; i < len; i++ ) {
		init = callback.call( thisArg, init, this[i], i, this );	
	}

	return init;
};

```

<br />

이 책에서는 reduce 를 어떻게 활용하는지 보여준다.

<br />

### 평균구하기

<br />

```javascript

const values = [100, 94, 80, 75, 80 ];
const callback = ( acc, cur, i, { lenght } ) => i == lenght - 1 ? acc / lenght : acc + cur; 

values.reduce( callback, 0 );

```

<br />

### 최대값 구하기

<br />

```javascript

const values = [100, 94, 80, 75, 80 ];
const callback = ( acc, cur ) => acc > cur ? acc : cur;

const max = values.reduce( callback, 0 );

console.log( max );
/* 85.8 */
```

<br />

### 중복 횟수 구하기

<br />

```javascript

const names = [ 'Kim', 'Lee', 'Kim', 'Park' ];
const callback = ( acc, cur ) => {
	acc[cur] = ( acc[cur] || 0 ) + 1;
	return acc;
}

const count = names.reduce( callback, {} );
console.log( count );
/*
	{ Kim: 2, Lee: 1, Park: 1 } 
*/

```

<br />

### 중첩 배열 평탄화

<br />

```javascript

const values = [ 100, [ 94 ], 80, [ 75 ], 80 ];
const callback = ( acc, cur ) => {
	acc.concat( cur );
	return acc;
};

const result = values.reduce( callback, [] );
console.log( result );
/*
	[ 100, 94, 80, 75, 80 ] 
 */

// flat 은 다음과 같이 사용할 수 있을 듯 싶다.

Array.prototype._flat = function( len ) {
	len = len || 1;
	const callback = function( acc, cur ) {
		acc.concat( cur );	
		return acc;
	};
	let result = this;
	for( var i = 0; i < len; i++ ) {
		result = result.reduce( callback, [] );			
	} 

	return result;
};

const arr = [ 1, [ 2, [ 3, [ 7 ], 4 ], 5 ];

let flatArr = arr._flat()
console.log( flatArr );
/*
	 [ 1, 2, [ 3, [ 7 ] ], 4, 5 ]
*/

let flatArr = arr._flat(2)
console.log( flatArr );
/*
	 [ 1, 2, 3, [ 7 ], 4, 5 ]
*/

let flatArr = arr._flat(3)
console.log( flatArr );
/*
	 [ 1, 2, 3, 7, 4, 5 ]
*/
```

<br />

### 중복 요소 제거

<br />

```javascript

const values = [ 100, 100, 84, 89, 90, 89, 90 ];
const callback = ( acc, cur, i, arr ) => {
	if( arr.indexOf( cur ) == i ) acc.push( cur );
	return acc;
};

let result = values.reduce( callback, [] );
console.log( result );
/*
[ 100, 84, 89, 90 ] 
*/

```

<br />

```javascript

/* 유일한 값의 집합인 Set 를사용할 수 있다고 한다. 책을 보니 유용해 보인다.*/
const values = [ 100, 100, 84, 89, 90, 89, 90 ];
let result = [ ...newSet( values ) ];

console.log( result );
/*
[ 100, 84, 89, 90 ] 
*/

```

<br />

이 책에서 reduce 를 사용할때 초기값을 전해주라고 하는데,   
이는 내가 위에 만든 \_reduce  로직에 위한 것으로 생각된다.   
( 아닐수 있다. 저런식으로 구성되었을것 같다하는 느낌이다. )
   
init 은 callback 에 재삽입된다. 그로인해 항상 callback 은 return 되어야한다.   
그래야만 init 에 삽입되고 다시 callback 에서 재사용하기 때문이다.   
만약 init 값을 지정하지 않는다면 NaN 이나 TypeError 가 발생할 수 있다.   
이는 undefined 와 어떠한 연산을 하기 때문이다.   
예측할수 없는 값이 나올 수 있으므로 항상 초기값을 지정해야 한다.

<br />

## Array.prototype.some

<br />

이 함수는 원소중 하나라도 조건에 맞으면,  true 를 반환한다.   
아니면 전부 조건에 안맞으면 false 이다.

<br />

```javascript

/* 대략 이런 로직이지 않을까? */

Array.prototype._some = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function.' );
	var len = this.length;	
	thisArg = thisArg || global;

	for( var i = 0; i < len; i++ ) {
		if( callback.call( thisArg, this[i], i, this ) ) return true; 	
	}

	return false;
};


```

<br />


```javascript

let bol = [ 1, 2, 3 ].some( v => v > 2 );
console.log( bol ); // true

bol = [ 1, 2, 3 ].some( v => v > 3 );
console.log( bol ); // false

```

<br />

## Array.prototype.every

<br />

```javascript

/* 로직은 다음과 비슷할 것이다. */

Array.prototype._every = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function' );
	thisArg = thisArg || global;
	var len = this.length;

	for( var i = 0; i < len; i++ ) {
		if( !( callback.call( thisArg, this[i], i, this ) ) ) return false;	
	}
	return true;
};

```

<br />

```javascript

let bol = [ 1, 2, 3 ].every( v => v > 1 );
console.log( bol ); // false

let bol = [ 1, 2, 3 ].every( v => v > 0 );
console.log( bol ); // true

```

<br />

## Array.prototype.find

<br />

배열의 요소중 인수로 전달된 콜백함수를 호출하여 조건이 맞는다면, 해당 값을 출력하는 메서드이다.

<br />

```javascript

Array.prototype._find = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function.' );
	var len = this.length;	
	thisArg = thisArg || global;

	for( var i = 0; i < len; i++ ) {
		if( callback.call( thisArg, this[i], i, this ) ) return this[i]; 	
		/* 값 반환*/
	}

	return undefined;
	/* 검색에 없을시 undefined 반환 */
};

```

<br />

로직을 보면 알수 있듯이, find 함수는 일치하는 값을 반환한다.   
이말은 일치하는 첫번째 요소만을 반환하고 로직이 끝난다는 것이다.

<br />

## Array.prototype.findIndex

<br />

배열의 원소에 일치하는 인덱스 값을 반환한다. 일치하는 원소가 없다면 -1 을 반환한다.

<br />

```javascript

Array.prototype._findIndex = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function.' );
	var len = this.length;	
	thisArg = thisArg || global;

	for( var i = 0; i < len; i++ ) {
		if( callback.call( thisArg, this[i], i, this ) ) return i; 	
	}

	return -1;
}

```

<br />

## Array.prototype.flatMap

<br />

>
***" ES10 에서 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다.  "***
>

<br />

```javascript

Array.prototype._flatMap = function( callback, thisArg ) {
	if( typeof callback !== 'function' ) throw new TypeError( 'not function.' );
	thisArg = thisArg || global;

	var len = this.length;	
	var result = []; 

	for( var i = 0; i < len; i++ ) {
		result.push( callback.call( thisArg, this[i], i, this ) );
	}

	return result.flat();
};

```

<br />

```javascript

arr.map( x => x.split('') ).flat(); // --> flatMap 과 결과가 같다.


```
<br />

>
***" flatMap 메서는 flat 메서드처럼 인수를 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화한다. "***
>

