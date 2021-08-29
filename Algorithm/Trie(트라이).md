# Trie(트라이)

* = radix tree = prefix tree = digital search tree
* 탐색이라는 뜻의 retrieval에서 trie를 따온 것.

* 문자열 집합을 표현하는 트리 자료구조. 하나하나 비교가 아닌 key로 노드를 탐색.
* 원하는 원소를 찾는 작업을 O(M)에 해결
  * 이진트리는 O(logN)*문자열의 길이 M => O(MlogN)
* 빠르지만 저장공간의 크기가 매우 크다.(자식들의 포인터를 모두 배열로 저장하고 있기때문.)
* 문자열 검색 문제에서 입력되는 문자열이 많을 경우 사용. 

#### 적용되는 기능.

* 검색엔진 사이트에서 제공하는 자동완성 및 검색어 추천기능에서 Trie 알고리즘 사용.
* 맞춤법 검사, IP라우팅 (라우터에서 packet을 포워딩해줄때 다음 라우터로 경로를 결정하기 위해 패킷의 목적지 주소와 라우팅 테이블의 각 엔트리에 있는 prefix와 비교함. 즉 라우팅 엔트리의 prefix mask를 목적지 주소에 masking한 후 prefix와 비교. 가장 많이 매칭되는 엔트리를 채택.)

![img](https://mblogthumb-phinf.pstatic.net/MjAxOTEyMTdfODEg/MDAxNTc2NTYwMDI4MTMz.1e8_EuMhqD4ihjX8HbSYlQr_j7QvNseEVxH--dAjfH0g.he-A1TE1CTSRij8udR5LWSvgrZmggeRO9MmVS0lz0a0g.PNG.cjsencks/image.png?type=w800)

* Tree 형태, 문자열 끝을 알리는 flag가 존재.

![img](https://media.vlpt.us/images/gojaegaebal/post/e228f6a3-a0f9-4019-92a8-27a32460f686/image.png)

* 이런형태로 data값에 문자열 전체를 저장.

## 노드

```python
class Node(object):
    def __init__(self, key, data=None):
        self.key = key
        self.data = data
        self.children = {}
```

key = 입력 문자

data = 문자열 종료를 알리는 flag

children = 자식 노드



## Trie

```python
class Trie:
    def __init__(self):
        self.head = Node(None)

    def insert(self, string):
        current_node = self.head
		# 현재 들어온 단어들을 char별로 비교해 현재노드의 자식노드에 없다면 하나 자식 노드 새로 만들어서 넣어줌.
        for char in string:
            if char not in current_node.children:
                current_node.children[char] = Node(char)
            current_node = current_node.children[char]
        # for문끝나면 현노드 data에 문자열 저장.
        current_node.data = string

    def search(self, string):
        current_node = self.head
		
        for char in string:
            # 있으면 계속 다음 children으로 탐색.
            if char in current_node.children:
                current_node = current_node.children[char]
            # 문자가 중간에 발견이 안되면? -> 저장된 문자가 아니니 False 반환
            else:
                return False
		# for문 검색 끝났는데 data가 들어있으면 문자열이 있는 것. 없으면 그냥 경로일뿐 단어는 존재하지 않는 것.
        if current_node.data:
            return True
        else:
            return False

    def starts_with(self, prefix):
        current_node = self.head
        words = []

        for p in prefix:
            if p in current_node.children:
                current_node = current_node.children[p]
            else:
                return None

        current_node = [current_node]
        next_node = []
        while True:
            for node in current_node:
                if node.data:
                    words.append(node.data)
                next_node.extend(list(node.children.values()))
            if len(next_node) != 0:
                current_node = next_node
                next_node = []
            else:
                break

        return words
```

* head는 빈노드로 설정
* insert함수. : 트리를 생성하기 위한 함수. 
  * 입력된 문자열을 하나씩 children에 확인 후 저장.
  * 문자열을 다 돌면 마지막 노드의 data에 문자열을 저장(flag)

* search 함수: 문자열이 존재하는지에 대한 여부를 리턴하는 함수. 
  * 문자열을 하나씩 돌면서 확인 후 마지막 노드가 data가 존재한다면 True
  * 그렇지 않거나 애초에 children에 존재하지 않으면 False 리턴
* starts_with 함수 : prefix단어로 시작하는 단어를 찾고 배열로 리턴하는 함수. // 단어를 BFS로 트라이에서 찾아 반환한다.
  * prefix단어까지 tree를 순회 한 이후 그 다음부터 data가 존재하는 것들만 배열에 저장하여 리턴합니다.



## 예시

```python
trie = Trie()
word_list = ["frodo", "front", "firefox", "fire"]
for word in word_list:
    trie.insert(word)
```

Trie 생성, trie에 문자열 삽입.

```python
trie.search("friend")
>>False
trie.search("frodo")
>>True
trie.search("fire")
>>True
trie.starts_with("fire")
>>['fire', 'firefox']
trie.starts_with("fro")
>>['frodo', 'front']
trie.starts_with("jimmy")
>>None
trie.starts_with("f")
>>['fire', 'frodo', 'front', 'firefox']
```

출력.

