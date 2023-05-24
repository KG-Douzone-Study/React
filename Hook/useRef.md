# Functional Components - React Hooks

## useRef

함수형 컴포넌트에서 useRef 를 호출하면 ref 객체를 반환해준다.

```jsx
const ref = useRef(value)
반환값(return) -> { current: value }
```

---

## useRef 가 유용한 순간

**useRef 는 useState 처럼 정보를 저장해두는 저장공간으로 사용된다.**

예를 들어

함수형 프로그래밍에서 State가 변경되면 재 렌더링이 되고
이는 컴포넌트 내부 변수들의 초기화를 불러온다.

대신 Ref 안에 값을 저장해두면 
재 렌더링 되지않고 변수들의 값이 유지된다.

또한 State가 변경되어 재 렌더링이 일어나도 Ref의 값은 유지된다.

그리고 

**Ref를 가지고 DOM 요소에 접근이 가능하다**

예들 들어 input 태그에 focus()를 자동으로 맞춰준다.

---

useRef 는 함수형 컴포넌트를 재실행하지 않기 때문에 로직 상으로 동작하더라도
화면상으로 재 랜더링이 되지 않는 한 알수가 없다.

<img width="524" alt="스크린샷 2023-05-24 오후 3 09 46" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/3afbaec5-2590-4cb5-843e-cca94fb8b014">

<img width="700" alt="스크린샷 2023-05-24 오후 3 11 04" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/64b7f02f-93ff-4e83-9b39-bf766784bd7f">

Ref 올려 버튼을 여러번 클릭했지만 화면 그리고 console 상으로 아무런 반응이 없다.
로직은 정상적으로 동작하고 있지만 보이지 않는 것이다.

이때 State 의 값을 변경해준다면 함수형 컴포넌트는 재 랜더링되기에
변화된 Ref의 값도 확인이 가능할 것이다.

<img width="700" alt="스크린샷 2023-05-24 오후 3 13 34" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/d2d66fbe-289f-4ded-ae1b-c275993798da">

<img width="451" alt="스크린샷 2023-05-24 오후 3 15 14" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/4939eac8-aae9-4d3a-9eaa-543ceca068bf">

---

## useRef 로 DOM 요소에 접근해보자.

```jsx
const ref = useRef(value)
-> <input ref={ ref } />

이렇게 ref 속성으로 넣어주기만 한다면 손쉽게 해당 요소에 접근할 수 있다.
```

<img width="575" alt="스크린샷 2023-05-24 오후 3 38 36" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/9bf2c796-3622-4d91-9692-6304b14d2471">

<img width="477" alt="스크린샷 2023-05-24 오후 3 39 07" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/48ad97f6-96fb-4894-ae95-360555c57d3c">

```txt
input 태그 안 ref 속성으로 우리가 설정한 useRef 를 넣어주었더니
input 값을 가지게 되었다.
```

<img width="564" alt="스크린샷 2023-05-24 오후 3 42 50" src="https://github.com/whochucompany/ByteClone-BE/assets/96435200/b1373107-c11d-4fde-abba-7bc81e59e9cd">

```txt
이런식으로 input에 focus를 줄 수도 있다.
```
