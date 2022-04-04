# react-study (리액트를 다루는 기술 1~2장)

### **리액트 개요 :**

기존 프레임 워크들은 MVC 혹은 MVVM의 아키텍처를 사용 이들의 공통점은 애플리케이션에 사용하는 데이터를 관리하는 모델과 사용자에게 보여지는 뷰를 가지고 있다는 것이다. 
 보통 사용자는 업데이트 내용을 생각하고 변화를 주게된다. 애플리케이션은 이에 따라 모델을 조회하여 수정하고 이를 뷰로 보여준다.(렌더링한다.)
하지만 애플리케이션의 규모가 커질수록 이와 같은 방법은 어떤 변화를 줄지 고민해야 하고 복잡해진다. 리액트는 이런 고민을 해결하기 위해 **기존 뷰를 아예 날려버리고 처음부터 새로 렌더링**하는 방식이다. 변경될 내용을 생각하는게 아니라 **그냥 어떻게 뷰로 보여질지만 생각하면 되는것이다.** 
하지만 이 또한 DOM의 일이 증가하게 되는것이고 메모리도 많이 사용하게 되는데 이런 단점을 극복하면서 최대의 성능을 내고자 만든것이 바로 리액트이다. 그 방법은 밑에서 소개한다.
(오직 view만 신경쓰는 라이브러리!)

### 컴포넌트 :

- 리액트에서 특정부분이 어떻게 생길지 정하는 선언체
- 컴포넌트 하나에서 해당 컴포넌트의 생김새와 작동방식을 정의한다.
- 컴포넌트는 재사용이 가능한 API로 수많은 기능을 내장하고 있다.

## 1. 리액트의 작동방식을 알기 위한 기본 개념!

### 리액트 컴포넌트의 최초실행 - **초기 렌더링**

: 어떤 UI관련 프레임워크, 라이브러리든 맨 처음 보여줄 초기 렌더링이 필요한데 리액트에서는 이를 위해 render함수라는게 있다. 

render(){}

이 함수는 컴포넌트가 어떻게 생겼는지를 정의한다. 뷰가 어떻게 생겼고 어떻게 작동하는지에 대한 정보를 지닌 객체를 반환하게된다. 

컴포넌트 내부에는 또 다른 컴포넌트가 들어갈 수 있게되고 이때 render함수를 실행하면 재귀적으로 내부 컴포넌트까지 모두 렌더링하게 된다. 즉 다시 최상위 컴포넌트로 돌아와 렌더링 작업이 끝나면
지니고 있는 정보를 통해 HTML을 만들고 이를 우리가 정하는 실제 페이지에 DOM요소에 주입한다.

### 컴포넌트의 데이터 변경으로 다시 실행되는 - **리렌더링**

 리액트는 업데이트 한다기 보다 조화과정을 거친다라고 표현한다. 컴포넌트에서 데이터의 변화가 있을때 뷰가 변형되는 것처럼 보이지만 실제론 새로운 요소로 갈아끼우기 때문이다.
 이 작업또한 render함수가 하게 되는데 새로운 데이터로 다시 render함수를 호출하면 새로운 결과를 바로 DOM에 반영하지 않고 이전에 render 함수가 만들었던 컴포넌트 정보와 현재 render함수가 만든 컴포넌트 정보를 비교합니다. 그리곤 자바스크립트를 통해 둘의 차이를 최소한의 연산으로 찾아내 dom트리를 업데이트한다. 즉 결국 최소한으로 바뀐 부분만 변경해서 virtual dom을 반영한다. 

## 2. **virtual Dom**: 딱한번의 리렌더링, 메모리에서 변경 내용 찾아 반영 딱 그 부분만 수정하므로 속도가 빠르고 실제 dom부담 완하

- Dom이란?

Document Object Model의 약어로 객체로 문서구조를 표현하는 방법으로 XML이나 HTML을 사용하여 작성

![https://miro.medium.com/max/1088/1*NA2VKR09ECb8PEgYDteR3w.gif](https://miro.medium.com/max/1088/1*NA2VKR09ECb8PEgYDteR3w.gif)

- virtual Dom을 사용하여 view를 변경하는 과정
1. 자바스크립트는 기존의 렌더링 되어있던  Dom을 복사하여 하나의 가상 dom을 만들어낸다.
(자바스크립트의 객체형태)  
2. 데이터를 업데이트하면 전체 UI를 virtual dom에 리렌더링 한다.
3. 이전 virtual dom에 있던 내용과 현재 내용(현재 virtual dom)을 비교한다.
4. 바뀐 부분만 실제 dom에 적용한다.

![https://codingmedic.files.wordpress.com/2020/11/virtualdom.png?w=1024](https://codingmedic.files.wordpress.com/2020/11/virtualdom.png?w=1024)

- virtual Dom 작동메커니즘

```jsx
render함수{

최상위컴포넌트-하위컴포넌트-(하위의하위컴포넌트)-하위컴포넌트 .....들

}

렌더링하고 정보를 얻어내면그 정보들을 통해 업데이트 부분만 HTML만들고 실제
 dom요소에 투입
```

**>>>조금 더 구체적으로!!**

**렌더링**- 새로 데이터만듬 - 뷰(virtual dom)생성 - 분석(전과 차이비교 수정부분만 찾음) -수정부분만 HTML만들어 수정 - 실제 dom에 반영(모든 수정부분을 단 한번의 렌더링으로 업데이트!)
**이로인해 dom 전체를 다시 리렌더링하지 않고 수정부분만 리렌더링 할 수 있다.**

**>>>기존 dom api를 사용하면 여러번  리렌더링 했어야 되는 문제를 이제 개선할 수 있다.**

document.getElementById(”li1”) = “kim”  //리렌더링
document.getElementById(”li2”) = “lee”  //리렌더링
document.getElementById(”li3”) = “kii”  //리렌더링 총 3번 리렌더링

### @ 참조 DIffing:

virtual dom이 업데이트되면 리액트는 업데이트 이전의 virtual dom 스냅샷과 비교하여 정확히 어떤 virtual dom 요소가 바뀌었는지 검사한다. 이 과정을 말한다. 

## 3. 리액트 사용을 위한 환경

참조: create-react-app : 리액트 프로젝트를 생성할 때 필요한 웹팩, 바벨의 설치 및 설정 과정을 생략하고 바로 간편하게 프로젝트 작업 환경을 구축해 주는 도구입니다.

원래 브라우저에 없던 파일(모듈)을 묶듯이 연결해줄 수 있는 **번들러**

모듈: 함수나 변수, 클래스등을 모아놓은 파일

**대표적인 번들러 : 웹팩**, parcel, browserify

- 웹팩구조
1. 웹팩 로더(파일들을 불러오는 역할)
2.  웹팩의 file loader(웹 폰트나 미디어 파일 등을 불러오는 역할)
3.  **웹팩의 바벨로더**(자바스크립트 파일들의 최신 문법으로 작성된 코드를 es5문법으로 변환)

- **바벨로더를 통해 코드를 변환하는 이유**: 구버전의 웹브라우저와의 호환을 위함과 컴포넌트의 jsx또한 자바스크립트 정식 문법이 아니기 때문에 es5형태의 코드로 변환해 주어야 한다.

```jsx
function Welcome(props) { return <h1>Hello, {props.name}</h1>; }
```

이러한 것을 함수 컴포넌트라고 한다.

렌더링 == 함수에서 반환하고 있는 내용!!

# 2. JSX  :실제는 자바스크립트의 객체!

: 자바스크립트로 일일이 만들기 불편함을 개선한 가독성좋은 문법, 형태가 html이나 xml과 비슷하며 html문법을 그대로 따르며 컴포넌트도 jsx안에서 작성될 수 있다. 

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "뤼액트";
  return (
    <>
      <h1>{name === "리액트" ? "안녕" : <h2>잘 작동하니?</h2>}리액트 안녕!</h1>
      <h1>잘 작동하니?</h1>
    </>
  );
}

export default App;
```

- JSX : 자바스크립트의 확장 문법 XML과 매우 비슷하게 생겼으며 브라우저 실행전 코드가 번들링 되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환되게 된다.
- import : 특정 파일을 불러오는 것을 의미,  원래 브라우저에는 없는 노드js에서 사용할 수 있는 기능. 이 기능을 브라우저에서도 사용하기 위해 **번들러(웹팩)**라는 것을 사용한다.

>단순히 다른 경로에 있는 자바스크립트를 불러오는 용도의 브라우저 기본 import와는 다르다!

- import와 export의 개념

참조 : [https://pooney.tistory.com/85](https://pooney.tistory.com/85)

참조 : [https://codingmania.tistory.com/333](https://codingmania.tistory.com/333)

- ReactDom.render(렌더링할 내용JSX형태작성**,** 렌더링 될 document요소설정):
 컴포넌트를 페이지에 렌더링 하는 역할을 하며 모듈을 불러와 사용할 수 있다. index.js에 존재

## 2.1 감싸인 요소

: 컴포넌트에 여러 요소가 있다면 반드시 부모 요소하나로 감싸줘야 합니다.
<div>,<Fragment>이 사용가능하며 이때 <Fragment>는 내부를 생략하고 <></>이렇게 표현할 수 있다. 

```jsx
function App() {
  const name = "뤼액트";
  return (
    <> 
      <h1>{name === "리액트" ? "안녕" : <h2>잘 작동하니?</h2>}리액트 안녕!</h1>
      <h1>잘 작동하니?</h1>
    </>
  );
}
```

Fragment생략안하고 작성시에는 꼭 import넣어줘야함

```jsx
import logo from "./logo.svg";
import "./App.css";
import {Fragment} from 'react';

function App() {
  const name = "뤼액트";
  return (
    <Fragment>
      <h1>{name === "리액트" ? "안녕" : <h2>잘 작동하니?</h2>}리액트 안녕!</h1>
      <h1>잘 작동하니?</h1>
    </Fragment>
  );
}

export default App;
```

## 2.2 JSX내부 자바스크립트 표현

{}안에 자바스크립트의 표현식을 삽입하여 표현할 수 있다.

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "리액트";
  return (
    <>
      <h1>{name} 안녕!</h1>
      <h1>잘 작동하니?</h1>
    </>
  );
}

export default App;
//결과: 리액트 안녕!
//      잘 작동하니?
```

## 2.3 if문 대신 조건부 연산자

jsx내부에서는 if문의 사용이 불가하다 jsx밖에서 사용하던지 {}안에 표현식으로 삼항 연산자를 사용하면 가능하다. 

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "뤼액트";
  return (
    <>
      <h1>{name === "리액트" ? "안녕" : <h2>리액트입니다.</h2>}리액트가 아닙니다.</h1>
      <h1>잘 작동하니?</h1>
    </>
  );
}

export default App;
//결과: 리액트가 아닙니다.
```

## 2.4 AND 연산자(&&)사용 조건부 렌더링

단축평가를 사용하면 간다하게 표현가능
리액트에서 false를 렌더링 할 때는 아무것도 나타나지 않는다.
**다만 falsy한 값 0은 0이 출력된다.**

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "뤼액트";
  return <div>{name === "리액트" && <h1>리액트입니다.</h1>}</div>;
}

export default App;
//결과 : 아무것도 나오지 않음
```

## 2.5 undefined 렌더링하지 않기

리액트 컴포넌트에서는 함수에서 undefined만 반환한다면 요류를 발생시킨다. 어떤 값이 undefined라면 ||(OR연산자)를 사용해 표현할 수 있다.

**반면 JSX내부에서는 undefined 렌더링 가능!**

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = undefined;
  return name || '값이 undefined입니다.'  

//return <div>name</div> 이건 가능
//return <div>{name ||'리액트'}</div> 이렇게 표현도 가능
}

export default App;
//결과 : 값이 undefined입니다.
```

## 2.5 inline 스타일링

리액트에서 dom요소에 스타일을 적용해 줄때는 문자열 형태가 아닌 객체의 형태로 넣어주어야 하며 background-color와 같이 - 는 -를 제거하고 카멜 표기법으로 작성해주어야 한다.

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = '리액트'
  const style = {
    backgroundColor: 'black',
    color: 'red',
    fontSize:'48px',
    fontWeight:'bold'  //font-weight가아님
  };
  return <div style = {style}>name</div>;
 
}

export default App;
```

아래와 같이 바로 반환같으로 적용하는 것도 가능하다.

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "리액트";
  return (
    <div
      style={{
        backgroundColor: "black",
        color: "red",
        fontSize: "48px",
        fontWeight: "bold",
      }}
    >
      name
    </div>
  );
}

export default App;
```

## 2.6 css는 class 대신 className을 사용한다.

```jsx
.react {

  backgroundColor: "black";
  color: "red";
  fontSize: "48px";
  fontWeight: "bold";

}

//app.js에서는 <div className = 'react'></div>로 가져온다.
```

## 2.6 주석

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  const name = "리액트";
  return (
    <>
      {/* jsx내부에 주석은 이렇게 작성합니다. 
       여러줄도 가능합니다.*/}

       <div className='react' 
				//시작태그를 여러줄로 작성할 때는 이런형태도 가능합니다.
       >
         {name} 

       </div>

       //이런 주석은 안됩니다.
       /*이런 주석은 안됩니다.*/
    </>
  );
}

export default App;
```
