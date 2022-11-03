# Algorithm

## Sorting Algorithm
### Insertion Sort
* 왼쪽이 이미 정렬 됐다는 가정하에 오른쪽의 원소들을 왼쪽의 올바른 위치에 **삽입** 하는 것.
* Worst Case(& Average Case) : O(n^2) - 역정렬일때 
* Best Case: O(n) - 이미 정렬 되어 있을 때
* 장점
	- 제자리 정렬(추가 메모리 필요 없음)
	- 안정 정렬
	- O(n^2) 중에선 빠른 편
* 단점: 느림
```
for i = 2 to len
	j = i - 1
	while j >= 1 and A[i] < A[j]
		swap(A[i], A[j])
```

### Selection Sort
* 정렬 안 된 원소 중 가장 작은 원소를 **선택** 하여 위치로 가지고 오는 것.
* 복잡도 : O(n^2)
* 장점
	- 제자리 정렬(추가 메모리 필요 없음)
	- 안정 정렬
	- O(n^2) 중에선 빠른 편
* 단점: 느림
```
for i = 1 to len
	min = A[i]
	idx = i
	for j = i + 1 to len
		if A[j] < min
			min = A[j]
			idx = j
	swap(i, idx)
```