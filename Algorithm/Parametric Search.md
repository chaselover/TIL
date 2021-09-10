# Parametric Search

* 최적화 해법인 이분탐색을 결정문제로 바꾸어 푸는 기법.
* ex.) 고기를 200g잘라라 -> 고기가 200g보다 가벼운가? 무거운가? 의 결정문제로 바꿈.(예 아니오)
* 특정 값을 찾는게 아닌 오차 범위 내에서 가장 가까운 부등호 식으로 결과를 판별한다.
* 가능한 해의 영역이 연속적이어야 한다.
* (최솟값 결정문제 a가 만족한다면 a이상의 값은 모두 만족.)



## 예제 BOJ 1637 날카로운 눈

```python
import sys
input = sys.stdin.readline

def get_sum(target):
    total = 0
    for i in range(N):
        if target >= arr[i][0]:
            total += ((min(arr[i][1],target) - arr[i][0])//arr[i][2]) + 1
    return total

N = int(input())
arr = [list(map(int, input().split())) for _ in range(N)]
left = 0
right = 2147483648
while left < right:
    mid = (left + right)//2
    if not get_sum(mid)&1:
        left = mid + 1
    else:
        right = mid
if left == 2147483648:
    print('NOTHING')
else:
    print(left,get_sum(left) - get_sum(left-1))
```

