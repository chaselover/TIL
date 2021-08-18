# 강한 연결 요소(SCC)

무향 그래프에서 절단점이 존재하지 않는 그래프를 우리는 **Biconnected component**라고 부릅니다. 

절단점이 없다면 그래프 상의 어떤 점을 삭제하더라도 임의의 두 정점은 서로로 가는 경로가 존재하게 됩니다.

<br>

* **Biconnected Component**
  * Undirected Graph에서 절단점이 존재하지 않는 그래프

<br>

* **Strongly Connected Component**
  * 만약 그래프의 임의의 한 정점에 대해 다른 모든 정점으로 가는 경로가 존재한다면 그 그래프를 **SCC(Strongly Connected Component)**라고 한다.

<br>

## SCC(Strongly Connected Component)특징

1. **같은 SCC**내에서 뽑은 임의의 U, V 점에서 U->V 혹은 V->U의 경로(직/간접적)는 **항상 존재**한다.

2. **서로 다른 SCC**에서 뽑은 임의의 U, V 점에서 U->V의 직/간접적 경로 혹은 V->U 직/간접적 경로는 **존재하지 않는다**. 즉, 서로다른 SCC끼리는 사이클이 존재하지 않는다는 의미이다.

3. SCC는 **Maximal하기 때문에 SCC가 형성될 수 있는 **가장 큰 집합으로 형성 된다.

![img](https://t1.daumcdn.net/cfile/tistory/2167EE4F596EDAD327)

A와 B는  SCC이다.

A와 D는 같은 SCC이다(A,B,C,D)

A, H는 다른 SCC이다.

A, E는  다른 SCC이다.

![img](https://t1.daumcdn.net/cfile/tistory/252D78425971B1C519)

결국 초록색 원으로 서로 다른 SCC가 구성되었을 때 사이클이 모두 없어지게 된다는 것을 알 수 있다.

-> 큰 묶음을 정점으로 쳤을 때 DAG가 구성 되어 사이클이 존재하는 그래프의 위상정렬을 가능하게 한다.

```c++
#include <cstdio>
#include <vector>
#include <stack>
#include <algorithm>
 
using namespace std;
 
const int MAX = 10010;
 
// SCCtotal : SCC 개수, sn[i]: i가 속한 SCC의 번호
int V, E, cnt, dfsn[MAX], SCCtotal, sn[MAX];
vector<int> adj[MAX];
bool finished[MAX]; // SCC가 확정된 정점 판별
stack<int> s;
vector<vector<int>> SCC;
 
// SCC에 사용될 dfs
int dfs(int cur_node)
{
    dfsn[cur_node] = ++cnt; // dfsn 결정 최초 0
    s.push(cur_node); // 스택에 자신을 push // dfs를 탐사할 시작 node
 
    // 자신의 dfsn, 자식들의 결과나 dfsn 중 가장 작은 번호를 result에 저장
    int result = dfsn[cur_node];
    for (int next : adj[cur_node])
    {
        // 아직 방문하지 않은 이웃 방문. result값으로 내 scc의 대표노드번호 or 사이클에 속하면 next가 물어오는 노드번호
        if (dfsn[next] == 0)
            result = min(result, dfs(next));
 
        // 방문은 했으나 아직 SCC로 추출되지는 않은 이웃 부모값 or 내 값. 둘 중 작은 것.(사이클이 만들어지면dfsn[next]가 보통 작은 숫자의 노드이다)
        else if (!finished[next])
            result = min(result, dfsn[next]);
    }
 
    // 자신, 자신의 자손들이 도달 가능한 제일 높은 정점이 자신일 경우 SCC 추출
    // 반환된 값들이 내려가다가 result(자신)과 scc대표번호가 같다면?
    if (result == dfsn[cur_node])
    {
        vector<int> curSCC;
        // 스택에서 자신이 나올 때까지 pop(여기서 pop된 것들은 scc를 완성했기에 sn[t]에 등록되고 finished=True된다.)
        while (1)
        {
            int t = s.top();
            s.pop();
            curSCC.push_back(t);
            finished[t] = true;
            sn[t] = SCCtotal;
 			//자기자신까지 pop이 이루어지면 반복문 종료
            if (t == cur_node)
                break;
        }
 
        // 출력을 위해 원소 정렬
        sort(curSCC.begin(), curSCC.end());
 
        // SCC에 현재 이루어진 SCC 정보 push_back
        SCC.push_back(curSCC);
        SCCtotal++; // SCC 개수 하나 증가(집단갯수)
    }
 	//result를 리턴함으로써 이 값이 어디까지 back되어 가냐로 scc 구성 여부를 정한다.
    return result;
}
 
int main() {
    // node의 수 V edge의 수 E
    scanf("%d %d", &V, &E);
    for (int i = 0; i< E; i++) {
        int from, to;
        scanf("%d %d", &from, &to);
        // 인접정점에 대한 그래프 adj
        adj[from].push_back(to);
    }
 
    // DFS를 하며 SCC 추출
    for (int i = 1; i <= V; i++)
        if (dfsn[i] == 0)
            dfs(i);
 
    // 출력을 위해 SCC들을 정렬
    sort(SCC.begin(), SCC.end());
 
    // SCC 개수
    printf("%d\n", SCCtotal);
 
    // 각 SCC 출력
    for (auto& currSCC : SCC)
    {
        for (int curr : currSCC)
            printf("%d ", curr);
 
        printf("-1\n");
    }
}


출처: https://www.crocus.co.kr/950?category=150836 [Crocus]
```

<br>

![img](https://t1.daumcdn.net/cfile/tistory/245C98425971B8B407)

A부터 출발하면 dfsn[A] = 1이 되고 스택에 push된다 (stack : A)

result는 1이 되고 A와 연결되어있는 간선 즉 B로 다음 dfs 탐색이 실행된다.

B는 그럼 dfsn[B] = 2가 되고 스택에 push가 된다.(stack : B A)

그리고 result는 2가 되고  B와 인접한 간선 D로 향하게 된다.

D는 dfsn[D] = 3, (stack : D B A), result = 3, C로 향하게 된다.

C에서는 dfsn[C] = 4, (stack : C D B A)가 된다.

이때 C의 인접 간선이 A뿐인데 dfsn[next] 가 이미 존재한다. dfsn[A] = 1이기에 else if로 향한다.

A가 아직 finished배열이 이용되지 않았기에(SCC가 확정나지 않은 노드이기에) 

result = min(4, dfsn[A]) 즉 min(4,1)이 되어 result는 1이 된다.

이렇게 계속 dfs의 result값은 1로(SCC의 대푯값으로) 리턴되다가 마지막 A의 dfs로 돌아왔을 때 

if(result == dfsn[curr]에 걸리게 되어 현재 스택에 존재한 값 C D B A 가 하나의 SCC를 이루게 된다.

결과적으로 총 3개의 SCC가 생성된다.

---

<br>

#### **코사라주 알고리즘(kosaraju's algorithm)**

- dfs를 활용하여 그래프 내의 ssc를 찾는 알고리즘
- 시간 복잡도 O(V+E)
- 정방향 그래프에서 dfs를 실행하여 post가 높은 정점이 sCc의 source가 되고,
- 역방향 그래프에서 dfs를 실행하여 post가 높은 정점이 scc의 sink가 된다.
- 찾은 source와 sink를 묶어 scc를 생성한다.

<br>

#### **작동 원리**

(1) 방향 그래프 내에서 dfs를 실행하여, dfs 탐색을 마친 정점을 stack에 삽입한다.

(DFS를 통한 위상정렬 후 스택에 삽입.)(stack에서pop하기 시작하면 scc에 묶인 점들끼리 같이나온다(위상이 같기때문에 순서(서열)이 생긴다.))

(2) 역방향 그래프를 생성한다.(모든 간선의 방향을 반대로 한 그래프)

(3) stack에서 정점을 pop하여, 역방향 그래프에서 dfs를 실행한다.

(4) 방문하는 정점을 scc 에 삽입한다.

(5) 탐색이 종료되면, 검출되는 모든 정점이 scc

(6) stack에서 방문하지 않은 정점을 추출하여, 역방향 그래프에서 dfs를 실행

덩어리들의 그래프는 DAG(순환없는 유향그래프) 를 이룬다.

<br>

#### **파이썬 코드**

```python
import sys
sys.setrecursionlimit(10 ** 6)

v, e = map(int, sys.stdin.readline().split())
graph = [[] for _ in range(v + 1)]
reverse_graph = [[] for _ in range(v + 1)]
for _ in range(e):
    a, b = map(int, sys.stdin.readline().split())
    # 정방향 그래프 및 역방향 그래프에 주어진 그래프를 추가해준다.
    graph[a].append(b)
    reverse_graph[b].append(a)


# 정방향 dfs, dfs 가 종료되는 노드를 stack 에 쌓는다.
def dfs(node, visited, stack):
    visited[node] = 1
    for ne in graph[node]:
        if visited[ne] == 0:
            dfs(ne, visited, stack)
    stack.append(node)


# 역방향 dfs, 탐색하는 순서대로 stack (scc) 에 쌓는다.
def reverse_dfs(node, visited, stack):
    visited[node] = 1
    stack.append(node)
    for ne in reverse_graph[node]:
        if visited[ne] == 0:
            reverse_dfs(ne, visited, stack)

# 코사라주 알고리즘
stack = []
visited = [0] * (v + 1) 
# 모든 노드에서 정방향 dfs 를 수행한다.
for i in range(1, v + 1):
    if visited[i] == 0:
        dfs(i, visited, stack) # stack에 탐색을 마친 정점 순으로 저장
visited = [0] * (v + 1) # visited 초기화
result = [] # ssc를 담을 result 생성
# 스택이 빌 때까지 pop 되는 요소에서 역방향 dfs 를 진행하여 scc 를 결과에 추가해준다.
while stack:
    ssc = []
    node = stack.pop() # stack에서 ssc의 source 추출
    if visited[node] == 0:
        reverse_dfs(node, visited, ssc) # dfs를 돌며 ssc의 source부터 탐색을 진행한다. 탐색한 정점은 ssc에 저장
        result.append(sorted(ssc)) # 재귀가 끝난 정점이 ssc의 sink

print(len(result))
results = sorted(result)
for i in results:
    print(*i, -1)
```

<br>

---

<br>

## Tarjan's Algorithm

* 코사라주는 DFS 2개
* 타잔은 DFS 1개
* DFS를 돌며 만들어지는  DFS spanning tree에서 간선을 적당히 자르면 SCC를 구분 할 수 있다.
* Directed graph의 DFS 신장 트리는 forward edge가 없다
  * tree edge,cross edge, back edge뿐
* cross edge가 없는 경우 DFS 신장트리의 리프 노드 부터 sub-tree가 SCC인지 판단해 간선을 잘라내는 방식.
  * sub-tree에 갈 수 있는 최소 순서 정점(low-link)를 잘라낸다.
* cross edge(교차 간선)이 있는 경우는 **도달하는 정점이(교차간선의 도착점)아직 SCC가 구분 되어 있지 않은 경우**에만 없을 떄와 마찬가지로 최소 순서 정점으로 계산해 값을 return하면 된다.
  * 있을 때는 이미 방문 처리됐으므로 root로 돌아갈 길이 없음. 즉 SCC 를 이룰 수 없다.

<br>

### low-link값

* DFS 할 때 자기 자신을 포함, 도달할 수 있는 노드 id가 가장 작은 값이다.(그냥 번호가 가장작은 값)
* SCC를 이루는 cycle의 대푯값.

<br>

1. stack에 노드를 추가한다
2. u와 v가 그래프에 있고, 현재 u 노드를 탐색 중이라면
3. u의 low-link값은 자기 자신 값과 v의 low-link 값중 작은 값이며
4. stack에 있다면 u의 low-link 값은 v 노드 id 값 중 작은 값이다.
   * stack에 다음 노드가 들어있으면 부모에 대한 DFS가 바뀌지 않음을 알고 부모값을 id로 부여
5. 한번 스택에서 뽑은 모든 노드들(DFS하나가 끝났을때)은 하나의 SCC다.

<br>

> 결론적으로 stack에 다음 노드가 있다면(cycle에 속한다면) stack을 pop해주며 진행하고 더이상 진행할 길이 없고 부모값이 자기와 동일하면 자신이 부모임을 알고 SCC를 이룬다.

<br>

DFS하는 순서대로 노드 id값을 부여,

```python
from collections import defaultdict

UNVISITED = -1

class Graph:
    global UNVISITED
    
    def __init__(self, verticies):
        self.v = vertices
        self.g = defaultdict(list)
        self.id = 0
    
    def addEdge(self, u, v):
        self.g[u].append(v)
        
    def findSCCs(self):
        ids = [UNVISITED] * self.v
        low = [UNVISITED] * self.v
        
        onStack = [False] * self.v
        stack = []
        
        for i in range(self.v):
            if ids[i] == UNVISITED: # node i가 처음이면 dfs
                self.dfs(i, low, ids, onStack, stack)
        
        return low # low-link 값이 같은 것끼리 SCC를 만든다
    
    def dfs(self, at, low, ids, onStack, stack):
        ids[at] = self.id
        low[at] = self.id
        self.id += 1 # id 부여
        
        onStack[at] = True
        stack.append(at)
        
        for to in self.g[at]:
            if ids[to] == UNVISITED:
                self.dfs(to, low, ids, onStack, stack)
                low[at] = min(low[at], low[to])
            elif inStack[to] ==True:
                low[at] = min(low[at], ids[to])
                
        w = UNVISITED
        if low[at] == ids[at]:
         	print("Strong Connected Components: ", end="")
            while w != at:
                w = stack.pop()
                print(f'Node {w}',end=' ')
                onStack[w] = False
            print()
            
    def __str__(self):
        return self.print()
    
  	def print(self):
        for vsertice, edge in self.g.items():
            print(f'{vertice} ->', *edge)
            
if __name__ == '__main__':
    g = Graph(8)
    g.aeeEdge(0,1)
    g.aeeEdge(1,2)
    g.aeeEdge(2,0)
    g.aeeEdge(3,4)
    g.aeeEdge(3,7)
    g.aeeEdge(4,5)
    g.aeeEdge(5,0)
    g.aeeEdge(5,6)
    g.aeeEdge(6,0)
    g.aeeEdge(6,2)
    g.aeeEdge(6,4)
    g.aeeEdge(7,3)
    g.aeeEdge(7,5)
    
    g.findSCCs()
    
    """
    Strongly Connected Components: Node 2 Node 1 Node 0
    Strongly Connected Components: Node 6 Node 5 Node 4
    Strongly Connected Components: Node 7 Node 3
    """
    
    
```

다른코드

```python
import sys
sys.setrecursionlimit(10 ** 6)

v, e = map(int, sys.stdin.readline().split())
graph = [[] for _ in range(v + 1)]
for _ in range(e):
    a, b = map(int, sys.stdin.readline().split())
    graph[a].append(b)

stack = []
low = [-1] * (v + 1)
ids = [-1] * (v + 1)
visited = [0] * (v + 1)
idid = 0
result = []


def dfs(x, low, ids, visited, stack):
    global idid
    ids[x] = idid
    low[x] = idid
    idid += 1
    visited[x] = 1
    stack.append(x)

    for ne in graph[x]:
        if ids[ne] == -1:
            dfs(ne, low, ids, visited, stack)
            low[x] = min(low[x], low[ne])
        elif visited[ne] == 1:
            low[x] = min(low[x], ids[ne])

    w = -1
    scc = []
    if low[x] == ids[x]:
        while w != x:
            w = stack.pop()
            scc.append(w)
            visited[w] = -1
        result.append(sorted(scc))


for i in range(1, v + 1):
    if ids[i] == -1:
        dfs(i, low, ids, visited, stack)
print(len(result))
for i in sorted(result):
    print(*i, -1)
```

stack을 쓰는 이유는 같은 SCC정점을을 모아놓고 한번에 SCC로 묶기 위함. 과정이 끝나면 low에는 각정점에 대해 SCC가 구분되어 기록된다.

<br>

---

<br>

