---
title: javascript 가이드
description: javascript에서 필요한 개념들을 정리했습니다.
author: 류동훈
layout: post
part: developing
---

javascript 가이드

# JavaScript Guide

+++

## Babel

Babel is a JavaScript Compiler, Use next generation JavaScript, today.

바벨은 다음 버전의 자바스크립트 문법을 현재 사용가능한 문법으로 변환 시켜주는 역할을 한다.

```
npm install @babel/node @babel/core
npm install @babel/preset-env (여러 버전이 있다 그 중에 선택해서 다운 받기 )

.babelrc 파일 만들어서 preset 설정해주기.
{
    "presets": ["@babel/preset-env"]
}

scripts 에서
node index.js -> babel-node index.js
로 변경.
```

+++

```typescript
const getSummonerByName = async (
  res: express.Response,
  summonerName: string
) => {
  baseAPI
    .get(`summoner/v4/summoners/by-name/${encodeURI(summonerName)}`)
    .then((resDataFromRiotGames) => {
      res.send(resDataFromRiotGames.data); //라이엇게임즈로부터 받은 데이터를 클라이언트에 전송한다.
    });
};

app.get("/", (req: express.Request, res: express.Response) =>
  res.send(`TOKEN: ${TOKEN}`)
);
```

## 화살표 함수 표현

화살표 함수 표현은 function 표현에 비해 구문이 짧고, 자신의 this, arguments,super 또는 new.target을 바인딩하지 않습니다.

화살표 함수는 항상 익명입니다. 이 함수 표현은 메소드 함수가 아닌 곳에 가장 적합합니다. 그래서 생성자로서 사용할 수 없습니다.

## Promise

"A promise is an object that may produce a single value some time in the future"

프로미스는 자바스크립트 비동기 처리에 사용되는 객체이다. 자바스크립트의 비동기 처리란, '특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성'을 의미한다.

프로미스는 주로 <u>주로 서버에서 받아온 데이터를 화면에 표시할 때 사용한다.</u>

```javascript
get('url 주소/products/1' function(response){
	//...
});
```

위 API가 실행되면 서버에 '데이터 하나 보내주세요'라는 요청을 보낸다. 하지만, 여기서 데이터를 받기도 전에 데이터를 다 받은 것처럼 화면에 데이터를 표시하려하면 오류가 발생한다.
이와 같은 문제를 해결하기 위한 방법 중 하나가 프로미스이다.

#### 프로미스 기초

```javascript
function getData(callbackFunc) {
  $.get("url 주소/products/1", function (response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc에 넘겨줌.
  });
}
getData(function (tableData) {
  console.log(tableData); //get()의 response값이 tableData에 전달됨.
});
```

위 코드는 제이쿼리의 ajax통신을 이용하여 지정한 url에서 1번 상품 데이터를 받아오는 코드이다. 비동기 처리를 위해 프로미스 대신에 콜백 함수를 이용했음.

프로미스 적용하면,

```javascript
function getData(callback) {
  //new Promise() 추가
  return new Promise(function (resolve, reject) {
    $.get("url 주소/products/1", function (response) {
      //데이터를 입력받으면 resolve() 호춣
      resolve(response); // -> fulfilled(이행/완료) 된 상태.
    });
  });
}
//getData()의 실행이 끝나면 호출되는 then()
getData().then(function (tableData) {
  //resolve()의 결과 값이 여기로 전달됨.
  console.log(tableData); //$.get() 의 response 값이 tableData에 전달됨.
});
```

콜백 함수로 처리하던 구조에서 , new Promise() , resolve() ,then() 와 같은 프로미스 API를 사용한 구조로 바뀌었다.

#### 프로미스의 3가지 상태

new Promise() 로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖는다.

Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태.
Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태.
(resolve)를 실행하면, '이행'상태가 된다.

Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태.

![image-20200222032510402](/Users/dong149/Library/Application Support/typora-user-images/image-20200222032510402.png)

#### 프로미스 코드 - 쉬운 예제

```javascript
function getData() {
  return new Promise(function (resolve, reject) {
    $.get("url 주소/products/1", function (response) {
      if (response) {
        resolve(response);
      }
      reject(new Error("Request is failed"));
    });
  });
}
//Fulfilled 또는 Rejected의 결과 값 출력
getData()
  .then(function (data) {
    console.log(data);
  })
  .catch(function (err) {
    console.log(err);
  });
```

#### 여러 개의 프로미스 연결하기

프로미스의 또 다른 특징은 여러 개의 프로미스를 연결하여 사용할 수 있다는 점입니다. 앞 예제에서 then() 메서드를 호출하고 나면 새로운 프로미스 객체가 반환됩니다. 따라서, 아래와 같이 코딩이 가능합니다.

```javascript
function getData() {
  return new Promise({});
}
getData()
  .then(function (data) {})
  .then(function () {}).then;
//
```

예제를 살펴봅시다.

```javascript
new Promise(function(resolve,reject){
  setTimeout(function(){
    resolve(1); //setTimeout() 를 이용해 2초 후에 resolve()를 호출하는 예제이다.
  },2000);
  .then(function(result){
    console.log(result); //1
    return result+10;
  })
  .then(function(result){
    console.log(result); //11
    return result+20;
  })
});
```

<u>가급적이면, catch()를 사용해서 에러처리를 하라.</u>

## async await

async 와 await 은 자바스크립트 비동기 처리 패턴 중 가장 최근에 나온 문법이다.

기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 도와준다.

#### async & await 맛보기

```javascript
function logName() {
  var user = fetchUser("domain.com/user");
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

위 함수에다가 async 와 await 를 추가해주면,

```javascript
async function logName() {
  var user = await fetchUser("domatin.com/user");
  if (user.id === 1) {
    console.log(user.name);
  }
}
```

#### async & await 기본 문법

```javascript
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

먼저 함수의 앞에 async라는 예약어를 붙이고, 함수의 내부 로직 중 HTTP 통신을 하는 비동기 처리 코드 앞에 await 를 붙입니다. 여기서 주의해야 할 점은 비동기 처리 메서드가 꼭 프로미스 객체를 반환해야 await 가 의도한 대로 동작한다라는 것입니다.

일반적으로 await 의 대상이 되는 비동기 처리 코드는 Axios 등 프로미스를 반환하는 API 호출 함수입니다.

#### async & await 실용 예제

async & await 문법이 가장 빛을 발하는 순간은 여러 개의 비동기 처리 코드를 다룰 때이다.

```javascript
function fetchUser() {
  var url = "https:";
  return fetch(url).then(function (response) {
    return response.json();
  });
}
function fetchTodo() {
  var url = "https:";
  return fetch(url).then(function (response) {
    return response.json();
  });
}
```

위 함수들을 실행하면 각각 사용자 정보와 할 일 정보가 담긴 프로미스 객체가 반환된다.

로직은

1. fetchUser()를 통해 사용자 정보 호출
2. 유저 아이디가 1이면 할 일 정보 호출
3. 할 일의 제목을 출력

```javascript
async function logTodoTitle() {
  var user = await fetchUser();
  if (user.id === 1) {
    var todo = await fetchTodo();
    console.log(todo.title);
  }
}
```

위 비동기 처리 코드를 만약 콜백이나 프로미스로 했다면 훨씬 더 코드가 길어졌을 것이고 가독성도 좋지 않았을 것이다.
이렇게되면 기존의 비동기 처리 코드 방식으로 사고하지 않아도 되는 장점이 생깁니다.

#### async await 예외처리

try catch 를 사용하면 된다.

```javascript
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title);
    }
  } catch (errror) {
    console.log(error);
  }
}
```

위의 코드를 실행하다가 발생한 네트워크 통신 오류뿐만 아니라, 간단한 타입 오류 등의 일반적인 오류까지도 catch로 잡아낼 수 있습니다.

## 비동기 함수

https://developers.google.com/web/fundamentals/primers/async-functions?hl=ko

비동기 함수는 Chrome 55에서 기본적으로 활성화되어 있는데, 솔직히 꽤 놀라운 함수입니다. 비동기 함수를 사용하면, 메인 스레드를 차단하지 않고도, 마치 동기 함수인 것처럼 프라미스 기반 코드를 작성할 수 있습니다. 이 함수를 통해 비동기 코드를 덜 '똑똑'하고더 읽기 쉽게 만들 수 있습니다.

```javascript
async function myFirstAsyncFunction() {
  try {
    const fulfilledValue = await promise;
  } catch (rejectValue) {}
}
```

함수 정의 앞에 async 키워드를 사용하면 함수 내에 await 를 사용할 수 있습니다.
프라미스를 await 할 때, 함수는 프라미스가 결정될 때까지 방해하지 않는 방식으로 일시 중지됩니다. 프라미스가 이행되면 값을 돌려받습니다. 프라미스가 거부되면 거부된 값이 반환됩니다.

예) URL을 가져와서 응답을 텍스트로 로그하려는 경우

1. 프라미스

```javascript
function logFetch(url) {
  return fetch(url)
    .then((response) => response.text())
    .then((text) => {
      console.log(text);
    })
    .catch((err) => {
      console.error("fetch failed", err);
    });
}
```

2. 비동기함수

```javascript
async function logFetch(url) {
  try {
    const response = await fetch(url);
    console.log(await response.text());
  } catch (err) {
    console.error("fetch failed", err);
  }
}
```

줄 개수는 같지만, 콜백이 전부 사라졌다. 따라서 특히 프라미스에 익숙하지 않은 사람으로서는 훨씬 읽기 쉬워진다.

#### 비동기 반환 값

await 사용 여부와는 상관없이 , 비동기 함수는 항상 프라미스를 반환한다. 해당 프라미스는 무엇이든 비동기 함수가 반환하는 것과 함께 해결되거나 비동기 함수가 발생시키는 것과 함께 거부된다.

```javascript
function wait(ms) {
  return new Promise((r) => setTimeout(r, ms));
}
async function hello() {
  await wait(500);
  return "world";
}
```

## setInterval( ) & setTimeout( )

+++

자바스크립트로 주기적인 작업을 실행하기 위해서 setInterval 과 setTimeout 메소드를 사용할 수 있다.

setInterval 함수 : 일정한 시간 간격으로 작업을 수행하기 위해 사용. 주의할 점은 일정한 시간 간격으로 실행되는 작업이 그 시간 간격보다 오래걸릴 경우 문제가 발생할 수 있다.

setTimeout 함수 : 일정한 시간 후에 작업을 한번 더 실행한다. 보통 재귀적 호출을 사용하여 작업을 반복한다. 기본적으로 setInterval 과 달리 지정된 시간을 기다린후 작업을 수행하고, 다시 일정한 시간을 기다린후 작업을 수행하는 방식입니다.

## parseInt( )

+++

문자열 인자의 구문을 분석해 특정 진수의 정수를 반환한다.

## Metadata

+++

데이터에 대한 데이터이다

어떤 목적을 가지고 만들어진 데이터.

대량의 정보 가운데 찾고 있는 정보를 효율적으로 찾아내서 이용하기 위해 일정한 규칙에 따라 콘텐츠에 대하여 부여되는 데이터이다. 어떤 데이터 즉, 구조화된 정보를 분석, 분류하고 부가적 정보를 추가하기 위해 그 데이터 뒤에 함께 따라가는 정보를 말한다.

속성정보라고도 한다.

메타데이터의 또 다른 목적은 데이터를 빨리 찾기 위한 것으로, 컴퓨터에서 정보의 인덱스 구실을 한다.

## navaigator 객체

+++

navigator 객체는 브라우저와 관련된 정보를 컨트롤 한다.

## MediaDevices 인터페이스

+++

카메라, 마이크, 공유 화면 등 현재 연결된 미디어 입력 장치로의 접근 방법을 제공하는 인터페이스이다. 즉, 미디어 데이터를 제공하는 모든 하드웨어로 접근할 수 있는 방법이다.

## Ajax

+++

Ajax 는 Javascript 의 라이브러리중 하나이며, Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자이다. 브라우저가 가지고있는 XMLHttpRequest 객체를 활용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드하는 기법.

한마디로 정의하자면, Javascript를 사용한 비동기 통신, 클라이언트와 서버간의 XML데이터를 주고받는 기술이라고도 할 수 있다.
