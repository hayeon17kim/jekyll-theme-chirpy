---
title: "깊이우선탐색(DFS):괄호"
categories: algorithm
tags: [ algorithm, programmers ]
---

## 문제

괄호 쌍의 개수 n이 주어질 때, n개의 괄호 쌍으로 만들 수 있는 모든 가능한 괄호 문자열의 갯수를 반환하는 함수 작성하기

##### 제한사항

- 괄호 쌍의 개수 N : 1 ≤ n ≤ 14, N은 정수

##### 입출력 예

| n    | result |
| ---- | ------ |
| 2    | 2      |
| 3    | 5      |

##### 입출력 예 설명

입출력 예 #1
2개의 괄호쌍으로 [ "(())", "()()" ]의 2가지를 만들 수 있다.
입출력 예 #2
3개의 괄호쌍으로 [ "((()))", "(()())", "(())()", "()(())", "()()()" ]의 5가지를 만들 수 있다.

## 깊이우선탐색(Depth-First Search, DFS)

깊이우선탐색과 너비우선탐색은 맹목적인 탐색 기법이라고도 하는데, 원하는 결과를 도출하기 위한 방향 없이 해당 노드에서 모든 경우에 대해 탐색을 수행하기 때문이다.

깊이에 우선한다는 것은, 

- 현재 노드에 연결된 다음 레벨의 노드를 확인한 후, 곧바로 현재 노드 위치를 선택된 다음 레벨의 노드로 변경한다. 
- 이때, 다음 레벨의 노드에서 원하는 결과를 찾지 못할 때 **돌아올 수 있도록** 현재 노드의 위치를 **스택(Stack)**을 사용해 기억한다.
- 따라서 재귀함수의 형태로 사용하는 편이다.
- <mark>이전의 부모 노드로 돌아오는 과정</mark>을 **백트래킹(Back-Tracking)**이라고 한다. 

깊이 우선 탐색의 경우

- <mark>그래프 높이 만큼의 공간만을 요구한다. 따라서 최대 저장공간의 수요가 적다.</mark>
- 목표 노드가 깊은 단계에 있을 경우 빠르게 해를 구할 수 있다.
- 해가 없는 경로에 깊이 빠질 수 있다.
- <mark>얻어진 해가 최단 경로가 된다는 보장이 없다.</mark> 깊이 우선 탐색은 해에 다다르면 탐색을 끝내버리기 때문이다.

## 강의

N이 2일 경우, 주어진 괄호를 하나씩 쌓아가는 것을 찾아보자. 

- (  
  - ( 
    - )
      - )  => o
  - ) 
    - ( 
      - ) => o
    - ) => x, 닫히는 괄호가 먼저 나왔다.
- )  => x , 닫히는 괄호가 먼저 나왔다.

 **규칙**

- 열리는 괄호를 배치한다.
- 그 이후에 닫히는 괄호를 배치한다.
- 괄호는 N개 이상 사용할 수 없다.
- 열리기 전에는 닫히는 괄호를 사용할 수 없다.
  즉, **닫히는 괄호의 개수가 열리는 괄호의 개수보다 클 수 없다.**

즉, 중요한 것은 <mark>괄호의 개수로 문제를 접근</mark>한다는 것이다!

```java
import java.util.*;

class Solution {
  class P {
    int open;
    int close;
    P (int open, int close) {
      this.open = open;
      this.close = close;
    }
  }
  
  public int solution(int n) {
    int answer = 0;
    
    Stack<P> stack = new Stack<>();
    stack.push(new P(0, 0));
    while (!stack.isEmpty()) {
      P p = stack.pop();
      if (p.open > n) continue;
      if (p.open < p.close) continue;
      if (p.open + p.close == 2*n) {
        answer++;
        continue;
      }
      stack.push(new P(p.open + 1, p.close));
      stack.push(new P(p.open, p.close));
    }
  }
}
```

- `class P`: 열리는 괄호의 개수, 닫히는 괄호의 개수를 추적해야 한다. 따라서 열리는 괄호의 개수와 닫히는 괄호의 개수를 담고 있는 클래스 P를 정의한다. 

- `stack.push(new P(0, 0))`:시작값은 괄호의 개수가 모두 0

- `!stack.isEmpty()`: 스택이 모두 빌 때까지 루프를 돈다.

- `stack.push(new P(p.open+1, p.close))`
  `stack.push(new P(p.open, p.close+1))`

  다음 케이스는 열리는 괄호를 하나 더 넣고, 닫히는 괄호를 하나 더 넣도록 만든다. 열리는 괄호를 넣었으니 open(카운트)가 증가하고, 닫히는 괄호를 넣으니 close(카운트)가 증가하는 방식이다. 

- `p.open + p.close = 2*n`: **종료조건**: 주어진 괄호를 모두 다 쓰면 종료한다. 이 경우 다음 경우를 생각하지 않아도 되니 루프의 push 부분을 실행하지 않는다. 올바르게 열리고 닫혀서 모두 소진한 상태가 올바른 괄호의 상태이다. 이때 answer의 값을 증가시킨다. 

- `p.open < p.close`: 닫히는 괄호가 열리는 괄호보다 먼저 나오면 안된다. 즉, 닫히는 괄호는 열리는 괄호보다 항상 작으므로 그렇지 않은 경우 탐색을 그만두고 다시 위로 올라간다. 

- `p.open > n`: 열리는 괄호의 개수는 항상 n보다 작다. 따라서 open이 n보다 커질 경우 탐색을 중단하고 다시 위로 올라간다.  

사실 이 문제는 너비우선 탐색을 사용해도 똑같은 결과를 얻을 수 있다.

```java
import java.util.*;

class Solution {
  class P {
    int open;
    int close;
    P (int open, int close) {
      this.open = open;
      this.close = close;
    }
  }
  
  public int solution(int n) {
    int answer = 0;
    
    Queue<P> queue = new LinkedList<>();
    queue.offer(new P(0, 0));
    while (!queue.isEmpty()) {
      P p = queue.poll();
      if (p.open > n) continue;
      if (p.open < p.close) continue;
      if (p.open + p.close == 2*n) {
        answer++;
        continue;
      }
      queue.offer(new P(p.open + 1, p.close));
      queue.offer(new P(p.open, p.close));
    }
  }
}
```

### 결론

너비우선탐색과 깊이우선탐색은

- **비선형 데이터를 순회**하는 방법이다.
- 순서가 다르다.
- 순서를 무시하면 **결과는 동일하다.**

