---
title: "React 자습서 따라하기: #1 React 시작하기"
categories: react
tags: [ react, javascript ]
toc: true
---

### React란 무엇인가요?

- React는 **사용자 인터페이스를 구축**하기 위한 **선언적**이고 **효율적**이며 **유연한** JavaScript **라이브러리**이다. 
- "**컴포넌트**"라고 불리는 작고 고립된 **코드의 파편**을 이용하여 **복잡한 UI를 구성**하도록 돕는다.

```react
class ShoppingList extends React.Component {
  render() {
    return (
    	<div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WahtsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// 사용예제: <ShoppingList name="Mark"/>
```

- 개발자는 컴포넌트를 사용해 React에게 화면에 표현하고 싶은 것이 무엇인지 알려준다. 
- **데이터가 변경**될 때 React는 **컴포넌트를 효율적으로 업데이트**하고 다시 **렌더링**한다. 
- 여기서 ShoppingList는 React 컴포넌트 클래스 또는 React 컴포넌트 타입이다. 
- 개별 컴포는트는 `props`라는 **매개변수**를 받아오고 `render` 함수를 통해 **표시할 뷰 계층 구조를 반환**한다.
- React는 설명을 전달받고 결과를 표시한다. 특히 `render`는 렌더링할 내용을 경량화한 **React 엘리먼트를 반환**한다. 
- **JSX**라는 특수한 문법을 사용하여 React 구조를 쉽게 작성한다.
- `<div />` 구문은 빌드하는 시점에서 `React.createElement('div')`로 변환된다.

```js
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

- JSX는 내부의 중괄호 안에 **어떤 JS 표현식도 사용할 수 있다.**
- React 엘리먼트는 **JavaScript 객체**이며 변수에 저장하거나 프로그램 여기저기에 전달할 수 있다.
- `ShoppingList` 컴포넌트는 `<div />`와 `<li />` 같은 **내각 DOM 컴포넌트만을 렌더링**하지만 컴포넌트를 조합하여 커스텀 React 컴포넌트를 렌더링하는 것도 가능하다. 예를 들어 `<ShoppingList />` 를 작성하여 모든 쇼핑 목록을 참조할 수 있다. React 컴포넌트는 **캡슐화되어 독립적으로 동작**할 수 있다. 이렇게 **단순한 컴포넌트를 사용하여 복잡한 UI를 구현**할 수 있다.

### Props를 통해 데이터 전달하기

`Square`에 `value` prop을 전달하기 위해 Board의 `renderSquare` 함수 코드를 수정한다. 

```react
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
}
```

값을 표시하기 위해 `Square`의 `render` 함수에 `{this.props.value}`를 넣는다.

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

부모 Board 컴포넌트에서 자식 Square 컴포넌트로 `prop`을 이렇게 전달할 수 있따. **`prop` 전달하기**는 React 앱에서 **부모에서 자식으로 정보가 어떻게 흘러가는지** 알려준다.

**결과**

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.state.value}</button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />
  }
  
  render() {
    const status = 'Next player: X';
    
    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        
      </div>
    );
  }
}
```

