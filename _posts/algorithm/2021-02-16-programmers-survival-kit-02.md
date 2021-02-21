---
title: "정렬(Sort): 가장 큰 수"
categories: algorithm
tags: [ algorithm, programmers ]
---

## 가장 큰 수

프로그래머스의 [코딩테스트 광탈 방지 Kit: Java편](https://programmers.co.kr/learn/courses/10302) 강의를 참고하여 작성하였습니다.

### 문제

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내는 문제이다.

##### 제한 사항

- numbers의 길이는 1 이상 100,000 이하이다.
- numbers의 원소는 0 이상 1,000 이하이다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 한다.

##### 입출력 예

| numbers           | return    |
| ----------------- | --------- |
| [6, 10, 2]        | "6210"    |
| [3, 30, 34, 5, 9] | "9534330" |

### 내가 시도한 방법

```java
class Solution {
    public String solution(int[] numbers) {
        String answer = "";
        int size = numbers.length;
        String[] numStrings = new String[size];
        
        for (int i = 0; i < size; i++) {
            numStrings[i] = String.valueOf(numbers[i]);
        }
        
        for (int i = 0; i < size; i++) {
            int maxIndex = i;
            for (int j = i + 1; j < numbers.length; j++) {
                if (numStrings[j].compareTo(numStrings[maxIndex]) == 1) {
                    maxIndex = j;
                }
            }
            String temp = numStrings[i];
            numStrings[i] = numStrings[maxIndex];
            numStrings[maxIndex] = temp;
            
        }
    
        for (int i = 0; i < size; i++) {
            answer += numStrings[i];
        }
        
        return answer;
    }
}

```

일단 숫자 배열을 문자 배열로 바꾸고, 문자를 비교했다. 이때 그냥 비교하는 것이 아니라 첫번째 값과 두 번째 값을 연결한(더한) 값과, 두번째 값과 첫번째 값을 연결한 값을 비교해야 했다. 

### 수업

- 배열의 경우의 수를 모두 구하면 되지 않을까? **문제는 integer의 범위를 넘어갈 수도 있다**는 것이다. `numbers`의 길이가 `100,000` 이하이고, `numbers`의 원소는 0 이상 `1,000` 이하이니 말이다.
- 또한 **반환 타입**이 문자열 형태이기 때문에 문자열로 다루는 것을 생각하는 게 **자연스럽다.**
- 가장 큰 숫자를 만들려면, 큰 숫자부터 조합을 하면 된다. 이때, `[6, 10, 2]`에서 큰 숫자대로 조합한 `1062`는 가장 큰 수가 아니다. 가장 큰 수는 6210이다. 
- **숫자 -> 문자 -> 내림차순 정렬 -> 조합** 순서로 문제를 풀어나가야 한다. 

```java
class Solution {
  public String solution(int[] numbers) {
    
    // 숫자 -> 문자 -> 내림차순정렬 -> 조합
    String[] strNums = new String[numbers.length];
    for (int i = 0; i < numbers.length; i++) {
      strNums[i] = "" + numbers[i];
    }
    
    // bubble sort
    // 문자열을 비교하여 정렬한다.
    Arrays.sort(strNums, (s1, s2) 
            -> (s2 + s1).compareTo(s1 + s2));
    
    String answer = "";
    for (String s : strNums) {
      answer += s;
    }
    if (answer.charAt(0) == '0') return "0";
    return answer;
  }
}
```

- `(s1 + s2).compareTo(s2 + s1) < 0`

  만약 s1와 s2를 합한 값이 s2와 s1을 합한 값보다 작다면(compareTo 메서드 리턴값이 -1이라면) s2가 s1보다 앞에 와야 한다. 즉 자리를 바꿔야 한다. s1과 s2이 "10", "2"라고 해보자. 숫자로 생각해보면 값자체는 s1이 더 크지만, s1 + s2인 `102`은 s2 + s1인 `210`보다 작다. 따라서 두 값의 자리를 바꿔준다.

- `answer.charAt(0) == '0'`

  만약 numbers의 값이 `[0, 0, 0, 0]`이라고 가정하자. 그러면 결과값은 `"0000"`가 될 것이다. 이를 방지하려면 어떻게 할까? 
  이때 `answer`의 맨 앞 글자가 `"0"`인 경우는 뒤의 자리 모두 `0`인 경우밖에 없다. 다른 어떤 양의 정수 값이 와도 `0 + 양의정수` 보다는 `양의정수 + 0`이 더 크기 때문이다. 

  이를 활용하여, 맨 앞글자가 `"0"`이라면 `"0"`을 리턴하도록 만든다.

#### 자바 제공 라이브러리 사용하기

위의 코드는 기본 테스트 케이스는 통과하지만, 효율성 테스트에서 실패한다. 이를 위해서 bubble sort보다 효율적인 정렬 방식을 택할 수도 있지만, 자바에서 제공하는 라이브러리를 사용하는 것이 훨씬 간편하다.

```java
for (int i = 0; i < strNums.length - 1; i++) {
  for (int j = i + 1; j < strNums.length; j++) {
    String s1 = strNums[i];
    String s2 = strNums[j];
    // 중요!
    if ((s1 + s2).compareTo(s2 + s1) < 0) {
      strNums[i] = strNums[j];
      strNums[j] = s1;
    }
  }
}
```

이 코드를 다음과 같이 바꾼다.

```java
Arrays.sort(strNums, new Comparator<String>() {
  public int compare(String s1, String s2) {
    return (s2 + s1).compareTo(s1 + s2);
  } 
})
```

이때 기본으로 제공하는 정렬 방식이 아니라, 내 입맛대로 정렬을 하기 위해 Comparator를 구현하여 파라미터로 넘겨준다. Comparator 인터페이스에는 compare라는 메서드가 있다. `sort()`  메서드는 파라미터로 받은 배열을 오름차순 정렬한다. 만약 `compare()`의 리턴값이 1이면 s1이 s2보다 크다고 판단하고 s2 -> s1 순서로 정렬할 것이다. 

이때, Comparator는 메서드가 compare 하나밖에 없는 인터페이스기 때문에 람다식을 적용할 수 있다. 따라서 다음과 같이 바꿔준다.

```java
Arrays.sort(strNums, (s1, s2) 
            -> (s2 + s1).compareTo(s1 + s2));
```

훨씬 간단해졌다! 

```java
class Solution {
  public String solution(int[] numbers) {
    
    // 숫자 -> 문자 -> 내림차순정렬 -> 조합
    String[] strNums = new String[numbers.length];
    for (int i = 0; i < numbers.length; i++) {
      strNums[i] = "" + numbers[i];
    }
    
    // bubble sort
    // 문자열을 비교하여 정렬한다.
    Arrays.sort(strNums, (s1, s2) 
            -> (s2 + s1).compareTo(s1 + s2));
    
    String answer = "";
    for (String s : strNums) {
      answer += s;
    }
    if (answer.charAt(0) == '0') return "0";
    return answer;
  }
}
```

`Stream`을 사용하면 좀 더 효율적으로 만들 수 있다. 

```java
class Solution {
  public String solution(int[] numbers) {
    String answer = IntStream.of(numbers)
      .mapToObj(String:valueOf)
      .sorted((s1, s2) -> (s2 + s1).compareTo(s1 + s2))
      .collect(Collectors.joining());
    if (answer.startsWith("0")) return "0";
    return answer;
  }
}
```

- `IntStream.of(numbers)`

  `IntStream` 객체를 리턴해준다. 이 객체에는 `mapToObj` 이라는 유용한 메서드가 들어있다.

- `mapToObject(String:valueOf)`

  `<U> Stream<U> mapToObj(IntFunction<? extends U> mapper)` 메서드이다. 파라미터로 전달된 함수를 적용한 결과로 구성된 Stream 객체를 리턴한다.

- `sorted()`

  Stream 객체에 있는 메서드이다. 다른 정렬과 마찬가지고 Comparator를 이용하고, 인자 없이 그냥 호출할 경우 오름차순으로 정렬한다. Stream 객체를 리턴한다.

- `collect(Collector.joining())`

  `collect` 메서드는 종료 작업 중 일부이다. `Collector` 타입의 인자를 받아 처리를 한다. 자주 사용하는 작업은 `Collectors` 객체에서 제공하고 있다. 

  - `Collectors.toList()`: 스트림에서 작업한 결과를 담은 리스트로 변환한다.

  - `Collectors.joining()`: 스트림에서 작업한 결과를 하나의 문자열로 이어붙일 수 있다. 

    이 메서드는 세 개의 인자를 받을 수 있다.

    - `delimiter`: 각 요소 중간에 들어가 요소를 구분시켜주는 구분자
    - `prefix`: 결과 맨 앞에 붙는 문자
    - `suffix`: 결과 맨 뒤에 붙는 문자

    ```java
    String listToString = 
     productList.stream()
      .map(Product::getName)
      .collect(Collectors.joining(", ", "<", ">"));
    // <potatoes, orange, lemon, bread, sugar>
    ```

- `answer.startWith("0") return "0";`

  `charAt(0) == '0'`보다 더 직관적이고 좋은 방법이다.

  

## 결론

- 정렬 문제를 푼다고 해서 정렬을 직접 구현할 필요 없다.
- Java 언어를 사용한다는 것은 Java가 제공하는 기본 라이브러리를 사용한다는 것이다.
- 기본 라이브러리인 `java.lang.*`과 `java.util.*`의 사용법을 숙지하도록 하자.

