
# React 주요 개념 - JSX 소개

## **React는  JavaScript 라이브러리이다.**

```JSX
const element = <h1>Hello, world!</h1>;
```

위의 문자열도, HTML도 아닌 희한한 문법은 React의 JSX 문법이다.

JSX는 React "엘리먼트(element)" 를 생성한다.

---

## JSX란?

React에서는 본질적으로 렌더링 로직이 UI 로직인

- 이벤트가 처리되는 방식
- 시간에 따라 state가 변하는 방식
- 화면에 표시하기 위해 데이터가 준비되는 방식

등과 연결된다는 사실을 받아들여야 한다.

React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하는 대신,
둘 다 포함하는 **컴포넌트** 라고 부르는 느슨하게 연결된 유닛으로 *관심사* 를 분리한다.

그렇다고 React에서 JSX 사용이 필수는 아니다.

---

## JSX에 표현식 포함하기

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

JSX의 중괄호 안에는 유효한 모든 JavaScript 표현식을 넣을 수 있다.
예를 들어 2 + 2, user.firstName, formatName(user) 등은 모두 유효한 JavaScript 표현식이다.

```jsx
function formatName(user) {
return user.firstName + ' ' + user.lastName;
}


const user = {
firstName: 'Harper',
lastName: 'Perez'
};

const element = (
<h1>
Hello, {formatName(user)}!
</h1>
);
```

---

## JSX도 표현식이다.

컴파일이 끝나면, JSX 표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식된다.

즉, JSX를 if 구문 및  for loop 안에 사용하고, 변수에 할당하고, 인자로서 받아들이고,
함수로부터 반환할 수 있다.

---

## JSX 속성 정의

어트리뷰트에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.

```jsx
const element = <a href="https://www.reactjs.org"> link </a>;
```

중괄호를 사용하여 어트리뷰트에 JavaScript 표현식을 삽입할 수도 있다.

```jsx
const imageElement = <img src={user.avatarUrl}></img>;
```

**어트리뷰트에 JavaScript 표현식을 삽입할 때 중괄호 주변에 따옴표를 입력하지 마세요.**
**따옴표(문자열 값에 사용) 또는 중괄호(표현식에 사용) 중 하나만 사용하고,
동일한 어트리뷰트에 두 가지를 동시에 사용하면 안되요**

---

## JSX로 자식 정의

태그가 비어있다면 XML처럼 /> 를 이용해 바로 닫아주어야 합니다.

```jsx
const imageElement = <img src={user.avatarUrl}></img>;
```

JSX 태그는 자식을 포함할 수 있다.

```jsx
const children = (
<div>
	<h1>Hello!</h1>
	<h2>Good to see you here.</h2>
</div>
);
```

---

## JSX는 주입 공격을 방지합니다.

JSX에 사용자 입력을 삽입하는 것은 안전하다.

```jsx
const title = response.potentiallyMaliciousInput;
// 이것은 안전하다.
const element = <h1>{title}</h1>;
```

기본적으로 React DOM은 JSX에 삽입된 모든 값을 렌더링하기 전에 이스케이프 하므로,
애플리케이션에서 명시적으로 작성되지 않은 내용은 주입되지 않는다.

모든 항목은 렌더링 되기 전에 문자열로 변환된다.
이런 특성으로 인해 XSS (cross-site-scripting) 공격을 방지할 수 있다.

---

## JSX는 객체를 표현합니다.

Babel은 JSX를 React.createElement() 호출로 컴파일한다.

```jsx
const element = (
	<h1 className="greeting">
		Hello, world!
	</h1>
);

const element = React.createElement(
	'h1',
	{className: 'greeting'},
	'Hello, World!'
);
```

이 두 예시는 동일한 것을 출력해준다.
**React.createElement()** 는 버그가 없는 코드를 작성하는데 도움이 되도록 몇 가지 검사를 수행하며,
기본적으로 다음과 같은 객체를 생성한다.

```jsx
const element = {
	type: 'h1', // type 은 <h1></h1>
	props: { // 해당 element의 정보 class는 greeting, 기본 정보는 Hello, World!
		className: 'greeting',
		children: 'Hello, World!'
	}
};
```
