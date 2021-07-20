# 현재 파일은 실험용 파일입니다

목차  
[1.React Hook을 이해하기 위한 기초 지식입니다](#React-Hook을-이해하기-위한-기초-지식입니다)

---

## React Hook을 이해하기 위한 기초 지식입니다
이 내용은 Minjun Kim 님의 [리액트 Hooks 완벽 정복하기](https://velog.io/@velopert/react-hooks)를 공부한 내용입니다.
  
## 1. useState
useState 는 가장 기본적인 Hook입니다. 함수형 컴포넌트 상태를 관리할때 주로 사용합니다. 사용할 때는 import 내용에 { useState }를 추가해주면 사용 가능합니다.
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
useEffect는 리액트 컴포넌트가 랜더링 될 때마다 특정 작업을 수행하도록 설정 할 수 있는 Hook입니다.
>예시
```js
import React , { useEffect } from 'react'
```