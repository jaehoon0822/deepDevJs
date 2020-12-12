# CH32 String

<br />

## String 메서드

<br />

>
***" String 객체에는 원본 String 래퍼 객체를 직접 변겅하는 메서드는 존재하지 않는다. 즉, String 객체의 메서드는 언제나 새로운 문자열을 반환한다. 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.  "***
>

<br />

### String.prototype.indexOf

<br />

```javascript

const str = 'StarWars';

str.indexOf('s'); //7
str.indexOf('ar'); //2
str.indexOf('p'); //-1

```

<br />

### String.prototype.search

<br />

```javascript

/*
 정규표현식을 인자로 받아 문자 인덱스를 반환한다.
 */

const str = 'StarWars';

str.search(/r/); // 3
str.search(/p/); // -1

```

<br />

### String.prototype.includes (ES6)

<br />

```javascript

/*
	문자열 포함되었는지 확인 
 */

const str = 'StarWars';

str.includes( 'S' ); // true
str.includes( 'ars' ); // true

str.includes( 'r', 2 ); // true

```

<br />

### String.prototype.startsWith (ES6)

<br />

```javascript

/*
 	문자열로 시작하는지 확인
 */

const str = 'StarWars';

str.startsWith( 'S' ); // true
str.startsWith( 'a' ); // false
str.startsWith( 'Sta' ); // true

```

<br />

### String.prototyep.endsWith (ES6)

<br />

```javascript

const str = 'StarWars';

str.endsWith( 'ars' ); // true
str.endsWith( 'W' ); // false
str.endsWith( 'rWars' ); // true

```

<br />

### String.prototype.charAt

<br />

```javascript

const str = 'StarWars';

str.charAt( 0 ); // 'S'
str.charAt( 1 ); // 't'
str.charAt( 2 ); // 'a'
str.charAt( 3 ); // 'r'

```

<br />

### String.prototype.substring

<br />

```javascript

const str = 'StarWars';

str.substring( 0, 4 ); // 'Star'
str.substring( 4 ); // 'Star'
str.substring( -4 ); // 'StarWars'
str.substring( -1 ); // 'StarWars'
str.substring( 0, str.indexOf( 'W' ) ); // 'Star'
str.substring( 4, str.length ); // 'Wars'
str.substring( str.indexOf( 'W' ), str.length ); // 'Wars'

```

<br />

### String.prototype.slice

<br />

```javascript

const str = 'StarWars';

/* substring 과 같다. */

str.slice( 0, 4 ); // 'Star'
str.slice( 4, 8 ); // 'Wars'
str.slice( str.indexOf('W'), str.length ); // 'Wars'

/* 다른점은 음수를 인자로 받으면, 마지막 값부터 검색한다는 것이다. */
str.slice( -4 ); // 'Wars'

```

<br />

### String.prototype.toUpperCase

<br />

```javascript

const str = 'StarWars';

str.toUpperCase(); // 'STARWARS'

```

<br />

### String.prototype.toLowerCase

<br />

```javascript

const str = 'StarWars';

str.toLowerCase(); // 'starwars'

```

<br />

### String.prototype.trim

<br />

```javascript

let str = '  StarWars';
str.trim(); // 'StarWars'

str = '   StarWars   ';
str.trimStart(); // 'StarWars   '
str.trimEnd(); // '   StarWars'

```

<br />

### String.prototype.repeat (ES6)

<br />

```javascript

const str = 'StarWars';

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'StarWars'
str.repeat(2); // 'StarWarsStarWars'
str.repeat(3); // 'StarWarsStarWarsStarWars'
str.repeat(-1); // uncaught RangeError: Invalid count value at String.repeat (<anonymous>)

```

<br />

### String.prototype.replace

<br />

```javascript

const str = 'StarWars';

reStr = str.replace( 'Star', 'World' ); // WorldWars
reStr.str.replace( /s/, '' ); // WorldWar

str.replace('Star', '<strong>$&</strong>' );
// <strong>Star</strong>wars

```

<br />

```javascript

// 책에서 재미있는 예제가 소개 되었다.

const camelToSnake = ( camelCase ) =>  {
	return camelCase.replace( /.[A-Z]/g, match => `${match[0]}_${match[1].toLowerCase()}` );
};

console.log( camelToSnake( 'starWars' ) ); // 'star_wars'

const snakeToCamel = ( snakeCase ) => {
	return snakeCase.replace( /_[a-z]/g, match => `match[1].toUpperCase()` );
};

console.log( snakeToCamel( 'star_wars' ) ); // 'starWars'

```

<br />

### String.prototypea.split

<br />

```javascript

const str = 'Im your father.';

str.split(); [ 'Im your father.' ];
str.split(''); 
[
	'I', 'm', ' ', 'y',
	'o', 'u', 'r', ' ',
	'f', 'a', 't', 'h',
	'e', 'r', '.'
]

str.split(' ');
[
	'Im', 'your', 'father.'
]

str.split(/\s/);;
[
	'Im', 'your', 'father.'
]

str.split( ' ', 2 );
[
	'Im',
	'your'
]

// 이 책에서 문자열을 거꾸로 하는 로직을 구현했다.

const reverseStr = ( str ) => str.split('').reverse().join('');
console.log( reverseStr( 'startwars' ) ); // 'srawrats'

```

<br />


