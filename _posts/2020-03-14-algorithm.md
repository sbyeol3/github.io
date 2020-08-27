---
layout: post
title: 프로그래머스 - 더 맵게
tags: [programmers, heap, python]
date: 2020-03-14
---

# 문제 1. 더 맵게

### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.
 
### 제한 사항

- scoville의 길이는 1 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.
 
## 내 코드

```python
import heapq
def solution(scoville, K):
    mixtree = []
    mix = 0

    if min(scoville) >= K : return 0
    for s in scoville : 
        heapq.heappush(mixtree, s)
    
    while(len(mixtree)>1):
        min1 = heapq.heappop(mixtree)
        min2 = heapq.heappop(mixtree)
        newScoville = min1 + min2 * 2
        mix = mix + 1
        heapq.heappush(mixtree, newScoville)
        if mixtree[0] >= K : return mix

    return -1
```

### 로직 설명

파이썬 내장모듈인 heapq를 임포트하고 mixtree(최소 힙 자료구조를 사용하는 리스트)와 섞는 횟수인 mix를 선언한다.
가장 작은 원소가 이미 K 이상이라면 섞을 필요가 없으므로 0을 리턴한다.
그렇지 않다면 섞어야 하므로 heappush 메소드를 이용하여 min heap tree를 만든다.

가장 작은 원소와 두번째로 작은 원소를 꺼내서 새로운 스코빌지수를 만든다. 이때 섞는 횟수도 +1해준다.
새로 만든 원소를 트리에 넣고 인덱스 접근으로 최솟값을 가져와 K 이상인지 비교한다.
K 이상이라면 mix를 리턴하고 그렇지 않으면 계속 루프를 돌다가 1개의 원소가 남아 while문을 나오면 -1를 리턴한다.
