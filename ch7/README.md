# CH7 연산자

> ***" 연산자는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산등을 수행해 하나의 값을 만든다 "***   
>

```javascript
	let arr = [ 1, 2, 3, 4 ]; // 할당
	
	function map( arr, func ) {
		let newArr = [];
		for( let i = 0; i < arr.length; i++ ) { // 비교 -> i < arr.length
			newArr.push( func( arr[i], i, arr) );
		}
		return newArr;
	}

	let newArr = map( arr, ( val, i, arr ) => { 
			if ( typeof arr === 'object' && Object.prototype.toString.call( arr ) == '[object Array]' ) { // 타입 연산 -> typeof arr
				if ( i === 1 && val === 2  ) return val * 2; 
				else return val;
			}
		}
	); // 논리 연산 -> i === 1 && val === 2  
	console.log( newArr ); // [1, 4, 3, 4]

	console.log( `${2 ** 8} 은 1byte 입니다.` ); // 256 은 1byte 입니다. --> 2 ** 8 ( 지수 연산자 )
```

## 산술 연산자
   
> ***" 산술 연산이 불가능한 경우, NaN 을 반환한다. "***
 산술 연산자는 피연산자의 개수에 따라 이항 산술 연산자( binary arithmetic operator )와 단항 산술 연산자( unary arithmetic operator )로 나누어 진다.
>   

## 문자열 연결 연산자
   
> + 연산자는 2가지 방식이 있다. 숫자값을 더하기하거나, 문자열을 연결하는 방식이다.
	피연산자가 string 일 경우 + 연산자를 사용하면 연산자끼리 문자열로 연결한다. 
>   

| 연결 연산자 | 평가값 |
| :---: | :---: |
| '10' + 5 | '105' |
| 10 + '5' | '105' |
| 10 + 5 | 15 |
| 10 + true | 11 |
| 10 + false | 10 |
| 10 + null | 10 |
| +undefined | NaN |
| 10 + undefined | NaN |
   
## 할당 연산자 ( assignment operator )
   
> 할당 연산자는 rvalue 의 값을 평가 연산후 lvlaue 에 할당하는 단축 표현식이다. 
>   

## 비교연산자

> lvalue 와 rvalue 를 비교한다.
>

... 나머지 아는 것들은 생략한다.

## 그 외의 연산자

| 연산자 | 개요 |
| :---: | :---: |
| ?. | 옵셔널 체인 연산자 |
| ?? | null 병합 연산자 |
| delete | 프로퍼티 삭제 연산자 |
| new | 생성자 함수에서 인스턴스 생성 연산자 |
| instanceof | lvalue 가 rvalue 의 instance 인지 확인하는 연산자 |
| in | 프로퍼티 존재를 확인하는 연산자 |
   
## 연산자 우선순위

| 우선순위  | 연산자 |
| :---: | :---: |
| 1 | () |
| 2 | new(), [], func(), ?. |
| 3 | new() |
| 4 | x++, x-- |
| 5 | !x, +x, -x, ++x, typeof, delete |
| 6 | \*\* |
| 7 | \*, /, % |
| 8 | +, - |
| 9 | <, <=, >, >=, in, instanceof |
| 10 | ==, !=, ===, !== |
| 11 | ??() |
| 12 | && |
| 13 | \|\| |
| 14 | ? aaa : bbb |
| 15 | =, +=, -=, \=, \*=, %= |
| 16 | , |

## 연산자 결합 순서

| 결합순서 | 연산자 |
| :---: | :---: |
| lvalue -> rvlaue | + - / % < <= > >= && \|\| . \[\] \(\) ?? ?. in instanceof | 
| rvalue -> lvlaue | ++ -- = += -= \*= %= /= !x +x -x ++x --x typeof delete ?aaa:bbb | 





