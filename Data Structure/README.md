# Data Structure

## 선형 구조(linear)

### Array
* 논리적 저장 순서와 물리적 저장 순서가 일치
* 인덱스로 해당 원소 접근 가능
* Search: O(1) = random access가능
* Insert: O(n) (worst case) 
* Delete: O(n) (worst case)

### Linked List
* 각 원소들이 자기 자신 다음 원소를 기억하고 있음
* Insert나 Delete은 O(1)가 가능하나 Search는 O(n).
* Insert와 Delete에는 Search가 거의 필수적으로 필요하기 때문에 O(n)이나 다름없다.
* Tree 구조의 기본이 된다.

### Stack
* LIFO(Last In First Out)
* Push, Pop

### Queue
* FIFO(First In First Out)
* Enqueue, Dequeue

## 비선형

### Tree
* Parent와 Child가 연결, Child가 Ancestor과 연결되어 있거나 하는 Cycle를 허용하지 않는다.
* Node(노드)
* Edge(간선)
* Root Node(루트)
* Terminal Node(= Leaf Node, 단말 노드)
* Internal Node(내부 노드, 비단말 노드)

#### Binary Tree(이진 트리)
* 모든 root은 두 개의 subree를 가진다(이 때 공집합도 이진 트리이다)
* 배열로 구성되었고, root이 1에서 시작하며, 노드의 개수가 n개 일 때:
    + Left(i) = 2 * i
    + Right(i) = 2 * i + 1
    + Parent(i) = lower_bound(i / 2)
    + Height = lower_bound(n / 2)
* Full Binary Tree(정 이진 트리): 모든 비단말 노드가 2개 자식을 가지고 있는 이진 트리
* Perfect Binary Tree(포화 이진 트리): 모든 레벨이 꽉 찬 이진 트리
* Complete Binary Tree(완전 이진 트리): 마지막 level을 제외한 모든 level에 노드가 왼쪽에서부터 차례로 다 차 있는 이진 트리

#### Heap
* Complete Binary Tree
* 배열 형식을 주로 사용.
* Max Heap: 모든 노드는 parent >= child를 만족한다.
* Min Heap: 모든 노드는 parent <= child를 만족한다.
* Search: Heap은 최댓값/최솟값만을 access하기에 적합하다. O(1)
* Insert: O(log n)
    1. 가장 마지막에 삽입한다.
    2. parent와의 관계가 맞지 않다면 parent와 위치 교환.
    3. 2번 반복
* Delete: O(log n)
    1. Root 삭제
    2. 마지막 leaf node를 root에 배치
    3. root가 조건에 맞을 때 까지 child와 교환.

#### Binary Search Tree
* 쉽게 찾기 위해 정렬한 트리
* 규칙
    - 각 키는 Unique
    - left child < parent < right child
    - 각 Subtree도 BST여야 한다.
* Search: O(h)
* Insert: O(h)
    - root에서 시작해서 맞는 조건에 따라 leaf node까지 내려가 삽입.
* Delete: O(h)
    1. Search For Target
    2. Target을 발견하면
        1. Leaf Node라면 삭제
        2. Child가 하나라면 child를 parent 자리에 배치
        3. Child가 둘이라면 right subtree에서 가장 작은 노드 혹은 left subtree에서 가장 큰 노드를 parent 자리에 배치
* 높이가 알고리즘의 성능을 결정하기 때문에 skew 방지를 위한 balancing 사용

#### Red - Black Tree
* BST에 balancing 알고리즘을 적용한 경우
* c++ map 에 적용
* 조건
    - 모든 노드는 red or black
    - root과 leaf는 black
    - red노드는 parent와 children이 black이어야 한다.
    - 한 노드에서 모든 후손 NIL까지의 black node의 개수가 동일하다(= black height)
    - 모든 노드는 child가 없을 경우 NIL을 child로 갖는다.
    - longest path <= shortest path * 2
* Search : O(log n)
* Insert : O(log n)
    1. node를 red로 지정한다(Height 변경의 최소화를 위해)
    2. BST에 따라 삽입
    3. 규칙에 위배된다면 rotation을 통해 height를 조정한다.
* Delete : O(log n)
    1. BST에 따라 노드 삭제
    2. 노드의 child 개수에 따라 rotation을 진행
    3. 기존 노드가 black이라면 해당 경로에 black node가 1개 추가되도록 rotation.
##### Rotation
* Red - Black Tree에서 rebalancing할 때 중심으로 쓰이는 알고리즘
* O(1)
    - Left Rotate : right child를 parent로 두고 right child의 left child를 right child로 두는 과정
        ```
        y = x.right
        x.right = y.left
        if(x.right != null) x.right.parent = x
        y.parent = x.parent
        if(x.parent.left == x) x.parent.left = y
        else x.parent.right = y
        y.left = x
        x.parent = y
        ``` 
    - Right Rotate : left child를 parent로 두고 left child의 right child를 left child로 두는 과정
        ```
        y = x.left
        x.left = y.right
        if(x.left != null) x.left.parent = x
        y.parent = x.parent
        if(x.parent.left == x) x.parent.left = y
        else x.parent.right = y
        y.right = x
        x.parent = y
        ``` 
##### Insertion
1. BST로 올바른 곳에 삽입
2. if(x == root) x = black 끝
3. if(x.parent == black) 끝
4. if(x.uncle == red) 
    1. x.parent = black
    2. x.uncle = black
    3. x.parent.parent = red
5. if(x.uncle == black)
    1. Left Left
        1. right rotate grandparent
        2. grandparent.color = parent.color
        3. parent.color = grandparent.color
    2. Left Right
        1. left rotate parent
        2. Left Left 적용
    3. Right Right
        1. left rotate grandparent
        2. grandparent.color = parent.color
        3. parent.color = grandparent.color
    4. Right Left
        1. right rotate parent
        2. Right Right 적용

##### Deletion
* v = 삭제 대상 
* u = v를 대체하게 되는 v의 child
1. BST 삭제 작용 
2. if(u == red || v == red)
    1. u = black
3. if(u == black && v == black)
    1. u = black black(double black)
    2. while (u == double black && u != root)
        1. if u.sibling == black && one of u.sibling.child == red
#### B-Tree
* Balanced Search Tree
* 이진트리를 확장해서 더 많은 수의 자식을 가질 수 있게 함
* 파일 관리 등에서 사용
* 규칙
    - 노드의 자료수가 n개면 자식 노드는 n + 1
    - 각 자료는 정렬 됨
    - 루트는 자식이 2개 이상 있음
    - 그 외의 노드는 n/2개 이상 있음
    - 모든 leaf의 위치는 동일함
    - 중복 없음
* B+ : B-Tree에서 검색을 더 빠르게 하기 위해 B에 index역할의 비단말 노드 추가



### Hash Table
* 고유한 인덱스 값으로 데이터에 접근
* Search: O(1) - average case(collision 일어나면 느려짐)
* Hash Function으로 유리한 보안
* _Collision_ : 두 개의 키가 같은 인덱스로 hashing 되는 현상
* 중복 제거에 활용 가능

#### Resolving Hashing Conflict
1. Open Address
    * Collision이 일어나면 다른 저장할 장소를 찾는 방식
    * 방법
        - Linear Probing: 순차적으로 다음 장소를 찾는 것
        - Quadratic Probing: 2차 함수를 이용해 탐색할 위치를 찾는 것
        - Double Hashing Probing: 충돌이 일어날 때 두 번째 함수를 사용하는 것.
2. Separate Chaining
    * 충돌 시 해당 bucket에 자료구조를 추가하여 데이터를 같은 위치에 저장하는 방식.
    * 간단하다
    * 공간이 부족해지는 경우는 없다
    * 모든 정보가 테이블에 있는 것이 아니기 때문에 캐쉬 성능이 떨어진다
    * 쓰이지 않는 공간이 나올 수 있음
    * 체인이 길어지면 탐색 시간이 길어지며 공간을 더 사용하게 된다.
    * 방법
        + Linked List
            - Search : O(l) <- l = length of linked list
            - Delete : O(l)
            - Insert : O(l)
        + Dynamic Sized Array
            - Linked List와 복잡도는 동일
            - C++(Vector), Java(ArrayList), Python(List)
        + Red-Black Tree: 
            - Search : O(log l) <- l = length of linked list
            - Delete : O(log l)
            - Insert : O(l)
            - Java 8 (HashMap)
            - 트리는 데이터 사용량이 많기 때문에 개수가 적을 땐 linked list
    > _list -> tree : 8, tree -> list : 6가 데이터 개수 기준_
3. Resizing
    * 데이터 개수가 해쉬 버킷의 개수의 75%가 될 때마다 버킷이 미리 두 배로 확장 하는 것.

### Graph
* 정점: 노드
* 간선: 노드와 노드 연결
* Directed/Undirected Graph : 간선의 방향 유 / 무
* Degree(Undirected) : 각 정점에 연결된 간선의 개수
* Degree(Directed)
    - Outdegree : 정점으로부터 나가는 간선의 개수
    - Indegree : 정점으로 들어오는 간선의 개수
* Weight Graph: 간선에 가중치가 있는 것.
* Subgraph: 그래프의 정점과 간선들의 일부로 이루어짐
* 구현
    - 인접 행렬(adjacent matrix)
        + Space : O(V^2)
    - 인접 리스트(adjacent list)
        + Space : O(E + V)
* Minimal Spanning Tree
    - Spanning Tree: 모든 정점이 cycle 없이 연결되어 있는 것
    - Minimal Spanning Tree: 간선 가중치의 합이 최소