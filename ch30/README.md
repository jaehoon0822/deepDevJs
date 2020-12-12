# CH30 DATE

<br />

>
***" Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다. "***
>

<br />

>
***" 이값은 1970 년 1월 1일 00:00:00(UTC) 을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다. "***
>

<br />

### new Date()

<br />

>
***" new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다. "***
>

```javascript

new Date();
// 2020-12-10T16:25:06.941Z

Date();
// "Fri Dex 11 2020 01:27:10 GMT+0900 (Korean Standard Time)"

```

<br />

>
***" new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.  "***
>

<br />

### new Date(milliseconds)

<br />

>
***" 인수로 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC) 을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.  "***
>

<br />

```javascript

new Date( 24 * 60 * 60 * 1000 );
// 1970-01-02T00:00:00.000Z


```

<br />

### Date.UTC

<br />

>
***" UTC 를기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 변환한다. "***
>

<br />

```javascript

/*
Date.UTC( year, month[, day, hour, minute, second, milliscond ]); 
 */

Date.UTC( 1970, 0, 1 ); // 0
Date.UTC( 1970, 0, 1, 1 ); // 3600000
// -> 60 * 60 * 1000 == 3600000
Date.UTC( 1970, 0, 1, 1, 1 ); // 3660000
// -> 60*60*1000+60*1000 === 3660000

```

<br />

### new Date( dateString )

<br />

```javascript

new Date( 'Dec 10, 2020 02:00:00 );
// 2020-12-09T17:00:00.000Z
// 내 컴퓨터에서는 node js 설정이 필요한지 다른 시간을 나타낸다. 

new Date( '2020/12/09/02:00:00' );
// 2020-12-09T17:00:00.000Z

```

<br />

### new Date( year, month[, day, hour, minute, second, milisecond ])

<br />

| 인수 | 내용 |
| :---: | :---: |
| year | 1900년 이후의 정수. 0 ~ 99 는 1900 ~ 1999 이다.|
| month | 월을 나타내는 0 ~ 11 까지의 정수 ( 0 은 1 월이다.)|
| day | 일을 나타내는 1~31 까지의 정수 |
| hour | 시를 나타내는 0 ~ 23 까지의 정수 |
| minute | 분을 나타내는 0 ~ 59 까지의 정수 |
| second | 초를 나타내는 0 ~ 59 까지의 정수 |
| milisecond | 밀리초를 나타내는 0 ~ 999 까지의 정수 |

<br />

## Date 메서드 

<br />

### Date.now

<br />

>
***" 1970/01/01/00:00:00(UTC) 를 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환 "***
>

<br />

```javascript

Date.now();
// 1607620649968

```

<br />

### Date.parse

<br />

>
***" 1970/01/01/00:00:00(UTC) 를 기점으로 인수로 전달된 지정 시간( new Date( dateString ) 의 인수와 동일한 형식 ) 까지의 밀리초를 숫자로 반환한다. "***
>

<br />

### Date.prototype.getFullYear

<br />

```javascript

new Date( '2020/12/11' ).getFullYear(); // 2020

```

<br />

### Date.prototype.setFullYear 

<br />

```javascript

const today = new Date();

today.setFullYear(2020);
// 1607621342586

today.getFullYear(); // 2020

// year/month/day 
today.setFullYear(2020, 11, 12);
today.getFullYear(); //2020

```

<br />

### Date.prototype.getMonth

<br />

>
***" Date 객체의 월을 나타내는 0 ~ 11 정수를설정한다. "***
>

<br />

```javascript

new Date( '2020/11/12' ).getMonth(); // 11

/*
	 0 = 1월
	 11 = 12월
*/

```

<br />

### Date.prototype.setMonth

<br />

```javascript

const today = new Date();

today.setMonth(0);
today.getMonth(); // 0

// month/day
today.setMonth(11,11);
today.getMonth(); // Fri Dec 11 2020 02:56:28 GMT+0900 (KST)

```

<br />

### Date.prototype.getDate

<br />

```javascript

new Date('2020/12/11').getDate(); // 11

```

<br />

### Date.prototype.setDate 

<br />

```javascript

const today = new Date();

today.setDate(11);
today.getDate();// 11

```
<br />

### Date.prototype.getDay

<br />

| 요일 | 값 |
| :---: | :---: |
| 일 | 0 |
| 월 | 1 |
| 화 | 2 |
| 수 | 3 |
| 목 | 4 |
| 금 | 5 |
| 토 | 6 |

<br />

### Date.prototype.getHours

<br />

```javascript

new Date( '2020/12/11/03:08' ).getHours(); //3

```

<br />

### Date.prototype.setHours

<br />

```javascript

const today = new Date();

today.setHours( '3' );
today.getHours(); // 3

```

<br />

### Date.prototype.getMinutes

<br />

```javascript

new Date('2020/12/11/03:10').getMinutes(); // 10

```

<br />

### Date.prototyep.setMinutes

<br />

```javascript

const today = new Date();

today.setMinutes(50);
today.getMinutes(); // 50

// HH/minute/second/millisecond
today.setMinutes( 50, 20, 800 ); // HH:50:20:800

```

<br />

### Date.prototype.getSeconds

<br />

```javascript

new Date('2020/12/11/03:10:40').getSeconds(); // 40

```

<br />

### Date.prototyep.setSeconds

<br />

```javascript

const today = new Date();

today.setSeconds(50);
today.getSeconds(); // 50

// HH/MM/second/millisecond
today.setMinutes( 20, 800 ); // HH:MM:20:800

```

<br />

### Date.prototype.getMilliseconds

0~999 까지의 Milliseconds 를 반환한다.

<br />

```javascript

new Date( '2020/12/11/03:15:40:800' ).getMilliseconds(); //800

```

<br />

### Date.prototype.setMilliseconds

0~999 까지의 Milliseconds 를 인수로 받는다.

<br />

```javascript

const today = new Date();

today.setMilliseconds(500);
today.getSeconds(); // 500

```

<br />

### Date.prototype.getTime

<br />

>
***" 1970/01/01/00:00:00(UTC) 를 기점으로 Date 객체의 시간까지 경과한 밀리초를 반환."***
>

<br />

```javascript

new Date( '2020/12/11/03:31:20:800' ).getTime();
// 1607625080800


```

<br />

### Date.prototype.setTime

<br />

```javascript

const today = new Date();

today.setTime( 24 * 60 * 60 * 1000 );
// 86400000
// 1970-01-02T00:00:00.000Z

```

<br />

### Date.prototype.getTimezoneOffset 

<br />

>
***" UTC 와 Locale 시간과의 차이를 분 단위로 반환한다. "***
>

<br />

```javascript

const today = new Date();

let min = today.getTimezoneOffset(); // -540
let hour = min / 60;
console.log( hour ); // -9 

```

<br />

### Date.prototype.toDateString

<br />

```javascript

const today = new Date( '2020/12/11/03:42' );

today.toString(); // 'Fri Dec 11 2020 03:42:00 GMT+0900 (Korean Standard Time)
today.toDateString(); // 'Fri Dec 11 2020'

```

<br />

### Date.prototype.toTimeString

<br />

```javascript

const today = new Date( '2020/12/11/03:42' );

today.toString(); // 'Fri Dec 11 2020 03:42:00 GMT+0900 (Korean Standard Time)
today.toTimeString(); // '03:42:00 GMT+0900 (Korean Standard Time)'

```

<br />

### Date.prototype.toISOString

<br />

```javascript

const today = new Date( '2020/12/11/03:42' );

today.toString(); // ''Fri Dec 11 2020 03:42:00 GMT+0900 (Korean Standard Time)'
today.toISOString(); // '2020-12-10T18:42:00.000Z'
// 날짜가 달라진다. ISO 관련 정보를 봐야겠다...
// 뭔 이놈의 시간을 여러개로 만들어놓았는지 짜증난다...

```
<br />

### Date.prototype.toLocaleString

<br />

>
***" 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용한다. "***
>

<br />

```javascript

const today = new Date( '2020/12/11/03:42' );

today.toLocaleString(); // 2020. 12. 11 오전 3:42:00
today.toLocaleString('ko-KR'); // 2020. 12. 11 오전 3:42:00
today.toLocaleString('en-US'); // 12/11/2020, 3:42:00 AM

```

<br />

### Date.prototype.toLocaleTimeString

<br />

```javascript

const today = new Date( '2020/12/11/03:42' );

today.toLocaleTimeString(); // 오전 3:42:00
today.toLocaleTimeString('ko-KR'); // 오전 3:42:00
today.toLocaleTimeString('en-US'); // 3:42:00 AM

```

<br />

시계 만드는 예제가 있는데 이는 p577 을 보자.
