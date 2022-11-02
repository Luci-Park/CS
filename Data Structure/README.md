# Data Structure

## 선형 구조(linear)

### Array
+ 논리적 저장 순서와 물리적 저장 순서가 일치
+ 인덱스로 해당 원소 접근 가능
+ Search: O(1) = random access가능
+ Insert: O(n) (worst case) 
+ Delete: O(n) (worst case)

### Linked List
+ 각 원소들이 자기 자신 다음 원소를 기억하고 있음
+ Insert나 Delete은 O(1)가 가능하나 Search는 O(n).
+ Insert와 Delete에는 Search가 거의 필수적으로 필요하기 때문에 O(n)이나 다름없다.
+ Tree 구조의 기본이 된다.

### Stack
+ LIFO(Last In First Out)
+ Push, Pop

### Queue
+ FIFO(First In First Out)
+ Enqueue, Dequeue

## 비선형

### Tree
+ Parent와 Child가 연결, Child가 Ancestor과 연결되어 있거나 하는 Cycle를 허용하지 않는다.
+ Node(노드)
+ Edge(간선)
+ Root Node(루트)
+ Terminal Node(= Leaf Node, 단말 노드)
+ Internal Node(내부 노드, 비단말 노드)

#### Binary Tree(이진 트리)
+ 모든 root은 두 개의 subree를 가진다(이 때 공집합도 이진 트리이다)
+ 배열로 구성되었고, root이 1에서 시작하며, 노드의 개수가 n개 일 때:
    - Left(i) = 2 * i
    - Right(i) = 2 * i + 1
    - Parent(i) = lower_bound(i / 2)
    - Height = lower_bound(n / 2)
+ Full Binary Tree(정 이진 트리): 모든 비단말 노드가 2개 자식을 가지고 있는 이진 트리
+ Perfect Binary Tree(포화 이진 트리): 모든 레벨이 꽉 찬 이진 트리
+ Complete Binary Tree(완전 이진 트리): 마지막 level을 제외한 모든 level에 노드가 왼쪽에서부터 차례로 다 차 있는 이진 트리

#### Heap
+ Complete Binary Tree
+ 배열 형식을 주로 사용.
+ Max Heap: 모든 노드는 parent >= child를 만족한다.
+ Min Heap: 모든 노드는 parent <= child를 만족한다.
+ Search: Heap은 최댓값/최솟값만을 access하기에 적합하다. O(1)
+ Insert: O(log n)
    1. root에서 시작하여 
+ Delete: O(log n)
    - 


#### Binary Search Tree
+ 쉽게 찾기 위해 정렬한 트리
+ 규칙
    - 각 키는 Unique
    - left child < parent < right child
    - 각 Subtree도 BST여야 한다.
+ Search: O(h)
+ Insert: O(h)
    - root에서 시작해서 맞는 조건에 따라 leaf node까지 내려가 삽입.
+ Delete: O(h)
    1. Search For Target
    2. Target을 발견하면
        1. Leaf Node라면 삭제
        2. Child가 하나라면 child를 parent 자리에 배치
        3. Child가 둘이라면 right subtree에서 가장 작은 노드 혹은 left subtree에서 가장 큰 노드를 parent 자리에 배치
+ 높이가 알고리즘의 성능을 결정하기 때문에 skew 방지를 위한 balancing 사용

#### Red - Black Tree
+ BST에 balancing 알고리즘을 적용한 경우
+ 조건
    - 모든 노드는 red or black
    - root과 leaf는 black
    - red노드는 parent와 children이 black이어야 한다.
    - 한 노드에서 모든 후손 NIL까지의 black node의 개수가 동일하다(= black height)
    - 모든 노드는 child가 없을 경우 NIL을 child로 갖는다.
    - longest path <= shortest path * 2
+ Search : O(log n)
+ Insert : O(log n)
+ Delete : O(log n)

##### Rotation
<img scr = "https://github.com/Luci-Park/CS/blob/main/Images/Tree_rotation.png" width = 200>

+ Red - Black Tree에서 rebalancing할 때 중심으로 쓰이는 알고리즘
+ O(1)

- Left Rotate
- Right Rotate