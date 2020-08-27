---
layout: post
title: 프로그래머스 - K번째 수
tags: [programmers, sort, python]
date: 2020-02-19
---

# 문제 1. K번째수

### 문제 설명

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면
array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.

1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- ommands의 각 원소는 길이가 3입니다.


> ㅋㅋㅋㅋㅋㅋㅋ무난하게 풀긴 했는데 다른 사람 풀이가 대박인 문제...ㅎㅎ........

 
## 나의 코드

```python
def solution(array, commands):
    sliceArray = []
    answer = []

    for command in commands :
        i = command[0]-1
        j = command[1]
        sliceArray.append(array[i:j])

    for arr in sliceArray:
        arr.sort()

    for i in enumerate(commands):
        answer.append(sliceArray[i[0]][i[1][2]-1])
    return answer
```

### 나의 로직

로직이라고 할 게 있나...

i,j를 받아서 sliceArray에 슬라이싱한 배열을 넣어둔다. 이때 command[0]에서 1을 빼주어야 한다!
~~생각해보니까 sort에서 append 하면 되는구나..~~

정렬된 배열들에서 command[][2]-1 번째 인덱스를 가지는 아이템을 꺼내면 된다.

### 다른 분의 갓코드..ㅋㅋ

```python
def solution(array, commands):
    return list(map(lambda x:sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```

> 도랏.. ㅠ

## 새로 알게 된 것들

### `lambda` 에 대해 배워보자..
 
> https://docs.python.org/3/reference/expressions.html?highlight=lambda

> 6.13. Lambdas
Lambda expressions (sometimes called lambda forms) are used to create anonymous functions. The expression lambda parameters: expression yields a function object. The unnamed object behaves like a function object defined with:

- 람다식은 익명의 함수를 생성하기 위해 사용됨. 
- lambda parameters: expression  표현은 함수 객체를 생성함.
- 이름이 없는 객체는 함수 객체처럼 동작함

```python
def <lambda>(parameters):
    return expression
```
==> 함수를 한번만 쓸거라면 굳이 함수를 생성하지 않고 람다식으로 표현해서 사용하면 좋다고 한다.
~~사실 이거 배웠는데 머릿속에서 지워졌음 ㅠ~~

### 그렇다면 map 함수는?

map(fuction, iterable, ...)
 
> Return an iterator that applies function to every item of iterable, yielding the results. If additional iterable arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel. With multiple iterables, the iterator stops when the shortest iterable is exhausted. For cases where the function inputs are already arranged into argument tuples, see itertools.starmap().

✔️ 반복할 수 있는 함수의 모든 원소에 적용되어 결과를 리턴, 추가적인 argumnets는 패스되고 함수는 반드시 여러 argumnets를 취해야 하고 병렬적으로 원소들에 적용됨. 가장 짧은 것을 기준으로 반복이 종료됨. 

✔️ 반복문을 쓰지 않고 함수에 배열을 적용시켜 결과값을 리턴해주는 built-in 함수라는 것!

```python
def solution(array, commands):
    return list(map(lambda x:sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```

그럼 다시 이 코드를 이해해보자.

일단 이름없는 람다함수와 commands(배열)을 map함수를 이용해 매핑시켜준다.
그럼 이제 commands가 x 역할을 한다.

풀어서 쓰면 c = commands라 할때,
`array[c[0]-1:c[1]]` -> array의 i-j까지 슬라이싱한 것을 sorted한다. 그 후 k번째 숫자를 리턴하는 것인데 그것들을 list로 받음.

내가 생각했던 것을 한줄로 표현하셨군..ㅎㅎ...

---
 
### 출처
> https://programmers.co.kr/learn/courses/30/lessons/42748