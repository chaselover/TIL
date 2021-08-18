## Cut Vertex ( 절단점 찾기 )

**Cut Vertex(절단점)**

------

그래프의 절단점이란 해당 정점을 삭제했을 때 그래프가 두 개 이상의 그래프로 나누어 지는 점을 말합니다. 이 때 그래프는 항상 **undirected graph**입니다.

<br>

1) 직관적으로 절단점을 찾는 방법을 생각해 봅시다. (러프한 방법. 직접 삭제해본다. 섬나누기 or 연구소 같은 방법.)

   느낌상 모든 정점에 대해 그 정점을 삭제한 후의 그래프를 먼저 따로 구합니다.

   그 다음 DFS 혹은 BFS 로 몇 번만에 전체 그래프를 cover할 수 있는지를 보면 될 것 같습니다.

   그러나 이 방식은 각 정점마다 알고리즘을 돌려야 하니 정점이 많아 질수록 매우 불리해 집니다.

<br>

2) 무향그래프의 DFS Spanning tree를 보면 cross edge가 없다는 사실을 알 수 있습니다.

   한 정점 u를 기준으로 생각해 보면 u를 지웠을 때 절단점이 되지 않는 경우는 언제일까요?

   DFS Spanning tree에서 u의 자식 정점을 root로 하는 sub tree에서 u의 선조로 역방향 간선이 모두 존재하면 됩니다. 

   그렇게 되면 u를 지워도 u의 자식들이 root로 있는 sub tree는 모두 그대로 연결이 되어 그래프가 하나로 유지되기 때문입니다.

   즉 u의 자식들이 root인 sub tree로부터 u의 선조로 가는 역방향 간선이 하나라도 없다면 u가 절단점이라는 사실을 얻었습니다.

   이렇게 되면 DFS한번에 모든 절단점을 찾을 수 있으니 매우 효율적인 방법이 됩니다.

<br>

> #### 참고
>
> - **트리간선( Tree Edge )** : DFS의 결과로 완성된 트리의 모든 간선들
>   - BFS는 하나의 트리를 완성하지만 DFS는 여러개의 서브트리를 만든다.
> - **역행간선(** **Back Edge )** : 트리에서 자식이 조상을 연결하는 간선들(밑에서 위로 올라오는)
> - **순행간선( Forward Edge )** : 트리에서 조상이 자식을 연결하는 Tree Edge가 아닌 간선들(DFS를 돌지 않은 sub tree)
> - **교차간선( Cross Edge )** : 이 외의 나머지모든 간선들

<br>

> **절단점을 찾는 방법**
>
> DFS spanning tree에서 정점 u의 자식들이 root가 되는 sub tree로부터 u의 선조로 가는 역방향 간선이 하나라도 없다면,
>
> u는 절단점이다. (다른 sub tree로 부터 u가 속한 sub tree로 돌아오는 간선이 없다면 절단점이다.)

<br>

### 구현

```c++
#include<cstdio>
#include<vector>
#define min(a, b) (((a) < (b))? (a) : (b))
using namespace std;
 
int cnt;
vector< vector<int> > adj;
vector<int> order;
vector<bool> isCutVertex;
 
/*
here를 root로 하는 tree에서 발견된 역방향 간선으로 이어진 정점 중
가장 상위 정점 order를 반환한다.
(전체 DFS spanning tree에서 상대적으로 가장 높은 정점의 순서)
*/
int findCutVertex(int here, bool isRoot) {
    order[here] = cnt++;
    int child = 0
    int ret = order[here];
    for (auto& there : adj[here]) {
        if (order[there] == -1) { //tree edge
            /* root인 경우 DFS spanning tree의 자식의 갯수를 알아야 한다. 
            자식의 갯수가 1개인 경우는 절단점이 될 수 없기 때문이다. */
            child++;
 
            /* 자식 sub tree에서 역방향 간선으로 가는 
            가장 상위 정점 order를 반환받는다. */
            int v_order = findCutVertex(there, false);
 
            /* 받은 order가 현재 정점의 순서보다 뒤라면 절단점이다. */
            if (!isRoot && v_order >= order[here])
                isCutVertex[here] = true;
 
            /* 현재 tree에서 갈 수 있는 가능한 상위 정점의 order를 업데이트 */
            ret = min(ret, v_order);
        }else {
            /* 역방향 간선으로 가는 정점 중 가장 상위 order를 업데이트*/
            ret = min(ret, order[there]);
        }
        
    }
    if (isRoot && child >= 2) isCutVertex[here] = true;
    return ret;
}
 

```

<br>

문제 : https://www.acmicpc.net/problem/11266

```python
import sys;
input=sys.stdin.readline
sys.setrecursionlimit(99999)
# 그래프 그리기.
V,E=map(int,input().split())
adj=[set() for _ in range(10002)]
for _ in range(E):
	a,b=map(int,input().split())
	adj[a].add(b)
	adj[b].add(a)

discovered=[0]*10002
isCut=set()
idx=0
def dfs(node,isRoot):
	global idx
	idx+=1
    # idx는 몇번째 순회냐를 말함(몇번째 subtree냐)
	discovered[node]=ret=idx
	cnt=0
	for i in adj[node]:
        # 이미 순회된 노드라면 ret값에 몇번째 sub tree인지 값을 리턴해주고 다음 노드 탐색.
		if discovered[i]:
			ret=min(ret,discovered[i])
			continue
		cnt+=1
        # 탐색 안된 노드면 다음 노드 순회 리턴값으로 타잔이 물어오는 ret값이나 내 ret중 작은걸 가져감.
		prev=dfs(i,False)
		ret=min(ret,prev)
        # 내 idx값이 탐색한놈 idx보다 작거나 같고(idx가 다름은. 다른 서브트리를 말한다.) 나는 루트가 아니면? 자른다.
		if discovered[node]<=prev and not isRoot:
			isCut.add(node)
    # 루트노드고 인접정점에 진입 한 적 있다면?(sub tree가 있다면?) 자른다.
	if isRoot and cnt>1:
		isCut.add(node)
	return ret
# 정점을 돌며 체크.
for i in range(1,1+V):
    # 이미 발견된 정점이면 continue 아니면 dfs실시.
	discovered[i] or dfs(i,True)

print(len(isCut))
print(*sorted(isCut))
```

문제 : https://www.acmicpc.net/problem/11400

<br>

### 단절선.

선을 삭제했을 때 그래프가 나뉘는 간선

1. 단절점에서 root  node의 개념을 빼고
2. r >= ord[n]을 r>ord[n]으로 등호를 없애준다.

<br>

> 단절 선을 찾을때는 부모 정점으로 가는 간선을 제외하고 찾아야 하냐면 사이클이 존재하는 경우 단절 선이 아닌데도 단절선이라고 착각하기 때문이다.

<br>

> 한 정점에서 부모 정점으로 가는 간선을 제외하고 남은 연결된 간선들 중에서 되돌아오는 최소 순번값이 본인의 순번값보다 크다면 단절선이라고 판단합니다.

<br>

```python
import sys;input=sys.stdin.readline
sys.setrecursionlimit(200000)
V,E=map(int,input().split())
adj=[set() for _ in range(100001)]
for _ in range(E):
	a,b=map(int,input().split())
	adj[a].add(b)
	adj[b].add(a)

discovered=[0]*100001
isCut=set()
idx=0
# 단절점과 다른점은 root노드에 대한 회귀검사를 하지 않는다는 것.
def dfs(node,parent=0):
	global idx
	idx+=1
	discovered[node]=ret=idx
	for i in adj[node]:
		if i==parent: continue
		if discovered[i]:
			ret=min(ret,discovered[i])
			continue
		prev=dfs(i,node)
		ret=min(ret,prev)
		if discovered[node]<prev:
			isCut.add(tuple(sorted([node,i])))
	return ret

for i in range(1,1+V):
	discovered[i] or dfs(i,0)
isCut=sorted(isCut)
print(len(isCut))
[*map(print,*zip(*isCut))]
```

