---
layout: post
title: 프로그래머스 - H-Index
tags: [programmers, sort, python]
date: 2020-04-03
---

# 문제 3. H-Index

### 문제 설명

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.
 
## 내 코드

```python
def solution(citations):
    citations.sort()
    h = []
    l = len(citations)
    i = int(l/2) #2
    if citations[0]>=l : return l # add
    
    while(i):
        if citations[l-i]>= i:
            h.append(i)
            i += 1
        else : break

    if len(h) == 0 : return 0 
    h.sort()
    return h[-1]
```

먼저 각 논문의 인용수를 sort 메소드를 이용해 오름차순 정렬한다.
만약 0번째 인용 수가 논문 편 수보다 크다면 모든 인용 횟수가 논문 갯수보다 큰 것이므로 논문 갯수를 리턴하면 된다.

그렇지 않다면 while문으로 들어간다. l-i번째의 논문 인용 수가 i보다 크면 i 이상으로 인용된 논문이 i개 이상이라는 것이다.
그러면 h 배열에 넣어두고 i를 1씩 증가한다. 그렇지 않으면 break. h에 넣어둔게 없으면 return 0
아니면 가장 큰 h를 리턴한다.

---

### 문제 출처

> https://programmers.co.kr/learn/courses/30/lessons/42747