---
title: "그리디(Greedy): 기지국 설치 문제"
categories: algorithm
tags: [ algorithm, programmers ]
---

## 그리디: 기지국 설치

프로그래머스의 [코딩테스트 광탈 방지 Kit: Java편](https://programmers.co.kr/learn/courses/10302) 강의를 참고하여 작성하였습니다.

### 내가 시도했던 방법

나는 다음과 같이 풀었다. `checker`라는 배열을 만들어 기본적으로 모든 값을 0으로 셋팅하고, 전파 범위에 속한다면 1을 셋팅하였다. 그리고 이 checker를 탐색하면서 전파 범위가 아니라면 기지국을 세우는 방식으로 하였다. 

그러나 이럴 경우 일부 테스트 케이스에서는 통과되지 않았다. 

```java
class Solution {
    // n 아파트의 개수  stations 기지국이 설치된 아파트 번호  W 전파의 도달 거리
    public int solution(int n, int[] stations, int w) {
        int answer = 0;
        int[] checker = new int[n];
        for (int i = 0; i < stations.length; i++) {
            for (int j = -w; j <= w; j++) {
                if (stations[i] - j < 0 || stations[i] - j >= n) {
                    continue;
                }
                checker[stations[i] - j] = 1;
            }
        }
        int count = 0;
        for (int i = 0; i < n; i ++) {
            
            if (checker[i] == 0) {
                count++;
            }
            
            if (checker[i] == 1 && i != 0 && checker[i - 1] == 0 || i == n-1 ) {
                int temp = count / (2*w + 1);
                if (count % (2*w + 1) > 0) {
                    answer += temp + 1;
                } else {
                    if (temp == 0) {
                        answer += 1;
                    } else {
                        answer += temp;
                    }
                }
                count = 0;
            }
        }
        return answer;
    }
}
```

### 수업

- 아파트를 순회하면서 전파범위에 속하지 않으면 일단 기지국을 세우고 본다.
- 이때, 기지국을 세울 때 **전파범위의 효과가 최대가 되도록**, 전파 유효범위만큼 **이동해서** 기지국을 세운다.
- 전파범위가 끝나는 지점 이후부터 계속 살펴본다.
- 만약 중간에 이미 설치된 기지국이 있다면, **이미 설치된 전파범위 밖으로 벗어나서** 같은 작업을 하면 된다.
- 현재 전파가 없으면 현재 조건에 의해서 **일단 설치하고 본다.** 이렇게 전체적인 조건을 따지는 것이 아니라, **현재의 상태만 보고 탐욕적으로 당장의 해결방법을 적용**하는 방식을 **욕심쟁이 알고리즘**, 또는 **그리디(Greedy) 알고리즘**이라고 부른다.

#### 효율성 테스트를 통과하지 못한 코드

```java
class Solution {
  public int solution(int n, int[] stations, int w) {
    int answer = 0;
    
    Queue<Integer> sq = new LinkedList<>();
    for (int s : stations) sq.offer(s);
    
    int position = 1;
    while (position <= n) {
      // sq.peek() - w: 기존에 설치되어 있던 기지국 전파범위의 왼쪽 끝보다 오른쪽이라면 기지국 전파범위 안에 있다.
      if (sq.isEmpty() && sq.peek() - w <= position) {
        position = sq.poll() + w + 1;
      } else {
        // 기지국을 세운다!
        answer += 1;
      	position += w * 2 + 1;
      }
    }
    
    return answer;
  }
}
```

- `Queue`: 그래프의 넓이 우선 탐색(BFS)에서 주로 사용
- `peek()`: 첫번째로 저장된 값 참조
- 기존에 설치되어 있던 기지국을 하나씩 꺼내 전파의 범위를 계산할 것이기 때문에 Queue 자료구조를 사용한다.
- `sq.peek() - w <= position`: 기존에 설치되어 있던 기지국 전파범위의 왼쪽 끝보다 오른쪽이라면 position은 기지국 전파범위 안에 있다고 할 수 있다. 
- `sq.isEmpty()`: 큐가 다 소진되어서 끝날 수 있으니까 비어있는지 여부를 확인한다.



이 코드의 경우 효율성 테스트에서 실패한다. 효율성을 높이기 위해서는 어떻게 해야할까? 크게 3가지 방법이 있다.

- Loop 개선: 전체를 다 돌지 않고서도 처리할 수 있는지
- 적절한 자료구조 사용
- 불필요한 Object의 제거: **원시값을 사용하는 것이 훨씬 더 빠르다.**

여기서는 3번째 방법을 사용하였다. Queue 대신 배열을 사용하자.

```java
class Solution {
  public int solution(int n, int[] sations, int w) {
    int answer = 0;
    int si = 0;
    
    int position = 1;
    while (position <= n) {
      if (si < stations.length && stations[si] - w <= position) {
        position = stations[si] + w + 1;
        si += 1;
      } else {
        answer += 1;
        position += 2 * w + 1;
      }
    }
    return answer;
  }
}
```

