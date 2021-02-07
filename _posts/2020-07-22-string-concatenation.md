---
title: "🍵 문자열과 다른 데이터타입의 값을 연결하기"
categories: java
tags: [ java ]
toc: true
---

### 개요
기본적으로 문자열은 문자열 연결 연산자(concatenation)인 `+`를 사용하여 연결할 수 있다. 문자열은 문자열끼리만이 아니라 다른 종류의 값도 연결할 수 있는데, 그럴 경우 연결되기 전 다른 종류의 값이 먼저 문자열로 바뀐 후 연결된다.

### 예시
0. 문자열 + **문자열**
```java
System.out.println("Hello, " + "world!");
//"Hello, world!"
```

1. 문자열 + **논리 값**
```java
System.out.println("졸린가?: " + true);
//"졸린가?: true"
```

2. 문자열 + **문자**
```java
System.out.println("성적: " + 'A');
//"성적: A"
```

3. 문자열 + **부동소수점**
```java
System.out.println("키: " + 164.9f);
//"키: 165.9"
```
이때 부동소수점 접미사 `f`(4비트 메모리에 저장하라는 접미사)는 새로 만들어진 문자열에 포함되지 않는다!
