# CH10 객체 리터럴

<br />

> ***Instance***   
	***클래스에 의해 생성되어 메모리에 저장된 실체***
>

> ***ES6 의 계산된 프로퍼티 이름***
>
```javascript
let key = 'key';

let obj = {
	[key]: 'objKey',
};

console.log( obj[key] );
```

> ***키의 이름은 빈문자열도 허용된다.***
>

```javascript
let obj = {
	'': 'objKey',
}

console.log( obj[''])
```

> ***숫자 리터럴은 문자열로 변환된다.***
>

```javascript
let obj = {
	0: 'zero',
	1: 'one',
	2: 'two',
};

console.log( obj[0] ); // 숫자 리터럴은 따옴표가 생략해도 된다. 하지만 편의를 위한 것이지 문자열이다.
console.log( obj['0'] ); // 문자열로 변환되어 [] 연산자로 접근할수 있다.
```

> ***키가 중복되면, 가장 나중에 선언된 프로퍼티를 덮어쓴다.***
>

```javascript
let obj = {
	redundantName: 'Anakin',
	redundantName: 'Luke',
};

console.log( obj.redundantName ) // Luke
console.log( obj ) { 'redundantName': 'Luke' }
```

> ***. 연산자로 프로퍼티를 불러올때 브라우저 및 NODE 의 작동방식***   
   
	이 채의 예시에서는 다음과 같은 경우를 말해준다.   
```javascript
obj.last-name; // 이때 NODE 와 브라우저의 동작방식은?
```
<br \>	

	일단 둘다 동작방식은 같다. 하지만 네이티브 객체에 따라 해석하는 방식이 달라짐을 이야기한다.   
	NODE 와 브라우저 둘다 처음 - 이전의 name property 를 obj 객체에서 찾아본다.   
	하지만 그런 property는 없으므로. undefined 로 변환된다.   

***- 브라우저의 경우-***   
	이것 다음으로 부터 동작방식이 달라지는데, 브라우저는 네이티브 속성인 name 이 존재한다.   
	브라우저는 undefined 이후로 name 속성의 값을 찾는다. 브라우저의 name 속성은 윈도우 창의 이름을 나타낸다.   
	이름이 없으므로 ''(빈문자열) 을 반환하고, undefined 와 '' 을 -( 빼기 ) 연산 한다.   
	그리고 결과값은 NaN( 숫자로 표현할 수 없는 값 ) 을 반환한다.   

***- NODE 의 경우***   
	NODE 는 undefined 이후로 name 이라는 속성을 찾는다.   
	name 식별자는 NODE 에 존재하지 않으므로, ERROR 난다.   
	ReferenceError: name is not defined 으로, 종료된다.
	만약, 다음과 같이 name 이 존재한다면,      
```javascript
	let name = 'Luke';
	obj.last-name;
````
	결과는 위와 같이 NaN 이다.

	name 값이 'Luke' 이므로, undefiend - 'Luke' 의 빼기연산을 하기 때문이다.
>

<br />

## ES6 에서 추가된 객체 리터럴의 확장 기능

<br />

### 프로퍼티의 축약 표현

<br />

```javascript
let first = 'Luke';
let last = 'Skywalker';

const hero = { first, last }; // { first: 'Luke', last: 'Skywalker' }
```

<br />

### 계산된 프로퍼티 이름

<br />

```javascript
let prefix = 'X_wing';
let num = 0;

const spaceShip = {
	[`${prefix}_${num}`]: `T-${63 + num++}`,
	[`${prefix}_${num}`]: `T-${63 + num++}`,
	[`${prefix}_${num}`]: `T-${63 + num++}`,
}	

console.log( `인컴 ${spaceShip.X_wing_2} 은 Death_Star 를 파괴한 전투기다.` ); // 인컴 T-65 는 Death_Star 를 파괴한 전투기다.
```

<br />

### 메서드 축약 표현

<br />

```javascript
let prefix = 'X_wing';
let num = 0;
const spaceShip = {
	[`${prefix}_${num}`]: `T-${63 + num++}`,
	[`${prefix}_${num}`]: `T-${63 + num++}`,
	[`${prefix}_${num}`]: `T-${63 + num++}`,
	iskilledDeathStar() {
		let i = 0;
		while( i < num ) {
			if( this[`X_wing_${i}`] == `T-65` ) break;
			i++;
		}
		console.log( this[`X_wing_${i}`] );
	}
}

spaceShip.iskilledDeathStar(); // T-65
```
<br />






