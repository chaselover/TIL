# 파이썬 개인적으로 기억해야 할 것 정리(210721)

​	**순서는 그냥 쓰는대로 썼음**		



# 뜬금 정렬 정리.



## 버블 정렬

> O(n^2)



```python
def bubbleSort(lst):
    LEN = len(lst)
    for last_idx in range(LEN-2, 0, -1):  # 각 턴의 마지막 인덱스
        isSwap = False  # 최적화
        for l_idx in range(0, last_idx+1):
            if lst[l_idx] > lst[l_idx+1]:
                lst[l_idx], lst[l_idx+1] = lst[l_idx+1], lst[l_idx]
                isSwap = True
        if not isSwap:  # swap이 일어나지 않았으면 정렬 완료상태
            return lst
    return lst
```

나랑 내뒤에랑 비교해서 큰게 뒤로감. 여러번 순회해서 



---



## 선택정렬

> O(n^2)



```python
def selectSort(lst):
    LEN = len(lst)
    for cur_idx in range(0, LEN-1):  # 최소값을 위치시킬 인덱스
        search_start, min_idx = cur_idx+1, cur_idx
        for search_idx in range(search_start, LEN):
            if lst[search_idx] < lst[min_idx]:
                min_idx = search_idx
        lst[cur_idx], lst[min_idx] = lst[min_idx], lst[cur_idx]
    return lst
```



나로부터 뒤로가며 가장 작은 것 찾고 둘이 자리바꾸는 정렬 내가 i로 가고 비교군 j로 가서 n^2.



---



## 삽입정렬

> O(n^2)



```python
def insertSort(lst):
    LEN = len(lst)
    for search_start in range(1, LEN):  # 탐색 시작 인덱스
        while search_start > 0:
            if lst[search_start] < lst[search_start-1]:
                lst[search_start], lst[search_start - 1] \
                    = lst[search_start-1], lst[search_start]
                search_start -= 1
            else:
                break
    return lst
```

순서대로 하나씩 빼서 내 앞에있는 애들이랑만 비교함.쭉 가다가 나보다 작은애 만나면 그 뒤에 삽입.



---



## 퀵정렬

> O(nlogn)

분할정복써서 좀 더 빠르고 내장함수sort도 이거임. 최악은 n^2이긴 함.

```python
def quickSort(lst):
    LEN = len(lst)
    if LEN <= 1:
        return lst
    middle = len(lst) // 2  # 최악의 케이스를 막기 위해  pivot을 middle로 설정
    pivot = lst[middle]
    rest = lst[:middle] + lst[middle+1:]
    left = [num for num in rest if num <= pivot]
    right = [num for num in rest if num > pivot]
    return quickSort(left) + [pivot] + quickSort(right)
```

피봇을 기준 좌우 분할하고 작은거 전부 왼쪽으로 큰거 전부 오른쪽으로 보낸다음 다시 솔트



## 병합 정렬

> O(nlogn)

요새 관심갖는 분할정복의 대표적인 예시.

핵심은 분할 분할 분할 최소단위 정복 병합 병합 병합으로 원문제 해결.

```python
def merge(left, right):
    merged = []
    while left or right:
        # left나 right 가 비었을 때
        if not left:
            merged += right
            return merged
        elif not right:
            merged += left
            return merged
        # left나 right 첫번째 원소값 비교
        if left[0] < right[0]:
            merged += [left[0]]
            left = left[1:]
        else:
            merged += [right[0]]
            right = right[1:]
    return merged


def mergeSort(lst):
    LEN = len(lst)
    if LEN <= 1:
        return lst
    middle = len(lst) // 2
    left = mergeSort(lst[:middle])
    right = mergeSort(lst[middle:])
    return merge(left, right)
```



---



