# CH11 원시 값과 객체의 비교

<br />

## 원시 값

<br />

### 변경 불가능한 값

<br />

> ***" primitive type 의 값, 즉 원시 값은 변경 불가능한 값 ( immutable value ) 이다.  "***   
	즉,  원시값은 read only 값이다.
>

<br /> 

> ***" 변경 불가능하다는 것은 변수가 아니라 값에 대한 진술이다. "***   
>

<br />

> 메모리 주소공간을 가리키는 것이 변수이다. " 가리킨다 " 라고 하는 단어가 중요하다.   
	이는 메모리에 값이 할당되면 그 메모리를 가리킨다는 것이다. 그러므로 변수의 값이 변경되면 변경된 값을 저장한 메모리를 가리킨다.   
	즉 메모리에 저장된 값을 변경하는 것이 아니라, 값이 생성된 메모리의 위치를 변경한다는 것이다.   
	그렇다면 변경 불가능한 값이라는 건 메모리에 할당된 값이다. 변수에 새로운 값을 할당한다는건   
	단지 값이 새로운 메모리에 할당되는 것이고, 그 할당된 메모리의 위치를 변수가 가리키고 있는 것일 뿐이다.   
***값은 변경되지 않고, 메모리주소 위치를 새롭게 할당할 뿐이다.***   
***" 이러한 값의 특성을 불변성( immutability ) 라 한다. "***
>

<br />

> ***" 상수는 재할당이 금지된 변수를 말한다. "***   
	이 말은 한번 값이 할당되면 해당 할당된 값을 가진 메모리를 가리키고 다른 메모리를 가리킬 수 없다는 것이다.   
	상수는 할당된 처음 그 메모리 주소만을 가리킨다.
>

<br />

> ***" javascript 에서는 문자열을 원시타입으로 처리한다. "***   
***" C 에서는 하나의 문자를 위한 데이터 타입( char ) 만 있을 뿐 문자열 타입은 존재하지 않는다. ... JAVA 에서는 문자열을 String 객체로 처리한다. "***		
	자바스크립트는 따로 타입 키워드가 존재하지 않는다. 그러므로 자동 형변환을 지원한다.   
	자바스크립트는 문자열에 대해서도 원시타입을 지원하며, 문자열 원시 타입은 하나의 데이터로써 취급된다.   
	이 말은 문자열을 하나의 데이터 취급하므로, 문자열을 메모리에 저장하기 위해 타입검사 평가후 문자열의 byte 에 맞는 메모리 공간을 만들어 낸후   
	할당한다고 생각한다. ( 유니코드를 지원하는 것으로 알고 있다. 유니코드는 문자당 2byte 의 공간을 차지한다. )   
	변수는 이렇게 생성된 메모리 공간의 주소를 가리킨다.   
***문자열 역시 불변성( immutability ) 하다.***
>

<br />

> ***" 문자열은 유사 배열 객체이면서 이터러블 이므로 배열과 유사하게 각 문자에 접근할 수 있다,( 유사 배열 객체 ).  "***   
>

<br />

```javascript
const myString = function( str ) {
	let i = 0;
	while( str[i] ) {
		this[i] = str[i];
		i++;
	}
	this.length = i; 	
	this._value = str;
}

myString.prototype = {
constructor: myString,
	valueof() {
		return this._value;
	},
	toString() {
		return this.valueOf();
	},
	charAt( idx ) {
		if( isNaN( parseInt( idx ) ) ) return this[0];
		else return this[ parseInt( idx ) ];	
	}
};

let str = new myString("StarWars");
console.log( str ); /*
	 myString {
		'0': 'S',
		'1': 't',
		'2': 'a',
		'3': 'r',
		'4': 'W',
		'5': 'a',
		'6': 'r',
		'7': 's',
		length: 8,
		_value: 'StarWars',
  } -> 유사 배열 객체
*/
```

<br />

> ***" 이미 생성된 문자열의 일부를 변경해도 반영되지 않는다. 문자열은 변경 불가능한 값이기 때문이다. "***   
>

```javascript
const str = 'StarWars';
str[0] = 's';
console.log( str ) // "StarWars"  변경되지 않았다. 변경 불가능한 값이기 때문이다.
```

<br />

> 마치 C 언어에서 문자열 상수를 다루는것과 비슷하다.   
	이는 char* 로써 접근하는데, 데이터 영역에서 생성된 문자열 상수의 주소값을 가진다.   
	이때 해당 문자를 배열처럼 접근하여 변경이 불가능하며, 변경하려면 새롭게 값을 할당하여야 한다.   
	새롭게 할당한다는 건 새롭게 생성된 문자열의 주소값을 가진다는 이야기이다.   
	위의 문자열 처리가 문자열 상수를 다루는 것과 비슷한 것 같다.
>

<br />

### 값에 의한 전달

<br />

> ***" 변수는 복사값이다. "***   
	a 변수에 b 변수를 할당하면, a 는 b 의 값을 갖는다. 그럼 메모리 주소역시 같을까?   
	그렇지 않다. 이런 경우 새로운 매모리 주소에 b 의 메모리 주소값을 할당한후, a 는 새로운 메모리 주소를 가리킨다.   
>	 

```javascript
let a = 1;
let b = a;

console.log( b ); // 1
a = 10;
console.log( b ); // 1
console.log( a ); // 10
```

<br />

## 객체


<br />

> ***객체의 관리 방식***   
[Fast properties in V8]:(https://v8.dev/blog/fast-properties)    
[V8 히든 클래스 이야기]:(https://engineering.linecorp.com/ko/blog/v8-hidden-class)   
[자바스크립트 엔지의 최적화 기법(2)]:(https://meetup.toast.com/posts/78)   
[How the V8 engine works?]:(http://thibaultlaurens.github.io/javascript/2013/04/29/how-hte-v8-engine-works)   
[How Javascript works: inside the V8 engine + 5 tips on how to write optimized code]:(https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e)   
[Breking the Javascript Speed Limit with V8]:(https://www.youtube.com/watch?v=UJPdhx5zTaw)   
>

<br />

### 변경 가능한 값

<br />

> ***" 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값 ( mutalbe value ) 이다."***   
>

> 객체는 마치 포인터 참조와 비슷하다. 해당 포인터의 주소르 담고 있는 변수 포인터처럼 행동한다.   
	그 이유는 객체 자체의 크기가 커질 수 있으므로, 매번 해당 객체가 변경될때 마다 새로운 객체를 생성하는건 비효율적이 될수 있기 때문이다.   
	해당 객체는 새로운 프로퍼티를 생성하더라도 원시타입처럼 새로운 주소를 할당하지 않는다. 원래 주소에서 프로퍼티가 생성된다.   
	이는 예기치 않은 상황을 만들어 버리는데 만약 변수 두개가 같은 객체를 참조한다고 가졍해 보자.   
	이때 객체를 가진 변수들중 하나의 변수에서 객체의 프로퍼티를 수정하게 되면 다른 변수역시 변경된 객체를 가리킨다.   
	이는 변경 가능한 값( mutalble value ) 의 개념이다.   
	변수들이 참조하는 객체가 다른 변수에 위해 계속 변경된다면 예기치 못한 상황이 발생할 수 있다. 이는 충분히 조심해야 하는 방법이다.   
	그래서 객체를 복사하는 방법으로 불변값 ( imutable value ) 을 만든다. 이는 흔히 깊은 복사 / 얕은 복사를 통해 이루어진다.   
>

> ***깊은복사와 얕은복사***   
	얕은복사( shallow copy ) 는 중첩되어 있는 객체는 복사하지 않고 참조값으로 유지한다.   
	반면 깊은복사( deep copy ) 는 중첩되어 있는 객체 역시 복사한다.   
	이러한 복사 방법을 통해 객체의 불변성을 유지할 수 있다.
>

<br />

```javascript
let num = 0;
const prefix = 'x-wing';

const spaceShip = {
	rabelsAlliance:  {
		[`${prefix}_${num}`]: `T-${63 + num++}`,
		[`${prefix}_${num}`]: `T-${63 + num++}`,
		[`${prefix}_${num}`]: `T-${63 + num++}`,
	},
	GalacticEmpire: {
		planerKiller: `deathStar_I`	
	},
};

function shallowCopy( copyObj ) {
	const newObj = {};
	for( const name in copyObj ) {
		if( copyObj.hasOwnProperty( name ) ) {
			newObj[name] = copyObj[name]; 
		}
	}
	return newObj;
}

function deepCopy( copyObj ) {
	const newObj = {};
	for( const name in copyObj ) {
		if( copyObj.hasOwnProperty( name ) ) {
			if( typeof copyObj[name] === 'object' ) newObj[name] = deepCopy( copyObj[name] );
			else newObj[name] = copyObj[name];
		}
	}
	return newObj;
}

const spaceShip_shallow = shallowCopy( spaceShip ); 
const spaceShip_deep = deepCopy( spaceShip ); 

// shallowCopy
console.log( spaceShip == spaceShip_shallow ); // false
console.log( spaceShip.rabelsAlliance == spaceShip_shallow.rabelsAlliance ); // true

// deepCopy
console.log( spaceShip == spaceShip_deep ); // false
console.log( spaceShip.rabelsAlliance == spaceShip_deep.rabelsAlliance ); // false
```	

<br />

> 책에서는 ***" 원시 값을 항당한 변수를 다른 변수에 할당하는 것을 깊은 복사, 객체를 할당한 변수를 다른 변수에 할당하는 것을 얕은복사라고 부르는 경우도 있다. "*** 라고 이야기 한다.    
	이러한 경우에도 이러한 용어를 사용한다는 건 알아두는것이 좋을 듯 싶다. 하지만 별로 내키지는 않는다. 서로 이해하는 바가 달라질수 있기 때문이다. 
>

<br />

> ***" 객체를 가리키는 변수( 원본 ) 을 다른 변수( 사본 ) 에 할당하면 원본의 참조 값이 복사되어 전달된다. 이를 참조에 의한 전달이라 한다. "***
>

> 이 책에서 말하는 바는 명확하다. 변수는 메모리 주소를 가리킨다. 그러므로 ***" 참조에 의한 전달 "*** 이나 ***" 값에 의한 전달 "*** 이나 의미하는 바는 같다.   
	변수는 그냥 해당 메모리를 가진것 뿐이다. 그리고 원시값일 경우 새롭게 생성된 메모리 주소를 가지고, 참조값일 경우 원래 참조하던 메모리주소를 가리킬 뿐이다.   
	그리고 변수를 다른 변수에 할당하는 경우, 그냥 메모리 주소를 복사해서 넘기는것 뿐이다.   
	이 책에서는 이러한 용어에 대해 ***" 공유에 의한 전달 "*** 이라고도 말한다.   
>













