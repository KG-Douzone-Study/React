# Components 와 Props

컴포넌트를 통해서 UI를 재사용이 가능한 개별적인 여러 조각으로 나눌 수 있다.

개념적으로 컴포넌트는 JavaScript 함수와 유사하다.
"props"라고 하는 임의의 입력을 받은 후,
화면에 어떻게 표시되는지 기술하는 React 엘리먼트를 반환한다.

---

## 함수 컴포넌트와 클래스 컴포넌트

컴포넌트를 정의하는 가장 간단한 방법은 JavaScript 함수를 작성하는 것이다.

```jsx
// 함수 컴포넌트
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}
```

이 함수는 데이터를 가진 하나의 "props" 객체 인자를 받은 후 
React 엘리먼트를 반환하므로 유효한 React 컴포넌트이다.
- props 는 속성을 나타내는 데이터이다. (속성은 없거나 1개 이상일 수 있다.)

또, ES6 class 를 사용하여 컴포넌트를 정의할 수 있다.

```jsx
// 클래스 컴포넌트
class Welcome extends React.Component {
	render() {
		return <h1>Hello, {this.props.name}</h1>
	}
}
```

React 관점에서 볼 때 위 두 가지 유형의 컴포넌트는 동일하다.

**함수 컴포넌트와 클래스 컴포넌트 둘 다  몇 가지 추가 기능이 있다.**
코드의 간결성에서는 함수형 컴포넌트가 좋다.

---

## 컴포넌트 렌더링

이전까지는 DOM 태그만을 사용해 React 엘리먼트를 나타냈다.

```jsx
// DOM 태그 <div/>
const element = <div />;
```

React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있다.

```jsx
const element = <Welcome name="Sara" />;
```

React 가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트와 자식을 해당 컴포넌트에
단일 객체로 전달한다.
이 객체를 "props" 라고 한다.

**페이지에서 "Hello, Sara" 를 렌더링하는 예시를 보자.**

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```

이 예시에서는 다음과 같은 일들이 일어난다.

```txt
1. <Welcome name="Sara" /> 엘리먼트로 root.render() 를 호출한다.
2. React는 {name : 'Sara'} 를 props로 하여 Welcome 컴포넌트를 호출한다.
3. Welcome 컴포넌트를 결과적으로 <h1>Hello, Sara</h1> 엘리먼트를 반환하다.
4. React DOM은 <h1>Hello, Sara</h1> 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트한다.
```

**1. 컴포넌트의 이름은 항상 대문자로 시작한다.**

예를 들어 DOM 태그 < div > 같은 HTMl div 태그는 첫글자가 소문자로 나타내지만,
함수 컴포넌트인 Welcome 은 사용시 컴포넌트임을 나타내기 위해 < Welcome > 대문자로 시작한다.

즉, Element가 소문자로 시작하는 경우에는 < div > 나 < span > 같은 내장 컴포넌트라는 것을 뜻하며
'div' 'span' 같은 문자열 형태로 React.createElement 에 전달된다.

< Welcome /> 과 같이 대문자로 시작하는 타입들은 React.createElement(Welcome) 의 형태로
컴파일되며 JavaScript 파일 내에 사용자가 정의했거나 import 한 컴포넌트를 가리킨다.

**2. 실행 중에 타입 선택하기**

React element 타입에 일반적인 표현식은 사용할 수 없다.
element 타입을 지정할 때 일반적인 표현식을 사용하고자 한다면 대문자로 시작하는 변수에
배정한 후 사용할 수 있다.

예를 들어 아래와 같이 prop 에 따라 다른 컴포넌트를 render 해야 하는 경우들이 종종 있다.

```jsx
import React from 'react';
import {PhotoStory, VideoStory} from './stories';

const components = {
	photo: PhotoStory,
	video: VideoStory
};

function Story(props) {
	// 잘못도니 사용법이다! JSX 타입은 표현식으로 사용될 수 없다.
	return <components[props.storyType] story={props.story} />;
}
```

이를 고치기 위해서는 우선 타입을 대문자로 시작하는 변수에 저장한다.

```jsx
import React from 'react';
import {PhotoStory, VideoStory} from './stories';

const components = {
	photo: PhotoStory,
	video: VideoStory
};

function Story(props) {
	// 잘못도니 사용법이다! JSX 타입은 표현식으로 사용될 수 없다.
	const SpecificStory = components[props.storyType];
	return <SpecificStory story={props.story} />;
}
```

추가적인 JSX 이해하기 정보는 이곳에서 알아보자.

---

## 컴포넌트 합성

컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있다.
이는 모든 세부 단계에서 동일한 추상 컴포넌트를 사용할 수 있음을 의미한다.
React 앱에서는 버튼, 폼, 다이얼로그, 화면 등의 모든 것들이 흔히 컴포넌트로 표현된다.

**예를 들어 Welcome을 여러 번 렌더링하는 App 컴포넌트를 만들 수 있다.**

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name} </h1>;
}

function App() {
	return (
		<div>
			<Welcome name="Sara" />
			<Welcome name="Cahal" />
			<Welcome name="Edite" />
		</div>
	);
}
```

일반적으로 새 React 앱은 최상위에 단일 App 컴포넌트를 가지고 있다.

---

## 컴포넌트 추출

컴포넌트를 여러 개의 작은 컴포넌트로 나누는 것을 두려워하지 말자.

다음 Comment 컴포넌트를 살펴보자.

```jsx
function Comment(props) {
	return (
		<div className="Comment">
			<div className="UserInfo">
				<img className="Avator"
					src = {props.author.avatorUrl}
					alt = {props.author.name}
				/>
				<div className="UserInfo-name">
					{props.author.name}
				</div>
			</div>
			<div className="Comment-text">
				{props.text}
			</div>
			<div className="Comment-date">
				{formatDate(props.date)}
			</div>
		</div>
	);
}
```

이 컴포넌트는 author , text , date 를 props로 받은 후 소셜 미디어 웹 사이트의 코멘트를 나타낸다.

이 컴포넌트는 구성요소들이 모두 중첩 구조로 이루어져 있어서 변경하기 어려울 수 있으며, 
각 구성요소를 개별적으로 재사용하기도 힘들다.

따라서 이 컴포넌트에서 몇 가지 컴포넌트를 추출해 만들어 사용해보자.

**1. Avator 추출**

```jsx
function Avator(props) {
	return (
		<img className="Avator"
						src = {props.user.avatorUrl}
						alt = {props.user.name}
		/>
	);
}
```

src = {props.user.avatorUrl} 를 보면 이전 {props.author.avatorUrl} 중간 부분에 변경이 생겼는데
이는 Comment 컴포넌트에서 Avator 컴포넌트로 props를 전달해줄 때 키를 user 로 넘겨줄 것이기 때문이다.

```jsx
import Avator from './Avator';

function Comment(props) {
	return (
		<div className="Comment">
			<div className="UserInfo">
				<Avator user={props.author} // 이런식으로 변경이 가능하다.
				<div className="UserInfo-name">
					{props.author.name}
				</div>
			</div>
			<div className="Comment-text">
				{props.text}
			</div>
			<div className="Comment-date">
				{formatDate(props.date)}
			</div>
		</div>
	);
}
```

이런식으로 여러가지 컴포넌트를 분리해서 만들어 부품처럼 사용할 수 있다.
처음에는 컴포넌트를 추출하는 작업이 지루해 보일 수 있다.
하지만 재사용이 가능한 컴포넌트를 만들어 놓는 것은 더 큰 앱에서 작업할 때 두각을 나타낸다.

---

## props 는 읽기 전용이다.

함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 props를 수정해서는 안된다.

다음 sum 함수를 살펴보자.

```jsx
function sum(a, b) {
	return a + b;
}
```

이런 함수들은 순수 함수라고 호칭한다.
입력값을 바꾸려 하지 않고 항상 동일한 입력값에 대해 동일한 결과를 반환하기 때문이다.

반면에 다음 함수는 자신의 입력값을 변경하기 때문에 순수 함수가 아니다.

```jsx
function withdraw(account, amount) {
	account.total -= amount;
}
```

React는 매우 유연하지만 한 가지 엄격한 규칙이 있다.

**!! 모든 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 한다. !!**
