---
title: "핵심 자료구조와 알고리즘 #1장: 인터페이스"
categories: data-structure
tags: [ data-structure, java ]
---

## 리스트가 두 종류인 이유

자바는 List인터페이스에 ArrayList와 LinkedList라는 두 가지 구현을 제공한다. ArrayList와 LinkedList는 모두 동일한 메서드를 제공한다. 그렇다면 이 클래스의 차이점은 무엇일까? 

![arraylist-linkedlist](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/1335/2885.png)

이미지 출처: 한빛미디어

**ArrayList**는 **메모리에 요소가 나란히 붙어서 위치**해있는다. 이렇게 나란히 있기 때문에 **요소의 위치를 찾기가 매우 수월하다**. 그러나 이러한 방식은 **빈 공간을 허용하지 않기** 때문에 중간에 요소를 **제거**될 경우 전체 요소가 빈 공간을 채우려 이동해야 한다. 마찬가지로 중간에 요소를 **추가**할 경우 **전체 요소가 위치를 이동**해야 할 것이다.

**LinkedList**는 요소가 **산발적으로 연결(Linked)된 방식**이다. 이 경우 한 요소의 위치를 찾기 위해서는 첫 요소부터 다음 요소의 위치를 알아내고, 그리고 그 요소로부터 다음 요소의 위치를 알아내는 과정을 원하는 요소를 찾을 때까지 수행해야 한다. 즉 **탐색이 매우 번거롭다**. 그러나 이 방식은 ArrayList와 같이 요소가 나란히 있는 게 아니기 때문에 **중간에 요소가 삭제되거나 추가되어도 전혀 상관 없다.** 

## 자바 interface

- 자바 interface는 메서드 집합을 의미한다.
- interface를 구현하는 클래스는 interface에 정의된 메서드를 제공해야 한다.

책에서는 다음의 예제를 들고 있다.

```java
public interfac Comparable<T> {
  public int compareTo(T o);
}
```

Comparable 인터페이스를 구현하려면 클래스는

- T 타입을 명시해야 한다.
- T 타입의 객체를 인자로 받고 int를 반환하는 compareTo() 메서드를 제공해야 한다.

```java
public final class Integer extends Number implements Comparable<Integer> {
  public int compareTo(Integer anotherInteger) {
    int thisVal = this.value;
    int anotherVal = anotherInteger.value;
    return (thisVal<anotherVal ? -1 : (thisVal==anotherVal ? 0 : 1));
  }
  //..
}
```

이 클래스는 Number 클래스를 확장한다. Number 클래스의 메서드와 인스턴스 변수를 상속하고 `Comparable<Integer>` 인터페이스를 구현한다. 

## List Interface

interface는 List가 된다는 의미가 무엇인지 정의한다.

List를 구현한 ArrayList와 LinkedList는 List가 정의하고 있는 add, get, remove 등 약 20가지 메서드를 포함한 특정 메서드 집합을 제공해야 한다. 다시 말해,  ArrayList와 LinkedList는 List에 정의된 메서드 집합을 제공하고 있기 때문에 두 클래스를 포함해 List를 구현한 클래스는 서로 교체 가능하다. 

```java
public class ListClientExample {
	private List list;
  
	public ListClientExample() {
		list = new ArrayList();
	}

	public List getList() {
		return list;
	}

	public static void main(String[] args) {
		ListClientExample lce = new ListClientExample();
		List list = lce.getList();
		System.out.println(list);
	}
}
```

위의 코드처럼 필요한 경우가 아니면 LinkedList나 ArrayList같이 **구현 클래스를 사용하지 않고**  가능한 List **인터페이스를 사용해야 한다**. 이를 **인터페이스 기반 프로그래밍(interface-based programming)**이라고 한다. 

**라이브러리를 사용**할 때 코드는 오직 **인터페이스만 의존**하고 ArrayList 클래스와 같은 **특정 구현에 의존해서는 안 된다**. 이런 방식으로 하면 **나중에 구현이 변경되어도 인터페이스를 사용하는 코드는 그대로 사용할 수 있다**.

반면 인터페이스가 변경되면 인터페이스를 의존하는 코드는 변경되어야 한다. 꼭 필요한 일이 아니면 라이브러리 개발자가 인터페이스를 변경하지 않는 이유이다.



## 실습1

다음의 테스트 코드로 LinkedClientExample 클래스를 테스트해보겠다. 

```java
public class ListClientExampleTest {

	/**
	 * Test method for {@link ListClientExample}.
	 */
	@Test
	public void testListClientExample() {
		ListClientExample lce = new ListClientExample();
		@SuppressWarnings("rawtypes")
		List list = lce.getList();
		assertThat(list, instanceOf(ArrayList.class) ); // ArrayList 객체 기대
	}
}
```

> `assertThat(T actual, Matcher<? super T> matcher)`: 첫 번째 파라미터에는 비교대상 값을, 두 번째 파라미터로는 비교로직이 담긴 Matcher가 사용되고, 메서드는 두 값을 비교한다. 같으면 테스트를 통과하게 된다.

위의 테스트는 실패한다. Failure Trace를 보면 `java.lang.AssertionError: Expected: an instance of java.util.ArrayList but <> is a java.util.LinkedList`라고 나와 있다. 즉 getList()의 리턴값이 ArrayLIst인지 확인하기 위해 `instanceof(ArrayList.class)`를 assertThat의 두 번째 파라미터로 넣었지만, 실제 코드에서는 LinkedList 객체를 리턴하기 때문에 테스트가 실패한 것이다.

### 쉬운 대체

테스트를 통과하기 위해 실제 코드의 LinkedList 클래스를 ArrayList 클래스로 대체하자. 다음과 같이 **선언 부분을 바꿀 필요 없이 객체 생성 부분만 변경하면 된다.** 즉, 인터페이스 구현체는 다른 구현체로의 대체가 매우 편하다. 변화에 유연하다!

```java
public class ListClientExample {
	private List list;

	public ListClientExample() {
		list = new ArrayList(); // 여기만 바꾸면 된다!
	}

	public List getList() {
		return list;
	}

	public static void main(String[] args) {
		ListClientExample lce = new ListClientExample();
		List list = lce.getList();
		System.out.println(list);
	}
}
```

### 과다 지정

선언 부분을 변경하면 어떻게 될까?

```java
public class ListClientExample {
	private ArrayList list; // 변경부분

	public ListClientExample() {
		list = new ArrayList(); // 변경부분
	}

	public List getList() {
		return list;
	}

	public static void main(String[] args) {
		ListClientExample lce = new ListClientExample();
		ArrayList list = lce.getList(); // 변경부분
		System.out.println(list);
	}
}

```

프로그램은 정상적으로 동작하지만, 이것은 **과다지정(overpecified)**이다. 이렇게 할 경우 다시 인터페이스로 돌아가려면 **더 많은 코드를 수정**해야 할 수도 있다. 즉 변화에 유연하지 않은 아주 딱딱한 코드가 된다!



### 인터페이스로 인스턴스가 생성되지 않는 이유

그렇다면 ArrayList 객체를 List 인터페이스로 교체하면 어떻게 될까? List의 실체(인스턴스)가 없으므로 컴파일이 제대로 되지 않는다. 자바의 interface는 인스턴스를 생성하는 객체로 사용되지 않는다. interface를 사용해서 어떠한 클래스를 디자인할 것인지 규칙만을 제공한다. 

자바에서 추상 클래스와 인터페이스는 그 자체로 인스턴스를 생성할 수 없는데, 이는 둘 다 추상메서드를 가지고 있기 때문이다. 만약 추상 메서드를 가지고 있는 클래스의 인스턴스를 만들 수 있다면 개발자들은 그 클래스의 추상 메서드를 사용하려고 할 것이다. 그러나 추상 메서드는 구현이 아예 되어 있지 않고 선언만 되어 있기 때문에 메서드를 사용할 수 없다. 따라서 이를 원천 차단하기 위해 추상 메서드를 갖고 있는 클래스는 객체를 생성할 수가 없다. 그저 '이 메서드를 구현하라'라는 가이드라인/스케치만 제공하는 역할에 그친다. 