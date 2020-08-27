---
layout: post
title: 프로그래머스 - 다음 큰 숫자
tags: [programmers, python]
date: 2020-05-20
---

# 다음 큰 숫자

### 문제 설명

자연수 n이 주어졌을 때, n의 다음 큰 숫자는 다음과 같이 정의 합니다.

- 조건 1. n의 다음 큰 숫자는 n보다 큰 자연수 입니다.
- 조건 2. n의 다음 큰 숫자와 n은 2진수로 변환했을 때 1의 갯수가 같습니다.
- 조건 3. n의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수 입니다.

예를 들어서 78(1001110)의 다음 큰 숫자는 83(1010011)입니다.
자연수 n이 매개변수로 주어질 때, n의 다음 큰 숫자를 return 하는 solution 함수를 완성해주세요.

### 제한사항

- n은 1,000,000 이하의 자연수 입니다.
 
## 코드

```python
def solution(n):
    binary = str(bin(n))[2:]
    if len(set(binary)) == 1 : return int('10' + binary[1:],2)
    one = binary.count('1')

    while(True):
        n += 1
        if str(bin(n))[2:].count('1') == one : return n
```

### 로직

bin을 이용하여 2진수로 표현하면 앞에 0b가 붙기 때문에 순수한 이진수만 가져오기 위해 slice 해주었다.
생각해보니까 bin은 문자열을 리턴하기 때문에 str를 붙여서 형변환을 해주지 않아도 된다.

만약 binary의 set의 길이가 1이라는 것은 1로만 이루어진 이진수이므로 앞에 1을 추가해주고
그 다음 자리의(=원래 가장 앞에 있던) 1을 0으로 바꿔주면 된다.
그렇지 않은 경우는 1과 0이 혼합되어 있는 이진수이다.

사실 별 귀찮은 방법으로 로직을 생각했었는데 단순하게 생각하면 되는 문제였다.
n에 1을 더 해가면서 count 함수를 이용해 1의 갯수가 같은 숫자를 찾으면 리턴했다.
count 메소드를 생각 못해서 일일이 for문에서 비교해주려다가 갑자기 생각났다. 헛짓거리 할뻔..

## 다른 사람 코드

```python
def nextBigNumber(n):
    c = bin(n).count('1')
    for m in range(n+1,1000001):
        if bin(m).count('1') == c:
            return m
```

내가 앞에서 set을 고려한 부분이 없다! 뭐가 더 나은지는 잘 모르겠다.

---

### 문제 출처

> https://programmers.co.kr/learn/courses/30/lessons/12911