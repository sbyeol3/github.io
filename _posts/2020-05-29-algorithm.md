---
layout: post
title: 프로그래머스 - 방금그곡
tags: [programmers, python, kakao]
date: 2020-05-29
---

# 2018 카카오 블라인드 테스트 문제 : 방금그곡

### 문제 설명

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 `(None)`을 반환한다.

### 입력 형식

입력으로 네오가 기억한 멜로디를 담은 문자열 m과 방송된 곡의 정보를 담고 있는 배열 musicinfos가 주어진다.
- m은 음 1개 이상 1439개 이하로 구성되어 있다.
- musicinfos는 100개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 ','로 구분된 문자열이다.
- 음악의 시작 시각과 끝난 시각은 24시간 HH:MM 형식이다.
- 음악 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1 이상 64 이하의 문자열이다.
- 악보 정보는 음 1개 이상 1439개 이하로 구성되어 있다.

### 출력 형식

조건과 일치하는 음악 제목을 출력한다.

## 풀이

```python
from datetime import datetime
def solution(m, musicinfos):
    def replaceFunc(s) :
        s = s.replace('A#','H')
        s = s.replace('F#','I')
        s = s.replace('C#','J')
        s = s.replace('D#','K')
        s = s.replace('G#','L')
        return s

    candidate = []
    m = replaceFunc(m)
    for i in range(len(musicinfos)):
        start, end, title, melody = musicinfos[i].split(',')
        melody = replaceFunc(melody)
        if not set(list(m)).issubset(set(list(melody))) : continue
        time = str(datetime.strptime(end,'%H:%M')-datetime.strptime(start,'%H:%M')).split(':')
        total = int(time[0])*60 + int(time[1])
        info = {'T':total, 't':title, 'm':melody}
        if len(melody) >= total : 
            if m in melody[:total] : candidate.append(info)
        else :
            q, r = divmod(total,len(melody))
            fullMelody = melody * q + melody[:r]
            if m in fullMelody : candidate.append(info)
    
    if len(candidate) == 1 : return candidate[0]['t']
    elif len(candidate) == 0 : return "(None)"
    max = candidate[0]['T']
    maxIdx = 0
    for i in range(1,len(candidate)):
        if candidate[i]['T'] > max :
            maxIdx = i
            max = candidate[i]['T']
    
    return candidate[maxIdx]['t']
```

### 설명

처음에 #을 어떻게 구분해야 하나 이것 때문에 막혔는데 귀찮은 것들은 다른 문자로 치환하면 쉽게 해결된다.
replaceFunc을 통해서 각각의 멜로디, 주어진 멜로디를 쉽게 변환한다.

최종 답이 될 수 있는 곡을 넣어두는 배열 candidate를 선언한다.
그리고 각 정보를 하나씩 for문을 통해 순환한다.
정보들은 시작 시간, 종료 시간, 제목, 멜로디로 콤마를 기준으로 split하여 변수에 담아준다.
찾고 싶은 멜로디의 m의 set이 멜로디의 set의 부분집합이 아니라면 따져볼 필요 없이 continue한다. 절대 겹칠 수 가 없기 때문.

그리고 각 음악의 총 재생 시간을 구하기 위해서 datetime 모듈을 이용해서 구했다.
strptime은 문자열을 첫 번째 파라미터로, 포맷을 두 번째 파라미터로 넣어 시간으로 변환하기 때문에 쉽게 시간 계산이 가능하다.
단, 그냥 사용하면 type이 datetime이므로 str로 변환을 해주어야 split해서 사용할 수 있다.
결과물은 hh:mm:ss로 나오는 데 우리는 초는 따질 필요 없으므로 time[0]에 60을 곱하고 time[1]을 더해주어 시간을 분으로 환산한다.

이제 전체 시간인 T, 제목 t, 멜로디 m를 담아두는 info 딕셔너리를 하나 만든다.

1) 만약 멜로디 길이가 전체 시간보다 길거나 같다면 중간에 멜로디가 잘라진다는 것이다.

그럼 멜로디의 처음부터 전체 시간까지의 멜로디에서 우리가 찾는 m이 있는지 따져보고 있으면 candidate에 넣는다.

2) 전체 시간보다 짧다면 몫과 나머지를 구해서 melody를 시간만큼 연장한 fullMelody를 구해 m이 있는지 따져본다.

이제 candidate를 다 구했으면 배열의 길이가 1이라면 후보가 1개이므로 바로 제목 리턴, 0이라면 후보가 없으므로 (None) 리턴
2개 이상인 경우에는 가장 재생시간이 길면서 앞에 있는 음악의 타이틀을 리턴한다.

---

### 문제

> https://programmers.co.kr/learn/courses/30/lessons/17683