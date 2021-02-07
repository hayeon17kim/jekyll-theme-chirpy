---
title: ":coffee: JVM 메모리: Method Area, Stack, Heap"
categories: java
tags: [ java ]
toc: true
---

비트캠프 엄진영 강사님의 수업을 들으며 정리하였습니다.

## 메서드와 JVM 메모리

![JVM-memory](https://user-images.githubusercontent.com/50407047/89724528-2c584a80-da3f-11ea-9e16-b9d6be7b1101.png)

프로그램이 실행되면 JVM은 OS로부터 사용할 메모리를 할당받는다. JVM은 할당받은 메모리를 크게 Method Area, JVM Stack, Heap이라는 세 영역으로 나누어 관리한다. JVM이 종료하면 JVM이 사용했던 모든 메모리를 OS에 반납한다.

### Method Area

- 클래스 명령 코드를 둔다.
- static 변수(클래스 변수)를 둔다.

### JVM Stack

- 스레드 별로 JVM 메모리를 따로 관리한다.
- 메서드의 로컬 변수를 둔다.
- 각 메서드마다 프레임 단위로 관리한다.
- 메서드 호출이 끝나면 해당 메서드가 사용한 프레임 메모리가 제거된다.
- 메서드가 호출될 때 로컬 변수가 준비되고, 맨 마지막에 호출한 메서드가 먼저 삭제된다. Last In First Out(LIFO, 후입선출)이라 부른다.

### Heap

- 인스턴스(new 명령으로 만든 메모리-변수들의 덩어리)를 둔다.
- 가비지 컬렉터(Garbage Collector)가 관리하는 영역이다. 
- 필요에 의해 동적으로 메모리를 할당한다.
- Stack은 할당될 메모리의 크기를 컴파일 과정에서 알 수 있지만 Heap은 컴파일 과정에서는 크기를 알 수 없다. 실행 시 크기가 결정된다. 



