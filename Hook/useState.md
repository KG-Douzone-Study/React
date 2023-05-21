# Functional Components - React Hooks

## useState

```txt
State 란?
- 컴포넌트의 상태
```

```jsx
const [state, setState] = useState(초기값);
// const [최산 상태 스냅샷, 스냅샷을 변경할 함수] = useState(초기값);

// ex
const [time, setTime] = useState(5);
// 배열 안의 이름은 사용자가 직접 지정
// useState 로 time을 5로 초기 지정
// time: 5 값을 변경하고 싶다면
// setTime(10) 함수 안에 값을 넣어서 변경
// time: 10 으로 변경 된다.
```

```txt
setState() 함수로 값을 변경하게 되면 react는 렌더링을 새로 하게 된다.
즉, 화면이 자동으로 업데이트 된다.
```

```jsx
import React, { Fragment, useState } from "react";

function Clock() {
	const [time, setTime] = useState(1);
	
	const handleClick = (event) => {

	setTime(time + 1); // setTime 함수를 이용해 최신 스냅샷 time을 가져와 +1 해준다.
	};

	return (
	<Fragment>
	{/* {time} ... {} 을 사용하면 동적으로 값을 가져올 수 있다. */}
	<span>현재 시각 : {time}시</span>
	<button onClick={handleClick}>Update</button>
	</Fragment>
	);

};

export default Clock;
```

```jsx
import React, { Fragment, useState } from "react";

function Clock() {
	const [time, setTime] = useState(1);
	
	const handleClick = (event) => {

		let overTime;

// 시간이 24시가 넘어가면 다시 1로 초기화하는 로직
// 이렇게 시간을 동적으로 변경할 수 있다.
		if (time > 23) {
			overTime = 1;
		} else {
			overTime = time + 1;
		}
		setTime(overTime);

	return (
	<Fragment>
	{/* {time} ... {} 을 사용하면 동적으로 값을 가져올 수 있다. */}
	<span>현재 시각 : {time}시</span>
	<button onClick={handleClick}>Update</button>
	</Fragment>
	);

};

export default Clock;
```

---

위처럼 

function Clock() {

} 

함수형 방식으로 컴포넌트를 만들수도 있고 

const Clokc = () => {

}

화살표 함수형으로 만들 수도 있다.

```jsx
const InputText = () => {

const onInputTextHandler = (event) => {
	// onChange에 function은 event라는 값을 받을 수 있는데
	// event.target.value 로 입력된 값을 가져올 수 있다.
	console.log(event.target.value);
}

return (
	<div>
		<!-- onChange 를 사용하면 input text에 값이 변경되었을 때 값을 읽어들인다. -->
		<input type="text" onChange={onInputTextHandler}/>
		<button>Upload</button>
	</div>
	);
};

export default InputText;
```

```jsx
const InputText = () => {

// useState 초기화 및 생성
const [names, SetNames] = useState({'박상욱', '남지훈'});

const onInputTextHandler = (event) => {
	// onChange에 function은 event라는 값을 받을 수 있는데
	// event.target.value 로 입력된 값을 가져올 수 있다.
	console.log(event.target.value);
}

return (
	<div>
		<!-- onChange 를 사용하면 input text에 값이 변경되었을 때 값을 읽어들인다. -->
		<input type="text" onChange={onInputTextHandler}/>
		<button>Upload</button>
		<!-- useState 의 값을 출력해보자.-->
		<!-- map(names의 값 하나씩을 name이라는 이름으로, index를 넣으면 자동으로 값 지정) -->
		{names.map(name, index) => { 
			<!-- react에서 list 처럼 하나씩 출력하려면
			react에서 구별이 가능하게 key 값을 지정해주어야 한다. -->
			return <p key={index}>{name}</p>
		}}
	</div>
	);
};

export default InputText;
```

```jsx
import React, { useState } from "react";

const InputText = () => {

const [names, setNames] = useState(['박상욱', '남지훈']);
const [input, setInput] = useState('');

// input으로 사용자가 입력하면 들어올 함수
const onInputTextHandler = (event) => {
	setInput(event.target.value);
}

// useState 로 button을 클릭시 값을 추가로 저장할 함수
const handleUpload = () => {
	// prevState 를 받으면 현재 state 정보를 전부 가지고 온다.
	setNames((prevState) => {
	console.log("이전 state = ", prevState);
	// ...prevState를 넣으면 현재 state에 들어있는 정보를 복사해오고
	// input 에 있는 값도 추가해 새로운 배열을 만든다.
	// 배열을 복사하는 여러가지 방법이 있지만 
	// prevState로 가지고 배열을 새로 만드는 방법이 가장 좋다.
	return [...prevState, input]
	});
}

return (
	<div>
		<input type="text" onChange={onInputTextHandler} value={input}/>
		<button onClick={handleUpload}>Upload</button>
		{names.map((name, index) => {
		return <p key={index}>{name}</p>
		})}
	</div>
	);
};

export default InputText;
```

<img width="1000" alt="스크린샷 2023-05-21 오후 7 45 14" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/09aed07a-c014-4557-aa2b-3b66f5e6c045">
<img width="1000" alt="스크린샷 2023-05-21 오후 7 45 56" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/65d35ceb-610b-4019-934a-e4307e503514">
이처럼 배열에 새로운 값을 추가할 수 있다.
하지만 useState에 set으로 값이 변경되면 React는 항상 새로 렌더링을 하기에
useState에 작업이 무겁다면 성능에 악영향을 미칠 수 있다.

이 부분을 수정해보자.

일단 기존 코드로 어떻게 동작하나 보자.

```jsx
import React, { useState } from "react";

const heavyWork = () => {
	console.log('엄청 무거운 작업 로직이 있다.');
	return ['박상욱', '남지훈'];
}

const InputText = () => {
	const [names, setNames] = useState(heavyWork());
	const [input, setInput] = useState('');
	// input으로 사용자가 입력하면 들어올 함수
	const onInputTextHandler = (event) => {
	setInput(event.target.value);
}

// useState 로 button을 클릭시 값을 추가로 저장할 함수

const handleUpload = () => {
	setNames((prevState) => {
	console.log("이전 state = ", prevState);
	return [...prevState, input]
	});
}

return (
	<div>
		<input type="text" onChange={onInputTextHandler} value={input}/>
		<button onClick={handleUpload}>Upload</button>
		{names.map((name, index) => {
		return <p key={index}>{name}</p>
		})}
	</div>
	);
};

export default InputText;
```

위에 heavyWork() 함수를 만들어 useState 초기값으로 지정하고 SetInput이 변경될때 마다
전부 새로 렌더링이 되는데 heavyWork() 의 엄청 무거운 작업 로직이 있다가 계속 호출된다.
<img width="1000" alt="스크린샷 2023-05-21 오후 7 53 19" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/63564157-d323-4001-bdbf-de0eaf469e0f">
따라서 새로 호출을 하고 싶지 않다면 콜백 메소드를 useState()를 넣어주면 된다.

<img width="1000" alt="스크린샷 2023-05-21 오후 7 57 12" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/82579305-6b34-4f05-8c15-7992510d97c0">
