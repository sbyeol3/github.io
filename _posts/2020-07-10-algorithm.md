---
layout: post
title: 프로그래머스 - 프렌즈4블록
tags: [programmers, python, kakao]
date: 2020-07-10
---

# 2018 카카오 블라인드 채용 - 프렌즈4블록

### 문제 설명

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다.  이번에 출시할 게임 제목은 "프렌즈4블록"
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다.
같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.
블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.
만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다
입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

### 입력 형식

입력으로 판의 높이 m, 폭 n과 판의 배치 정보 board가 들어온다. `2 ≦ n, m ≦ 30`
board는 길이 n인 문자열 m개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### 출력 형식

입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

> 주의 : 시간복잡도가 최악이므로 로직만 참고할 것. . . ? 

## 풀이

```python
def solution(m, n, board):
    result = 0

    for i in range(m) : board[i] = list(board[i])
    while True:
        block = []

        for i in range(0,m-1): # 4블록 찾기
            line = board[i]
            for j in range(1,n) :
                b = line[j]
                if b == '-' : continue
                if line[j-1] == b :
                    nextLine = board[i+1]
                    if nextLine[j-1] == b and nextLine[j] == b: block.extend([(i,j-1),(i,j),(i+1,j-1),(i+1,j)])
                        
        blockSet = list(set(block))
        if len(blockSet) == 0 : break
        result += len(blockSet)

        for b in blockSet: # '-'로 변환하기
            i, j = b
            board[i][j] = '-'
        for i in range(n): # 블록 내리기
            for j in range(m-1,0,-1):
                if board[j][i] == '-' :
                    start = j-1
                    value = '-'
                    while(start > -1):
                        if board[start][i] != '-' :
                            value = board[start][i]
                            print(value,start,i)
                            break
                        start -= 1
                    if value != '-' : 
                        board[j][i] = value
                        board[start][i] = '-'
                    else : break

    return result
```

### 자세한 풀이

```python
for i in range(0,m-1): # 4블록 찾기
            line = board[i]
            for j in range(1,n) :
                b = line[j]
                if b == '-' : continue
                if line[j-1] == b :
                    nextLine = board[i+1]
                    if nextLine[j-1] == b and nextLine[j] == b: block.extend([(i,j-1),(i,j),(i+1,j-1),(i+1,j)])
```

4블록으로 깨지는 블록찾는 부분 -> 4개 중에 왼쪽 위에 있는 블록을 기준으로 찾아서 block 배열에 좌표를 넣어둔다.

```python
        blockSet = list(set(block))
        if len(blockSet) == 0 : break
        result += len(blockSet)
```

blockSet은 깨지는 블록들을 집합으로 변환하고 0인 경우에는 while문 종료하고 0보다 큰 경우 result에 더해준다.

```python
        for b in blockSet:
            i, j = b
            board[i][j] = '-'
```

블록 집합을 순회하면서 -로 바꿔준다.

```python
for i in range(n):
            for j in range(m-1,0,-1):
                if board[j][i] == '-' :
                    start = j-1
                    value = '-'
                    while(start > -1):
                        if board[start][i] != '-' :
                            value = board[start][i]
                            print(value,start,i)
                            break
                        start -= 1
                    if value != '-' : 
                        board[j][i] = value
                        board[start][i] = '-'
                    else : break
```

블록을 내리는 부분.

왼쪽 밑에서부터 각 열을 순회하는데 해당 블록과 열이 같으면서 가장 행이 큰 블록을 찾는다.
없으면 그 행은 위에서 꺼내올 것이 없으므로 break


-> 여기서 5번 10번 케이스가 실패했는데 if 문에서 blockset에 조건도 추가했어서 그랬다.
다른 테스트케이스 돌려보면 왜인지 알게된다.

`하 시간복잡도는 너무 별로지만 그래도 푼거에 의의를 두겠다..!`

---

### 문제 출차 

> https://programmers.co.kr/learn/courses/30/lessons/17679