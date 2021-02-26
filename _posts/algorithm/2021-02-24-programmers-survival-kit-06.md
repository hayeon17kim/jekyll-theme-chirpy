---
title: "너비우선탐색(BFS): 게임 맵 최단거리"
categories: algorithm
tags: [ algorithm, programmers ]
---

## 문제

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution함수를 완성해야 한다. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 한다.

- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수이다.
  - n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않는다.
- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타낸다.
- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있다.

입출력 예

| maps                                                         | answer |
| ------------------------------------------------------------ | ------ |
| [[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,1],[0,0,0,0,1]] | 11     |
| [[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,0],[0,0,0,0,1]] | -1     |

## 너비우선탐색(Breadth-First Search, BFS)

너비 우선 탐색을 쓰는 경우

- 최단 경로를 구할 때,<mark> **목적지를 찾자마자 최단경로임이 보장되어 탐색을 종료할 수 있는 장점**이  있다.</mark> 따라서 DFS에 비해 빠른 경우가 많고
- 보통 **재귀를 통해 시스템스택을 사용하는 DFS**에 비해 queue를 사용하기 때문에 **스택오버플로에서 비교적 자유**롭고 **힙공간을 넓게 쓸 수 있어 좀 더 넓은 범위 탐색에 유리**하다.

## 강의

- 미로의 **시작점에서 목적지까지 가는 최단 거리**를 구하는 문제이다.
- **너비 우선 탐색(BFS)**의 전형적인 문제이다.
- **전체적으로 모든 정보를 다 확인**하는 방식으로 해결한다.
- 맵에서 플레이어가 특정 위치에 있을 때 이동할 수 있는 경우의 수는 4가지가 있다. 이 4가지 경우의 수를 모두 다 확인한다. 한 칸씩 움직인 후 다음 경우의 수를 생각한다.
- 현재 시작점의 위치를 0으로 두고, 1씩 더해서 이동하게 한다. 이렇게 이동한 이후에 그 위치에서 갈 수 있는 경우의 수를 본다. 다시 돌아갈 수는 없고 판을 넘어갈 수는 없다. 이런 식으로 범위를 확장한다.
- 너비 우선 탐색이나 깊이 우선 탐색은 한꺼번에 여러 값이 움직이기 때문에 복잡하다. 그러나 데이터 값을 추적하지 말고 규칙만 생각하면 된다. 모든 경우에 똑같이 적용할 수 있는 규칙만 생각하고 종료 조건을 잘 세워 두기만 하면 된다.
- 규칙
  - 네 방향으로 한 칸씩 이동한다.
  - 이동한 후에는 현재 값보다 1 큰 값을 채운다.
  - 벽, 미로 밖, 왔던 길은 못 간다.
  - 종료 조건: 이동한 위치가 목적지이다.
- 복잡한 데이터를 계산하는 것은 사람에게는 어렵지만 컴퓨터에게는 쉽다. 규칙을 세우는 것은 사람에게는 쉽지만 컴퓨터에게는 어렵다. 따라서 우리는 규칙을 만들고 계산은 컴퓨터에게 시키면 된다.
- 너비 우선 탐색은 Queue를 사용한다. 
- 자바에서는 Queue를 인터페이스로만 제공해준다. Queue 인터페이스를 구현하고 있는 구조 중 LinkedList를 사용해보자.
- **플레이어가 움직이고 있는 현재 위치를 큐에 저장**하자.
- 두 개의 값을 동시에 사용하기 위해 클래스(Position)를 정의하였다.

```java
public Solution {
  class Position {
    int x, int y;
  }
  
  public int solution(int[][] maps) {
    // BFS: Queue
    
    Queue<Position> queue = new LinkedList<>();
    
    queue.offer(new Position(0, 0));
    
    while(!queue.isEmpty()) {
      Position current = queue.poll();
      
      // 4가지 경우
      queue.offer(new Position(current+1, current));
      queue.offer(new Position(current-1, current));
      queue.offer(new Position(current, current+1));
      queue.offer(new Position(current, current-1));
    }
  }
}
```

- BFS 문제를 푸는 가장 기본적인 형태이다.
- Queue를 이용해서 Queue에 초기값을 넣고 큐가 빌 때까지 루프를 돌면서 처리할 수 있는 값을 계속 큐에 집어넣는 방식
- 이동 경로의 거리가 필요하기 때문에 count 값이 필요하다.

{% raw %}

```java
import java.util.*;

class Solution {
    
    class Position {
        int x, y;
        Position (int x, int y) {
            this.x = x;
            this.y = y;
        }
        boolean isValid(int width, int height) {
            if (x < 0 || x >= width) return false;
            if (y < 0 || y >= height) return false;
            return true;
        }
    }
    
    public int solution(int[][] maps) {
        int mapHeight = maps.length;
        int mapWidth = maps[0].length;
        
        Queue<Position> queue = new LinkedList<>();
        
        int[][] count = new int[mapHeight][mapWidth];
        boolean[][] visited = new boolean[mapHeight][mapWidth];
		
        queue.offer(new Position(0, 0));
        count[0][0] = 1;
        visited[0][0] = true;
        
        while (!queue.isEmpty()) {
            Position current = queue.poll();
            int currentCount = count[current.y][current.x];
            int[][] moving = {{1, 0}, {-1,-0}, {0, 1}, {0, -1}};
            for (int i = 0; i < moving.length; i++) {
                Position moved = new Position(current.x + moving[i][0], current.y + moving[i][1]);
                // 검증
                if (!moved.isValid(mapWidth, mapHeight)) continue;
                if (visited[moved.y][moved.x]) continue;
                if (maps[moved.y][moved.x] == 0) continue;
                
                visited[moved.y][moved.x] = true;
                count[moved.y][moved.x] = currentCount + 1;
                queue.offer(moved);
            }
        }
        
        int answer = count[mapHeight - 1][mapWidth - 1];
        if (answer == 0) return -1;
		return answer;
	}
}
```

{% endraw %}