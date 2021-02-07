---
title: ":coffee: [Java] wrapper 클래스"
categories: java
tags: [ java ]
toc: true
---

# Wrapper Class

## wrapper 클래스란?

- 자바는 primitive data type의 값을 다룰 때 기본 연산자 외에 좀 더 다양한 방법으로 다루기 위해 primitive data type에 대응하는 클래스를 제공한다.

- primitive data를 포장하는 객체라고 해서 랩퍼(wrapper) 클래스라고 부른다.
- primitive data type의 값을 객체로 주고 받을 때 사용한다. 랩퍼 클래스가 없다면 데이터 타입에 따라 메서드를 따로따로 정의해야 한다. 

```java
// 모든 객체를 받을 수 있다.
void m1(Object value) {
    System.out.printf("value=%s\n", value);
}

// wrapper클래스가 없다면? 
// 각타입의 데이터를 받는 메서드를 따로 정의해야 한다.
void m2(long value) { // byte, short, int, long, char
	System.out.printf("value=%s\n", value);
} 
void m3(double value) {// float, double
	System.out.printf("value=%s\n", value);
} 
void m4(boolean value) {// boolean
	System.out.printf("value=%s\n", value);
} 
```

## 생성자 vs valueOf()

- 생성자 사용해서 생성 (new 명령)
  - 무조건 새 인스턴스를 생성한다.
- valueOf()호출하여 생성
  - -128 ~ 127 범위의 수(자주 사용되는 수)로 호출하면 상수 풀에 Integer 객체를 생성한다. 같은 값의 Integer 객체가 여러 개 생성되지 않도록 한다.
  - 범위 외의 수는 무조건 heap에 새 인스턴스를 생성한다.
  - 다루는 숫자가 너무 많기 때문에 무조건 상수 풀에 만들기에는 메모리 낭비가 심해지기 때문이다. 상수풀에 생성된 객체는 JVM이 종료되기 전까지 유지되고, 가비지가 되지 않는다. 반면 heap에 생성된 객체는 주소를 잃어버리면 가비지가 되기 때문에 메모리를 좀 더 효율적으로 사용할 수 있다. 

- 따라서  wrapper 객체의 값을 비교할 때는 `equals()` 메서드를 사용하는 것이 좋다. `==` 연산자를 사용하면 매번 비교할 때마다 범위의 유효성을 생각해야 하기 때문이다. 

## auto-boxing / auto-unboxing

JDK 1.5 부터 지원되는 기능이다. 여기서는 Integer 클래스로 예시를 들었다.

### auto-boxing

```java
int i1 = 100;
Integer obj1 = Integer.valueOf(i1);

// auto-boxing
Integer obj2 = 100; // => Integer.valueOf(100);
```

- 자바는 primitive data type 값을 wrapper클래스의 인스턴스에 바로 할당할 수 있다. obj는 레퍼런스인데 어떻게 가능할까?
- 내부적으로 `Integer.valueOf(100)`으로 바뀐다. 즉 int 값이 obj에 바로 저장되는 것이 아니라 내부적으로 Integer 객체가 생성되어 그 주소가 저장된다.
- 이렇게 Int값을 자동으로 Integer 객체로 만드는 것을 auto-boxing이라고 한다.

### auto-unboxing

```java
// Integer ==> int
Integer obj1 = Integer.valueOf(200);
int i1 = obj1.intValue();

Integer obj2 = Integer.valueOf(300);
int i2 = obj2; // obj2.intValue();
```

- 자바는 wrapper 객체의 값을  primitive data type의 변수에 직접 할당할 수 있다. obj에 저장된 것은 Int 값이 아니라 Integer 객체의 주소인데 어떻게 가능한가?
- 내부적으로 `obj.intValue()`로 바뀐다.즉 obj에 들어있는 인스턴스 주소가 i에 저장되는 것이 아니라 obj 인스턴스에 들어 있는 값을 꺼내서 i에 저장하는 것이다.
- 이렇게 wrapper 객체 안에 들어 있는 값을 꺼낸다고 해서 auto-unboxing이라고 한다.



### 응용

#### 다형적 변수: 메서드의 파라미터

```java
static class Member {
    String name;
    String tel;
    
    @Override
    public String toString() {
        return name + "," + tel;
    }
}

public static void main(String[] args) {
    int i = 100;
    Member member = new Member();
    member.name = "monica";
    member.tel = "010-1111-2222";
    
    print(100);
    // 파라미터 타입이 Object이면
    // 자바 컴파일러는 auto-boxing 코드로 변환한다.
    // 즉 Integer.valueOf(100)로 바꾼다. 
    
    print(member);
}

// auto-boxing/auto-unboxing 기능이 제공되기 때문에
// primitive type의 값과 객체를 구분하지 않고 
// Object 파라미터를 사용하여 처리할 수 있다.
// print(Member member){}, print(int number){}를 따로따로 정의할 필요x
static void print(Object obj) {
    System.out.println(obj);
}
```

### wrapper 객체 생성

```java
// 1. 생성자
Integer obj1 = new Integer(100); // Heap에 인스턴스 생성
Integer obj2 = new Integer(100); // Heap에 인스턴스 생성
System.out.println(obj1 == obj2); // false

// 2. auto-box 방식으로 wrapper 객체 생성 (valueOf() 메서드 호출)
// 1) 정수값이 -128 ~ 127 범위
Integer obj3 = 100; //Integer.valueOf(100);
Integer obj4 = 100; //Integer.valueOf(100);
System.out.println(obj3 == obj4); // true
// auto boxing이나 valueOf로 생성한 wrapper 객체는
// constants pool에 오직 한 개만 생성되기 때문에 
// 값을 비교할 때 그냥 == 연산자를 사용하여 인스턴스 주소로 비교해도 된다.

// 2) 정수값이 범위 초과
Integer obj5 = 128;
Integer obj6 = 128;
System.out.println(obj5 == obj6); // fasle

// Integer 클래스가 오버라이딩한 equals()를 호출해야 한다.
System.out.println(obj5.equals(obj6)); //true
```

- wrapper 객체를 생성할 때는 new를 사용하지 말고, valueOf()나 auto boxing 기능을 이용하자.
- 값을 비교할 때는 equals()를 사용하자. auto-boxing으로 객체를 만들 경우 -128 ~ 127 범위 내의 숫자는 캐시에 보관하기 때문에 같은 값은 같은 객체이지만, 이 범위를 벗어나면 값이 같더라도 객체가 다르다. 따라서 일관성을 위해 값을 비교할 때 equals를 사용하자.



