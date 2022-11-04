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

## 그래프 탐색 알고리즘

### BFS

### DFS

## Minimal Spanning Tree

## 최단 거리 알고리즘

### BFS

* 가중치가 없는 그래프에서의 최단 거리 알고리즘
* 시간 복잡도(인접 행렬) : O(V^2)
* 시간 복잡도(인접 리스트) : O(V + E)

### Dijkstra

* 알고리즘
	1. 모든 노드의 거리가 최댓값이고 방문이 안된채로 시작한다.
	2. 시작 노드의 거리를 0으로 둔다.
	3. 현재 노드의 이웃들에 대해 기존 거리와 연결 겨리 중 짧은 것으로 갱신한다.
	4. 현재 노드는 방문 처리가 되고 다시 선택되지 않는다.
	5. 이웃들 중 거리가 가장 짧은 노드를 골라 3 ~ 5를 반복한다.

* *음의 가중치가 없는 그래프에서의* 1:N 최단 거리 알고리즘
* 목표 노드를 찾으면 멈추는 방식으로 1:1도 가능 
* 이웃들 중 가장 짧은 노드를 골라야 하기 때문에 priority queue(heap)을 사용하는 것이 유리하다.
* greedy algorithm
* 시간 복잡도(인접 행렬) : O(v^2)
* 시간 복잡도(인접 리스트) : O((V + E) log V)
```
for each vertex v in Graph.Vertices
	dist[v] = INFINITY
	add v to Q
dist[source] = 0

while Q is not empty
	u = Q.pop

	for each neighbor v of u in Q
		d = dist[u] + Graph.Edges(u, v)
		if d < dist[v]
			dist[v] = d
```

### A*(A 스타)

* 1:1 최단 거리 알고리즘
* 다익스트라와 비슷하나 다음 노드를 고를 때 휴리스틱 함수를 따름
* 시작과 끝지점이 멀면 priority queue에 들어가는 원소가 많아지므로 느려짐
* 시간 복잡도: O(E)
* 공간 복잡도: O(V)

### Bellman Ford

* 음의 사이클이 없는 그래프에서 1:N 최단거리 알고리즘

### Floyd-Warshall

* 음의 사이클이 없는 그래프에서의 N:N 최단거리 알고리즘
* i -> j 로 갈 때 직접 가는 것과 다른 노드 k를 거쳐갈 때 경우 중 빠른 경로를 선택한다.
* Dynamic Programming Algorithm
* 시간 복잡도: O(V^3)
* 공간 복잡도: O(V^2)
```
dist[N][N] // dist[y][x] -> y -> x 까지 경로
for k = 1 to len // 거쳐가는 노드
	for i = 1 to len // 출발 하는 노드
		for j = 1 to len //도착 하는 노드
			dist[i][j] = min(dist[i][k] + dist[k][j], dist[i][j])
```