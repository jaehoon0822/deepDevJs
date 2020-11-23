# CH16 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

<br />

> ***" 내부 슬롯과 내부 메소드는 자바스크립트 엔진이 구현 알고리즘을 설명하게 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티( pseudo property ) 와 의사 메서드( pseudo method ) 이다. "***   
***" ECMAScript 사양에 등장하는 이중대괄호로 감싼 이름들이 내부 슬롯과 내부 메서드이다. "***
>

<br />

> ***" 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 원칙적으로는 접근할 수 없지만 [[Prototype]] 내부 슬롯의 경우, __proto__ 를 통해 간접적으로 접근할 수 있다. *"***
>

<br />

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

<br />

> ***" 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태값( meta-property ) 인 내부슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 이다. "***
>

<br />

> 이 프로퍼티 어트리뷰트는 Object.getOwnPropertyDescriptor 를 통해 확인 가능하다.
>

| 메서드 | 인자값 | 
| :---: | :---: |
| Object.getOwnPropertyDescriptor | ( ObjectName, propertyName ) |
| Object.getOwnPropertyDescriptors | ( ObjectName ) |

<br />

```javascript

const spaceShip = {
	"x-wing": "T-64",
};

Object.getOwnPropertyDescriptor( spaceShip, "x-wing" );
// { value: 'T-64", writable: true, enumerable: true, configurable: true } -->  PropertyDescriptorObject  

```

> ***" ES8 에서 제공하는 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립트 객체를 반환한다. "***
>
<br />

## 데이터 프로퍼티와 접근자 프로퍼티( accessor property )

<br />

| 프로퍼티 종류 | 설명 |
| :---: | :---: |
| 데이터 프로퍼티 | 키과 값으로 구성된 프로퍼티 |
| 접근자 프로퍼티 | 값을 읽거나 저장할때 사용하는 접근자 함수로 구성된 프로퍼티 |

<br />

| property descriptor 의 프로퍼티 | 설명 |
| :---: | :---: |
| value | * 프로퍼티 값    
					* value에 값을 할당하면, 해당 프로퍼티에 값이 할당된다. 프로퍼티가 존재하지 않으면 생성되어 값이 할당된다.
| writable | * 프로퍼티를 읽기 전용으로 만든다.
						 * 값은 boolean 값으로 설정한다.   
						 	 true = 읽기 / 쓰기   
							 false = 읽기 |
| enumerable | * 프로퍼티가 열거가능하게 만든다.   
							 * 값은 boolean 값으로 설정한다.   
							   true = 열거 가능
								 false = 열거 불가능 |
| configurable | * 프로퍼티 재정의 가능여부   
								 * 값은 boolean 값으로 설정한다.
								  true = 프로퍼티의 삭제 및 프로퍼티 어트리뷰트 변경 가능
									false = 프로퍼티의 삭제 및 프로퍼티 어트리뷰트 변경 불가능
								 * 추가적으로, writable 이 true면, writable 설정이 가능하며, 프로퍼티 읽기 쓰기가 가능하다. |

<br />

> 나머지는 헷갈리지 않는데, configurable 의 추가적인 부분 "writable 이 true 이면 value 쓰기 및 writable 설정이 가능하다." 가
조금 헷갈리기는 하다. 뭐.. 말그대로 이해하면 되기는 하지만, 이렇게 한 이유가 있을것이다.
>

<br />

| accessor property | 설명 |
| :---: | :---: |
| get | accessor property 를 통해 값을 읽을때 호출하는 getter 함수 |
| set | accessor property 를 통해 값을 설정할떼 호출하는 setter 함수 |
| enumerable | 데이터 프로퍼티의 enumerable 동일 |
| configurable | 데이터 프로퍼티의 configurable 동일 |

<br />

나머지는 p224 를 살펴보자.

<br />

### 프로퍼티 정의

<br />

> Object.defineProperty 메서드는 프로퍼티의 어트리뷰트를 정의할 수 있다.   
   
주의해야 할 점은 해당 어트리뷰트를 생략하면 value, set, get 은 undefined, 나머지는 false 값이 되어 버린다는 것이다. 이를 주의해서 작성해야 한다.
>

<br />

> Object.defineProperties 는 여러개의 프로퍼티를 한번에 정의한다.
>

<br />

## 객체 변경 방지

<br />

> 자바스크립트는 객체의 변경을 방지하는 메서드들을 제공한다.
>

| 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| :---: | :---: | :---: | :---: | :---: | :--: | 
| Object.preventExtensions | x | o | o | o | o |
| Object.seal | x | x | o | o | x |
| Object.freeze | x | x | o | x | o |

| 메서드 | 설명 |
| :---: | :---: |
| Object.isExtensible | 확장 가능한 객체인지 확인하는 메서드 |
| Object.isSealed | 밀봉된 객체인지 확인하는 메서드 |
| Object.isFrozen | 동결된 객체인지 확인하는 메서드 |

<br />

> 이에 대해서 자세한건 229p ~ 231p 를 살펴보자.
>

<br />

## 불변 객체

> ***" 지금까지 살펴본 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다. "***
>

> 중첩 객체까지 변경 방지 하고 싶다면, 중첩 객체까지 메서드를 적용하는 함수를 만들면 된다. 이건 너무나 당연한 이야기다.
>

