# LCA(lowest common ancestor)최소 공통 조상



* 모든 노드에 대한 깊이(depth)를 계산합니다.
* LCA를 찾을 두 노드를 확인합니다. 
  * depth가 동일하도록 거슬러 올라갑니다.
  * 이후 부모가 같아질때까지 반복적으로 두 노드의 depth를 낮춥니다.
* 모든 LCA(a,b)연산에 대해 2번의 과정을 반복합니다.

## 느린 탐색

```python
import sys
sys.setrecursion(int(1e5))

# 각 노드의 depth를 찾아 기록하기 위한 dfs
def find_depth(cur_node, depth):
    check[cur_node] = True
    depth[cur_node] = depth
    for next_node in graph[x]:
        if not check[next_node]:
            parent[next_node] = cur_node
            find_depth(next_node, depth+1)
# 공통조상 찾는 함수
def LCA(a,b):
    # 길이가 같아질때까지 깊이가 더 깊은 변수의 부모를 불러내 대입시키며 tree를 위로 올린다.
    while depth[a] != depth[b]:
        if depth[a] > depth[b]:
            a = parent[a]
        else:
            b = parent[b]
    # 노드가 같아질 때까지 올린다.
    while a != b:
        a = parent[a]
        b = parent[b]
    return a

n = int(input())

parent = [0] * (n+1) # 부모 노드 정보
depth = [0] * (n+1) # 각 노드까지의 깊이
check = [0] * (n+1) # 깊이가 계산 되었는지 여부

graph = [[] for _ in range(n+1)]
for _ in range(n-1):
    a,b = map(int,input().split())
    graph[a].append(b)
    graph[b].append(a)
    
find_depth(1,0) # 루트노드는 1번 노드

m = int(input())

for i in range(m):
    a,b = map(int,input().split())
    print(lca(a,b))
            
```



> 매 쿼리마다 부모의 방향으로 거슬러 올라가기 위해 최악의 경우 O(N)
>
> M번의 쿼리면 O(MN)



## 노드 수가 많아지면? 너무 느리지 않을까?

* 2의 제곱형태로 거슬러 올라가도록 하면 O(logN)의 시간 복잡도를 보장한다.

* 메모리를 조금 더 사용하여 각 노드에 대해 2**i 번째 부모에 대한 정보를 기록하자.



## 빠른 탐색

* 다이나믹 프로그래밍을 이용해 시간 복잡도의 개선
  * 세그먼트 트리를 이용하는 방법도 존재합니다.
* 매 쿼리마다 부모를 거슬러 올라가기 위해 O(logN)의 복잡도가 필요합니다.
  * 따라서 모든 쿼리를 처리할 때의 시간 복잡도는 O(MlogN)입니다.



```python
import sys
sys.setrecursion(int(1e5))
input = sys.stdin.readline
LOG = 21 # (1000000의 log2를 취한 값의 올림값)(2의 i승 단위의 부모값을 저장하기 위한 크기.)

# 각 노드의 depth를 찾아 기록하기 위한 dfs
def find_depth(cur_node, depth):
    check[cur_node] = True
    depth[cur_node] = depth
    for next_node in graph[x]:
        if not check[next_node]:
            parent[next_node][0] = cur_node
            find_depth(next_node, depth+1)
# 공통조상 찾는 함수
def LCA(a,b):
    # b가 더 깊도록 설정
    if depth[a] > depth[b]:
        a,b = b,a
        # 더 깊은 b에 대해 동일해질때까지 올린다.
   	for i in range(LOG-1,-1,-1):
        if depth[b] - depth[a] >= (1<<i):
            b = parent[b][i]
    # 노드가 같아질 때까지 올린다.
    if a==b:
        return a
    
    for i in range(LOG - 1,-1,-1):
        if parent[a][i] != parent[b][i]:
        	a = parent[a][i]
        	b = parent[b][i]
    # 이후에 부모가 찾고자 하는 조상.
    return parent[a][0]

def set_parent():
    find_depth(1,0)
    for i in range(1,LOG):
        for j in range(1, n+1):
            # preorder로 순회하며 root부터 top-down으로 부모노드를 채워 내려간다.
            parent[j][i] = parent[parent[j][i-1]][i-1]
	# 각 최초의 부모 노드로 부터 그 노드의 부모노드를 기록하도록 한다.()

n = int(input())

parent = [[0]*LOG for _ in range(n+1)] # 부모 노드 정보(n+1개의 노드에 대해 1,2,4,8,16..번째 부모값을 전부 기록.)
depth = [0] * (n+1) # 각 노드까지의 깊이
check = [0] * (n+1) # 깊이가 계산 되었는지 여부

graph = [[] for _ in range(n+1)]
for _ in range(n-1):
    a,b = map(int,input().split())
    graph[a].append(b)
    graph[b].append(a)
    
set_parent()

m = int(input())

for i in range(m):
    a,b = map(int,input().split())
    print(lca(a,b))
```

### sparse table

```python

ac[here][i] = ac[ac[here][i - 1]][i-1]; << 핵심 dp코드

> tmp = ac[here][i-1];
> ac[here][i] = ac[tmp][i-1];
>
> tmp는 here의 2^(i-1)번째 조상

> 즉, ac[here][i] = ac[tmp][i-1]은
>
> here의 2^i번째 조상은 tmp의 2^(i-1)번째 조상의 2^(i-1)번째 조상과 같다는 의
>
> 예를들어 i = 3일때
>
> here의 8번째 조상은 tmp(here의 4번째 조상)의 4번째 조상과 같다.
>
> i =  4일때 here의 16번째 조상은 here의 8번째 조상(tmp)의 8번째와 같다.
```

