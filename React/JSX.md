# JSX

```react
import React from 'react';
```

* node_modules 디렉터리의 react 모듈을 import를 통해 불러와서 사용할 수 있게한다.
* 모듈을 불러와서 사용하는 것은 원래 브라우저(2017이전)에는 없던 기능.(Node.js에서 지원하는 기능(require))
* 지금도 브라우저는단순 js만 불러오는 용도. 프로젝트 번들링은 불가능.
* 이러한 기능을 브라우저에서 사용하기위해 `bundler`를 사용(코드를 묶는다)
* Webpack, Parcel, browserify...
* react는 편의성과 확장성이 뛰어난 Webpack을 주로 사용.
* 번들러는 import로 모듈을 불러왔을 때 불러온 모듈을 모두 합쳐 하나의 파일을 생성해준다.
* 또 최적화 과정에서 여러개로 분리될 수도 있다.
* src/index.js를 시작으로 필요한 파일을 다 불러와 번들링한다.

* 웹팩의 loader기능은 svg파일이나 css파일도 불러오게 해줌.(css-loader,file-loader,bebel-loader)
* 바벨은 최신 js문법들을 ES5로 번역해 구버전 웹 브라우저와 호환시킨다.
* JSX문법도 마찬가지다.

```react
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;

```

함수형 컴포넌트.



## JSX란?

* jsx는 xml과 비슷하게 생긴 js의 확장문법이다.
* 브라우저에서 실행되기 전에 코드 번들링 과정에서 바벨이 일반 js코드로 변환시킨다.
* jsx코드를 작성하지 않는다면 매번 React.createElement함수로 html을 그려줘야한다.
* jsx를 사용하면 매우 편하게 UI를 렌더링 할 수 있다.

## JSX의 장점

1. 보기 쉽고 익숙하다.
   * html코드를 작성하는 것과 비슷하기 때문에 가독성이 높고 작성하기 쉽다.
2. 활용도가 더 높다.
   * JSX에서는 컴포넌트도 마치 html태그처럼 작성해 삽입시킬 수 있다.

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

* ReactDOM.render : 컴포넌트를 페이지에 렌더링하는 역할. react-dom 모듈을 불러와 사용, 첫번째 인자는 렌더링할 내용의JSX, 두번째 인자는 렌더링할 document 내부 요소.
* React.StrictMode 는 리액트의 레거시 기능들을 사용하지 못하게 하는 기능. red, componentWillMount 등 나중에 사라지게 될 옛날 기능을 사용했을 때 경고를 출력함.



## JSX 문법.

1. 감싸인 요소(Fragment)

   * Virtual DOM에서 컴포넌트 변화를 감지할 떄 효율적으로 비교 하기 위해 컴포넌트는 하나의 DOM tree로 이루어져야 한다는 규칙.

2. JS 표현

   * {}로 감싼 JS표현식. 변수등 넣을 수 있다.

     >  ES6의 const와 let, var
     >
     > const는 한번 지정하면 변경할 수 없는 상수를 선언.
     >
     > let은 동적인 값을 담을 수 있는 변수를 선언
     >
     > var는 ES6이전에 쓰였으나 scope가 함수단위기 때문에 변수값이 자꾸 바뀌는 단점이 있었음.
     >
     > let과 const는 scope가 함수단위가 아닌 블록단위기 때문에 if문 내부의 변경은 외부값을 변경시키지 않음.
     >
     > 하지만 같은 블록에서 중복 선언이 불가능함. 
     >
     > 기본적으로var는 ES6이후로 쓰지않고 const를 쓰되 값이 바뀌는 변수 (for문 등)는 let을 유동적으로 사용.

3. IF문 대신 조건부 연산자.

   * if문이 아닌 3항 연산자는 JSX표현식에서 쓸수있음.

   * ```react 
     {name==='리액트' ? (<h1>리액트입니다</h1>) : (<h2>리액트가 아닙니다.</h2>)}
     ```

4. AND 연산자(&&)를 사용한 조건부 렌더링

   * ```react
     {name==='리액트' ? && <h1>리액트입니다</h1>}
     ```

   * 이게 가능한 이유는 리액트는 false를 렌더링 할때 null값으로 아무것도 나타나지 않기 때문. 하지만 0은 나타나므로 조심.

5. undefined를 렌더링 하지 않음.

   * ```react
     name || 'error'
     ```

   * or연산자를 사용 예외처리 가능.

   * JSX내부는 가능한데 브라우저는 렌더링 x

6. 인라인 스타일링.

   * 속성명이 케밥케이스에서 카멜케이스로 바뀜. 이하 동일

7. class 대신 className 속성.

8. input, br은 닫아줘야함

   * <input />

9. 주석

   * {/* 일케 작성 */}
   * 이렇게 안하면 렌더링됨..

## 결론

* XML형식이지만 실제로는 JS 객체라는 사실.