---
title: "System Design: Capacity Estimate"
categories: system-design
tags: [ system-design ]
---

## Capacity Estimation

### Capacity

- 8 bits = 1 byte
- 1024 bytes = 1 kb
- 1024 kb = 1 mb

- char = 1 byte
- int = 4 byte
- timestamp = 4 bytes

### Time

- 60 x 60 = 3,600s (hour)
- 3600 x 24 = 86,400s (day)
- 86,400s x 30 = 2,500,000s (month)

<br>

**CPU와 메모리 속도(절대시간, CPU cycle 대비 상대시간)**
| type      | time  | sec      |
|-----------|-------|----------|
| CPU cycle | 3ns   | 1s       |
| RAM       | 120ns | 6m       |
| SSD       | 150ns | 6days    |
| HDD       | 10ms  | 12 month |

![cpu-memory](https://user-images.githubusercontent.com/50407047/126295636-1c6d9d6a-dc51-49a4-9380-657c35d4e99b.png)

## 실제 사례

백엔드 개발자들이 고려해야 하는 부분

- 일일 total requests 수: 평균 DAU * reads(get) + write(post)

Daily Active Users(DAU): 하루 동안 해당 서비스를 이용한 순수 이용자 수

### Instagram을 예로 들어 보자.

하루에 30개의 포스트를 읽고, 1개의 포스트를 업로드한다고 해보면 일일 요청 수는 다음과 같다.

- 10 mil DAU x 30 photos = 300 mil photo get
- 10 mil DAU x 1 photo updates = 10 mil photo post

이에 1초당 요청 수를 계산하면

- 300 mil get / 86400s = 3472 reads per sec
- 10 mil post / 86400s = 115 writes per sec

보통 글로벌 대기업에서 읽기쓰기 비율을 **80(읽기):20(쓰기)**로 본다.

**캐시 개념 적용**

- 이때 팔로워 수에 따라 포스트가 얼마나 많이 노출되는 지는 다를 것이다. 이 비율을 20%라고 하자.
- 그럼 이 팔로워가 많은 사람들의 포스트들은 DB에서 바로 가져오는 것이 아니라 캐싱을 하고 그것을 가져오는 것이 더 효율적일 것이다 (실제 서비스에서도 이런 방식을 사용한다.)
- 대략적으로 나타내면, 일일 총 요청 트래픽은 300 mil req * 500 bytes(사진) * 0.2 = 30gb이다.

**레플리카 적용**

위의 계산은 한 서버에 국한된다. 하지만 서버가 다운되는 상황을 예방하기 위해 서버를 5개 만들었다 하면(replica), 30gb * 5 = 150GB라는 어마어마한 데이터량이다.

자, 생각해보자. 이것만 해도 어마어마한 데이터량인데 유저들은 사진만 보는 것이 아니라 영상도 보고, 포스팅도 하고, 대화도 한다. 이를 고려한다면 데이터량은 정말 많을 것이다. 이것을 감당하는 시스템을 디자인하는 것은 매우 중요하다.
