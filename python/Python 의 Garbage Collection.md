# Python 의 Garbage Collection

## GC는 언제 사용되는가?

* 파이썬은 메모리관리에 reference counting과 garbage collection을 이용한다.

* reference counting :  참조 횟수가 0이 된 객체를 메모리에서 해제
* garbage collection : 참조 횟수가 0에 도달할 순 없지만 reference cycle(순환참조)가 일어날 때.



> 엄밀히 말하면 reference counting 도 garbage collection의 한 형태.

<br>

### 레퍼런스 카운팅

* 모든 객체는 참조 당할 때 레퍼런스 카운터를 증가시키고 참조가 없어질 때 카운터를 감소시킨다.
* 카운터가 0이 되면 객체가 메모리에서 해제된다. sys.getrefcount()로 확인 가능.

<br>

### 순환 참조

간단한 예제는 자기 자신을 참조하는 객체.

```python
l = []
l.append(l)
del l
```

ㅣ의 참조 횟수는 1이지만 이 객체는 더 이상 접근 할 수 없으며 레퍼런스 카운팅 방식으로는 메모리에서 해제될 수 없다.

<br>

또 다른 예제로는 서로를 참조하는 객체이다.

```python
a = Foo() # 0x60
b = Foo() # 0xa8
a.x = b # 0x60의 x는 0xa8를 가리킨다. 카운터2
b.x = a # 0xa8의 x는 0x60를 가리킨다. 카운터2
del a # 카운터 1
del b # 카운터 1
```

카운터는 둘 다 1이지만 둘 다 도달할 수 없는 가비지가 됐다.

<br>

### gc 모듈

> 순환 참조 해결을 위한 gc모듈을 지원.

gc.diable()을 통해 비활성화도 가능하다.

<br>

## GC의 작동 방식

* 어떤 기준으로 GC가 발생?
* 어떻게 순환 참조를 감지하는가?

<br>

#### 어떤 기준으로 GC가 발생하는가?

* generation(세대), threshold(임계값)으로 가비기 컬렉션 주기와 객체를 관리한다.
* 최근 생성된 객체는 0세대 -> 오래된 객체일수록 2세대로 간다.
* 0세대 일수록 더 자주 gc를 발생 시킨다.
* 주기는 threshold와 관련있고 gc.get_threshold()로 확인 가능하다.

```python
gc.get_threshold()
# (400, 10, 5)
# 각 0,1,2세대이며 횟수를 초과하면 gc가 수행된다.
# gc가 수행되면 0 -> 1로 옮기고 1-> 2로 옮긴다.
# 곱연산으로 객체생성 400번 -> 4000번 -> 20000번만에 각 세대gc가 일어난다.
```

- [gc모듈 코드](https://github.com/python/cpython/blob/main/Modules/gcmodule.c#L832-L836)

<br>

#### Life Cycle(어떻게 순환 참조를 발견하는가?)

* 새로운 객체가 만들어질 때 파이썬은 객체를 메모리와 0세대에 할당.

* 0세대 객체수가 threshold 0보다 크면 collect_generations() 실행
* collect_generations() 는 2세대부터 0세대 까지 객체 할당 횟수를 확인해 임계점을 넘었으면 collect()를 호출해 gc를 수행한다.
* collect()는 순환 참조 탐지 알고리즘을 수행하고 도달할 수 없는 객체집합을 찾아 콜백을 수행한 후 메모리에서 해제시킨다. 도달할 수 있는 집합은 다음세대로 보낸다.

<br>

#### 순환 참조 알고리즘.

* 순환 참조는 컨테이너 객체(e.g. `tuple`, `list`, `set`, `dict`, `class`)에 의해서만 발생할 수 있다.
* 컨테이너 객체는 다른 객체에 대한 참조를 가질 수 있다. 그러므로 정수, 문자열은 무시할 수 있다.

* 순환 참조를 해결하기 위한 아이디어로 모든 컨테이너 객체를 추적한다. 
* 여러 방법이 있겠지만 객체 내부의 링크 필드에 더블 링크드 리스트를 사용하는 방법이 가장 좋다.
*  이렇게 하면 추가적인 메모리 할당 없이도 **컨테이너 객체 집합**에서 객체를 빠르게 추가하고 제거할 수 있다. 
* 컨테이너 객체가 생성될 때 이 집합에 추가되고 제거될 때 집합에서 삭제된다.

<br>

이제 모든 컨테이너 객체에 접근할 수 있으니 순환 참조를 찾을 수 있어야 한다. 순환 참조를 찾는 과정은 다음과 같다.

1. 객체에 `gc_refs` 필드를 레퍼런스 카운트와 같게 설정한다.
2. 각 객체에서 참조하고 있는 다른 컨테이너 객체를 찾고, 참조되는 컨테이너의 `gc_refs`를 감소시킨다.
3. `gc_refs`가 0이면 그 객체는 컨테이너 집합 내부에서 자기들끼리 참조하고 있다는 뜻이다.
4. 그 객체를 unreachable 하다고 표시한 뒤 메모리에서 해제한다.



<br>

<br>

## example

실제 [collect() 코드](https://github.com/python/cpython/blob/master/Modules/gcmodule.c#L797-L981)

가비지 컬렉터가 순환 참조 객체인 `Foo(0)`과 `Foo(1)`을 해제하는 절차.

```python
a = [1]
# Set: a:[1]
b = ['a']
# Set: a:[1] <-> b:['a']
c = [a, b]
# Set: a:[1] <-> b:['a'] <-> c:[a, b]
d = c
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b]
# 컨테이너 객체가 생성되지 않았기에 레퍼런스 카운트만 늘어난다.
e = Foo(0)
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> e:Foo(0)
f = Foo(1)
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> e:Foo(0) <-> f:Foo(1)
e.x = f
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> e:Foo(0) <-> f,Foo(0).x:Foo(1)
f.x = e
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> e,Foo(1).x:Foo(0) <-> f,Foo(0).x:Foo(1)
del e
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> Foo(1).x:Foo(0) <-> f,Foo(0).x:Foo(1)
del f
# Set: a:[1] <-> b:['a'] <-> c,d:[a, b] <-> Foo(1).x:Foo(0) <-> Foo(0).x:Foo(1)
```

위 상황에서 각 컨테이너 객체의 레퍼런스 카운트는 다음과 같다.

```py
# ref count
[1]     <- a,c      = 2
['a']   <- b,c      = 2
[a, b]  <- c,d      = 2
Foo(0)  <- Foo(1).x = 1
Foo(1)  <- Foo(0).x = 1
```

1번 과정에서 각 컨테이너 객체의 `gc_refs`가 설정된다.

```py
# gc_refs
[1]    = 2
['a']  = 2
[a, b] = 2
Foo(0) = 1
Foo(1) = 1
```

2번 과정에서 컨테이너 집합을 순회하며 `gc_refs`을 감소시킨다.

```py
[1]     = 1  # [a, b]에 의해 참조당하므로 1 감소
['a']   = 1  # [a, b]에 의해 참조당하므로 1 감소
[a, b]  = 2  # 참조당하지 않으므로 그대로
Foo(0)  = 0  # Foo(1)에 의해 참조당하므로 1 감소
Foo(1)  = 0  # Foo(0)에 의해 참조당하므로 1 감소
```

3번 과정을 통해 `gc_refs`가 0인 순환 참조 객체를 발견했다. 이제 이 객체를 unreachable 집합에 옮겨주자.

```py
 unreachable |  reachable
             |    [1] = 1
 Foo(0) = 0  |  ['a'] = 1
 Foo(1) = 0  | [a, b] = 2
```

이제 `Foo(0)`와 `Foo(1)`을 메모리에서 해제하면 가비지 컬렉션 과정이 끝난다.

<br>

## 상세한 절차

* `collect()` 메서드는 현재 세대와 어린 세대를 합쳐 순환 참조를 검사한다. 
* 이 합쳐진 세대를 `young`으로 이름 붙이고 최종적으로 도달할 수 없는 객체가 모인 unreachable 리스트를 메모리에서 해제하고 young에 남아있는 객체를 다음 세대에 할당한다.

```c
update_refs(young)
subtract_refs(young)
gc_init_list(&unreachable)
move_unreachable(young, &unreachable)
```

* `update_refs()`는 모든 객체의 레퍼런스 카운트 사본을 만든다. 이는 가비지 컬렉터가 실제 레퍼런스 카운트를 건드리지 않게 하기 위함이다.

* `subtract_refs()`는 각 객체 i에 대해 i에 의해 참조되는 객체 j의 `gc_refs`를 감소시킨다. 
* 이 과정이 끝나면 (young 세대에 남아있는 객체의 레퍼런스 카운트) - (남아있는 `gc_refs`) 값이 old 세대에서 young 세대를 참조하는 수와 같다.

* `move_unreachable()` 메서드는 young 세대를 스캔하며 `gc_refs`가 0인 객체를 `unreachable` 리스트로 이동시키고 `GC_TENTATIVELY_UNREACHABLE`로 설정한다. 왜 완전히 `unreachable`이 아닌 임시로(Tentatively) 설정하냐면 나중에 스캔 될 객체로부터 도달할 수도 있기 때문이다.

* 0이 아닌 객체는 `GC_REACHABLE`로 설정하고 그 객체가 참조하고 있는 객체 또한 찾아가(traverse) `GC_REACHABLE`로 설정한다. 만약 그 객체가 `unreachable` 리스트에 있던 객체라면 `young` 리스트의 끝으로 보낸다. 굳이 `young`의 끝으로 보내는 이유는 그 객체 또한 다른 `gc_refs`가 0인 객체를 참조하고 있을 수 있기 때문이다.
* `young` 리스트의 전체 스캔이 끝나면 이제 `unreachable` 리스트에 있는 객체는 **정말 도달할 수 없다**. 이제 이 객체들을 메모리에서 해제되고 `young` 리스트의 객체들은 상위 세대로 합쳐진다.

