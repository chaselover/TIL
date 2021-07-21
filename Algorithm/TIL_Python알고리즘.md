# 파이썬 개인적으로 기억해야 할 것 정리(210721)

​	**순서는 그냥 쓰는대로 썼음**		



## 1. lambda



​	수업듣고 이해하게 됐음. 함수를 표현식형태로 다른곳에 인자로 쓰기위한 느낌.

​	lambda 매개변수 리턴값 뒤에 조건문 오던 말던.

​	sorted의 key값으로 쓰면 매우 유용함. sorted(list, lambda x: (x[1],x[0]),reverse=True)

​	=> 2번째 열, 1번째 열 순으로 이중정렬을 할수도있음.

​	sorted(dict,lambda x: dict[x]/[2])로 key값 대신서 index값 같고 key다른 여러값 동시에 뽑아 리스트로 만들수도있음.



## 2. map,reduce,filter



​	앞에 설명한 lambda를 쓰기 가장 좋은 함수를 인자로 받는 메소드들.

​	map,filter은 (함수,리스트)를 인자로 받고 reduce는 (함수,시퀀스)를 인자로 받음.

​	map은 리스트 인자 각각에 함수 적용.

​	filter은 함수 조건에 맞는 요소만 걸러서 리턴해줌.

​	둘다 기본값은 객체로 반환되니 리스트로 돌리기 위해선 list()로 감싸주어야 함.

​	reduce는 functools에 속한 메소드로 뭔가 유용한데 쓰기 어려움.

​	기본적으로는 연산값을 앞에서부터 시작해서 맨끝에 이를때까지 순차적으로 연산한 함수의 결과값을 반환함.

​	각자계산, 조건계산인 map,filter랑은 다르게 좀 과격한 느낌이 있음. (((a+b)+c)+d)이런느낌으로 retern값을 시퀀스(문자열, 리스트, 튜           플의 -1인덱스까지 돌려줌.)

```py
>>> reduce(lambda x, y: y + x, 'abcde')
'edcba'
```



## 3. 진법 변환

* int(string,진법수) => 10진법으로 변함. ex. int('111',2) = 7

* bin,oct,hex => 각 2진법, 8진법, 16진법. 앞에 0b, 0o, 0x 붙으니 쓰려면 변환한 값에 [2:]붙혀야 숫자만 나옴

* 10진수 -> n진수

```python
import string

tmp = string.digits+string.ascii_lowercase
def convert(num, base) :
    q, r = divmod(num, base)
    if q == 0 :
        return tmp[r] 
    else :
        return convert(q, base) + tmp[r]
```

이거짜서 하면됨.

convert(10,2)하면 해당 진법으로 바뀜. 16진법이상도 가능.



## 4. 패킹 언패킹 

* 패킹 반환값을 여러개 한번에 주어 하나에 담으면 자동으로 튜플로 들어감

* 하나의 튜플을 여러 변수에 각 할당시키면 언패킹.



## 5.표준 라이브러리

* itertools - permutations, combinations, product(중복순열) (n의 r승 ''ㅠ''모양), combinations_with_replacement중복허용한 조합.
* heapq -heapq, heappush, heappop
* bisect - bisect, left, right(잘 모름)
* collections - defaultdict, deque, Counter([배열])-> 모든 원소 갯수세서 dict객체로 반환 => dict(Counter([배열]))로 씀.
* math- gcd(a,b) => 최대공약수.

이 다섯개는 따로 찾아보고 써먹기.



## 6. Dictionary

​	hash table

.keys() -> list에 key들 들어있는 객체 반환

.get(key) -> value값 조회 key가 없을 시 에러 안나오고 None나옴.

.items() ->list안의 튜플쌍 객체 반환

.update -> 삽입

.pop(key) => key제거함과 동시에 value return

.defaultdict -> 존재하지 않는 키 접근 시 error대신 default값으로 생성.

.Ordereddict

.Counter -> 리스트 값의 갯수 집계해 딕셔너리로 리턴.



* list, dict comprehention

  {x:[] for x in range(1,41)} -> key값 1~40까지 빈배열을 value로 가지는 dictionary생성.

* zip(a,b) -> a,b는 리터럴 한 자료형 두개 리스트 안의 튜플 쌍으로 인덱스 같은 값끼리 묶어서 반환.

  => 짝짓기 할때 유용



## 7. Immutable 세가지 

## 튜플, frozenset(변경불가 셋), collections의 

## namedtuple(type명,필드목록(안에 어떤 속성값들을 지닐껀지.))





## 8. 순열  조합 하드코드

```python
def permute(arr): 
    result = [arr[:]] 
    c = [0] * len(arr) 
    i = 0 
    while i < len(arr):
        if c[i] < i: 
            if i % 2 == 0: 
                arr[0], arr[i] = arr[i], arr[0] 
            else: 
                arr[c[i]], arr[i] = arr[i], arr[c[i]] 
            result.append(arr[:]) 
            c[i] += 1 
            i = 0 
        else: 
            c[i] = 0 
            i += 1 
    return result


# 재귀
def perm(lis, n): 
    result = [] 
    if n > len(lis): 
        return result 
    if n == 1: 
        for li in lis: 
            result.append([li]) 
    elif n > 1: 
        for i in range(len(lis)): 
            tmp = [i for i in lis] 
            tmp.remove(lis[i]) 
            for j in perm(tmp, n-1): 
                result.append([lis[i]]+j) 
    return result 
n = int(input()) 
lis = list(i for i in range(1, n+1)) 

for li in perm(lis,n): 
    print(' '.join(list(map(str, li))))

```



위에는 둘다 순열, depth조절하면 조합으로 바꿀수있음.



## 9. visted를 dict로 짜는 이유.

```python
def bfs_queue(graph, start): 
    visited = {} 
    q = Queue() 
    q.put(start) 
    while q.qsize() > 0:
        n = q.get() 
        if n not in visited: 
            visited[n] = True 
            for adj in graph[n]: 
                q.put(adj) 
    return list(visited.keys())

```



not in visited를 순회로 접근이 아닌 hash로 접근하기 위해 visited를 dict나 set(인덱싱하지 않는 자료형들)로 가는게 좋음.



## 10. stack dfs

```python
def dfs(graph, start): 
    visited = [] 
    stack = [start]
    while stack: 
        n = stack.pop() 
        if n not in visited: 
            visited.append(n) 
            stack += graph[n] - set(visited) 
    return visited

        
```

DFS는 재귀로 많이들 하지만 원래 stack으로도 구현하는 알고리즘.



## 11. 숫자 문자열 앞에 0채우는 A.zfill(자리수)

## A문자열을 x자리수만큼 앞에 0을 채워준다.

