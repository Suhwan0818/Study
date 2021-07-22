# 현재 파일은 실험용 파일입니다

목차  
[1.React Hook을 이해하기 위한 기초 지식입니다](#React-Hook을-이해하기-위한-기초-지식입니다)

---

## React Hook을 이해하기 위한 기초 지식입니다
이 내용은 **Minjun Kim** 님의 [리액트 Hooks 완벽 정복하기](https://velog.io/@velopert/react-hooks)를 공부한 내용입니다.
  
## 1. useState
```useState``` 는 가장 기본적인 **Hook**입니다. 함수형 컴포넌트 상태를 관리할때 주로 사용합니다. 사용할 때는 ```import``` 내용에 ```{ useState }```를 추가해주면 사용 가능합니다.
>예시  
```js
import React, { useState } from 'react'
```
기본적인 사용 방법은 배열 비구조화 할당 문법과 같이 사용합니다. 배열을 이용한 간단한 예제를 보겠습니다.
>예시
```js
const array = ['dog', 'cat', 'sheep'];
const [first, second] = array; // useState의 경우
//const [value, setValue] = useState(0);
console.log(first, second); // dog cat
```
위의 내용을 이용할 경우 함수형 컴포넌트에서 상태 관리가 가능합니다.

## 2. useEffect
```useEffect```는 리액트 컴포넌트가 랜더링 될 때마다 특정 작업을 수행하도록 설정 할 수 있는 **Hook**입니다.
>예시
```js
import React , { useEffect } from 'react'
```

간단한 예제를 하나 만들어 보겠습니다.
>예제
```js
import React, { useState, useEffect } from 'react';

const Info = () =>{
    const[id, setId] = useState('');
    const[password, setPassword] = useState('');
    useEffect(() => {
        console.log({
            id,
            password
        });
    });

    const onChangeId = e => {
        setId(e.target.value);
    };
    
    const onChangePassword = e => {
        setPassword(e.target.value);
    };

    return (
        (...)
    )
}
```
보기와 같이 작성했을 경우 아래와 같이 보입니다.

![example](./img/useEffect_test_2.PNG)  
그리고 콘솔에는 다음과 같이 출력됩니다.
![example](./img/useEffect_test_2-2.PNG)

```useEffect``` 는 기본적으로 랜더링 직후 실행이 됩니다. 여기에 추가를 할 수 있는 부분은 **cleanup** 함수가 있습니다.   
**cleanup** 함수는 만약 컴포넌트가 언마운트되기 전이나, 업데이트 되기 직전에 어떠한 작업을 수행하고 싶을때 사용할 수 있습니다. 간단한 예시를 보겠습니다
  
  Info 컴포넌트의 useEffect 부분을 다음과 같이 수정합니다.
  ### Info.js - useEffect
  ```js
  useEffect(() = > {
      console.log('effect');
      console.log(name);
      return () => {
          console.log('cleanup');
          console.log(name);
      }
  })
  ```

  크게 바뀐점으로 보이는 곳은 ```return``` 부분입니다. 비슷하게 **App.js**도 바꿔보겠습니다
  ### App.js
  ```js
  import React, { useState } from 'react';
  import Info from './Info';

  const App = () => {
      const[visible, setVisible ] = useState(false);
      return(
          <div>
            <button
            onClick={() => {
                setVisible(!visible);
            }}
            >
            {visible ? '숨기기' : '보이기'}
            </button>
            <hr />
            {visible && <Info />}
          </div>
      );
  };
  ```

  실행결과는 다음과 같습니다.  
  ![example](./img/useEffect_test_3.PNG)  
  
  보이기와 숨기기를 각각 눌러보면 **console**에 아래와 같이 출력이 됩니다.  
  ![example](./img/useEffect_test_3-1.PNG)  

  아까와 같이 아이디란에 글자를 입력하게 되면 아래와 같이 출력됩니다.  
  ![example](./img/useEffect_test_3-3.PNG)  

  만약 오직 언마운트 될 때만 뒷정리 함수를 호출하고 싶다면 ```useEffect``` 함수의 두번째 파라미터에 비어있는 배열을 넣으면 됩니다.

  ### Info.js - useEffect
  ```js
  useEffect(() => {
      console.log('effect');
      console.log(name);
      return () => {
          console.log('cleanup');
          console.log(name);
      };
  }[])
  ```

  ## 3. useContext

  이 **Hook**을 사용하면 컴포넌트에서 Context를 보다 더 쉽게 사용 할 수 있습니다.

  Context 객체(```React.createContext```에서 반환된 값)을 받아 그 context의 현재 값을 반환합니다. context의 현재 값은 트리 안에서 이 Hook을 호출하는 컴포넌트에 가장 가까이에 있는 ```<MyContext.Provider>```의 ```value``` prop에 의해 결정됩니다.

 ### 주의할 점  
 ```useContext```로 전달한 인자는 ```context``` 객체 그 자체이어야 합니다.

 >예제
 - 맞는 사용 : useContext(MyContext)
 - 틀린 사용 : useContext(MyContext.Provider)
 - 틀린 사용 : useContext(MyContext.Consumer)


## 4. useReducer  
```useReducer``` 는 ```useState```보다 컴포넌트에서 더 다양한 상황에 따라 다양한 상태를 다른 값으로 업데이트 해주고 싶을 때 사용하는 Hook입니다.  

리듀서는 현재 상태와, 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달 받아 새로운 상태를 반환하는 함수입니다. 리듀서 함수에서 새로운 상태를 만들 때는 꼭 불변성을 지켜주어야 합니다.

>예시
```js
function reducer(state, action){
    return{...};
}
```
액션값은 주로 다음과 같은 형태로 생겼습니다.
```js
{
    type: 'INCREMENT',
    //다른 값이 필요하다면, 추가적으로 들어감
}
```

이전에 작성했던 Counter 컴포넌트를 useReducer를 사용하여 다시 구현해봅시다.

### Counter.js  
```js
import React,{ useReducer } from 'react';

function reducer(state, action){
    switch(action.type){
        case 'INCREMENT':
            return { value:state.value + 1 }
        case 'DECREMEMT':
            return { value:state.value - 1 }
        default:
            return state
    }
}

const Counter = () => {
    const[state, dispatch] = useReducer(reducer, { value:0 });

    return (
        <div>
            <p>
                현재 카운터 값은 <b>{state.value}</b> 입니다.
            </p>
            <button onClick={() => dispatch({ type : 'INCREMENT'})}>+1</button>
            <button onClick={() => dispatch({ type : 'DECREMENT'})}>-1</button>
        </div>
    );
};

export default Counter;
```  
```useReducer```을 사용했을 때의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 점 입니다.  

코드를 다 작성하셨다면 App에서 Counter를 다시 랜더링 해주고 브라우저에서 +1/-1 버튼들을 눌러보세요.

### App.js
```js
import React from 'react';
import Counter from './Counter';

const App = () => {
    return <Counter />;
};

export default App;
```
잘 작동되는 것을 확인할 수 있습니다.

이번에는 useReducer를 사용하여 Info 컴포넌트에서 인풋 상태를 관리해보겠습니다.

기존에 인풋이 여러개여서 useState를 여러개 사용했던 방법을 useReducer를 이용하여 다시 만들어보겠습니다

### Info.js
```js
import React, { useReducer } from 'react';

function reducer(state, action){
    return{
        ...state,
        [action.name]: action.value
    };
}

const Info = () => {
    const [state, dispatch] = useReducer(reducer, {
        name : '',
        nickname: '',
    });
    const { name, nickname } = state;
    const onChange = e => {
        dispatch(e.target);
    };
    return (
        <div>
            <div>
                <input name="name" value = {name} onChange = {onChange} />
                <input nickname="nickname" value = {nickname} onChange = {onChange} />
            </div>
            <div>
                <div>
                    <b>이름:</b> {name}
                </div>
                <div>
                    <b>닉네임:</b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

useReducer에서의 액션은 그 어떤 값이 되어도 됩니다. 그래서 이번에 우리가 이벤트 객체가 지니고있는 e.target 값 자체를 액션 값으로 사용하였습니다.

이런식으로 인풋을 관리하면, 아무리 인풋의 갯수가 많아져도 코드를 짧고 깔끔하게 유지 할 수 있습니다.

이제 App에서 Info 컴포넌트를 랜더링해보고 잘 작동하는지 확인해보세요

### App.js
```js
import React from 'react';
import Info from './Info';

const App = () => {
    return <Info />;
};

export default App;
```

## 5. useMemo
