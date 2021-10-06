# Splay tree

* 이진 탐색 트리의 한 종류.
* 삽입, 삭제, 검색 등 쿼리 O(logn)에 처리.
* AVL, Red-Black 보다 구현이 단순한 편.
* splay 연산을 이용.



## Splay 연산

* 임의의 노드 x를 루트로 만드는 연산.

![img](https://t1.daumcdn.net/cfile/tistory/2210373457CAF50E0F)

1. x가 루트이면, 루트를 만드는 데 성공했으므로 종료한다.

2. x의 부모 p가 루트이면, Rotate(x)를 행하고 종료한다. (Zig Step)

3. x의 조부모를 g라고 하면, 다음 두 가지 경우가 있다.
   1. g→p의 방향과 p→x의 방향이 같은 경우, Rotate(p) 이후 Rotate(x)를 행한다. (Zig-Zig Step)

   2. g→p의 방향과 p→x의 방향이 다른 경우, Rotate(x)를 두 번 행한다. (Zig-Zag Step)

4. 1로 돌아가서 루트가 될 때까지 반복한다.



### Zig-Zig Step



![img](https://t1.daumcdn.net/cfile/tistory/2428274157CAFAAD04)



### Zig-Zag Step



![img](https://t1.daumcdn.net/cfile/tistory/2662E54757CAFC9434)

```python
# Splay tree implementation in Java
# Author: AlgorithmTutor
# Tutorial URL: http://algorithmtutor.com/Data-Structures/Tree/Splay-Trees/

# data structure that represents a node in the tree

import sys

class Node:
	def  __init__(self, data):
		self.data = data
		self.parent = None
		self.left = None
		self.right = None

class SplayTree:
	def __init__(self):
		self.root = None

	def __print_helper(self, currPtr, indent, last):
		# print the tree structure on the screen
	   	if currPtr != None:
			sys.stdout.write(indent)
			if last:
			  	sys.stdout.write("R----")
			  	indent += "     "
			else:
				sys.stdout.write("L----")
				indent += "|    "

			print currPtr.data

			self.__print_helper(currPtr.left, indent, False)
			self.__print_helper(currPtr.right, indent, True)
	
	def __search_tree_helper(self, node, key):
		if node == None or key == node.data:
			return node

		if key < node.data:
			return self.__search_tree_helper(node.left, key)
		return self.__search_tree_helper(node.right, key)

	def __delete_node_helper(self, node, key):
		x = None
		t = None 
		s = None
		while node != None:
			if node.data == key:
				x = node

			if node.data <= key:
				node = node.right
			else:
				node = node.left

		if x == None:
			print "Couldn't find key in the tree"
			return
		
		# split operation
		self.__splay(x)
		if x.right != None:
			t = x.right
			t.parent = None
		else:
			t = None

		s = x
		s.right = None
		x = None

		# join operation
		if s.left != None:
			s.left.parent = None

		self.root = self.__join(s.left, t)
		s = None

	# rotate left at node x
	def __left_rotate(self, x):
		y = x.right
		x.right = y.left
		if y.left != None:
			y.left.parent = x

		y.parent = x.parent
		if x.parent == None:
			self.root = y
		elif x == x.parent.left:
			x.parent.left = y
		else:
			x.parent.right = y
		y.left = x
		x.parent = y

	# rotate right at node x
	def __right_rotate(self, x):
		y = x.left
		x.left = y.right
		if y.right != None:
			y.right.parent = x
		
		y.parent = x.parent;
		if x.parent == None:
			self.root = y
		elif x == x.parent.right:
			x.parent.right = y
		else:
			x.parent.left = y
		
		y.right = x
		x.parent = y

	# Splaying operation. It moves x to the root of the tree
	def __splay(self, x):
		while x.parent != None:
			if x.parent.parent == None:
				if x == x.parent.left:
					# zig rotation
					self.__right_rotate(x.parent)
				else:
					# zag rotation
					self.__left_rotate(x.parent)
			elif x == x.parent.left and x.parent == x.parent.parent.left:
				# zig-zig rotation
				self.__right_rotate(x.parent.parent)
				self.__right_rotate(x.parent)
			elif x == x.parent.right and x.parent == x.parent.parent.right:
				# zag-zag rotation
				self.__left_rotate(x.parent.parent)
				self.__left_rotate(x.parent)
			elif x == x.parent.right and x.parent == x.parent.parent.left:
				# zig-zag rotation
				self.__left_rotate(x.parent)
				self.__right_rotate(x.parent)
			else:
				# zag-zig rotation
				self.__right_rotate(x.parent)
				self.__left_rotate(x.parent)

	# joins two trees s and t
	def __join(self, s, t):
		if s == None:
			return t

		if t == None:
			return s

		x = self.maximum(s)
		self.__splay(x)
		x.right = t
		t.parent = x
		return x

	def __pre_order_helper(self, node):
		if node != None:
			sys.stdout.write(node.data + " ")
			self.__pre_order_helper(node.left)
			self.__pre_order_helper(node.right)

	def __in_order_helper(self, node):
		if node != None:
			self.__in_order_helper(node.left)
			sys.stdout.write(node.data + " ")
			self.__in_order_helper(node.right)

	def __post_order_helper(self, node):
		if node != None:
			self.__post_order_helper(node.left)
			self.__post_order_helper(node.right)
			std.out.write(node.data + " ")

	# Pre-Order traversal
	# Node->Left Subtree->Right Subtree
	def preorder(self):
		self.__pre_order_helper(self.root)

	# In-Order traversal
	# Left Subtree -> Node -> Right Subtree
	def inorder(self):
		self.__in_order_helper(self.root)

	# Post-Order traversal
	# Left Subtree -> Right Subtree -> Node
	def postorder(self):
		self.__post_order_helper(self.root)

	# search the tree for the key k
	# and return the corresponding node
	def search_tree(self, k):
		x = self.__search_tree_helper(self.root, k)
		if x != None:
			self.__splay(x)

	# find the node with the minimum key
	def minimum(self, node):
		while node.left != None:
			node = node.left
		return node

	# find the node with the maximum key
	def maximum(self, node):
		while node.right != None:
			node = node.right
		return node

	# find the successor of a given node
	def successor(self, x):
		# if the right subtree is not null,
		# the successor is the leftmost node in the
		# right subtree
		if x.right != None:
			return self.minimum(x.right)

		# else it is the lowest ancestor of x whose
		# left child is also an ancestor of x.
		y = x.parent
		while y != None and x == y.right:
			x = y
			y = y.parent
		return y

	# find the predecessor of a given node
	def predecessor(self, x):
		# if the left subtree is not null,
		# the predecessor is the rightmost node in the 
		# left subtree
		if x.left != None:
			return self.maximum(x.left)

		y = x.parent
		while y != None and x == y.left:
			x = y
			y = y.parent
		return y

	# insert the key to the tree in its appropriate position
	def insert(self, key):
		node =  Node(key)
		y = None
		x = self.root

		while x != None:
			y = x
			if node.data < x.data:
				x = x.left
			else:
				x = x.right

		# y is parent of x
		node.parent = y
		if y == None:
			self.root = node
		elif node.data < y.data:
			y.left = node
		else:
			y.right = node
		# splay the node
		self.__splay(node)

	# delete the node from the tree
	def delete_node(self, data):
		self.__delete_node_helper(self.root, data)

	# print the tree structure on the screen
	def pretty_print(self):
		self.__print_helper(self.root, "", True)

if __name__ == '__main__':
	tree = SplayTree()
	tree.insert(33)
	tree.insert(44)
	tree.insert(67)
	tree.insert(5)
	tree.insert(89)
	tree.insert(41)
	tree.insert(98)
	tree.insert(1)
	tree.pretty_print()
	tree.search_tree(33)
	tree.search_tree(44)
	tree.pretty_print()
	tree.delete_node(89)
	tree.delete_node(67)
	tree.delete_node(41)
	tree.delete_node(5)
	tree.pretty_print()
	tree.delete_node(98)
	tree.delete_node(1)
	tree.delete_node(44)
	tree.delete_node(33)
	tree.pretty_print()
```

9548 무제

```python
import sys

class Item():
    def __init__(self):
        self.p = -1
        self.l = -1
        self.r = -1
        self.it = 0
        self.cnt = 1

def update(x):
    global nodes
    global root
    nodes[x].cnt = 1
    if nodes[x].l != -1:
        nodes[x].cnt += nodes[nodes[x].l].cnt
    if nodes[x].r != -1:
        nodes[x].cnt += nodes[nodes[x].r].cnt

def rotate(x):
    global nodes
    global root
    p = nodes[x].p
    b = -1
    if nodes[p].l == x:
        b = nodes[x].r
        nodes[p].l = b
        nodes[x].r = p
    else:
        b = nodes[x].l
        nodes[p].r = b
        nodes[x].l = p
    if b != -1:
        nodes[b].p = p
    nodes[x].p = nodes[p].p
    nodes[p].p = x
    if nodes[x].p == -1:
        root = x
    elif nodes[nodes[x].p].l == p:
        nodes[nodes[x].p].l = x
    else:
        nodes[nodes[x].p].r = x
    update(p)
    update(x)

def splay(x):
    global nodes
    global root
    while nodes[x].p != -1:
        p = nodes[x].p
        g = nodes[p].p
        if g != -1:
            if (nodes[p].l == x) == (nodes[g].l == p):
                rotate(p)
            else:
                rotate(x)
        rotate(x)

def find_k(k):
    global nodes
    global root
    k += 1
    x = root
    while 1:
        while nodes[x].l != -1 and nodes[nodes[x].l].cnt > k:
            x = nodes[x].l
        if nodes[x].l != -1:
            k -= nodes[nodes[x].l].cnt
        if k == 0:
            break
        k -= 1
        x = nodes[x].r
    splay(x)

def interval(l, r):
    global nodes
    global root
    find_k(l - 1)
    x = root
    root = nodes[x].r
    nodes[root].p = -1
    find_k(r - l)
    nodes[root].p = x
    nodes[x].r = root
    root = x

def init():
    global nodes
    global root
    del nodes
    nodes = []
    nodes.append(Item())
    nodes.append(Item())
    l = len(nodes)
    nodes[l - 2].r = l - 1
    nodes[l - 1].p = l - 2
    nodes[l - 2].cnt = 2
    root = l - 2

nodes = []
init()
t = int(sys.stdin.readline())
if t > 1:
    exit(0)
i = 0
while i < t:
    s = sys.stdin.readline().rstrip()
    l = len(s)
    init()
    find_k(0)
    x = nodes[root].l
    nodes.append(Item())
    y = len(nodes) - 1
    nodes[root].l = y
    nodes[x].p = y
    nodes[y].p = root
    nodes[y].l = x
    nodes[y].it = s[0]
    nodes[y].cnt = l + nodes[x].cnt
    j = 1
    while j < l:
        nodes.append(Item())
        x = len(nodes) - 1
        nodes[x].p = y
        nodes[y].r = x
        nodes[x].it = s[j]
        nodes[x].cnt = l - j;
        y = x
        j += 1
    update(root)
    while 1:
        q = sys.stdin.readline()
        if q[0] == 'E':
            break
        if q[0] == 'I':
            a, b, c = q.split()
            c = int(c)
            l = len(b)
            find_k(c);
            x = nodes[root].l
            nodes.append(Item())
            y = len(nodes) - 1
            nodes[root].l = y
            nodes[x].p = y
            nodes[y].p = root
            nodes[y].l = x
            nodes[y].it = b[0]
            nodes[y].cnt = l + nodes[x].cnt
            j = 1
            while j < l:
                nodes.append(Item())
                x = len(nodes) - 1
                nodes[x].p = y
                nodes[y].r = x
                nodes[x].it = b[j]
                nodes[x].cnt = l - j;
                y = x
                j += 1
            update(root)
        else:
            a, b, c = q.split()
            b = int(b)
            c = int(c)
            j = b
            while j <= c:
                find_k(j)
                print(nodes[root].it, end = '')
                j += 1
            print('');
    i += 1
```



## Reference

https://cubelover.tistory.com/10

