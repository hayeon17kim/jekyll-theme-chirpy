---
title: "시뮬레이션(Simulation): 숫자게임"
categories: algorithm
tags: [ algorithm, programmers ]
---

프로그래머스의 [코딩테스트 광탈 방지 Kit: Java편](https://programmers.co.kr/learn/courses/10302) 강의를 참고하여 작성하였습니다.

### 문제

- A팀은 숫자와 숫서가 정해져 있다.
- B팀의 순서를 조합했을 때 B팀이 배열될 수 있는 모든 경우의 수를 비교해 보고, 그 상황에서의 승점을 계산해서 최댓값을 구하라.

### 내가 시도한 코드

```java
public class Simulation {
  public static void main(String[] args) {
    System.out.println(solution(new int[] {5, 1, 3, 7}, new int[] {2, 2, 6, 8}));
  }

  public static int solution(int[] A, int[] B) {
    int score = 0;
    Arrays.sort(B);
    List<Integer> blist = new LinkedList<>();
    for (int b : B) blist.add(b);

    for (int i = 0; i < A.length; i++) {
      for (int j = 0; j < blist.size(); j++) {
        if (A[i] < blist.get(j)) {
          blist.remove(j);
          score++;
          continue;
        } else {
          blist.remove(0);
        }
      }
    }
    return score;
  }
}
```

이건 내가 작성했던 코드인데, 이렇게 할 경우 샘플 테스트케이스는 통과하지만 대부분의 테스트 케이스는 통과하지 못하고, 효율성 테스트에서도 모두 시간초과가 났다. 

### 수업

- 만약 전체 모든 경우의 수를 다 확인하고, 그 안에서 원하는 값을 찾는다면 탐색법이다. 하지만 **B가 최대 승점으로 이긴다**는 조건이 있고, **이 조건에 맞는 조합을 재현**해낼 수 있다.
- **특정 상황을 재현**해내는 것이 시뮬레이션한다는 의미이다.
- **B의 숫자가 근소한 숫자로 이기는 경우**를 봐야 한다. 
- 그래서 B의 작은 값부터 시작해서 A를 이길수 있는 값을 하나씩 제거해가면서 계산해보자.

시뮬레이션

- **일련의 명령에 따라서 개체를 차례대로 이동**시키는 것.
- 일반적으로 **2차원 공간을 다루는 문제**가 많이 나온다.

```java
class Solution {
  public int solution(int[] A, int[] B) {
    Arrays.sort(B);
    
    int answer = 0;
    for (int i = 0; i < A.length; i++) {
      for (int j = 0 ; i < B.length; j++) {
        if (A[i] < B[j]) {
          answer++;
          B[j] = 0;
          break;
        }
      }
    }
  }
}
```

- `Arrays.sort(B)`**B팀이 근소한 차이로 이기도록**하는 것이 중요하다. 이를 위해서는 B팀의 작은 숫자부터 비교를 해야 한다. 따라서 우선 B팀을 정렬한다.
- `B[j] = 0`:  이미 사용한 숫자가 되었으니 다시 사용하지 못하도록 최솟값을 준다. 이 문제에서 1보다 큰값을 조건으로 주고 있기 때문에, 0은 어떤 값과 비교해도 가장 작은 값이 될 테니 사용되지 않을 것이다. 
- `break`: 그리고 가장 작은 값을 찾아냈기 때문에 **B의 루프는 멈춰도 될 것이다.** 
  - 이부분에서 나는 continue를 썼는데 break를 썼어야 했다.
- 이러면 A의 하나의 숫자를 이길 수 있는 B의 가장 작은 수를 찾아서 사용하게 된다.

**효율적으로 만들기**

- 위 코드는 기본 테스트는 통과하였으나 효율성 테스트에서 실패하였다.
- 우선 루프를 의심해보자. 이중 루프를 루프 하나로 처리해보자.

```java
class Solution {
  public int solution(int[] A, int[] B) {
    Arrays.sort(A);
    Arrays.sort(B);
    int index = B.length - 1;
    
    int answer = 0;
    for (int i = A.length - 1; i >= 0; i--) {
      if (A[i] < B[index]) {
        index--;
        answer++;
      }
    }
  }
}
```

- A도 B처럼 sort를 해놓고 큰 순서대로 따라가면서 B에 있는 큰 수와 비교해 하나씩 제거해나간다.

- A의 가장 큰 값과 B의 가장 큰 값을 비교했는데 A가 더 크다면 A는 이길 수 없는 숫자가 된다. 그럼 B에서 가장 큰 수를 A의 다음번 큰 숫자와 비교해야 한다.
- B가 이길 수 있는 숫자라면 B의 숫자가 사용된다. index는 하나 감소되고, 승점이 하나 올라간다.
- **A와 B 모두 정렬을 해놓고 서로 큰 값을 비교해가면서 계산을 해줬다. 이중루프에서 단일루프로 변경되었고, 따라서 효율성테스트에 통과되었다.**



