---
title: "정규표현식: 전역 플래그(/g)와 test()"
categories: javascript
tags: [ javascript, regular-expression ]
toc: true
---

![image](https://user-images.githubusercontent.com/50407047/108622329-6a690900-747b-11eb-85c3-7f94194cd485.png)

오늘 자바스크립트 스터디 중 은버그님께서 질문(과제?!)을 주셔서 test 메서드와 g 플래그와의 관계에 대해 정리를 해보았다. 

`test()` 주어진 문자열이 정규 표현식을 만족하는지 판별하고, 그 여부를 `true` 또는 `false`로 반환한다고 한다.

그런데 왜 같은 정규표현식을 가지고 `test`를 수행했는데도 `true`와 `false`가 반복해서 나타나는 걸까? 상식적으로는 `true`가 계속 나와야 할 것 같은 데 말이다.

`test()`는 전역 탐색 플래그(`/g`)를 제공한 정규 표현식에서 여러 번 호출하면, **이전 일치 이후부터 탐색한다**. 이건 `exec()`도 마찬가지이고, `exec()`와 `test()`를 혼용해 사용하는 경우에도 해당한다고 한다. `test()` 메서드나 `exec()` 메서드는 는 정규 표현식의 `lastIndex`를 업데이트 하는 방식으로 구현되어 있다.  `test(str)`을 호출하면 `str` 검색을 `lastIndex`부터 계속 진행하고, `lastIndex` 속성은 매번 `test()`가 `true`를 반환할 때마다 증가하게 된다. 이때, `test()`가 `false`를 반환하면 `lastIndex` 속성이 0으로 초기화된다.

이제 다시 코드를 살펴보자.

```js
let re = /abc/g;

re.test("abc"); // true
re.lastIndex; // 3

re.test("abc");
re.lastIndex; // 0

re.test("abc"); // true
re.lastIndex; // 3

re.test("abc");
re.lastIndex; // 0
```

- 맨 처음에 `test` 메서드를 호출하면,  `"abc"` 이후부터 탐색하기 위해 `test` 메서드는 `lastIndex`에 3을 저장한다. 
- 또 `test()`를 호출하면 문자열의 3번 인덱스부터 일치 여부를 살펴볼 것이다. 그러나 인덱스 3은 존재하지 않는다. `test()`은 정규 표현식을 만족하지 않았기 때문에 `false`를 리턴하고, `lastIndex`값을 0으로 초기화한다. 
- 또 `test()`를 호출하면 문자열의 0번 인덱스부터 일치 여부를 살펴볼 것이다. `"abc"`가 일치하기 때문에 일치 이후 인덱스인 `3`을 `lastIndex`에 저장하고 `true`를 반환한다.



