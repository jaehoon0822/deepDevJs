# CH33 7번째 데이터 타입 Symbol

<br />

>
***" Symbol 은 ES6 에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.  "***
>

<br />

## 심벌 값의 생성

<br />

### Symbol 함수

<br />

>
***" 생성된 심벌값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.  "***
>

<br />

```javascript

const sym = Symbol();
conole.log( typeof sym ); 'symbol'
console.lgo( sym ); Symbol()

```

<br />

```javascript

/* new 연산자와 호출하지 않는다. */
new Symbol(); // Uncaugth TypeError: Symbol is not a constructor at new Symbol (<anonymous>)

```

<br />

```javascript

const first_sym = Symbol('sym');
const second_sym = Symbol('sym');

first_sym === second_sym; // false
// 같은 이름을 사용한 심볼이라도 서로 다르다. 

```

<br />

```javascript

const sym = Symbol('sym');
sym.toString(); //'Symbol(sym)' 

// Symbol 도 접근시 래퍼객체를 생성한다.

```

<br />

```javascript

const numSym = Symbol(1);
let str = sym + '이다.'; // Uncaught TypeError: Cannot convert a Symbol value to a string 
let num = sym + 1; // Uncaught TypeError: Cannot convert a Symbol value to a number

```

<br />

```javascript



```
