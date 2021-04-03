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

### 사용자와 상호작용하는 컴포넌트 만들기

Square 컴포넌트를 클릭하면 "X"가 체크되도록 만든다. 

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={funciton() {
          alert('click'); }}>
        {this.props.value}
      </button>
    )
  }
}
```

타이핑 횟수를 줄이고 this의 혼란스러운 동작을 피하기 위해서 이벤트 핸들러에 화살표 함수를 사용해보도록 하자.

```react
<button class="square" onClick={() =>
                               alert('click')}>
  {this.props.value}
</button>
```

> this의 혼란스러운 동작?
>
> : 화살표 함수로 하면 전역 this이기 때문에 오작동을 막는다. 

```react
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value = null;
    };
  }
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

- JavaScript에서 하위 클래스의 생성자를 정의할 때 항상 super를 호출해야 한다. 모든 React 컴포넌트 클래스는 생성자를 가질 때 super(props) 호출 구문부터 작성해야 한다.

이제 Square를 클릭할 때 현재 state 값을 표시하기 위해 다음과 같이 render 함수를 변경한다.

- `<button>` 태그 안  `this.props.value` 를 `this.state.value`로 변경
- `onClick={...}` 이벤트 핸들러를   `onClick={() => this.setState({value:'X'})}`로 변경한다.
- 가독성을 높이기 위해 ClassName 과 `onClick` props를 별도의 줄에 넣는다.

```react
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value = null;
    };
  }
  render() {
    return (
      <button 
        className="square" 
        onClick={() => this.setState({value: 'X'})}>
        {this.state.value}
      </button>
    );
  }
}
```

- onClick  이벤트 핸들러를 통해 this.setState를 호출하는 것으로 React에게 `<button>`을 클릭할 때 Square가 다시 렌더링해야 한다고 알릴 수 있다.
- 업데이트 이후의 Square의  `this.state.value`는 'X' 가 되어 게임판에서X가 나타난다.
- 컴포넌트에서 setState를 호출하면 React는 자동으로 컴포넌트 내부의 자식 컴포넌트 역시 업데이트한다. ㅇㄹㅇㄹㅇㄹㅇ기ㅏ얼 안녕하세요
- 

### State 끌어올리기

- 현재 게임의 state는 Square 컴포넌트에서 유지하고 있다. 
- 승자를 확인하기 위해 9개의 사각형의 값을 한 곳에 유지해야 한다.
- Board가 각 Square에 각 Square의 state를 요청하는 방식은 가능하긴 하지만 코드를 이해하기 어렵고, 버그에 취약하며, 리팩토링이 어렵다.
- 각 Square가 아닌 부모 Board 컴포넌트에 게임의 상태를 저장하는 것이 가장 좋은 방법이다. 각 Square에 숫자를 넘겨 주었을 때처럼 Board 컴포넌트는 각 Square에게 prop을 전달하는 것으로 무엇을 표시할 지 알려준다.
- **여러 개의 자식으로부터 데이터를 모으**거나 두 개의 **자식 컴포넌트들이 서로 통신**하게 하려면 **부모 컴포넌트에 공유 state를 정의**해야 한다. **부모 컴포넌트는 props를 사용하여 자식 컴포넌트에 state를 다시 전달**할 수 있다. 이것은 **자식 컴포넌트들이 서로 또는 부모 컴포넌트와 동기화하도록 만든다**.  
- state를 부모 컴포넌트로 끌어올리는 것은 React 컴포넌트를 리팩토링할 때 흔히 사용한다.
- Board에 생성자를 추가하고 9개의 사각형에 해당하는 9개의 null 배열을 초기 state로 설정한다.

```react
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }
  
  renderSquare(i) {
    return <Square value={i} />
  }
}
```

- 처음에는 Square에서 0부터 8까지의 숫자를 보여주기 위해 Board에서 value prop을 자식으로 전달했다. 또 다른 이전 단계에서는 숫자를 Square의 자체 state에 따라 "X" 표시로 바꾸었다. 그렇기 때문에 현재 Square는 Board에서 전달한 value prop을 무시하고 있다.
- 이제 prop을 전달하는 방법을 다시 사용할 것이다. 각 Square에게 현재 값 ('X', '0' 또는 null)을 표현하도록 renderSquare 함수를 수정한다.

```react
renderSquare(i) {
  return <Square value={this.state.squares[i]}/>
}
```

- 이제 Square는 빈 사각형에 'X', 'O', null인 value prop을 받는다.
- Board컴포넌트는 어떤 사각형이 채워졌는지 여부를 관리하기 때문에 Square가 Board를 변경할 방법이 필요하다. 컴포넌트는 자신이 정의한 state에만 접근할 수 있으므로 Square에서 Board의 state를 직접 변경할 수 없다.
- 대신 Board에서 Square로 함수를 전달하고 Square는 사각형을 클릭할 때 함수를 호출할 것이다. 

```react
renderSquare(i) {
  return (
    <Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />
  )
}
```



일반적인 함수와 화살표함수의 this

- undefined는 값이 정해지지 않았다는 뜻으로 디폴트 값이 정해졌다면 그 값을 따른다. 
- undefined가 되어 있다면 define을 해야 하기 때문에 