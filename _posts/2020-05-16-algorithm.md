---
layout: post
title: 프로그래머스 - 124 나라의 숫자
tags: [programmers, python]
date: 2020-05-16
---

# 124 나라의 숫자

### 문제 설명

124 나라가 있습니다. 124 나라에서는 10진법이 아닌 다음과 같은 자신들만의 규칙으로 수를 표현합니다.
124 나라에는 자연수만 존재합니다.124 나라에는 모든 수를 표현할 때 1, 2, 4만 사용합니다.
예를 들어서 124 나라에서 사용하는 숫자는 다음과 같이 변환됩니다.
자연수 n이 매개변수로 주어질 때, n을 124 나라에서 사용하는 숫자로 바꾼 값을 return 하도록 solution 함수를 완성해 주세요.

### 제한사항

n은 500,000,000이하의 자연수 입니다.
 
## 내 코드

```python
def solution(n):
    if n < 4 : return '124'[n-1]
    division = []
    while(n>3):
        q,r = divmod(n-1,3)
        n = q
        division.append(r)
        if n <= 3 : division.append(n-1)
    
    answer = ''
    while(division):
        d = division.pop()
        answer += '124'[d]

    return answer
```

---

### 문제 출처

> https://programmers.co.kr/learn/courses/30/lessons/12899