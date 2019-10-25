---
layout: post
title: Python 프로그래머스 - 완주하지 못한 선수
iamge: https://drive.google.com/uc?id=19JOAJmDm3xiyfB7xmxDclbuqgpmtjXOJ
date: 2019-9-25
categories: [algorithm]
---
![프로그래머스](https://drive.google.com/uc?id=19JOAJmDm3xiyfB7xmxDclbuqgpmtjXOJ)

#### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

***

#### 제한사항
* 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
* completion의 길이는 participant의 길이보다 1 작습니다.
* 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
* 참가자 중에는 동명이인이 있을 수 있습니다.

***

#### 입출력 예
![counter](https://drive.google.com/uc?id=1m4m2zlPZQhGEdVRU0uWCFeYoGzw212kz)

***

#### 입출력 예 설명
예제 #1“leo”는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.  
예제 #2“vinko”는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.  
예제 #3“mislav”는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

***

#### 풀이
```python
import collections

def solution(participant, completion):
    participant_counter = collections.Counter(participant)
    completion_counter = collections.Counter(completion)
    result = list((participant_counter - completion_counter).keys())[0]
    return result
```
  
파이썬의 Counter 모듈을 사용하였습니다.

**`Counter`**는 **collections** 아래에 정의된 딕셔너리의 서브 클래스로 일련의 집합에서 각 원소의 출현 횟수를 세어서 유지합니다. 즉 **원소:출현빈도**의 딕셔너리가 생성됩니다. 생성된 **`Counter`** 딕셔너리는 일반 딕셔너리와 유사하게 키:값 쌍을 추가/삭제하거나 업데이트 할 수 있고, 각 원소를 출현 횟수만큼 반복하여 조합해서 복구합니다.  

Counter 모듈을 사용하여 문제를 풀어보면 리스트 인자로 주어진 participant, completion을 Couter를 이용하여 각 원소의 출현 횟수를 얻어낼 수 있습니다.
```python
participant = ["mislav", "stanko", "mislav", "ana"]
completion = ["stanko", "ana", "mislav"]
completion_counter = collections.Counter(completion)
participant_counter = collections.Counter(participant)
print(completion_counter)
print(participant_counter)
```
위 코드의 결과는 아래와 같습니다.

```python
Counter({'mislav': 2, 'stanko': 1, 'ana': 1})
Counter({'mislav': 1, 'stanko': 1, 'ana': 1})
````
문제에서 완주하지 못한 선수는 한명이기 때문에 두 결과를 '-'로 연산하여 나온 결과값의 key 값을 가져오면 완주하지 못한 선수의 이름을 가져올 수 있습니다.



