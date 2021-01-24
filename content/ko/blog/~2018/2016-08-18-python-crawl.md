---
title: "PYTHON"
date: 2016-08-18
---

virtualenv로 가상환경을 만들어 python을 실행해야하는데, 귀찮다..

python3

python은 여러모로 충격적인 언어이다.
일단 x라는 문자를 입력받았을때 **int(x)하면 걍 정수가 나온다**. (atoi(const char*), Int32.parseInt(char*)이런거 필요없음)

DashInsert문제를 풀때 Python으로 풀면,
{% highlight python %}
def even(x,y):
  if int(x)%2 == 0 and int(y)%2==0:
    return True
  else:
    return False

def odd(x,y):
  if int(x)%2 == 1 and int(y)%2==0:
    return True
  else:
    return False

def DashInsert(s):
  ans = []
  for i in range(0,len(s)-1):
    if even(s[i],s[i+1]):
      ans.append('\*')
    elif odd(s[i],s[i+1]):
      ans.append('\-')
    else:
      ans.append(s[i])
  ans.append(s[-1])
  return ans

print(DashInsert(input()))
```
위와 같이 풀수 있다. input()을 통해 문자열입력을 받고, ans라는 리스트를 선언하고,
입력받은 문자열 배열에서 연속적인 두 수가 둘 다 짝수이면, 또는 홀수이면, 각 함수를 실행하게 만들 수 있다.

여기서,충격적인 점은 다음과 같다.
람다를 사용하면,

*lambda 인자 : 표현식* 와 같이 함수를 간단히 작성할 수 있다.
lambda를 이용한 파이썬 응용은, map, reduce, filter가 있는데,

 - map(함수, 리스트) : map은 순서형 자료(리스트)를 받아서 모든 항을 함수처리를 해준 후 새로운 리스트를 반환한다.
> list(map(lambda x: x ** 2, range(5)))     # 파이썬 2 및 파이썬 3
 [0, 1, 4, 9, 16]

 - filter(함수, 리스트) : filter의 함수는 무조건 true/false를 반환하는 함수로 true를 반환하는 항만 살리고 나머지는 삭제한 리스트를 반환하는 함수이다.
>filter(lambda x: x < 5, range(10))       # 파이썬 2
[0, 1, 2, 3, 4]  

 - reduce(함수, 순서형 자료) : reduce는 특이하게 인자가 두개이고, 순서형자료(리스트, 튜플, 문자열)를 받고 원소들을 누적적으로 함수에 적용시킨다.
 즉, 순서형 자료형에서 1,2번째 원소의 함수 적용값이 2,3번째 원소의 x값이 되는 것.
>reduce(lambda x, y: x + y, [0, 1, 2, 3, 4])
10//0에다 1더하고, 그 결과인 1에다 2더하고, 그 결과인 3에다 3을, 그 결과인 6에다 4를 더해 10을 결과로 반환.

암튼 충격적인건 이문제를 reduce로 풀 수있다는 것이다.
{% highlight python %}
from functools import reduce
def DashInsert(s): return reduce(lambda x, y: x + ['\*','','\-'][int(x[-1])%2 + int(y) % 2] + y, s)
```
와 진짜 이런생각 어떻게 하지?
ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ

연속되는 두 원소 x,y에 대해서, 두 수가 모두 짝수이면 나머지가 0이고, 두 수가 모두 홀수이면 나머지가 모두 1이기 때문에,

- 두 원소 모두 짝수
- 두 원소 모두 홀수
- (짝.홀) 또는 (홀.짝)
세 조건 중 하나라는 점을 이용해서,

reduce함수가 원소를 누적적으로 함수에 적용시킨다는 점을 이용해서, 문제를 풀고 단, 연속적인 두 원소에 대해서 짝/홀을 검사해야하므로 int(x[-1])로 이전 결과값 문자열의 마지막 값에 대해 검사하도록 했다.

이거 보고 박수쳤다...ㅠㅠㅠ

예를 들어, 32493이면,

3 + ['\*','','\-'][1+0] + 2 이므로 32가 반환,
32 + ['\*','','\-'][0+0] + 4 이므로 32*4가 반환,
32*4 + ['\*','','\-'][1+0] + 9 이므로 32*49가 반환,
32*49 + ['\*','','\-'][1+1] + 3 이므로 32*49-3가 반환
결과적으로 32*49-3이 반환된다.

문자열의 길이를 계산할 필요도 없고, 정말 좋다...ㅠㅠㅠ 으악 파이썬 람다식 대박, 리듀스 대박.
