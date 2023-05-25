# Functional Components - React Hooks

## useContext + Context API

```txt
React 컴포넌트들을 보면 App() 컴포넌트에서 prop으로 하위 컴포넌트들에게 { isDark, setIsDark }
를 내려보내주고 있다.

화면 색상만 흰색에서 검은색으로 바꾸는 간단한 로직에도 prop를 이렇게 내려보낸다면
개발자는 죽어나는거다...

!물론 그렇다고 prop 대신 context만을 쓰는것도 안된다.
context를 사용하면 React의 장점인 재활용이 가능한 모듈화가 되지 않는다.
```

<img width="489" alt="스크린샷 2023-05-25 오후 3 36 22" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/d3886e4f-85fa-4d3a-a709-ebc0c47d4079">
<img width="519" alt="스크린샷 2023-05-25 오후 3 37 40" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/c02ccd66-e8ab-430e-9b1e-7846f578f75b">
<img width="549" alt="스크린샷 2023-05-25 오후 3 38 12" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/ab068926-4aaa-4e87-96a2-2dfd002f8dae">
<img width="550" alt="스크린샷 2023-05-25 오후 3 38 20" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/d333bfc4-8ed2-46b2-821d-2e24767c1bbb">
<img width="516" alt="스크린샷 2023-05-25 오후 3 38 26" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/226570e8-00f9-4050-9981-78d7258de3a5">

---

```txt
그럼 useContext Hook을 이용해 prop를 사용하지 않고 데이터를 보내보자.
```

```jsx
// context를 사용하기 위해선 일단 createContext를 선언해야한다.
// context를 만들 파일을 하나 만들자. (Ex : ThemeContext.js)

import { createContext } from 'react';

const ThemeContext = createContext(null);

export default ThemeContext;
```

```txt
컴포넌트를 만들었다면 이제 사용해보자.
App.js 파일에 import 시키자.

그리고 우리가 사용할 부분을 <Context.Provider/> 로 감싸주자.
```

```jsx
import ThemeContext from './context/ThemeContext';

function App() {
	const [isDark, setIsDark] = useState(false);

	return (
		<ThemeContext.Provider> 
			<Page isDark={isDark} setIsDark={setIsDark} />
		<ThemeContext.Provider />
	);
}

```

```jsx
<ThemeContext.Provider> 에는 태그 안에 value로 값을 넣어 지정해 줄 수 있다.
```

```jsx
// 지금같은 경우에는 isDark, setIsDark 를 사용하는 컴포넌트에 전부 보내주어야하기 때문에
// Object 형으로 만들어주자.

import ThemeContext from './context/ThemeContext';

function App() {
	const [isDark, setIsDark] = useState(false);

	return (
		<ThemeContext.Provider value={{isDark, setIsDark}}> 
			<Page isDark={isDark} setIsDark={setIsDark} />
		<ThemeContext.Provider />
	);
}

export default App;
```

```jsx
// 지금같은 경우에는 isDark, setIsDark 를 사용하는 컴포넌트에 전부 보내주어야하기 때문에
// Object 형으로 만들어주자.

import ThemeContext from './context/ThemeContext';

function App() {
	const [isDark, setIsDark] = useState(false);

	return (
		<ThemeContext.Provider value={{isDark, setIsDark}}> 
			<Page />
		<ThemeContext.Provider />
	);
}

export default App;
```

```txt
prop 없이 Page.jsx 파일에 데이터가 넘어가는지 확인하기 위해
Page.jsx 를 수정해보자.
```

```jsx
// 우리가 만든 Createcontext의 이름은 ThemeContext 이다.
// context에 저장된 데이터를 가져올 컴포넌트에서 useContext(context 컴포넌트)를 사용해
// 데이터를 가지고 올 수 있다.

import React, { useReact } from 'react';
import ThemeContext from '../context/ThemeContext';

const Page = () => {
	const data = useContext(ThemeContext);
	// 데이터가 나오는지 콘솔 로그를 찍어보자.
	console.log('data: ', data);

	return (
		<div className="page">
		<!-- 지금 Header, Content, Footer 안에 값은 일단 다 지우자.-->
			<Header />
			<Content />
			<Footer />
		</div>
	);
}

export default Page;
```

<img width="261" alt="스크린샷 2023-05-25 오후 4 12 13" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/0ce6cb15-b5b3-4c0b-b738-eefe3a1a3cf1">

```txt
console.log('data: ', data)에 값이 잘 출력되는 것을 확인할 수 있다.
```

```txt
Header.jsx 파일로 가서 수정해보자.
```

```jsx
import React, { useContext } from "react";

import ThemeContext from "../context/ThemeContext";

  
const Header = () => {

	// context에 저장된 것과 다른
	// 예를 들어 지금은 isDark, setIsDark 가 context에 저장되어 있지만
	// context에 저장되어 있지 않은 Hi 같은것을
	// const { Hi } = useContext(ThemeContext); 라고 하면
	// Hi = undifiend 가 나온다.
	const { isDark } = useContext(ThemeContext);
	console.log(isDark); // false 값 출력 됨

	return (
		<header
		className="header"
		style={{
		backgroundColor: isDark ? 'black' : 'lightgray',
		color: isDark ? 'white' : 'black',
		}}
		>		
	<h1>Welcome 홍길동!</h1>
	</header>
	);
}

export default Header;
```

```txt
이런식으로 다른 컴포넌트들도 변경해주고 실행해보자.
```

<img width="800" alt="스크린샷 2023-05-25 오후 4 22 36" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/35e3434d-7fbc-4147-84f8-1b158f12a3e8">
<img width="800" alt="스크린샷 2023-05-25 오후 4 22 43" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/053d9874-808d-47e2-8bd1-fa1f3588996c">

```txt
잘 나오는 것을 확인할 수 있다.
```
