---
layout: post
title: 프로그래머스 - 가장 큰 수
tags: [programmers, sort, python]
date: 2020-02-20
---

# 문제 2. 가장 큰 수

### 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이 중 가장 큰 수는 6210입니다. 0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

### 제한 사항

-numbers의 길이는 1 이상 100,000 이하입니다.
-numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.
 
> 안풀려.... 모르겠어........ 대가리가 안돌아감....... 그래서 코드 봄...........
오늘은 다른 사람의 코드를 뜯어보며 이해하기로 함^^

## 다른 사람 코드
 
```python
def solution(numbers):
    numbers = list(map(str, numbers))
    numbers.sort(key = lambda x: x*3, reverse=True)
    return str(int(''.join(numbers)))
```

👍 wow......~~!

```python
numbers = list(map(str, numbers))
```

어제 공부했던 map 함수.. numbers 배열을 str으로 변환하게끔 매핑하여 배열로 리턴함

```python
numbers.sort(key = lambda x: x*3, reverse=True)
```

> 어제 공부한 람다...
list.sort()와 sorted()는 모두 비교하기 전에 각 리스트 요소에 대해 호출할 함수를 지정하는 key 매개 변수를 가지고 있습니다.

x * 3 -> 문자열에 3을 곱해주면 해당문자열을 3개 나열하는 것과 같으니 '333', '303030', '343434', '555', '999'를 key로 넣어주면.

정렬을 하면 303030 -> 333 -> 343434 -> 555 -> 999가 될 것인데 reverse=True로 해서 거꾸로 정렬된 결과가 리턴됨.

```python
numbers = ['9', '5', '34', '3', '30']
return str(int(''.join(numbers)))
``` 

python에서 join 함수는 배열을 특정 문자로 구분하여 문자열로 변환해주는 함수이다.
근데 왜 string을 int로 변환하고 다시 string으로 변환해서 리턴하는지 이해가 안됐는데
그렇게 join만 사용하면 0일 때가 문제다.

[0,0,0,0] 을 input으로 넣는다면 '0000'이 리턴되므로 int로 변환해서 '0'으로 바꿔야 한다.
그리고 오버플로우 방지를 위해 다시 str으로 변환해야 하는 것!!!!

---

> https://programmers.co.kr/learn/courses/30/lessons/42746