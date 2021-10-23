# 0 - 1Knapsack Problem

* 쪼갤 수 없는 물건을 배낭에 담는문제로 DP로 풀어야하는 대표적인 문제

* DP로 풀수 있는지 여부는 Principle of Optimality 성립 여부를 보면 된다.

  > **Principle of Optimality**
  > 어떤 문제의 입력 사례의 최적해가 그 입력 사례를 분할한 부분사례에 대한 최적해를 항상 포함하고 있으면, 그 문제에 대하여 최적의 원리가 성립한다
  >
  > 즉 부분사례의 최적해가 현 사례의 최적해의 부품이 된다면 DP로 풀 수 있다.

<br>

<br>

![img](https://blog.kakaocdn.net/dn/mu4jC/btqvpjvm4kE/GrSkkh7biC6auBoVx0tscK/img.png)

<br>

<br>

|      | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| 1    | 0    | 0    | 0    | 0    | 0    | 0    | 13   | 13   |
| 2    | 0    | 0    | 0    | 0    | 8    | 8    | 13   | 13   |
| 3    | 0    | 0    | 0    | 6    | 8    | 8    | 13   | 14   |
| 4    | 0    | 0    | 0    | 6    | 8    | 12   | 13   | 14   |

* 가로는 담을 수 있는 가방의 용량
* 세로는 각 물건의 종류
* 칸에는 최대가치가 담긴다.

<br>

```java
import sys

def knapsack(W, wt, val, n): #W 무게 한도, wt 각 보석 무게, val 각 보석 가격, n 보석 수
    K = [[0 for x in range(W+1)] for x in range(n+1)]
    for i in range(n+1):
        for w in range(W+1):
            if i==0 or w==0:
                K[i][w] = 0;
            elif wt[i-1] <= w:
                K[i][w] = max(val[i-1]+K[i-1][w-wt[i-1]], K[i-1][w])
            else:
                K[i][w] = K[i-1][w]
            #print(K[i][w], end=' ')
        #print('\n')
    return K[n][W]

wt = []
val = []
N,K = map(int,sys.stdin.readline().strip().split())
for i in range(N):
    w,v = map(int, sys.stdin.readline().strip().split())
    wt.append(w)
    val.append(v)
print(knapsack(K,wt, val, N))
```

