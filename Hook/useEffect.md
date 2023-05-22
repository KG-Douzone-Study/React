# Functional Components - React Hooks

## useEffect

```txt
useEffect Hook은

어떤 컴포넌트가 
- Mount(화면에 첫 렌더링 되었을 때)
- Update(다시 렌더링 되었을 때)
- Unmount(화면에서 사라질때)

할 때 특정 작업을 실행하고 싶다면 useEffect Hook 을 사용한다.
```

```jsx
// 기본적으로 useEffect() 함수는 인자로 콜백함수를 받는다.
useEffect( () => {})
// 콜백 함수란? 함수로 인자로 전달된 함수를 이야기한다.
```

- useEffect는 딱 두 가지 형식만 알고있으면 된다.
```jsx
1.
useEffect(() => {
	// 작업...
});

// 첫번째로는 인자로 콜백함수만 받는 형식

2.
useEffect(() => {
	// 작업...
}, [value]);

// 두번째로는 인자로 콜백함수와 배열(dependency array)을 받는 형식이 있다.
```

- useEffect()의 첫번째 방법으로는
```jsx
useEffect(() => {
	// 작업...
})

// 컴포넌트가 가장 처음 렌더링 될떄, 다시 렌더링 되었을 때
// 렌더링 될때마다 실행된다.
```

- useEffect()의 두번째 방법으로는
```jsx
useEffect(() => {
	// 작업...
},[value]);

// 화면에 첫 렌더링 될 때 실행
// 두번째 인자의 배열(dependenct array)의 value 값이 바뀔때 실행된다.

// 만약 value 값에 [] 빈 배열이 인자 값으로 주어진다면
// useEffect() 는 첫 렌더링 될 때만 실행된다.
```

---

## useEffect -> Clean Up (밑에서 추가적으로 설명)

```jsx
Clean Up - 정리

// 등록한 리스너를 정리해주어야 하거나
// 타이머 같은 경우 타이머를 스탑시키고 싶을 때 등

// 이럴경우 useEffect() 안에 return() 구독 해지... 로직을 넣는다.

useEffect(() => {
	// 구독...
	return () => {
		// 구독 해지...
	}
}, []);
```

---

## useEffect -> 코드 작성

- useEffect 적용 X
```jsx
function App() {

const [count, setCount] = useState(1);

const handleCountUpdate = () => {
	setCount(count + 1);
};

return (
	<Fragment>
		<button onClick={handleCountUpdate}>update</button>
		<span>count: {count}</span>
	</Fragment>
	);
}

export default App;
```

- useEffect 적용 O -> Dependency array X
```jsx
import React, { Fragment, useState, useEffect } from 'react';
import './App.css';

function App() {

	const [count, setCount] = useState(1);
	const [name, setName] = useState('');
	const handleCountUpdate = () => {
	setCount(count + 1);
	};

	const handleInputChange = (event) => {
	setName(event.target.value);
	}

// 렌더링 될때마다 매번 실행된다.
useEffect(() => {
//...
console.log('렌더링');
});

return (
	<Fragment>
		<button onClick={handleCountUpdate}>update</button>
		<span>count: {count}</span>
		<input type="text" value={name} onChange={handleInputChange}/>
		<span>name : {name}</span>
	</Fragment>
	);
}

export default App;
```

<img width="800" alt="스크린샷 2023-05-22 오후 2 05 32" src="https://github.com/ParkRio/MegaKGCoffee/assets/96435200/7706aa40-4b46-415e-a124-32ad134ab3ca">
사진 오른쪽 console.log('렌더링') 을 보면
input 태그에 onClick = {} 이벤트로 setName이 변경될때마다 렌더링이 새롭게 이루어지고,
이에 따라 useEffect() 함수가 자동으로 실행되는 것을 볼 수 있다.

하지만 useEffect() 안의 로직이 무거운 로직이라면 useEffect() 함수가 계속 재실행되는 것은
부담이 될 수 있다.

따라서 useEffect() 에 두번째 인자
즉, dependecy array 를 추가해 이를 방지해주자.
dependency array 를 넣으면 배열 안의 값이 변경 되었을 때만 useEffect()가 실행된다.

- useEffect 적용 O -> Dependency array X
```jsx
import React, { Fragment, useState, useEffect } from 'react';
import './App.css';

function App() {

	const [count, setCount] = useState(1);
	const [name, setName] = useState('');
	const handleCountUpdate = () => {
	setCount(count + 1);
	};

	const handleInputChange = (event) => {
	setName(event.target.value);
	}

// 렌더링 될때마다 매번 실행된다.
useEffect(() => {
//...
console.log('렌더링');
}, [count]); // 카운트가 변경될 때마다 useEffect()를 실행한다.

return (
	<Fragment>
		<button onClick={handleCountUpdate}>update</button>
		<span>count: {count}</span>
		<input type="text" value={name} onChange={handleInputChange}/>
		<span>name : {name}</span>
	</Fragment>
	);
}

export default App;
```

만약 useEffect() 를 가장 처음 렌더링 될때만 호출하고 싶다면 두번째 인자에 빈 배열을 넣어주면 된다.

```jsx
useEffect(() => {
	//...
	conosle.log('렌더링')
}, []);
```

---

## useEffect -> Clean Up

```jsx
import React, { Fragment, useState, useEffect } from 'react';
import './App.css';
import Timer from './component/Timer';

function App() {

const [showTimer, setShowTimer] = useState(false);

return (
	<div>
		{showTimer && <Timer />}
		<button onClick={() => setShowTimer(!showTimer)}>Toggle Timer</button>
	</div>
	);

}

export default App;
```

```jsx
import React, { useEffect } from "react";

const Timer = (props) => {

useEffect(() => {
	const timer = setInterval(() => {
	console.log('타이머 돌아가는 중...');
	}, 1000);

  

// clean up
// ... 정리 작업으로 쓸 함수
// 리턴 함수는 컴포넌트가 언마운트 될때 실행된다.
// clean up 함수가 없다면 컴포넌트가 언마운트되도 타이머는 계속해서 실행된다.
// 왜냐하면? 따로 언제 종료되는지 지정해주지 않았기 때문이다.

	return () => {
	clearInterval(timer);
	console.log('타이머가 종료되었습니다.');
	};
	}, []);

  
	return (
		<div>
			<span>타이머를 시작합니다. 콘솔을 보세요!</span>
		</div>
		);
};

export default Timer;
```
