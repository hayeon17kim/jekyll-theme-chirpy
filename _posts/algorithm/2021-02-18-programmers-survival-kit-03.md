---
title: "이분탐색(Binary Search): 예산"
categories: algorithm
tags: [ algorithm, programmers ]
---

프로그래머스의 [코딩테스트 광탈 방지 Kit: Java편](https://programmers.co.kr/learn/courses/10302) 강의를 참고하여 작성하였습니다.

## 예산

### 내가 푼 방법

```java
class Solution {
  public int solution(int[] budgets, int M) {
    int answer = 0;
    int min = 0;
    int max = IntStream.of(budgets).max().orElse(0);
    int mid = 0;
    
    while (min <= max) {
      mid = (max + min) / 2;
      int sum = 0;
      
      for (int b : budgets) {
        if (b > mid) sum += mid;
        else sum += b;
      }
      if (sum < M) {
        min = mid + 1;
        answer = mid;
      } else {
        max = mid - 1;
      }
    }
  }
}
```

### 수업

- 탐색(Search): 데이터들 중에서 특정 값을 찾아내는 것
- 가장 단순한 방법: 0부터 끝까지 모든 값을 순회한다. 
- 찾는 값은 금액이다. 문제에 최소값과 최소값이 주어져 있다. 그리고 금액이기 때문에 **이미 정렬된 데이터라고 할 수 있다. 따라서 이분탐색(Binary Search)를 사용할 수 있다.**

```java
class Solution {
  public int solution(int[] budgets, int M) {
    int min = 0;
    int max = 0;
    for (int b : bugets) {
      if (b > max) max = b;
    }
    
    if answer = 0;
    while (min <= max) {
      int mid = (min + max) / 2;
      
      int sum = 0;
      for (int b : bugets) {
        if (b > mid) sum += mid;
        else sum += b;
      }
      
      //총 합이 국가 전체 예산보다 작은 경우
      if (sum <= M) {
        min = mid + 1;
        // 이때 mid가 answer이 될 수 있다.
        answer = mid;
      } else {
        max = mid - 1;
      }
    }
    return answer;
  }
}
```

- 이분탐색

  **전체 데이터를 다 확인하지 않고 범위를 줄여가면서 값을 찾아나가는 방식**을 말한다. **최대값과 최소값** 사이 어딘가쯤에 원하는 값이 있을테니 이 **중간값을 취해 원하는 값인지 확인**한다.

- **이 값을 상한선으로 했을 때의 합이 전체 예산을 초과하는지 아닌지 확인**한다. 이때 합이 예산을 초과하지 않는다면 상한액은 중간값과 최댓값 사이, 초과한다면 중간값과 최솟값 사이일 것이다. 그러면 새로운 범위가 생기고, 이렇게 범위가 하향조정되고 그럼 다시 중간점을 상한선으로 하는 총합을 구해본다.

- 이 작업을 계속 하다보면 **최소값과 최대값이 한 시점으로 수렴**할 것이고, **수렴하는 값이 나올 때까지 반복**하면 된다. 범위를 1/2 씩 줄여나가기 때문에 이분탐색이다. 이 방법의 조건은 **데이터가 정렬되어 있어야 한다**는 것이다. 

**리팩토링 하기**

```java
import java.util.stream.*;

class Solution {
  public int solution(int[] budgets, int M) {
    int min = 0;
    int max = IntStream.of(budgets).max().orElse(0); // Optional(Int)
    
    if answer = 0;
    while (min <= max) {
      // final 사용
      final mid = (min + max) / 2;
      
      int sum = 0;
      IntStream.of(budgets)
        // 예상 값이 들어왔을 때
        // mid 값과 비교해서 더 큰 경우 mid 값을 취하도록 만든다.
        .map(b -> Math.min(b, mid))
        .sum();
      
      //총 합이 국가 전체 예산보다 작은 경우
      if (sum <= M) {
        min = mid + 1;
        // 이때 mid가 answer이 될 수 있다.
        answer = mid;
      } else {
        max = mid - 1;
      }
    }
    return answer;
  }
}
```

- `max()` 메서드를 사용하면 `Optional(Int)` 값을 리턴한다. 이 값은 값이 있을 수도, 없을 수도 있다는 걸 말한다. 이때 값이 없으면 0이 되도록 `orElse(0)` 메서드를 사용해서 `int`값이 정상적으로 리턴되도록 만들어주자.

- `final`로 변수를 선언해주는 경우:

  stream 중간에서 stream 외부에 있는 변수값을 참조하고 있는 경우이다. stream이 처리되는 도중에 참조하는 변수의 값이 변경되면 side-effect가 된다. stream의 흐름은 비동기 처리처럼 기존 프로그램 흐름과 다르다. 그래서 stream 외부에 있는 변수의 값을 참조할 때 immutable한 값을 참조한다. 자바 컴파일러에서도 가변 변수를 참조하고 있다면 경고를 띄우며 final 변수를 참조하라고한다. final이 아니여도 문제 풀이 코드를 작성하고 실행하는 데는 큰 문제는 없다. 

- `IntStream.of(budgets).map(b -> Math.min(b, mid))`

  - `map`: 요소들을 특정 조건에 해당하는 값으로 변환해준다.
  - `Math.min(arg1, arg2)`: 입력받은 두 개의 인자 값 중 작은 값을 리턴한다. 

### 결론

**좋은 프로그램 만드는 방법**

- 당면한 문제를 해결한다.
- 좋은 코드가 되도록 계속 리팩토링한다.