## 9.1 타입 변환이란?

- 명시적 타입 변환(타입 캐스팅)
- 암묵적 타입 변환(타입 강제 변환)

기존 값을 직접 변경하는 것은 아니다. 암묵적 타입 변환도 새로운 타입의 값을 만들어 한 번 사용하고 버린다. 

## 9.2 암묵적 타입 변환

자바스크립트 엔진은 문맥을 고려해 암묵적으로 타입을 변활할 때가 있다. 

- 문자열 타입으로 변환

```jsx
1 + '2' // 12 문자열 연결 연산자의 모든 피연산자는 문자열 타입

`1 + 1` // "1 + 1" 템플릿 리터럴은 결과를 문자열로 변환

주의할 것
undefined + '' // 'undefined'
(Symbol()) + '' // Typeerror

객체타입
[] + '' // ''
{} + '' // '[object object]'
[10, 20] + '' // '10,20'
function() {} + '' // 'function() {}' 
```

- 숫자 타입으로 변환

산술연산자 또는 비교 연산자를 사용하면 피연산자가 모두 숫자 타입으로 암묵적 타입 변환이 된다. 이때 숫자 타입으로 변환할 수 없는 경우에는 연산을 수행할 수 없으므로 표현식의 평과 결과는 NaN이 된다. 

```jsx
1 - '1' // 0
'1' > 0 // true

+ 'string' // NaN
+ undefined // NaN
+ Symbol() // Typeerror
+ {} // NaN
+ [] // 0
+ [10, 20] // NaN
```

- 불리언 타입으로 변환

제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Trythy 값은 true로 Falsy 값은 false로 암묵적 타입 변환된다. Falsy 값에는 false, undefined, null, 0, -0, NaN, 빈문자열 등이 있다. 

```jsx
주의
{} [] 빈 배열, 빈 객체는 truthy 값이다. 
```

### 9.3 명시적 타입 변환

표준 빌트인 생성자 함수(String, Number, Boolean)을 New 연산자 없이 호출하는 방법, 빌트인 메서드를 사용하는 방법이 있다. 

- 문자열 타입으로 변환

```jsx
String(1) // 1 String 생성자 함수를 new 생성자 없이 호출
(1).toString() // 1 Object.prototype.toString 메서드 사용
1 + '' // 1 문자열 연결 연산자 사용
```

- 숫자 타입으로 변환

```jsx
Number('0') // 0 Number 생성자 함수를 new 생성자 없이 호출
parseInt('0') // 0 문자열만 변환 가능
+ '0' // 0 산술 연산자 사용

* 연산자 이용할 때 
true * 1 // 1
false * 1 // 0
```

- 불리언 타입으로 변환

```jsx
Boolean('x') // true Boolean 생성자 함수를 new 생성자 없이 호출
!!'x' // true 부정 논리 연산자를 두 번 사용하는 방법
```

### 9.4 단축 평가

단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다. 

- 논리 연산자를 사용한 단축 평가

|| && 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```jsx
'Cat' && 'Dog' // 'Dog'
'Cat' || 'Dog' // 'Cat'
```

단축 평가를 사용하면 if문을 대체할 수 있다. 삼항 연산자도 마찬가지다. 

```jsx
let done = true;
const message = '';

message = done && '완료';
console.log(message) // '완료'

done = false;
message = done || '미완료';
console.log(message) // '미완료'

message = done ? '완료' : '미완료';
console.log(message) // '미완료'
```

객체가 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefined 일 경우 타입 에러가 발생한다. 이 때 단축평가를 사용하면 에러를 발생시키지 않는다. 

```jsx
let elem = null;
let value = elem && elem.value; // null

```

함수를 호출할 때 인수를 전달하지 않으면 undefined가 할당된다. 이때 단축평가를 사용해 기본값을 설정하면 에러를 방지할 수 있다. 

```jsx
const getStringLength = (str) => {
	str = str || '';
  return str.length;
}

const getStringLength = (str = '') => {
	return str.length
}
```

옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어간다. 

```jsx
let elem = null;
let value = elem?.value; // undefined

let str = '';
let length = str?.length // 0
// && 연산자는 좌항 피연산자가 Falsy 값이면 좌항 피연산자를 그대로 반환한다. 
// 하지만 ?.는 좌항 피연산자가 Falsy 값이어도 null || undefined 아니면 우항의 프로퍼티 참조를 이어간다. 
```

null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. 변수에 기본값을 설정할 때 유용하다. 

```jsx
let foo = null ?? 'default string'; // 'default string'

foo = '' || 'default string'; // 'default string'
```

|| 논리 연산자는 좌항의 연산자가 Falsy 값이면 우항의 피연산자를 반환하는데, 만약 Falsy 값인 0이나 ‘’도 기본값으로 유효하다면 예기치 않은 동작이 발생할 수 있다. 하지만 ??는 좌항의 피연산자가 Falsy 값이어도 null 또는 undefined가 아니라면 좌항의 피연산자를 그대로 반환한다.
