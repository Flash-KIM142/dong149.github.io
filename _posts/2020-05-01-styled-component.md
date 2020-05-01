---
title: styled-component 가이드
layout: post
part: developing
---

styled-component 가이드

Styled-component Guide
===

+++



## GlobalStyle을 정의한다. 

```typescript
import {createGlobalStyle} from 'styled-components';

export const GlobalStyle = createGlobalStyle` 
body{
	padding: 2rem;
	font-family: ${fonts.generalFont},sans-serif;
}
a{
	color: unset;
}

`;

```



##styled-components

```typescript
import styled from 'styled-components';

const MainBox = styled.div` 이름의 앞은 대문자여야 된다.
	text-align: center;
`;

const MainBoxButton = styled.button` 
	display: inline;
`
styled.?? 여기서 이 물음표 부분에 내가 넣고자하는 html 태그를 넣으면 된다.



return(
	<>
  	<GlobalStyle /> 
  	<MainBox>
  		검색
  	<MainBox/>
  	<MainBoxButton onClick = {() => getsummonerByName(summoners)}>
  	<MainBoxButton/>
  </>

)
```



## styled-components 와 typescript 를 같이 쓸 때 발생하는 Issue

```typescript
const styles = {
  container: styled.View`
		width: ${props} => props.isLarge ? '600' : '200'}px;
	`
};

const Example = (props) => {
  return(
  	<styles.container isLarge = {true} />
  )
}
```

이를 typescript에서 그대로 적용하려 하다면, 에러 발생.

styled component 내의 prop 에 isLarge라는 type및 변수를 정적으로 지정해주지 않았으므로, typescript 에서는 에러를 반환한다.

이를 수정하려면, interface를 통해 styled component 에서 사용할 prop 을 정의하거나, any type으로 무조건 변환해 주는 방법이 있다.

```typescript
// interface 를 이용한 방법.
interface ITest{
  isLarge : boolean,
}

const styles = {
  container: styled.View`
		width: ${(props:ITest) => props.isLarge ? '600' : '200'}px;
	` 
}
 //props을 any type으로 지정하는 방법.
const styles = { 
  container: styled.View`
		width: ${(props:any) => props.isLarge ? '600' : '200'}px;
	`
}
```
