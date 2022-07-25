# LRU cache O(1)

* 캐시는 불필요한 통신을 줄이고 기민한 UX를 제공
* DB과부하를 막음.(query결과 저장)
* Proxy, CDN(유저와 가까운 CDN node)
* 메모이제이션

## OrderedDict이용

```python
from collections import OrderedDict

MAX_SIZE = 2
cache = OrderedDict()

def get_user(user_id):
    if user_id in cache:
        cache.move_to_end(user_id) 
        return cache[user_id]

    if len(cache) == MAX_SIZE:
        cache.popitem(last=False) 

    cache[user_id] = fetch_user(user_id)
    return cache[user_id]
```

> last=False는 맨앞.

```python
from collections import OrderedDict
 
class LRUCache:
 
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity
 

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        else:
            self.cache.move_to_end(key)
            return self.cache[key]
 

    def put(self, key: int, value: int) -> None:
        self.cache[key] = value
        self.cache.move_to_end(key)
        if len(self.cache) > self.capacity:
            self.cache.popitem(last = False)
 
cache = LRUCache(2)
```

### move_to_end 

```python
def move_to_end(self, key, last=True):
    '''Move an existing element to the end (or beginning if last is false).
    Raise KeyError if the element does not exist.
    '''
    link = self.__map[key]
    link_prev = link.prev
    link_next = link.next
    soft_link = link_next.prev
    link_prev.next = link_next
    link_next.prev = link_prev
    root = self.__root
    if last:
        last = root.prev
        link.prev = last
        link.next = root
        root.prev = soft_link
        last.next = link
    else:
        first = root.next
        link.prev = root
        link.next = first
        first.prev = soft_link
        root.next = link
```

## functools 모듈 cache 데코레이터(3.9추가)

```python
from functools import cache

def fetch_user(user_id):
    print(f"DB에서 아이디가 {user_id}인 사용자 정보를 읽어오고 있습니다...")
    return {
        "userId": user_id,
        "email": f"{user_id}@test.com",
        "password": "test1234"
    }

@cache
def get_user(user_id):
    return fetch_user(user_id)

if __name__ == "__main__":
    from random import choice

    for i in range(10):
        get_user(user_id = choice(["A01", "B02", "C03"]))
        
10번 호출해도 3번만 갔다옴.
```



### 동일모듈 lru_cache 데코레이터

```python
from functools import lru_cache

def fetch_user(user_id):
    print(f"DB에서 아이디가 {user_id}인 사용자 정보를 읽어오고 있습니다...")
    return {
        "userId": user_id,
        "email": f"{user_id}@test.com",
        "password": "test1234"
    }

@lru_cache
def get_user(user_id):
    return fetch_user(user_id)

if __name__ == "__main__":
    from random import choice

    for i in range(10):
        get_user(user_id = choice(["A01", "B02", "C03"]))
    print(get_user.cache_info())
   
```

#### 출력 결과

```python
DB에서 아이디가 B02인 사용자 정보를 읽어오고 있습니다...
DB에서 아이디가 A01인 사용자 정보를 읽어오고 있습니다...
DB에서 아이디가 C03인 사용자 정보를 읽어오고 있습니다...
CacheInfo(hits=7, misses=3, maxsize=128, currsize=3)
```

```python
@lru_cache(maxsize=2)

이런식으로 캐시 크기 제어 가능
```

## OrderedDict x

```python
class Node(object):
    __slots__ = ['prev', 'next', 'me']
    def __init__(self, prev, me):
        self.prev = prev
        self.me = me
        self.next = None

class LRU:
    """
    Implementation of a length-limited O(1) LRU queue.
    Built for and used by PyPE:
    http://pype.sourceforge.net
    Copyright 2003 Josiah Carlson.
    """
    def __init__(self, count, pairs=[]):
        self.count = max(count, 1)
        self.d = {}
        self.first = None
        self.last = None
        for key, value in pairs:
            self[key] = value
    def __contains__(self, obj):
        return obj in self.d
    def __getitem__(self, obj):
        a = self.d[obj].me
        self[a[0]] = a[1]
        return a[1]
    def __setitem__(self, obj, val):
        if obj in self.d:
            del self[obj]
        nobj = Node(self.last, (obj, val))
        if self.first is None:
            self.first = nobj
        if self.last:
            self.last.next = nobj
        self.last = nobj
        self.d[obj] = nobj
        if len(self.d) > self.count:
            if self.first == self.last:
                self.first = None
                self.last = None
                return
            a = self.first
            a.next.prev = None
            self.first = a.next
            a.next = None
            del self.d[a.me[0]]
            del a
    def __delitem__(self, obj):
        nobj = self.d[obj]
        if nobj.prev:
            nobj.prev.next = nobj.next
        else:
            self.first = nobj.next
        if nobj.next:
            nobj.next.prev = nobj.prev
        else:
            self.last = nobj.prev
        del self.d[obj]
    def __iter__(self):
        cur = self.first
        while cur != None:
            cur2 = cur.next
            yield cur.me[1]
            cur = cur2
    def iteritems(self):
        cur = self.first
        while cur != None:
            cur2 = cur.next
            yield cur.me
            cur = cur2
    def iterkeys(self):
        return iter(self.d)
    def itervalues(self):
        for i,j in self.iteritems():
            yield j
    def keys(self):
        return self.d.keys()
```

## java

```java
public class LRUCacheImpl {

	private class ListNode {
		private int key;
		private int val;
		private ListNode prev;
		private ListNode next;

		public ListNode(int key, int val) {
			this.key = key;
			this.val = val;
			this.prev = null;
			this.next = null;
		}
	}

	private Map<Integer, ListNode> nodeMap;
	private int capacity;
	private ListNode head;
	private ListNode tail;

	public LRUCacheImpl(int capacity) {
		this.nodeMap = new HashMap<>();
		this.capacity = capacity;
		head = new ListNode(0, 0);
		tail = new ListNode(0, 0);
		head.next = tail;
		tail.prev = head;
	}

	private void remove(ListNode node) {
		node.prev.next = node.next;
		node.next.prev = node.prev;
		nodeMap.remove(node.key);
	}

	private void insertToHead(ListNode node) {
		this.head.next.prev = node;
		node.next = this.head.next;
		node.prev = this.head;
		this.head.next = node;
		nodeMap.put(node.key, node);
	}

	public int get(int key) {
		if (!nodeMap.containsKey(key)) {
			return -1;
		}
		ListNode node = nodeMap.get(key);
		remove(node);
		insertToHead(node);
		return node.val;
	}

	public void put(int key, int value) {
		ListNode newNode = new ListNode(key, value);
		if (nodeMap.containsKey(key)) {
			ListNode oldNode = nodeMap.get(key);
			remove(oldNode);
		} else {
			if (nodeMap.size() >= capacity) {
				ListNode tailNode = tail.prev;
				remove(tailNode);
			}
		}
		insertToHead(newNode);
	}
}
```

## OrderedDick

```python
# Located in Lib/collections/__init__.py

################################################################################
### OrderedDict
################################################################################

class _OrderedDictKeysView(KeysView):

    def __reversed__(self):
        yield from reversed(self._mapping)

class _OrderedDictItemsView(ItemsView):

    def __reversed__(self):
        for key in reversed(self._mapping):
            yield (key, self._mapping[key])

class _OrderedDictValuesView(ValuesView):

    def __reversed__(self):
        for key in reversed(self._mapping):
            yield self._mapping[key]

class _Link(object):
    __slots__ = 'prev', 'next', 'key', '__weakref__'

class OrderedDict(dict):
    'Dictionary that remembers insertion order'
    # An inherited dict maps keys to values.
    # The inherited dict provides __getitem__, __len__, __contains__, and get.
    # The remaining methods are order-aware.
    # Big-O running times for all methods are the same as regular dictionaries.

    # The internal self.__map dict maps keys to links in a doubly linked list.
    # The circular doubly linked list starts and ends with a sentinel element.
    # The sentinel element never gets deleted (this simplifies the algorithm).
    # The sentinel is in self.__hardroot with a weakref proxy in self.__root.
    # The prev links are weakref proxies (to prevent circular references).
    # Individual links are kept alive by the hard reference in self.__map.
    # Those hard references disappear when a key is deleted from an OrderedDict.

    def __init__(*args, **kwds):
        '''Initialize an ordered dictionary.  The signature is the same as
        regular dictionaries, but keyword arguments are not recommended because
        their insertion order is arbitrary.
        '''
        if not args:
            raise TypeError("descriptor '__init__' of 'OrderedDict' object "
                            "needs an argument")
        self, *args = args
        if len(args) > 1:
            raise TypeError('expected at most 1 arguments, got %d' % len(args))
        try:
            self.__root
        except AttributeError:
            self.__hardroot = _Link()
            self.__root = root = _proxy(self.__hardroot)
            root.prev = root.next = root
            self.__map = {}
        self.__update(*args, **kwds)
        
    def __setitem__(self, key, value,
                    dict_setitem=dict.__setitem__, proxy=_proxy, Link=_Link):
        'od.__setitem__(i, y) <==> od[i]=y'
        # Setting a new item creates a new link at the end of the linked list,
        # and the inherited dictionary is updated with the new key/value pair.
        if key not in self:
            self.__map[key] = link = Link()
            root = self.__root
            last = root.prev
            link.prev, link.next, link.key = last, root, key
            last.next = link
            root.prev = proxy(link)
        dict_setitem(self, key, value)

    def __delitem__(self, key, dict_delitem=dict.__delitem__):
        'od.__delitem__(y) <==> del od[y]'
        # Deleting an existing item uses self.__map to find the link which gets
        # removed by updating the links in the predecessor and successor nodes.
        dict_delitem(self, key)
        link = self.__map.pop(key)
        link_prev = link.prev
        link_next = link.next
        link_prev.next = link_next
        link_next.prev = link_prev
        link.prev = None
        link.next = None

    def __iter__(self):
        'od.__iter__() <==> iter(od)'
        # Traverse the linked list in order.
        root = self.__root
        curr = root.next
        while curr is not root:
            yield curr.key
            curr = curr.next

    def __reversed__(self):
        'od.__reversed__() <==> reversed(od)'
        # Traverse the linked list in reverse order.
        root = self.__root
        curr = root.prev
        while curr is not root:
            yield curr.key
            curr = curr.prev

    def clear(self):
        'od.clear() -> None.  Remove all items from od.'
        root = self.__root
        root.prev = root.next = root
        self.__map.clear()
        dict.clear(self)

    def popitem(self, last=True):
        '''od.popitem() -> (k, v), return and remove a (key, value) pair.
        Pairs are returned in LIFO order if last is true or FIFO order if false.
        '''
        if not self:
            raise KeyError('dictionary is empty')
        root = self.__root
        if last:
            link = root.prev
            link_prev = link.prev
            link_prev.next = root
            root.prev = link_prev
        else:
            link = root.next
            link_next = link.next
            root.next = link_next
            link_next.prev = root
        key = link.key
        del self.__map[key]
        value = dict.pop(self, key)
        return key, value

    def move_to_end(self, key, last=True):
        '''Move an existing element to the end (or beginning if last==False).
        Raises KeyError if the element does not exist.
        When last=True, acts like a fast version of self[key]=self.pop(key).
        '''
        link = self.__map[key]
        link_prev = link.prev
        link_next = link.next
        link_prev.next = link_next
        link_next.prev = link_prev
        root = self.__root
        if last:
            last = root.prev
            link.prev = last
            link.next = root
            last.next = root.prev = link
        else:
            first = root.next
            link.prev = root
            link.next = first
            root.next = first.prev = link

    def __sizeof__(self):
        sizeof = _sys.getsizeof
        n = len(self) + 1                       # number of links including root
        size = sizeof(self.__dict__)            # instance dictionary
        size += sizeof(self.__map) * 2          # internal dict and inherited dict
        size += sizeof(self.__hardroot) * n     # link objects
        size += sizeof(self.__root) * n         # proxy objects
        return size

    update = __update = MutableMapping.update

    def keys(self):
        "D.keys() -> a set-like object providing a view on D's keys"
        return _OrderedDictKeysView(self)

    def items(self):
        "D.items() -> a set-like object providing a view on D's items"
        return _OrderedDictItemsView(self)

    def values(self):
        "D.values() -> an object providing a view on D's values"
        return _OrderedDictValuesView(self)

    __ne__ = MutableMapping.__ne__

    __marker = object()

    def pop(self, key, default=__marker):
        '''od.pop(k[,d]) -> v, remove specified key and return the corresponding
        value.  If key is not found, d is returned if given, otherwise KeyError
        is raised.
        '''
        if key in self:
            result = self[key]
            del self[key]
            return result
        if default is self.__marker:
            raise KeyError(key)
        return default

    def setdefault(self, key, default=None):
        'od.setdefault(k[,d]) -> od.get(k,d), also set od[k]=d if k not in od'
        if key in self:
            return self[key]
        self[key] = default
        return default

    @_recursive_repr()
    def __repr__(self):
        'od.__repr__() <==> repr(od)'
        if not self:
            return '%s()' % (self.__class__.__name__,)
        return '%s(%r)' % (self.__class__.__name__, list(self.items()))

    def __reduce__(self):
        'Return state information for pickling'
        inst_dict = vars(self).copy()
        for k in vars(OrderedDict()):
            inst_dict.pop(k, None)
        return self.__class__, (), inst_dict or None, None, iter(self.items())

    def copy(self):
        'od.copy() -> a shallow copy of od'
        return self.__class__(self)

    @classmethod
    def fromkeys(cls, iterable, value=None):
        '''OD.fromkeys(S[, v]) -> New ordered dictionary with keys from S.
        If not specified, the value defaults to None.
        '''
        self = cls()
        for key in iterable:
            self[key] = value
        return self
        
    def __eq__(self, other):
        '''od.__eq__(y) <==> od==y.  Comparison to another OD is order-sensitive
        while comparison to a regular mapping is order-insensitive.
        '''
        if isinstance(other, OrderedDict):
            return dict.__eq__(self, other) and all(map(_eq, self, other))
        return dict.__eq__(self, other)
```



## Reference

[collections — Container datatypes — Python 3.10.5 documentation](https://docs.python.org/3/library/collections.html#collections.OrderedDict)

[Taking input in Python - GeeksforGeeks](https://www.geeksforgeeks.org/taking-input-in-python/?ref=lbp)

[파이썬 - LRU 캐시 - GeeksforGeeks](https://www.geeksforgeeks.org/python-lru-cache/?ref=lbp)