---
title: ":coffee: [Java] LinkedList 구현하기"
categories: Java
tags: [ Java ]
toc: true
---

# MyLinkedList 만들기

## 1단계:  LinkedList 클래스 정의

```java
public MyLinkedList {}
```

## 2단계: 값을 담을 노드 클래스 설계

- Node의 인스턴스 필드와 생성자를 정의하자.

```java
public class MyLinkedList {
    static class Node {
        Object value;
        Node next;
        
        Node(){}
        Node(Object value) {
            this.value = value;
        }
    }
}
```

## 3단계:  필드 추가

- 첫 번째 노드와 마지막 노드의 주소를 담을 필드를 추가한다.
- 목록 크기를 저장할 필드를 추가한다.

```java
public class MyLinkedList {
    Node first;
    Node last;
    public int size;
    
    static class Node {
        Object value;
        Node next;
    }
}
```



- 다음 단계부터 LinkedList의 메서드를 정의할 것이다. 

- 다음과 같은 LinkedList가 있다고 가정해보자.

![MyLinkedList](https://user-images.githubusercontent.com/50407047/91126977-39806500-e6e0-11ea-9ce4-ccca4dfc0e6a.JPG)

## 4단계:  add(Object) 메서드를 정의

![MyLinkedList add(Object)](https://user-images.githubusercontent.com/50407047/91126982-3ab19200-e6e0-11ea-8407-d3c9ddf7cafc.JPG)

- 목록에 값을 추가하는 `boolean add(Object e)` 메서드를 정의한다.

```java
public boolean add (Object element) {
    Node node = new Node(element);
    last.next = node;
    last = node;
    size++;
}
```

## 5단계:  get(int) 메서드 정의

![MyLinkedList get(int)](https://user-images.githubusercontent.com/50407047/91126987-3be2bf00-e6e0-11ea-8c21-0645f4458255.JPG)

- 목록에서 값을 조회하는 `Object get(int index)` 메서드를 정의한다.

```java
public Object get(int index) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException("인덱스가 유효하지 않습니다.");
    }
    Node cursor = first;
    for (int i = 1; i <= index; i++) {
        cursor = cursor.next;
    }
    return cursor.value;
}
```

## 6단계: add(int, Object) 메서드 정의

![MyLinkedList add(int, Object)](https://user-images.githubusercontent.com/50407047/91128269-dc39e300-e6e2-11ea-88f1-69d60a2755b6.jpg)

- 목록에서 특정 인덱스 위치에 값을 삽입하는 `void add(int, Object)` 메서드를 정의한다.

```java
public void add(int index, Object element) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException("인덱스가 유효하지 않습니다.")
    }
    
    Node node = new Node(element);
    size++;
    
    if (index == 0) {
        node.next = first;
        first = node;
        return;
    }    
    
    Node cursor = first;
    for (int i = 1; i <= index - 1; i++) {
        cursor = cursor.next;
    }
    
    node.next = cursor.next;
    cursor.next = node;
    
    if (node.next == null) {
        last = node;
    }
}
```



## 7단계: remove(int) 메서드 정의

![MyLinkedList remove(int)](https://user-images.githubusercontent.com/50407047/91126988-3c7b5580-e6e0-11ea-9277-b34048b6dbe6.JPG)

- 목록에서 특정 인덱스의 값을 제거하는 `Object remove(int)` 메서드를 정의한다.

```java
public Object remove (int index) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException("인덱스가 유효하지 않습니다.");
    }
    
    size--;
    
    if (index == 0) {
        Node old = first;
        first = node.next;
        old.next = null;
        return old.value;
    }
    
    Node cursor = first;
    for (int i = 1; i < index - 1; i++) {
        cursor = cursor.next;
    }
    
    Node old = cursor.next;
    cursor.next = old.next;
    old.next = null;
    
    if (cursor.next == null) {
        last = cursor;
    }
    
    return old.value;
}
```



## 8단계: set(int, Object) 메서드 정의

![MyLinkedList set(int, Object)](https://user-images.githubusercontent.com/50407047/91126989-3c7b5580-e6e0-11ea-8c06-250ac806e672.JPG)

- 목록에서 특정 인덱스의 값을 바꾸는 `Object set(int, Object)` 메서드를 정의한다.

```java
public Object set(int index, Object element) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException("인덱스가 유효하지 않습니다.");
    }
    
    Node cursor = first;
    for (int i = 1; i <= index; i++) {
        cursor = cursor.next;
    }
    Object old = cursor.value;
    cursor.value = element;
    return old;
}
```



## 10단계: toArray() 메서드를 정의한다.

- 목록의 데이터를 새 배열에 담아 리턴하는 `toArray()` 메서드를 정의한다.

```java
public Object[] toArray() {
    Object[] arr = new Object[this.size];
    
    int i = 0;    
    Node cursor = first;

    while (cursor != null) {
        arr[i++] = cursor.value;
        cursor = cursor.next;
    }
    
    return arr;
}
```



## 11단계: size() 메서드를 정의한다.

- 목록의 크기를 저장하는 필드 `size`를 `size()` 메서드로만 접근할 수 있게 `private`으로 전환한다. 
- 클래스 내부에서만 쓰이는 `first`, `last` 필드도 `private`으로 전환한다.

```java
public int size() {
    return this.size;
}
```

```java
public class MyLinkedList {
    private Node first;
    private Node last;
    private int size;
}
```



(단계 추가 예정)