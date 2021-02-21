---
title: "해시(Hash): 위장"
categories: algorithm
tags: [ algorithm, programmers ]
---

프로그래머스의 [코딩테스트 광탈 방지 Kit: Java편](https://programmers.co.kr/learn/courses/10302) 강의를 참고하여 작성하였습니다.

### 내가 푼 방법

```java
public static int solution(String[][] clothes) {
  int answer = 1;
  int count = 0;
  Map<String, Integer> map = new HashMap<>();
  for (int i = 0; i < clothes.length; i++) {
    map.put(clothes[i][1], count++);
  }

  for (String key : map.keySet()) {
    answer *= (map.get(key) + 1);
  }

  return answer - 1;
}
```

이러면 샘플 테스트 1번만 맞고 다른 것은 다 틀리다. 여기서 내가 실수한 것이 있는데, count라는 하나의 변수를 재사용한다는 것이다. 이 경우 한 키값에서 사용한 count가 다른 키값에서도 재사용되기 때문에 0부터 시작하지 않는다. 이를 위해서는 각 key에 해당하는 value가 null이면 0을 넣고 1을 증가시키고, null이 아니면1을 증가시키기만 하도록 만들어 줘야 한다. 

### 수업

- 각 종류들을 조합해서 위장을 한다. 모든 경우의 수가 몇 가지가 되는지 물어보는 문제이다. 
- 위장용품은 착용할 수도 있고 안 할 수도 있다. 
- 착용 안할 수도 있으니까 경우의 수에 1을 더해서 모두 곱해주면 된다. 
- 위장 용품을 모두 착용하지 않는 경우도 있으니 이 경우 나중에 1을 빼준다.
- 문제의 핵심은 경우의 수를 물어보는 것이 아니라, 아이템의 종류별로 개수를 카운트하는 것이다.
- 문제에서는 headgear 종류가 2개가 있고, eyewear가 1개가 있다. 이름은 중요하지 않고, 종류별로 총 몇 개가 나오는 지 카운트하는 것이 이 문제의 핵심이다.
- 쉽게 생각하면, 배열 몇 개를 만들어서, 얼굴용은 1번 인덱스에, 상의용은 2번 인덱스에 저장하는 식으로 카운팅할 수도 있지만, 종류가 미리 주어지지 않았다면 할 수 없다.
- 이 경우 인덱스가 아니라 키 값으로 정보를 저장할 수 있는 해시를 사용하는 것이 좋다.
- 해시? 배열에 특정 값을 담을 때 어떤 인덱스에 해당 값이 있는 지 찾기 위해서 처음부터 끝까지 모두 탐색을 해야 한다. 매번 인덱스에 접근할 때마다 탐색을 하는 것은 굉장히 비효율적이다. 그래서 해시에서는 key값을 사용한다. key로부터 Hash값을 얻어 Hash값을 인덱스로 사용하는 것이다. 언제든지 해시값만 얻을 수 있다면 탐색 없이 인덱스로 접근 가능하다.
- 여기서 Hash값은 Hash 함수를 통해서 얻은 값을 말한다. 
- Hash함수란 최대한 겹치는 값이 발생하지 않도록 유니크 값을 생성해내는 함수이다. 특정 Key에 Hash값을 얻어서 배열의 index로 사용하는 자료구조를 Hash라고 한다.
- Hash 자료구조는 배열, 리스트, 탐색, 해시 요소를 복합적으로 사용해야 한다. 그래서 자료구조를 질문할 때 Hash 하나만 물어봐도 잘 이해하고 있는 지 알 수 있다. 기술 면접에도 단골 문제이다. 
- 자바에서는 Hash를 사용한 자료구조를 Map이라고 부른다. 

```java
public int solution(String[][] clothes) {
  Map<String, Integer> counts = new HashMap<>();
  
  for (String[] c : clothes) {
    String type = c[1];
    //counts.put(type, counts.get(type) == null ? 0 : counts.get(type) + 1);
    counts.put(type, counts.getOrDefault(type, 0) + 1);
  }
  
  int answer = 0;
  
  for (Integer c : counts.values()) {
    // 위장용품을 사용하지 않는 경우도 있으니까 + 1
    answer *= c + 1;
  }
  // 위장용품 모두 사용하지 않는 경우는 없으니 - 1
  answer -= 1; 
  return answer;
}
```

- `counts.getOrDefault(type, 0)`: `type`에 해당하는 값이 null 이라면 default 값으로 0을 가지고, 그리고 그것에 1을 더해서 `put`한다. `counts.get(type) == null ? 0 : counts.get(type) + 1`과 같다.
- `counts.values()`: Map 자료구조에는 `values()`라는 메서드가 있다. 이 메서드는 해당 Map의 value들만 모아 Collection으로 리턴한다.
- `answer *= c + 1`: 위장 용품을 사용하지 않는 경우도 카운트하기 위해 1을 더해주었다.
- `answer -= 1`:  위에서 계산한 answer 값은 위장 용품을 모두 사용하지 않는 경우도 포함한다. 문제에 따르면 위장 용품을 하나라도 사용해야 하니 1을 빼주었다. 

stream을 사용하여 리팩토링해보자.

```java
public int solution(String[][] clothes) {
  Map<String, Integer> counts = new HashMap<>();
  
  String type;
  // 옷만 추려서 본다.
  Arrays.stream(clothes).filter(c -> c[1].equals(type)).count();
  
  for (String[] c : clothes) {
    String type = c[1];
    //counts.put(type, counts.get(type) == null ? 0 : counts.get(type) + 1);
    counts.put(type, counts.getOrDefault(type, 0) + 1);
  }
  
  int answer = 0;
  
  for (Integer c : counts.values()) {
    // 위장용품을 사용하지 않는 경우도 있으니까 + 1
    answer *= c + 1;
  }
  // 위장용품 모두 사용하지 않는 경우는 없으니 - 1
  answer -= 1; 
}
```

Stream을 이용하여 리팩토링해보자.

```java
public int solution(String[][] clothes) {
  return Arrays.stream(clothes)
    .map(c -> c[1])
    .distinct()
    .map(type -> (int) Arrays.stream(clothes).filter(c -> c[1].equals(type)).count())
    .map(c -> c + 1)
    .reduce(1, (c, n) -> c * n) - 1;
}
```

- 전체 옷을 stream을 사용하여 filter로 첫번째 요소가 타입과 같은지 보고 그것의 카운트를 구하면 type의 개수를 구할 수 있다. 
- 각 타입은 전체 위장용품 중에서 1번 인덱스에 있는 값이고, 이것이 여러개가 나올 수 있으니 중복을 제거해야 한다. 그리고 여기에 type값을 
- `Arrays.stream(clothes)`: 모든 옷의 종류가 있는데 
- `map(c -> c[1])`: 그 중에 1번 인덱스에 있는 type만 꺼내서 
- `distict()`: 중복 없이 가져온다.
- `map(type -> (int) Arrays.stream(clothes).filter(c -> c[1].equals(type)).count())`:  이 type에 해당하는 것만 필터링해서 count를 얻어오면 종류별로 몇 개인지 알 수 있고,
- `map(c -> c + 1)`: 그것에 1을 더해서
- `reduce(1, (c, n) -> c * n)`: 모두 누적해서 곱한 값을 구한다.

### 결론

**Tip 코딩 스킬을 향상시키는 방법**

- 문제를 풀이한다.
- 다른 방식으로 풀어본다.
- 다른 표현으로 바꿔본다. 