---
layout: post
title: 프로그래머스 - 캐시
tags: [programmers, python, kakao]
date: 2020-05-27
---

# 카카오 블라인드 테스트 문제 : 캐시

### 문제 설명

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다. 이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다. 어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식

- 캐시 크기(cacheSize)와 도시이름 배열(cities)을 입력받는다.
- cacheSize는 정수이며, 범위는 0 ≦ cacheSize ≦ 30 이다.
- cities는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다.
- 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식

-  입력된 도시이름 배열을 순서대로 처리할 때,  총 실행시간을 출력한다.

### 조건

- 캐시 교체 알고리즘은 LRU(Least Recently Used)를 사용한다.
- cache hit일 경우 실행시간은 1이다.
- cache miss일 경우 실행시간은 5이다.

> 와 카카오... 문제 진짜 재밌게 낸다.. 내가 한때 좋아했던 컴구에서 배운 캐시가 나온다.

## 풀이

이 문제는 대충 풀면 걸리는 조건?들이 여러 개 있다.

1. 대소문자를 구분하지 않는다 -> 도시의 이름을 단순 비교하면 캐시에 있더라도 miss로 체크될 수 있다.
2. 순서대로 처리한다 -> cities를 그냥 pop하면 안된다. reversed 해주어야 함.
3. LRU 알고리즘을 사용한다 -> cache hit일 경우 그냥 시간만 +1 해주면 안된다. 최근에 사용된 캐시는 나중에 kick out될 때 가장 낮은 우선순위를 갖도록 순서를 바꾸어야 한다.
4. 캐시 사이즈가 0일 수 있다. 조기 종결되는 조건이다.

### 코드

```python
def solution(cacheSize, cities):
    if cacheSize == 0 : return 5*len(cities)
    cities = list(reversed(cities))
    cache = []
    time = 0
    while cities :
        city = cities.pop().lower()
        if city in cache :
            idx = cache.index(city)
            if idx != len(cache)-1 : cache = cache[:idx] + cache[idx+1:] + [city]
            time += 1
            continue
        time += 5
        if len(cache) < cacheSize : cache.append(city)
        else : 
            cache = cache[1:]
            cache.append(city)
    
    return time
```

cacheSize가 0인 경우는 miss고, hit이고 따질 이유가 없다. 무조건 miss다. 바로 cities 원소 개수 * 5 리턴한다. (miss일 때 5이므로)

pop을 편하게 하기 위해 역으로 순서를 바꾸어주고 cache로 사용할 리스트를 선언한다. 리턴할 time도 0으로 초기화한다.

cities에서 하나씩 뺄 것이므로 while 문을 사용한다. 대소문자를 구분하지 않으므로 각 도시를 lower()해준다. (upper()도 가능)

이제 경우가 3가지가 있다.
1) 캐시에 데이터가 있는 경우 (cache hit)
2) 캐시에 데이터가 없고 (cache miss), 캐시가 가득차지 않은 경우
3) 캐시에 데이터가 없고 (cache miss), 캐시가 가득차서 무언가를 빼야 하는 경우


1)일 때는 time을 1 더해주고 LRU 알고리즘에 의해 순서를 바꿔주어야 한다.
나는 인덱스 0을 kick out할 것이므로 hit된 것을 가장 뒤에 놓도록 한다.
index 메소드를 이용해 인덱스를 구하고 가장 뒤에 있지 않으면 변경하는 것으로 했다.

2) 3)은 일단 시간을 +5해주고 가득 차지 않은 경우는 그냥 뒤에 넣어준다. 가득 찼으면 인덱스 0에 있는 거를 뺀다. (슬라이싱함)

---

### 문제 링크

> https://programmers.co.kr/learn/courses/30/lessons/17680