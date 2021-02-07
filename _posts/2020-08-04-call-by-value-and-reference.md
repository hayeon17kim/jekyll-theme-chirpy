---
title: ":coffee: 메서드 호출: 기본형과 참조형 매개변수의 차이점"
categories: java
tags: [ java ]
toc: true
---

## 메서드 호출

메서드를 호출할 때, 매개변수의 타입이 기본형(primitive type)일 때는 기본형 값이 복사되고, 참조형(reference type)이면 인스턴스의 주소가 복사된다. 기본형 매개변수의 경우 변수의 값을 읽기만 할 수 있지만, 참조형 매개변수의 경우 변수의 값을 읽고 변경할 수 있다. 

### call by value

```java
public static void main(String[] args){
    int a = 200;
    m1(a); // call by value
    System.out.println(a); // 200
}

static void m1(int a) {
    a = 100;
}
```

위의 코드를 다음과 같이 그림으로 표현해보았다. 아래와 같이 **메서드를 호출할 때 만들어진 값은 호출하고 나면 사라진다.** 따라서 main 메서드에서 a는 200으로 변화가 없다.

![call-by-value](https://user-images.githubusercontent.com/50407047/89713765-d353cc80-d9d4-11ea-86e5-e08f4664824b.png)

### call by reference

```java
public static void main(String[] args){
    int a;
    int[] arr = new int[3];
    arr[0] = 100;
    arr[1] = 200;
    arr[2] = 300;
    m2(arr); // call by reference
    System.out.printf("%d, %d, %d", arr[0], arr[1], arr[2]);
    // 200, 600, 1200
}

static void m2(int a) {
    arr[0] *= 2;
    arr[1] *= 3;
    arr[2] *= 4;
}
```

메서드를 호출할 때 참조형인 배열을 매개변수로 호출하고 있다. 기본형 매개변수로 호출할 때와는 달리 메서드에서 변경한 값이 main 메서드에서도 적용되었다. 위의 코드를 그림으로 표현해보았다.

![call-by-reference1](https://user-images.githubusercontent.com/50407047/89724300-2dd44380-da3c-11ea-9100-9315d8a55c7d.jpg)

기본형은 변수(메모리)에 값이 저장되는 반면, 참조형의 경우 변수에 주소값이 저장된다. 여기서는 임의로 주소값을 129번지로 설정했다. (c와 달리 자바는 주소값을 확인할 수 있는 방법이 없다.) 이 주소값은 Heap에 생성된 인스턴스를  참조하고(가리키고) 있다. 이것이 기본형과 다른 점이다.

![call-by-reference2](https://user-images.githubusercontent.com/50407047/89723879-0c248d80-da37-11ea-945f-e3b9c54e9e97.jpg)



m2()를 호출하면 main 메서드의 실행은 잠시 중단되고 m2() 프레임이 쌓인다. 그리고 이 안에 해당 메서드의 지역변수 arr의 메모리가 확보된다. main()에서 **주소값**을 아규먼트로 넘겨주고 m2()에서 그것을 파라미터로 받아 arr 메모리에 저장한다. 따라서 이제 m2()의 arr 역시 Heap에 생성되어 있는 인스턴스(실제 배열)을 참조한다.

![call-by-reference3](https://user-images.githubusercontent.com/50407047/89723878-0c248d80-da37-11ea-879e-6d30a8103adb.jpg)

m2() 메서드에서 129번지의 배열을 참조하고 있는 (주소값을 가지고 있는) arr 참조변수를 통해 Heap의 배열 안에 들어 있는 값을 변경하였다. 

![call-by-reference4](https://user-images.githubusercontent.com/50407047/89724339-a76c3180-da3c-11ea-999f-a483103cf1d4.jpg)

m2()의 호출이 끝나면 stack에 있는 arr 메모리는 해제되고 중단되었던 main 메서드가 실행된다. main 메서드의 arr 변수는 변경된 값이 들어 있는 129번지의 배열을 참조한다. 