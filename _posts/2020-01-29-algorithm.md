---
layout: post
title: [프로그래머스/python] 완주하지 못한 선수
tags: [programmers, hash, python]
---

백준 알고리즘으로 C언어 공부를 하고 있는데 파이썬도 틈틈이 해보기 위해 프로그래머스를 시작하기로 했다.
일단 첫 문제부터 푸는 것 자체는 어렵지 않은데 '효율성' 이 친구가 아주 어렵다. 정확성은 맞는데 효율성이 0점이라 결국 코드를 계속 고치고 고치고 다른 분들의 코드를 봐야 했다. 후...

# 문제 1. 완주하지 못한 선수
 
### 문제 설명

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때,
완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
 
### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

## 작성한 코드

```python
def solution(participant, completion):
  nonCompletion = dict()
  for player in set(participant): nonCompletion[player] = 0

  for player in participant :
    if (player not in completion) : nonCompletion[player] += 1
    else : completion.remove(player)

  answer = ''
  for player in nonCompletion :
    answer += player * nonCompletion[player]
  
  return answer
```

### 내가 생각한 로직

nonCompletion는 dictionary다.
key는 선수의 이름, value는 key의 이름을 가지는 선수 중에 완주하지 못한 선수의 수 (동명이인 고려)이다.
그리고 참여자 배열을 집합으로 하여 중복을 없애고 key로 넣고 value를 0으로 초기화한다. 

다음은 for 문을 돌면서 완주자 배열에 없는 player는 nonCompletion의 value를 1씩 증가시키고
완주자 배열에 있으면 player를 완주자 배열에서 제거한다. (동명이인이 있어서 제거를 해줬다.)
마지막으로 answer 문자열에 value만큼 key를 더해주었다. 

**-> 이렇게 하면 정확성은 만점인데 효율성은 0점이다.. ^^**

## 다른 분들의 코드를 참고하면서 수정한 코드

### sort 이용

```python
def solution(participant, completion):
    participant.sort()
    completion.sort()
    num = len(completion)

    for idx in range(num) :
        if participant[idx] != completion[idx] : return participant[idx]
    return participant[num]
```

-> 문제 설명 중에 'completion의 길이는 participant의 길이보다 1 작습니다.'가 있었다... 하나만 찾으면 되는 거 였다..ㅋ

-> 혹시 sort를 했는데 마지막 원소가 정답인 경우에는 if에서 걸리지 않는다. 그래서 return을 하나 더 추가했다.

**-> sort()를 잘 써먹자...**


### Counter 이용

```python
from collections import Counter
def solution(participant, completion): 
  answer = Counter(participant) - Counter(completion) 
  return list(answer.keys())[0]
```
-> ㅇㅅㅇ???????????????????????????....... 함수는 틈틈이 알아두자...

## 새로 알게 된 부분

#### hash(object)

- 해시값이 있는 경우 객체의 해시값 리턴, 해시값은 정수(int)
- 딕셔너리 키를 빨리 비교하는 데 사용
- 1과 1.0은 데이터 타입이 다름에도 같은 해시값을 가짐

>참고 : https://docs.python.org/ko/3.6/library/functions.html#hash

### class collections.Counter([iterable-or-mapping])

- dict의 서브클래스, 해시테이블의 객체를 카운팅하는 데 사용
- 원소가 key로 저장되고 원소들의 counts가 value로 저장되어 있으며 순서가 없는 collection
- 0이나 음의 정수 또한 value에 사용할 수 있음
- Counter끼리 더하거나(+) 빼면(-) 같은 key에 대해 value가 연산이 됨, 교집합(&) 연산과 합집합(|)도 가능함
- `+[Counter], -[Counter]`도 가능
- elements()	value만큼 key를 iterate하게 return한다. value가 1보다 작으면 해당 key를 리턴하지 않음
  - ```c = Counter(a=4, b=2, c=0, d=-2) >>> sorted(c.elements()) ['a', 'a', 'a', 'a', 'b', 'b']```

most_common([n])	가장 value가 큰 n개의 list를 리턴, n을 생략하면 모든 원소를 리턴, n이 같을 땐 임의로 리턴
 

 
### 참고
- https://docs.python.org/ko/3.6/library/collections.html?highlight=counter#collections.Counter
- 코딩테스트 URL : https://programmers.co.kr/learn/courses/30/lessons/42576
 