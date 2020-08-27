---
layout: post
title: 프로그래머스 - 짝지어 제거하기
tags: [programmers, python]
date: 2020-05-26
---
자료구조에 따른 시간복잡도의 중요성을 느낀 문제.. 3000등대로 떨어졌다가 다시 2000등대 진입! 예! 

# 짝지어 제거하기

### 문제 설명

짝지어 제거하기는, 알파벳 소문자로 이루어진 문자열을 가지고 시작합니다. 먼저 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾습니다. 그다음, 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙입니다. 이 과정을 반복해서 문자열을 모두 제거한다면 짝지어 제거하기가 종료됩니다. 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성해 주세요. 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴해주면 됩니다.

예를 들어, 문자열 S = baabaa 라면 b aa baa → bb aa → aa → 의 순서로 문자열을 모두 제거할 수 있으므로 1을 반환합니다.

### 제한 조건

- 문자열의 길이 : 1,000,000이하의 자연수
- 문자열은 모두 소문자로 이루어져 있습니다.

> 처음에는 o(n^2)로 했더니 시간초과로 실패했다. 질문하기에 Stack으로 풀라는 조언이 있어서 스택으로 풀었더니 O(n)으로 통과했다.

> 그 놈의 시간복잡도 ^_ㅠ 😭

## 코드

```python
def solution(s):
    s = list(reversed(list(s)))
    stack = [s.pop()]
    while s :
        c = s.pop()
        if len(stack) == 0 or stack[-1] != c : stack.append(c)
        else : stack.pop()

    if len(stack) == 0 : return 1
    else : return 0
```

pop을 편하게 하기 위해서 reversed를 해주고 list로 변환한다. input s가 'baabaa' 라면 s는 'aabaab'가 된다.
먼저 s의 top을 넣어준 스택을 하나 선언한다.
while 문 안에서 s에서 pop을 하고 stack의 top에 있는 원소와 비교한다.

단, stack이 비어있을 수 있으니까 len도 체크하고 같지 않으면 stack에 넣는다. 같으면 pop 해준다.
while 문이 끝나고 stack에 원소가 남아있다면 짝지어 제거하는 것이 불가능한 것이므로 return 0

---

### 문제 링크

> https://programmers.co.kr/learn/courses/30/lessons/12973