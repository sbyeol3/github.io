---
layout: post
title: 프로그래머스 - 베스트 앨범
tags: [programmers, hash, python]
date: 2020-01-30
---
*일단.. 문제 이해부터 어려웠다ㅋㅋㅋ... 그리고 자꾸 맞긴 맞는데 일부 테스트케이스에서 안맞아서 굉장히 오래 풀었다.
그래도 스스로 풀었다는 점에서 의미가 있는 문제였다.*

# 문제 4. 베스트앨범
 
### 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다.
노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

- 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
- 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
- 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.
- 노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때,
- 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

## 작성한 코드

### 첫 번째 코드 : 굉장히 더러움 주의 😂

```python
from collections import defaultdict

def solution(genres, plays):
  length = len(genres)
  answer = []
  dictGP = dict()
  dictGenres = defaultdict(list)

  for g, p in zip(genres, plays):
    if hash(g) in dictGP : dictGP[hash(g)] += p
    else : dictGP[hash(g)] = p

  for i in range(length):
    hashG = hash(genres[i])
    if len(dictGenres[hashG])==2 :
      if plays[i] > plays[dictGenres[hashG][0]] :
        tmp = dictGenres[hashG][0]
        dictGenres[hashG][0] = i
        dictGenres[hashG][1] = tmp
      elif plays[i] > plays[dictGenres[hashG][1]] : dictGenres[hashG][1] = i
    elif len(dictGenres[hashG])==1 : 
      if plays[i] > plays[dictGenres[hashG][0]] :
        tmp = dictGenres[hashG][0]
        dictGenres[hashG][0] = i
        dictGenres[hashG].append(tmp)
      else : dictGenres[hashG].append(i)
    else : 
      dictGenres[hashG].append(i)

  sDictGP = sorted(dictGP.items(), key=lambda x: x[1], reverse=True)
  for i in sDictGP:
    answer.append(dictGenres[i[0]][0])
    answer.append(dictGenres[i[0]][1])
  return answer
```

> 채점 결과 : 정확성: 53.3 => 합계: 53.3 / 100.0

▶️ 보기만 해도 더럽다.. 그래도 이렇게 쓰면서 문제를 이해했다..

### 첫 번째 코드의 로직

dictGP는 딕셔너리고, 각 장르 해시값을 key로 가질 때 재생횟수의 총합을 value로 넣어줬다.

hashG는 i번째 장르의 해시값이다. 새로운 원소를 넣을 때 2개가 이미 있으면 각 값들을 비교해서 가장 큰게 0번째, 두번째로 큰게 1번째로 넣어주었다. swap을 한건데 이거 땜에 코드가 더 더러워졌다.

새로운 원소가 0번째보다는 작고 1번째보다는 크면 인덱스 1의 원소를 바꿨다.
원소가 1개 있는 경우에는 또 값을 비교해주고 큰 게 앞으로 가도록 했다.
그리고 원소가 하나도 없는 경우에는 인덱스 0에 그냥 넣었다.

최대 2개로 원소를 가지게 for 루프를 돌면 dictGP를 재생횟수를 기준으로 내림차순 정렬을 한다.
answer에 정렬한 순서대로 장르의 해시값의 0번째 원소와 1번째 원소를 넣어주었다.

일단 그럴듯 해 보이지만(?) 지금 보니까 틀린 로직도 있고 중요한 건 런타임 에러가 아주 크게 발생했다.
15개 케이스 중에서 7개에서 런타임 에러가 났다..ㅋㅋ

### 두 번째 코드

```python
from collections import defaultdict

def solution(genres, plays):
  answer = []
  dictGenres = defaultdict(list)
  totalPlayes = defaultdict(lambda:0)

  for g, p in zip(genres, plays): totalPlayes[hash(g)] += p
  sTP = sorted(totalPlayes.items(), key=lambda x: x[1], reverse=True)

  for i in range(len(genres)) : dictGenres[hash(genres[i])].append(plays[i])
  for g in dictGenres : dictGenres[g].sort(reverse=True)
  for i in range(len(sTP)):
    answer.append(plays.index(dictGenres[sTP[i][0]][0]))
    if len(dictGenres[sTP[i][0]])>1: answer.append(plays.index(dictGenres[sTP[i][0]][1]))
  return answer
```

> 채점 결과 - 합계: 86.7 / 100.0

- 확실히 간결해지긴 했다. ^^ (뿌듯),, 그리고 ★런타임에러는 사라졌다.
- 근데 테스트 케이스 2번째와 15번째에서 실패가 떴다!? 왜일까?!
- 서치를 하니까 'stable'하게 요소를 놔야 한다.. 재생횟수가 같다면 고유번호(i)가 작은 애가 나와야 한다.
- 결국 풀면서 알았지만 나의 문제는 stable 문제가 아니라 재생횟수가 같은 경우를 고려하지 않은 코드였다.ㅋㅋ

### 두 번째 코드의 로직

첫 번째 코드와는 다르게 defalutdict를 활용하여 불필요한 if-else를 제거했다. 0을 기본값으로 가지게끔 초기화함
그리고 모든 장르는 재생 횟수가 다르다는 점을 이용했다. 재생 횟수를 알면 장르를 알 수 있다는 것!
그래서 재생 횟수들을 딕셔너리 value list에 넣어두고 내림차순 정렬을 한 후 0번째, 1번째 재생횟수에 대한 i를 구했다.

그런데도 문제가 있다. 만약 pop 장르에 재생횟수가 1000인 음악이 2개 이상이라면 같은 인덱스를 리턴할 것이다.
이거 때문에 빙빙 돌다가 결국 해결했다. ㅠㅠ

### 세 번째 코드 : 최종 코드 ✨

```python
from collections import defaultdict

def solution(genres, plays):
  answer = []
  dictGenres = defaultdict(list)
  totalPlayes = defaultdict(lambda:0)

  for g, p in zip(genres, plays): totalPlayes[hash(g)] += p
  sTP = sorted(totalPlayes.items(), key=lambda x: x[1], reverse=True)

  for i in range(len(genres)) : dictGenres[hash(genres[i])].append(plays[i])
  for g in dictGenres : dictGenres[g].sort(reverse=True)
  for i in range(len(sTP)):
    idx = plays.index(dictGenres[sTP[i][0]][0])
    answer.append(idx)
    plays[idx] = -1
    if len(dictGenres[sTP[i][0]])>1:
    	answer.append(plays.index(dictGenres[sTP[i][0]][1]))

  return answer
```

> 문제 해결!

### 세 번째 코드의 로직

해결 방법은 0번째 인덱스는 먼저 가져오고 해당 인덱스의 원소를 -1로 변경했다. 
처음에는 pop하면 되나 생각했는데 그러면 아예 없어지니까 뒤에 애들이 앞으로 땡겨지므로 소용없음
아예 투명인간처럼 취급을 하게끔 -1을 넣었다.
 
## 새로 알게 된 것들

### zip()

파라미터로 받은 리스트나 튜플들의 원소들을 짝지어서 zip 오브젝트를 리턴하는 함수

만약 파라미터들끼리 원소의 개수가 다르다면?
- 파라미터 중에 가장 원소의 개수가 작은 거를 기준으로 생성된다.
- 파라미터가 여러 개여도 가능하다.
- 그러나 딕셔너리로 만드려면 파라미터가 반드시 2개여야 한다. (key-value 구조니까!)
