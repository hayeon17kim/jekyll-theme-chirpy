---
title: "[JS] 생활코딩 #29: 참조"
categories: JavaScript
tags: [ JavaScript ]
---

# 참조

## 복제

```js
var a = 1;
var b = a; // 변수 b 값에 변수 a의 값 복제
b = 2;
console.log(a); // 1
```



## 참조

```js
var a = {'id':1};
var b = a; // 변수 b와 변수 a에 담긴 객체는 서로 같다.
b.id = 2;
console.log(a.id); // 2
```

- 복제는 파일을 복사하는 것이라면, 참조는 심볼릭 링크(바로가기)를 만드는 것과 비슷하다. 심볼릭 링크로 접근하면 컴퓨터는 심볼릭 링크에 저장된 원본의 주소를 참조해서 원본의 위치를 알아내고 원본에 대한 작업을 한다. 즉 원본을 복제한 것이 아니라 원본 파일을 참조하고 있는 것이다.
- 덕분에 저장장치의 용량을 절약할 수 있고, 원본 파일을 사용하고 있는 모든 복제본이 동일한 내용을 유지할 수 있게 된다. 
- 라이브로리도 일종의 참조이다. 라이브러리의 내용이 변경되면 이를 참조하고 있는 애플리케이션에도 내용이 반영되게 된다.

## 메소드의 매개변수 동작

- 원시 데이터 타입을 인자로 넘겼을 때

````js
var a = 1;
function func(b) {
    b = 2;
}
func(a);
console.log(a); //1
````

- 참조 데이터 타입을 인자로 넘겼을 때: (1) 객체 변경

```java
var a = {'id': 1};
function func(b) {
    b = {'id': 2}; //새로운 객체로 대체
}
console.log(a.id); //1
```

- 참조 데이터 타입을 인자로 넘겼을 때: (2) 속성 변경
  - 속성이 소속된 객체를 대상으로 수정작업을 한 것이 된다.
  - b의 변경은 a도 영향을 미친다.

```js
var a = {'id':1};
function func(b) {
    b.id = 2; // 속성 변경
}
func(a);
console.log(a.id); //2
```
