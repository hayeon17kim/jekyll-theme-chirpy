---
title: "🍵 부동소수점 리터럴"
categories: java
tags: [ java ]
toc: true
---


### 부동소수점이란?
Exponential 기호를 사용하여 소수점 위치를 조정할 수 있다. 3.14는 0.314e1, 31.4e-1, 314e-2로도 표현할 수 있는데, 이처럼 소수점이 둥둥 떠다니면서 움직인다고 하여서 이렇게 표현하는 방식을 부동소수점(floating point) 방식이라고 부른다. 

컴퓨터는 1과 0으로 데이터를 저장한다. 따라서 실수 값을 저장할 때 부동소수점 방식으로 저장하려면 1과 0으로 변환해야 한다. Java는 부동소수점을 저장할 때 전기전자기술자협회(IEEE)에서 개발한 IEEE 754 명세에 따라 2진수로 변환한다.

### 부동소수점의 최소값과 최대값
자바에서 부동소수점 데이터타입의 종류에는 `Float`과 `Double`이 있다. Float 값은 4바이트에 저장되고, Double 값은 8바이트에 저장된다. 

#### 최소값과 최대값 확인하기
`.MAX_VALUE`, `.MIN_VALUE`를 사용하여 최댓값과 최솟값을 확인할 수 있다.

```java
// 4바이트 부동소수점의 최대값과 최소값
System.out.println(Float.MAX_VALUE);    //3.4028235E38
System.out.println(Float.MIN_VALUE);    //1.4E-45

// 8바이트 부동소수점의 최대값과 최소값
System.out.println(Float.MAX_VALUE);    //1.7976931348623157E308
System.out.println(Float.MIN_VALUE);    //4.9E-324
```
4바이트 메모리는 최대 7자리 부동소수점을, 8바이트의 메모리는 최대 16자리의 부동소수점을 저장할 수 있다.

#### 메모리에 크기를 넘는 값 출력해보기

```java
// 유효자릿수를 
// 초과하는 경우 반올림 처리되거나 잘린다.
System.out.println(9999999.4f);     //9999999.0
System.out.println(9.9999994f);     //9.999999
```

```java
System.out.println(987654321.1234567f);
System.out.println(987654321123456.7f);
System.out.println(9.876543211234567f);
```