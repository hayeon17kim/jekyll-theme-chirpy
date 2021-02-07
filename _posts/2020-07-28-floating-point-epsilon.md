---
title: "🍵 부동소수점의 쓰레기값 처리하기"
categories: java
tags: [ java, linux ]
---

## 부동소수점의 극소수값

실제로 부동소수점(실수) 연산을 다루는 것은 쉽지만, 컴퓨터의 부동소수점 연산 처리는 쉽지만은 않다. 컴퓨터에서 값은 2진수로 다루고, 이 때문에 컴퓨터가 부동소수점 값을 연산할 때 IEEE 754 명세에 따라 작업을 수행한다. 이 과정에 값의 왜곡이 발생할 수 있다. 이 문제는 CPU나 OS, JVM의 문제가 아니라 명세에 따라 부동소수점을 처리하는 모든 컴퓨터에서 발생하는 문제이다.  

따라서 부동소수점은 비교연산자를 사용하여 비교할 때, 부동소수점의 값을 비교할 때 IEEE 754 변환 공식에 따라 발생하는 이러한 문제를 해결하기 위해서는 극소수값을 처리해주는 코드가 따로 필요하다. 비교연산을 할 때, 피연산자의 오차가 오차범위보다 작다면 같은 값으로 취급해주는 것이 그 방법이다.



## 극소수값 처리 방법 

**@직접 오차범위 지정**

```java
double d1 = 987.6543;
double d2 = 1.111111;
System.out.println(d1 + d2 == 988.765411);	// false
System.out.println(d1 + d2);				// 988.7654110000001
// 결과 뒤에 극소수의 값이 붙는다.

double x = 234.765411;
double y = 754.0;
System.out.println(x + y == 988.765411);	// true
System.out.println(x + y);					// 988.765411
// 결과 뒤에 극소수의 값이 붙지 않았다.

System.out.println(d1 + d2 == x + y); 		// false

//-------------------------------------------
double EPSILON = 0.00001;
System.out.println(Math.abs(d1 + d2 - (x + y)) < EPSILON);		//true
```

위의 코드에서 d1 + d2에는 극소수의 값이 붙고,  x + y에는 뒤에 극소수의 값이 붙지 않았다. 따라서 같은 값이 나와야 함에도 불구하고 극소수의 값 때문에 비교연산자를 사용했을 때 false가 나오게 되었다.  

아래의 두 줄을 설명하자면

1. EPSILON  값을 선언한다. 
2. 각각 연산에서 계산한 값의 오차를 구한다.
3. `Math.abs()` 메소드를 사용하여 오차에 절대값을 구한다. 
4. 이 오차의 절대값이 EPSILON보다 작다면 두 결과값을 같은 값으로 취급한다.

> 절대값으로 바꿔주는 이유
>
> => 음수의 경우 무조건 EPSILON 값보다 작다. 따라서 절대값을 쓰지 않는다면 음수의 경우 -EPSILON 보다 작아야 한다는 조건을 중복해서 코딩해야 함 

**@ INFINITY 값 사용**

```java
float f1 = 0.1f;
float f2 = 0.1f;

System.out.println(f1 * f2 == 0.01f); // false
System.out.println(f1 * f2); // 0.010000001

float r = f1 * f2 - 0.01f;
System.out.println(Math.abs(r) <= Float.POSITIVE_INFINITY);
```

위의 코드에서는 직접 오차범위를 정하지 않고 자바에 정의된 Float.POSITIVE_INFINITY 값을 오차범위로 사용하였다.
