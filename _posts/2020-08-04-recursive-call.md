---
title: ":coffee: Recursive Call(재귀호출), Stack Overflow"
categories: java
tags: [ java ]
toc: true
---

##  recursive call (재귀호출)

```java
int result = 1;
int n = 5;

public static void main(String[] args){
    for (int i = 2; i <= 5; i++) {
    	result *= i;
	}
	System.out.println(result);
    result = factorial(5);
    System.out.println(result);
}

static int factorial(int n) {
    if (n == 1)
        return 1;
    return n * factorial(n-1);
}
```

재귀호출은 **수학식**을 코드로 간결하게 표현할 수 있다. 그러나 **반복문**을 사용하는 경우보다 메모리를 많이 사용한다. 재귀호출을 할 경우 다른 메서드를 호출하는 것처럼, **스택에 해당 메서드가 사용할 변수가 생기기** 때문이다. 특히 **멈춰야 할 조건을 빠뜨리면 스택 메모리가 극한으로 증가해서 메모리가 부족해 stack overflow**가 발생할 수 있다. 큰 수(메서드가 많이 호출되는 경우)에 대해서 재귀호출을 할 때 이 스택 오버플로우가 자주 발생한다. 따라서 **메서드 호출이 너무 깊게 들어가지 않는 상황에서 재귀호출을 사용하여야 한다.** 

그림으로 표현하면 다음과 같다.

![recursive-call](https://user-images.githubusercontent.com/50407047/89728526-d6000180-da68-11ea-9ddb-187dec6f208f.jpg)



## Stack Overflow

스택 오버플로우는 JVM 스택 메모리가 꽉 차서 더 이상 메서드 실행을 위해 로컬 변수를 만들 수 없는 상태이다. 

```java
static int sum(int value) {
    if (value == 1)
        return 1;
    
    return value + sum(value - 1);
}

public static void main(String[] args){
    System.out.println(sum(1000000));
}
```

위의 코드는 재귀호출의 수가 높아져 스택 메모리가 부족해진 경우이다. 호출 단계가 깊지 않은 작은 수를 다룰 때는 재귀호출을 사용해도 되지만, 호출 단계가 많은 큰 수를 다룰 경우에는 재귀호출 대신 반복문을 사용하자.

반복문을 사용하면 다음과 같은 코드가 나온다.

```java
long sum = 0;
for (int i = 1; i <= 1000000; i++){
    sum += i;
}
System.out.println(sum);
```

주의할 점은 스택 오버플로우가 메서드 호출 회수에 영향을 받는 것이 아닌 **메서드에서 생성하는 로컬 변수의 크기**에 영향을 받는다는 것이다. 

```java
static int sum(int value) {
	if (value == 1)
    	return 1;

	long a1 = 100, a2 = 100, a3 = 100, a4 = 100, a5 = 100, a6 = 100, a7 = 100, a8 = 100, a9 = 100, long a10 = 100;
    a11 = 100, a12 = 100, a13 = 100, a14 = 100, a15 = 100, a16 = 100, a17 = 100, a18 = 100, a19 = 100, long a20 = 100;
    a21 = 100, a22 = 100, a23 = 100, a24 = 100, a25 = 100, a26 = 100, a27 = 100, a28 = 100, a29 = 100, a30 = 100;
	return value + sum(value - 1);
}

public static void main(String[] args) {
	System.out.println(sum(10000));
}
```

위의 코드에서 sum 메서드는 10000번 호출되는데, 그 때마다 8바이트인 long 타입 변수를 30개를 생성하고 그 변수들은 스택에 계속 쌓이고 결국 메모리가 부족해 stack overflow가 발생했다.

