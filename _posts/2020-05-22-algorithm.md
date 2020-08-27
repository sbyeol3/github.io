---
layout: post
title: 프로그래머스 - 최댓값과 최솟값
tags: [programmers, python]
date: 2020-05-22
---

# 최댓값과 최솟값

### 문제 설명

문자열 s에는 공백으로 구분된 숫자들이 저장되어 있습니다. str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 (최소값) (최대값) 형태의 문자열을 반환하는 함수, solution을 완성하세요.
예를들어 s가 "1 2 3 4"라면 "1 4"를 리턴하고, "-1 -2 -3 -4"라면 "-4 -1"을 리턴하면 됩니다.

### 제한조건

s에는 둘 이상의 정수가 공백으로 구분되어 있습니다.
 
## 코드

```python
def solution(s):
    params = list(map(int,s.split(" ")))
    return str(min(params)) + " " + str(max(params))
```

### 로직

띄어쓰기를 기준으로 숫자가 문자열에 있으므로 split으로 나눠주고 한번에 형변환을 하기 위해 map 함수를 이용한다.
이를 다시 list로 감싸줘야 타입 에러가 나지 않는다.
min(), max()로 간단하게 최소 최대를 찾아주고 문자열로 리턴한다.

---

### 문제 링크 

> https://programmers.co.kr/learn/courses/30/lessons/12939