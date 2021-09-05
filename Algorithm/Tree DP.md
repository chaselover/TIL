# Tree DP

* subtree에서 구한 해를 이용해 전체 트리의 해를 구하는 방식으로 진행이 됨.
* dp[i] = i를 루트로 하는 서브 트리의 ~~~같은 식으로 DP Table을 정의.
* dp는 보통 선형구조에서 이루어지지만 tree는 비선형 구조기 때문에 탐색 순서를 미리 정해주는게 일반적이다. / 그래프에서 우리가 dp를 사용하듯 트리도 dp를 사용하기 충분.
* 탐색 순서는 보통 dfs를 돌면서 나오는 트리인 dfs트리를 기준으로 한다.
* 트리 dp도 대부분 상태 dp가 들어간다. ex.) dp/[i]/[j] = i는 i를 루트로하는 subtree에서 i의 상태가 j일때.
* 보통 i번째 노드를 루트로 하는 서브트리에서 i번째 루트 노드를 포함했을 때와 포함 하지 않았을 떄 조건 중 최적값을 가져온다.

---



## 기본 구조

```python
function dfs(u):
    visit[u] = 1
    for v in edge[u]:
        if visit[v]:
            continue
        dfs(v)
        dp[u] = dosomething(dp[v]);
```

* 자식으로 먼저 진입하고 모든 진입이 끝나면 자식 dp연산들을 통해 부모 dp를 갱신해주는 구조.



---



## 예제 코드 : BOJ 2213 트리에서의 독립집합

```python
import sys
input = sys.stdin.readline

# tree dp를 그리는 dfs
def dfs(cur_node):
    visited[cur_node] = True
    dp[cur_node][0] = node_v[cur_node]
    dp[cur_node][1] = 0
    for next_node in tree[cur_node]:
        if not visited[next_node]:
            dfs(next_node)
            dp[cur_node][0] += dp[next_node][1]
            dp[cur_node][1] += max(dp[next_node][0],dp[next_node][1])

# pre_node의 존재 유무에 따라 분기를 타는 trace
def trace(cur_node,pre_node):
    trace_visited[cur_node] = True

    if not pre_node:
        for next_node in tree[cur_node]:
            if not trace_visited[next_node]:
                trace(next_node,1)
    else:
        if dp[cur_node][0] > dp[cur_node][1]:
            trace_res.append(cur_node)
            for next_node in tree[cur_node]:
                if not trace_visited[next_node]:
                    trace(next_node,0)
        else:
            for next_node in tree[cur_node]:
                if not trace_visited[next_node]:
                    trace(next_node,1)


n = int(input())
node_v = [0] + list(map(int,input().split()))
tree = {i: [] for i in range(1,n+1)}
dp = {i: [0,0] for i in range(1,n+1)}

for _ in range(n-1):
    a, b = map(int, input().split())
    tree[a] += [b]
    tree[b] += [a]

visited = {i: False for i in range(1,n+1)}
dfs(1)
print(max(dp[1][0],dp[1][1]))
trace_res = []
trace_visited = {i: False for i in range(1,n+1)}
trace(1,1)
trace_res.sort()
print(*trace_res)
```

노드 포함 여부에따라 상태인자를 집어넣어 리프노드부터 루트노드까지 쭉 올라온다.





## BOJ 1289 트리의 가중치

```python
import sys
input =sys.stdin.readline
sys.setrecursionlimit(10**6)

# dp[u]는 노드 u가 루트인 subtree에서 u부터 다른 모든 노드 까지 가는 모든 경로들의 곱의 합.
# 즉 (dp[v] * c) 들의 합. 그 값을 리스트 p에 저장해뒀다가 모든 값들에 대해 (dp[u] - dp[v]*c)*(c*dp[v])들의 합을 구해준다.
# 그 후 중복을 제거하기 위해 나누기 2를 해줘야 하나 MOD의 반인 500000004를 곱하고 MOD로 나눔으로써 2로 나누는 것과 같은 효과를 얻을 수 있다.
def dfs(u):
    global ans
    visited[u] = True
    p = []
    for i in range(len(edges[u])):
        v = edges[u][i]
        c = costs[u][i]
        if visited[v]:
            continue

        dfs(v)
        dp[u] += (dp[v] * c) % MOD
        dp[u] %= MOD

        p.append((dp[v] * c) % MOD)
    ans += dp[u]
    ans %= MOD

    sum_v = 0
    for v in p:
        sum_v += ((dp[u] - v + MOD) % MOD * v) % MOD
        sum_v %= MOD
    
    sum_v *= 500000004
    sum_v %= MOD
    
    ans += sum_v
    ans %= MOD                

    dp[u] += 1
    dp[u] %= MOD


MOD = 1000000007
N = int(input())
ans = 0
visited = {i: False for i in range(1,N+1)}
dp = [0 for _ in range(N+1)]
edges = {i: [] for i in range(1,N+1)}
costs = {i: [] for i in range(1,N+1)}
for _ in range(N-1):
    a, b, c = map(int, input().split())
    edges[a].append(b)
    edges[b].append(a)
    costs[a].append(c)
    costs[b].append(c)

dfs(1)
print(ans)
```

