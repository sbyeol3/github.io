---
layout: post
title: 프로그래머스 - 탑
tags: [programmers, stack, queue, python]
date: 2020-01-31
---

# 문제 1. 탑

### 문제 설명

수평 직선에 탑 N대를 세웠습니다. 모든 탑의 꼭대기에는 신호를 송/수신하는 장치를 설치했습니다.
발사한 신호는 신호를 보낸 탑보다 높은 탑에서만 수신합니다.
또한, 한 번 수신된 신호는 다른 탑으로 송신되지 않습니다.

예를 들어 높이가 6, 9, 5, 7, 4인 다섯 탑이 왼쪽으로 동시에 레이저 신호를 발사합니다. 그러면, 탑은 다음과 같이 신호를 주고받습니다. 높이가 4인 다섯 번째 탑에서 발사한 신호는 높이가 7인 네 번째 탑이 수신하고, 높이가 7인 네 번째 탑의 신호는 높이가 9인 두 번째 탑이, 높이가 5인 세 번째 탑의 신호도 높이가 9인 두 번째 탑이 수신합니다. 높이가 9인 두 번째 탑과 높이가 6인 첫 번째 탑이 보낸 레이저 신호는 어떤 탑에서도 수신할 수 없습니다.

맨 왼쪽부터 순서대로 탑의 높이를 담은 배열 heights가 매개변수로 주어질 때 각 탑이 쏜 신호를 어느 탑에서
받았는지 기록한 배열을 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- heights는 길이 2 이상 100 이하인 정수 배열입니다.
- 모든 탑의 높이는 1 이상 100 이하입니다.
- 신호를 수신하는 탑이 없으면 0으로 표시합니다.

## 나의 코드

```python
import copy

def solution(heights):
  laser = []
  answer = [0]*len(heights)

  for t in heights: laser.append(t)
  for t in heights:
    temp = laser.pop()
    tmpStack = copy.deepcopy(laser)
    while(len(tmpStack)>0):
      if tmpStack.pop() > temp :
        answer[len(laser)] = len(tmpStack)+1
        break
  
  return answer
```

> 채점 결과 : 정확성: 100.0 => 합계: 100.0 / 100.0

### 나의 로직

laser는 스택으로 사용될 리스트이다. push는 append, pop은 그대로 pop 함수를 이용해서 구현한다.
먼저 heights의 원소들을 인덱스 순서 그대로 laser에 push한다. (=append)

그 후 하나씩 pop하고 laser를 깊게 복사한 tmpStack을 while문에서 하나씩 pop해서 temp보다 큰 수가 나올 때까지 찾는다.
만약 큰 탑이 없는 경우에는 초기화할 때 넣어줬던 '0'이 남아있게 된다.

## 새로 알게 된 부분

### copy() vs. deepcopy()
특징 | copy(x) | deepcopy(x) 
--- | --- | ---
return | x의 얕은 복사 (a shallow copy of x) | x의 깊은 복사 (a deep copy of x)
construction | a new compound object | a new compound object
insertion | references | copies

---

### 참고자료
- 참고 : https://docs.python.org/ko/3/library/copy.html
- 문제 링크  https://programmers.co.kr/learn/courses/30/lessons/42588?language=python3