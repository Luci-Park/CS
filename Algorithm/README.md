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

### Bubble Sort
* 두 개씩 비교해서 순서가 맞지 않으면 두 위치를 swap
* 복잡도: O(n^2)
* 장점
 - 제자리 정렬
 - 구현이 쉽다
* 단점
 - 무조건 O(n^2)의 느림
 - 원소의 개수와 비교횟수가 비례해서 늘어남.
```
for i = 1 to len - 1
	for j = 1 to len - i - 1
		if A[j] < A[j + 1]
			swap(A[j], A[j + 1])
```

### Merge Sort
* 배열을 반씩 나눠서 순서대로 merge 하는 것
* merge는 배열의 앞끼리 비교해서 작은 것 부터 배치 함
* 장점
	- 빠름
* 단점
	- 제자리 정렬이 아님
	- 추가 공간 필요
* 시간 복잡도: O(nlogn)
* 공간 복잡도: O(n)

```
MergeSort(A)
	if A.len == 1
		return A
	A1 = MergeSort(A[1]~A[len/2])
	A2 = MergeSort(A[len/2 + 1]~A[len])
	return Merge(A1, A2)

Merge(A1, A2)
	RSLT
	while A1 and A2 is not empty
		if A1[0] < A2[0]
			RSLT.push(A1[0])
			A1.pop
		else
			RSLT.push(A2[0])
			A2.pop
	while A1 is not empty
		RSLT.Add(A1)
	while A2 is not empty
		RSLT.Add(A2)
	return RSLT
```

### Quick Sort
* 기준점을 가지고 기준점보다 작은 것은 왼쪽으로, 큰것은 오른쪽으로 두어서 정렬
* 장점
	- 불필요한 데이터의 이동을 줄임
	- 한번 결정된 pivot은 다시 건들이지 않아 nlogn 중 가장 빠르다
	- 추가 메모리 필요 없다
* 단점
	- 불안정(제자리 정렬이 아님)
* 평균 복잡도: O(nlogn)
* 최악 복잡도: O(n^2) - 정렬되어 있는 배열
```
QuickSort(A, lo, hi)
	if lo < hi
		pivot = Partition(A, lo, hi)
		Quicksort(A, lo, pivot - 1)
		Quicksort(A, pivot + 1, hi)

Partition(A, lo, hi)
	idx = random(lo, hi) // get a random pivot
	swap(A[idx], A[hi]) // place the pivot at the end of the array
	pivot = A[hi]
	idx = hi
	while lo <= hi // 양쪽에서 시작
		while A[lo] <= pivot // partion에 맞지 않은 lo 와 hi 검색
			lo += 1
		while A[hi] >= pivot
			hi -= 1
		else
			swap(A[lo], A[hi]) // 두 자리를 swap
			lo += 1
			high -= 1
	swap(A[lo], A[idx]) // lo 위치에 pivot을 place
	return lo
```

### Heap Sort
* max Heap으로 배열을 변경하여 가장 큰 점부터 뒤로 보내어 오름차순 정렬을 하는 방식
* 장점
	- 퀵 정렬과 달리 어느 케이스간에 NlogN이 보장된다.
* 단점
	- 이상적인 경우의 퀵정렬보단 빠르다.
	- 불안정 정렬이다.
* 시간 복잡도: O(nlogn)
```
HeapSort(A)
	BuildMaxHeap(A)
	n = A.len
	for i = n to 1
		swap(A[1], A[i]) // 가장 큰 원소를 뒤로 보낸다
		n -= 1 // sort된 노드를 제외하고 pop 대신의 온 노드에 대해 heapify
		MaxHeapify(A, n, 1)

BuildMaxHeap(A)
	size = A.len
	for i = size/2 to 1 // leaf는 이미 힙, heapify할 필요 없음.
		MaxHeapify(A, size, i)

MaxHeapify(A, size, i) // i 번째 노드에 대해 heapify 진행
	l = i * 2 // i 의 left child
	largest = i
	// i의 left와 right child 중 i 보다 큰 child를 골라냄
	if(l < size and A[l] > a[i])
		largest = l
	if(r < size and A[r] > a[largest])
		largest = r
	if(largest != i) // 만약 i 보다 큰 child가 있으면 자리 바꾸기
		swap(A[i], A[largest])
		MaxHeapify(A, size, largest) // swap이 일어났다면 노드에 대해 heapify 연산 수행
```

## 그래프 알고리즘

### 그래프 탐색

#### BFS
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
#### DFS
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
### Minimal Spanning Tree(최소 신장 트리)
Minimal Spanning Tree는 모든 정점이 연결되어 있고 사이클이 없는 그래프 간선의 합이 최소가 되는 트리의 형태.
네트워크 구축할 때 사용.

#### Kruskal 알고리즘

* 사이클이 생기지 않는 간선들 중에서 가장 가중치가 작은 간선부터 고르는 방법
* Union - Find 알고리즘을 기본으로 함.

*Union-Find*
각 노드의 parent를 기록함으로써 루트가 같으면 같은 cluster으로 판단 하는 것.

```
GetRoot(parent, int x)
	if parent[x] is x
		return x
	else
		return GetRoot(parent, root[x])

IsUnion(int a, int b)
	rootA = GetRoot(parent, a)
	rootB = GetRoot(parent, b)
	return rootA is rootB	

Kruskal(Graph)
	sort(Graph.edges)// 간선을 비용별로 오름차순 정렬
	for i = 1 to Graph.Nodes.Length
		parent[i] = i // 처음엔 모든 정점의 root이 본인
	for each edge in Graph.edges
		if not IsUnion(edge.a, edge.b) // 간선의 두 노드가 같은 cluster인가
			select edge
			if GetRoot(edge.a) < GetRoot(edge.b) // 더 작은 root의 노드가 parent이 된다.
				parent[b] = a
			else
				parent[a] = b
```
#### Prim 알고리즘

* 임의로 정점을 하나 선택해서 연결 간선 중 가장 작은 것을 선택한다.
* 해당 간선이 visited 배열 안에 포합되어 있다면 선택하지 않는다.

```
Prim(Graph, Weight, root)
	for each u in Graph.vertices
		u.key = INF
		u.parent = NIL
	root.key = 0
	PQ = Graph.vertices
	while PQ is not empty
		u = PQ.front
		for each v in Graph.adj[u]
			if v in PQ and w(u, v) < v.key
				v.parent = u
				v.key = w(u, v)
```

### 그래프 지름
그래프 지름이란 그래프 내에서 가장 긴 경로를 말한다.
이를 구하는 방법은 
1) 임의의 점에서 가장 멀리 떨어진 노드를 찾고
2) 1에서 찾은 노드에서 가장 멀리 떨어진 노드를 찾으면
그 길이가 지름이다.


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
	- 주식 시스템에서 사용.
* V - 1 번 검증하면 최단 거리가 나올것이라는 전제
* V - 1 반복해서 각 정점에서 간선을 통해 정점으로 가는 것이 더 빠르면 업데이트
* 변화가 없으면 Stop
* 시간 복잡도: O(VE)
```
BellmanFord(Graph, weight, source)
	for all u in Graph.vertices
		u.dist = INF
		u.prev = nill
	
	source.dist = 0
	repeat Graph.vertices.length - 1 times
		for all u in Graph.vertices
			for all v in u.adj
				if u.dist + Graph.edge(u, v) < v.dist
					v.dist = u.dist + Graph.edge(u, v)
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

## Dynamic Programming
Subproblems lead to total solution

 ### Longest Common Subsequence
 두 단어에 대해 글자들의 상대적인 위치를 바꾸지 않고 만들 수 있는 공통적 인 글자의 수열을 지칭한다.

because the orders won't change, the sub problem would be the longest subsquence when words have 1 characters, 2 characters etc.
when a word size increases, and the increased word has same word, the subsequence has gotten longer.

 ```cpp
int n = s1.length(), m = s2.length();
for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= m; ++j) {
        if (s1[i - 1] == s2[j - 1])
            dp[i][j] = dp[i - 1][j - 1] + 1;
        else
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
    }
}
return dp[n][m];
 ```

 ### Knapsack Problem
 the Knapsack problem, when with div possible, we use greedy. But the knapsack problem is usually 1/0, we use dp.

 the sub problem would be when there's only one item.
 when there are two items, we can choose either when the prv item is included or not.

 In brute Force, 
 ```cpp
 // Returns the maximum value that
// can be put in a knapsack of capacity W
int knapSack(int W, int wt[], int val[], int n)
{
    // Base Case
    if (n == 0 || W == 0)
        return 0;
 
    // If weight of the nth item is more
    // than Knapsack capacity W, then
    // this item cannot be included
    // in the optimal solution
    if (wt[n - 1] > W)
        return knapSack(W, wt, val, n - 1);
 
    // Return the maximum of two cases:
    // (1) nth item included
    // (2) not included
    else
        return max(
            val[n - 1]
                + knapSack(W - wt[n - 1],
                           wt, val, n - 1),
            knapSack(W, wt, val, n - 1));
}
 ```

Dp로 옮길 때 아이템의 개수와 weight가 각각 행과 열이 된다.
k[i][w], 즉 아이템 i 개와 weight w일 때 i 번째 아이템을 넣었을 때 무게를 추가한 것과 뺀 것 중 가치가 높은 것을 고른다.

```cpp
    int i, w;
    vector<vector<int>> K(n + 1, vector<int>(W + 1));
 
    // Build table K[][] in bottom up manner
    for(i = 0; i <= n; i++)
    {
        for(w = 0; w <= W; w++)
        {
            if (i == 0 || w == 0)
                K[i][w] = 0;
            else if (wt[i - 1] <= w)
                K[i][w] = max(val[i - 1] +
                                K[i - 1][w - wt[i - 1]],
                                K[i - 1][w]);
            else
                K[i][w] = K[i - 1][w];
        }
    }
    return K[n][W];
```
필요 공간을 줄이는 방법이 있다.
배낭 문제는 아이템에 대해 무게를 계산하며 계산된 것은 그 다음 아이템에 대해서만 쓰인다.
그래서 무게에 대해서만 array를 만들고 아이템 무게 계산은 그 전에 계산한 것을 덮어씌우는 방식이 존재한다.
```cpp
vector<int> dp(W + 1, 0);
for(int i = 1; i < n + 1; ++i){
	for(int w = W; w >= 0; w--){
		if(wt[i - 1] <= w>){
			dp[w] = max(dp[w], dp[w - wt[i - 1]] + val[i - 1]);
		}
	}
	return dp[W];
}
```
Weight가 float, double 등이라면 정수가 될 때까지 곱해서 정수일때의 문제와 동일하게 대하면 된다.

## Math

### 최대공약수, 최소공배수
__유클리드 호제법__
두 양의 정수에 대해 a = bq + r이면 a,b 의 최대공약수는 b,r의 최대공약수와 같다.
최대공약수는 a와 b의 곱을 최소공배수로 나눠주는 것과 동일하다.
```cpp
int gcd(int a, int b){
	int r = a% b
	return r == 0 ? b, gcd(b, r);
}

int lcm(int a, int b){
	return a * b / gcd(a, b);
}
```
### 순열, 조합
순열은 recursion에서 각 위치(depth)에서 모든 원소를 하나씩 넣어보는 것과 같다.
조합은 한 번호를 선택했을 때 나오는 모든 조합들의 합을 가진 재귀의 일종이다.

``` cpp
//nPr
void Permutation(int depth){
	if(depth == r) print(arr);
	else{
		for(int i = 0; i < n; ++i){
			if(visited[i]) continue;
			visited[i] = true;
			arr[depth] = i;
			Permutation(depth + 1);
			visited[i] = false;
		}
	}
}

void DuplicatePermutation(int depth){
	if(depth == r) print(arr);
	else{
		for(int i = 0; i < n; ++i){
			arr[depth] = i;
			DuplicatePermutation(depth + 1);
		}
	}
}

//nCr
void Combination(int depth, int next){
	if(depth == r) print(arr);
	else{
		for(int i = next; i < n; ++i){
			arr[depth] = i;
			Combination(depth + 1, i + 1);
		}
	}
}
void DuplicateCombination(int depth, int next){
	if(depth == r) print(arr);
	else{
		for(int i = next; i < n; ++i){
			arr[depth] = i;
			DuplicateCombination(depth + 1, i);
		}
	}
}
```
#### 조합의 성질
* nCm = n!/((n - m)! * m!)
* nCm = n-1Cm-1 + n-Cm => dp로 활용 가능