
# 엘리먼트 렌더링

엘리먼트는 React 앱의 가장 작은 단위이다.

엘리먼트는 화면에 표시할 내용을 기술한다.

```jsx
const element = <h1>Hello, world</h1>;
```

브라우저 DOM 엘리먼트와 달리 React 엘리먼트는 일반 객체 (plain object) 쉽게 생성할 수 있다.
React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트한다.

- **컴포넌트** 와 **엘리먼트** 를 혼동할 수 있다.
- 엘리먼트는 컴포넌트의 "구성 요소" 이다.

---

## 간단한 Element 와 Component 의 차이

```jsx
// 받는 인자 (props) 가 없고 return 하지 않는다.
// element는 태그를 지정하는 객체이고, 변하지 않는다.
// 우리가 화면에서 보는 것을 이야기한다.
const element = (
	<div>
		<HeadText>"This is Element"</HeadText>
		<Rio />
	</div>
);
```

```jsx
// Component는 props를 입력으로 받을 수 있고, Element를 반환하는 함수 그리고
// 클래스를 이야기 한다.
// 또한 State (상태) 가질 수 있다.
const Component = (props) => {
	console.log(props);

	return <h1>{props.children}</h1>
}
```

---

## DOM에 엘리먼트 렌더링하기

HTML 파일 어딘가에 < div > 가 있다고 가정해보자.

```jsx
<div id="root"></div>
```

이 안에 들어가는 모든 엘리먼트를 React DOM에서 관리하기 때문에 이것을 "Root" DOM 노드라도 부른다.

React 로 구현된 애플리케이션은 일반적으로 하나의 루트 DOM 노드가 있다.
React를 기존 앱에 통합하려는 경우 원하는 만큼 많은 수의 독립된 루트 DOM 노드가 있을 수 있다.

React 엘리먼트를 랜더링하기 위해서는 우선 DOM 엘리먼트를 ReactDOM.createRoot() 에 전달한 다음,
React 엘리먼트를 root.render() 에 전달해야 한다.

```jsx
const root = ReactDOM.createRoot(
	document.getElementById('root')
);
const element = <h1>Hello, world</h1>;
root.render(element);
```

---

## 렌더링 된 엘리먼트 업데이트 하기

React 엘리먼트는 불변객체이다. 
엘리먼트를 생성한 이후에는 해당 엘리먼트의 자식이나 속성을 변경할 수 없다.

엘리먼트는 영화에서 하나의 프레임과 같이 특정 시점의 UI를 보여준다.

지금까지 배운 내용을 바탕으로 하면 UI를 업데이트하는 방법은 새로운 엘리먼트를 생성하고
이를 **root.render()** 로 전달하는 것이다.

```jsx
const root = ReactDOM.createRoot(
	document.getElementById('root')
);

function tick() {
	const element = (
		<div>
			<h1>Hello, world!</h1>
			<h2>It is {new Date().toLocaleTimeString()}.<h2>
		</div>
	);
	root.render(element);
}

setInterval(tick, 1000);
```

위 setInterval() 콜백 함수는 현재 1초마다 root.render() 를 호출한다.

실제로 대부분의 React 앱은 root.render() 를 한 번만 호출한다.

---

## 변경된 부분만 업데이트하기

React DOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 DOM을 원하는 상태로
만드는데 필요한 경우에만 DOM을 업데이트한다.
