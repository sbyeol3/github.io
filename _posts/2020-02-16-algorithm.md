---
layout: post
title: 프로그래머스 - 다리를 지나는 트럭
tags: [programmers, stack, queue, python]
date: 2020-02-16
---

# 문제 2. 다리를 지나는 트럭
 
### 문제 설명

트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

### 제한사항

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.


## 내 코드

### 처음에 작성한 코드

```python
def solution(bridge_length, weight, truck_weights):
  if len(truck_weights)==1 : return bridge_length+1
  bridge = [0]*len(truck_weights)
  first = 0 
  last = 0 
  out = -1 
  time = 1
  bridge[0] += 1 

  while out < len(truck_weights) :
    if sum(truck_weights[first:last+1]) + truck_weights[last+1] <= weight: last += 1
    for i in range(first, last+1) : 
      bridge[i]+=1 # 2
      if bridge[i] == (bridge_length + 1) :
        out = i # 0
        bridge[out] = -1
        first = out + 1 # 1
      if i==first : time += 1
    if(last==len(truck_weights)-1) :
      while(bridge[last]<bridge_length+1) :
        bridge[last] += 1
        time += 1
      break
  
  return time
```

> 딱 봐도 코드가 더러움.

> 채점 결과 - 정확성: 23.1 => 합계: 23.1 / 100.0

당시에 나는 다리 위에 있는 트럭 중에 가장 앞에 있는 트럭의 인덱스를 first, 끝의 인덱스를 last로 했던 것 같다.
만약에 끝의 인덱스가 트럭 배열의 마지막 인덱스라면 나갈 때 까지 time ++ 해주었다.

### 수정한 코드

```python
def solution(bridge_length, weight, truck_weights):
    bridge = [0] * bridge_length
    time = 0
    while bridge:
        time += 1
        bridge.pop(0)
        if truck_weights:
            if sum(bridge) + truck_weights[0] <= weight:
                bridge.append(truck_weights.pop(0))
            else:
                bridge.append(0)

    return time
```

> 채점 결과 - 합계: 92.3 / 100.0 

🥺 위의 코드랑 비교하면 장족의 발전

### 나의 로직

while 문을 돌면서 다리 가장 앞에 있는 것을 뺀다. 트럭일 수도 있고, 아무것도 없을 수도 있고.
아직 truck_weights에 트럭이 남아 있다면 다리 위에 있는 트럭들의 무게의 합과 들어오는 트럭의 무게의 합을 비교한다.
들어올 수 있으면 다리에 트럭을 append하고 weights 배열에서 꺼낸다.
이런 식으로 반복하다보면 truck_weights에는 트럭이 없을 것이다.

근데 이렇게 하면... 시간 초과가 발생한다.
예상하기에는 pop(0)에서 시간복잡도가 커진 것이 아닐까라는 생각이 들었다.
어떻게 해결해야 할지 몰라서 다른 분들의 코드를 읽었다.

### 다른 분의 코드

```python
def solution(bridge_length, weight, truck_weights):
    time = 0
    on_bridge = []
    on_time = []
    
    while(truck_weights or on_bridge) :
        time += 1
        if(on_time) :
            if(on_time[0] + bridge_length == time) :
                on_bridge.pop(0)
                on_time.pop(0)
        if(truck_weights) :
            if(sum(on_bridge) + truck_weights[0] <= weight) :
                on_bridge.append(truck_weights.pop(0))
                on_time.append(time)
    
    return time
```

- on_time은 트럭이 들어간 시간을 담는 배열
- on_bridge는 다리 위에 있는 트럭의 무게를 담는 배열
- if(on_time[0] + bridge_length == time) : -> 트럭이 나가야 함

> 이 코드에도 pop(0)가 있는데 시간초과가 왜 안뜨는지는 모르겠다. ㅠㅠ
