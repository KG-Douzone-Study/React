# Functional Components - React Hooks

## useReducer

```txt
useReducer는 useState처럼 State를 생성하고 관리하게 해주는 Hook이다.

보통 여러개의 하위 값을 포함하는 복잡한 State를 다둘때 useReducer를 사용하면 편하다.
```

```txt
useReducer는
1. Reducer
2. Dispatch
3. Action
으로 이루어져 있다.


Dispatch ----> Action ----> Reducer ----> State

Dispatch(Action) ----> Reducer(State, Action) --(update)--> State
```

```jsx
// useReducer를 생성하는 방법은 다음과 같다
const [state, dispatch] = useReducer(reducer, state의 초기값);

// 예를 들면
const [money, dispatch] = useReducer(reducer, money의 초기값);
```

<img width="583" alt="스크린샷 2023-05-26 오전 11 47 43" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/0f39b8ad-c4a4-44c3-9d48-83245a91c75d">

```jsx
// 그럼 useReducer(reducer, money의 초기값)에서
// reducer에 들어갈 function을 만들어보자.
```

<img width="577" alt="스크린샷 2023-05-26 오후 1 12 11" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/cd26280c-6a79-4d8e-9d36-b4f76128219b">

```txt
위에 const reducer = (state, action) => {}
function에서 만든 reducer가 useReducer(reducer, 0);로
들어가게 되면 reducer의 state는 money의 값을 가지게 된다.
```

<img width="581" alt="스크린샷 2023-05-26 오후 1 16 52" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/adad9dd6-59b4-4410-88c3-ac2c1a953c6a">

```jsx
// <button> 태그에 onClick 이벤트를 넣고 () => {dispatch()} 를 호출하면
const reducer = (state, action) => {
	console.log('reducer가 일을 한다', state, action);
};
// console.log가 출력되는 것을 확인할 수 있다.
```

<img width="816" alt="스크린샷 2023-05-26 오후 1 19 15" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/8f1c54cd-53f8-4861-a84e-ac076d26a9dc">

```jsx
console.log('reducer가 일을 한다', state, action)
// 여기서 state는 money의 초기값 0으로 셋팅되어 있는것을 볼 수 있고
// action은 지정해준 것이 없기때문에 undifined 라고 출력되는 것을 볼 수 있다.

// 그럼 action을 추가해보자.
// dispatch() 안에 로직을 추가해주면 된다.
```

<img width="574" alt="스크린샷 2023-05-26 오후 1 22 17" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/4b68a44b-cd3a-48e8-96a5-1ab432a6f712">

```jsx
<button onClick={() => {
	dispatch({type: 'deposit'})
}}>예금</button>

// 여기서 dispatch() 안에 {type: 'deposit'} 이라고 action을 넣어보았다.
```

<img width="887" alt="스크린샷 2023-05-26 오후 1 24 02" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/ea7eb8f1-ca4e-40d8-bfc1-d81f5eee314c">

```txt
reducer가 일을 한다. 0 > {type: 'deposit'} 이라고 잘 출력된 것을 볼 수 있다.

조금 더 추가해보자.
```

<img width="401" alt="스크린샷 2023-05-26 오후 1 27 15" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/3a5152fb-55ea-446f-a962-c0bece37d6d4">

<img width="384" alt="스크린샷 2023-05-26 오후 1 27 44" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/09384d2d-78f4-480c-bb02-fa99898aaed0">


```txt
payload가 잘 출력되는 것을 확인할 수 있다.

그렇다면 이제 reducer state의 값을 변경해보자.
```

<img width="543" alt="스크린샷 2023-05-26 오후 2 19 11" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/45edfb92-8a62-483a-b673-cbc6fad2b96d">

```jsx
return state + action.payload;
// return은 새로 업데이트 될 state를 나타낸다.
// 추가적으로 보통 type으로 if, switch문을 활용해 action에 변화를 준다.
```

<img width="430" alt="스크린샷 2023-05-26 오후 2 22 43" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/13be65cf-f49f-43b4-a0e1-b04eeb41fda3">

<img width="577" alt="스크린샷 2023-05-26 오후 2 22 54" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/606ac2db-6ae4-4443-8ab4-4fc910f9cb31">

```jsx
// 추가적으로 type 부분이 지저분해 수정하자면
const ACTION_TYPES = {
	deposit: 'deposit',
	withdraw: 'withdraw',
}

// 이런식으로 설정한 뒤

const reducer = (state, action) => {
	if (action.type === ACTION_TYPES.deposit) {
		// 이런식으로 변경해주면 추후에 유지보수하기도 쉽다.
	}
}
```

---

### 조금 더 복잡한 State - reducer (출석부)

```jsx
// app.js
const initalState = {
	count: 0,
	students: [],
};

  

const reducer = (state, action) => {

	switch(action.type) {
		case 'add-student':
		const name = action.payload.name;
		const newStudent = {
			id: Date.now(),
			name: name,
			isHere: false,
		}

		return {
			count: state.count + 1,
			students: [...state.students, newStudent],
		};

		case 'delete-student':

		return {
		count: state.count - 1,
		students: state.students.filter((student) => student.id !== action.payload.id),
		};

		case 'mark-student' :

		return {
		count: state.count,
		students: state.students.map((student) => {
		if(student.id === action.payload.id) {
		return {...student, isHere: !student.isHere}
		}
		return student;
		})
	}

		default:
		return state;
	}
};

  
  

function App() {

const [name, setName] = useState("");
const [studentsInfo, dispatch] = useReducer(reducer, initalState);

return (
	<div>
	<h1>출석부</h1>
	<p>총 학생 수: {studentsInfo.count}</p>

	<input
	type="text"
	placeholder="이름을 입력해주세요"
	value={name}
	onChange={(e) => {
	setName(e.target.value);
	}}
	/>

<button onClick={() => {

	dispatch({
	type: 'add-student',
	payload: {name}
	})
}}>추가
</button>

{studentsInfo.students.map((student) => {
return <Student key={student.id} name={student.name} dispatch={dispatch} id={student.id} isHere={student.isHere}/>;
})}

</div>
);
}

export default App;
```

```jsx
// student.js
import React from 'react';

const Student = ({name, dispatch, id, isHere}) => {

return (
<div>
<span style={{
textDecoration: isHere ? "line-through" : 'none',
color: isHere ? 'gray' : 'black'
}}

onClick={() => {
dispatch({type: 'mark-student', payload: {id}})
}}
>{name}</span>

<button onClick={() => {
dispatch({
type: 'delete-student',
payload: {id},
})
}}>삭제</button>
</div>
);
}

export default Student;
```




