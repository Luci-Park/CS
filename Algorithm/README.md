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
* 이웃들을 먼저 탐색하고 자식들을 탐색하는 알고리즘
* 큐를 사용해서 구현
```
Queue Q;
root.visited = true;
add root to Q
while Q is not empty
	pop Q as node
	for each neighbour of node
		if neighbour is not visited
			neighbour.visited = true;
			add neighbour to Q
```
### DFS
* 자식들을 먼저 탐색하는 알고리즘
* 재귀 함수 or 스택 사용해서 구현
```
Stack S;
root.visited = true;
add root to S
while Q is not empty
	pop S as node
	for each neighbour of node
		if neighbour is not visited
			neighbour.visited = true;
			add neighbour to S
```

```
DFS(root )
	if root == null return
	root.visited = true;
	for each neighbour of node
		if neighbour is not visited
			DFS(neighbour);
```
## Minimal Spanning Tree(최소 신장 트리)
Minimal Spanning Tree는 모든 정점이 연결되어 있고 사이클이 없는 그래프 간선의 합이 최소가 되는 트리의 형태.
네트워크 구축할 때 사용.

### Kruskal 알고리즘

* 사이클이 생기지 않는 간선들 중에서 가장 가중치가 작은 간선부터 고르는 방법
* Union - Find 알고리즘을 기본으로 함.

*Union-Find*
각 노드의 parent를 기록함으로써 루트가 같으면 같은 cluster으로 판단 하는 것.
```
bool IsUnion(int a, int b){
	
}
```


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
* 모든 간선을 봐야 되는 다익스트라에 비해 실용성이 더 높다.
* 시작과 끝지점이 멀면 priority queue에 들어가는 원소가 많아지므로 느려짐
* 시간 복잡도: O(E)
* 공간 복잡도: O(V)

__heuristic function__
어떤 문제에 대해 답을 구할 수 없거나 구하기에 너무 느릴 때 대안책으로 쓰이는 함수.
최단 경로를 과대평가하지 않는 것을 admissible 이라고 한다.

*Manhattan Function*
(x1, y1), (x2, y2)간의 거리는 |x1 - x2| + |y1 - y2|
휴리스틱 함수로 쓰일 땐 h(n)=Σd(t,p)
- d = 두 위치 사이의 거리
- t = 어떤 숫자의 현재 위치
- p = 그 노드가 원래 있어야 할 위치
Manhattan Function은 admissible 하다.

#### 알고리즘
[링크](https://www.youtube.com/watch?v=-L-WgKMFuhE)
1. 시작 노드는 주변의 8노드에 대해 탐색한다. 이때 대각선의 거리는 루트2 이며 보통 10을 곱해서 생각한다.
2. 각 neighbore에 대해 cost를 계산
	- gcost = 시작노드와의 거리
	- hcost(heurisic) = 목표 노드와의 거리
	- fcost = gcost + hcost
	- 이미 탐색(방문 x) 했던 이웃노드는 fcost가 더 작으면 갱신
3. fcost가 가장 짧은 것부터 고른다.
	- fcost가 같으면 gcost가 짧은 것부터 고른다.
	- 둘다 같으면 아무거나 고른다.

_Pseudo Code_
``` 
OPEN //the set of nodes to be evaluated
CLOSED // the set of nodes already evaluated
loop
	current = node in OPEN with lowest fcost
	remove current from OPEN
	add current to CLOSED

	if current is the target node //target found
		return
	
	foreach neighbour of the current node
		if neighbour is not traversable or neighbour is in CLOSED
			skip to the next neighbour
		if new path to neighbour is shorter OR neighbour is not in OPEN
			set fcost of neighbour
			set parent of neighbour to current
			if neighbour is not in OPEN
				add neighbour to OPEN
```

### Bellman Ford

* 음의 사이클이 없는 그래프에서 1:N 최단거리 알고리즘
	- 음의 self loop
	- 정점의 cycle의 합이 음수일때
* 주식 시스템에서 사용.
* 시간 복잡도: O(V^3)
```

```
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