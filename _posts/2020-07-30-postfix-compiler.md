---
title: "🍵 후위증감연산자와 컴파일 최적화"
categories: java
tags: [ java ]
---

###  후위증감연산자와 컴파일러의 최적화 과정 이해하기

```java
int i = 7;

int j = i++;

System.out.println(i);	// 8
System.out.println(j);	// 7
```

위 코드를 이해하기 위해서는 컴파일러 과정에 대한 이해가 필요하다. 컴파일러는 컴파일 과정에서 원소스 코드를 똑같은 기능을 수행하는 다른 코드로 대체한다. 이것을 최적화 과정이라고 하기도 한다. 이때 이렇게 컴파일된 기계어 코드(Hello.class)를 원소스로 바꿀 때는 역 컴파일러를 사용하는데, 그렇게 생성된 코드는 최초의 소스코드 작성자가 작성한 코드와 동일하지 않다.

![postfix-compiler](https://user-images.githubusercontent.com/50407047/88869825-ce618100-d24e-11ea-9318-aa2f44aa71f3.jpg)



```java
int i = 5;
int j = i++;

System.out.printf("%d, %d", i, j); // 6, 5
```

위와 같이 결과가 나오는데, 이는 `int j = i++;`라는 문장이 어떤 식으로 컴파일 되는지를 파악하면 이해하기 쉽다.

```
int i = 5;
int j;
int temp;
temp = i;
i = i +1;
j = temp;
```

1. j 변수를 선언한다. 
2. 임시 메모리를 선언한다.
3. 임시 메모리에 **현재 i의 값**(5)을 넣는다. 
4. i 메모리에 현재 i의 값(5)에 1을 더한 값(6)을 넣는다. 
5. j에는 임시 메모리에 들어있는 값 (5)을 넣는다.

