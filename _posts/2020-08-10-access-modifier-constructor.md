---
title: ":coffee: [Java] 접근 제어자와 생성자"
categories: java
tags: [ java ]
---

## 접근 제어자와 생성자

자바는 인스턴스 변수를 초기화시키기 위해 여러 개의 생성자를 만들어 제공한다. 필요에 맞는 적절한 생성자를 호출하여 인스턴스를 초기화시킨 후 사용해야 한다. 그러나 생성자가 있어도 **접근 권한**이 없으면 생성자를 호출할 수 없다. java.util.Calendar 클래스 소스코드를 살펴보면서 이해하고, 생성자를 직접 접근제어자로 막아보자.

### java.util.Calendar

java.util.Calendar 클래스의 생성자는 접근제어자 `protected`로 보호되고 있다. Calendar를 상속 받은 자식 클래스만 해당 생성자를 호출할 수 있다는 뜻이다. 따라서 new 명령을 통해 바로 인스턴스를 생성할 수 없다. 

```java
// Java API
protected Calendar()
{
    this(TimeZone.getDefaultRef(), Locale.getDefault(Locale.Category.FORMAT));
    sharedZone = true;
}
```

- 생성자에 접근할 수 없을 때, **어떻게 인스턴스**를 생성할 수 있을까?
  1. **다른 클래스의 도움을 받는다.**
  2. 해당 클래스에서 제공하는 클래스 메서드를 사용해 간접적으로 생성한다.

Calendar 클래스는 2번째로, 클래스 메서드를 통해 생성하도록 유도하고 있다.

```java
//Calendar c = new Calendar(); => 컴파일 오류

Calendar c1 = Calendar.getInstance();
Calendar c2 = Calendar.getInstance();

System.out.println(c1 == c2); // false
```
`getInstance()`를 사용하면 오늘 날짜 및 시간 정보를 저장한 객체를 만들어 리턴한다.  호출 시점의 시각이 다르기 때문에 두 객체의 주소는 다르다.

- **왜 생성자로의 접근을 막을까?**
  - 같은 값을 갖는 객체를 여러개 생성하지 못하도록 하고 메모리를 절약할 수 있다 => `Singleton1` 기법
  - 객체 생성과정이 복잡할 때, 객체 생성 코드를 단순하게 만들 수 있고, 생성된 객체를 재활용할 수 있다. => `Factory Method`

> [싱글턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)이란? 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴. getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환한다. ex) 레지스트리와 같은 설정 파일(객체가 여러개 생성되면 설정 값이 변경될 위험이 있다.)
>
> [팩토리 메서드](https://jdm.kr/blog/180)란? 객체를 만들어내는 부분을 서브 클래스에 위임하는 패턴이다. 즉, new 키워드를 호출하는 부분을 서브 클래스에 위임하는 것이다. 객체를 만들어내는 공장을 만드는 패턴
>
> 생성(Creational) 패턴이란? 객체 생성에 관련된 패턴으로, 객체의 생성과 조합을 캡슐화해 특정  개체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다. 즉 인스턴스를 만드는 절차를 추상화하는 패턴이다.



### 실습

```java
    String model;
    int cc;
    static Car obj;
    
    // 외부에서 호출 못하는 생성자
    private Car() {}
    
    // 이미 인스턴스가 있다면 해당 인스턴스 리턴
    // 없다면 객체 생성 후 리턴
    static Car getInstance() {
        if (obj == null) {
            obj = new Car();
        }
        return obj;
    }
}
```


