### 작성자 : 박민지
<br>

# Binary Search Tree
<img width="400" src="https://github.com/user-attachments/assets/cbd7569c-b9ac-439c-9b40-1feb4a2b51aa">
<br><br>

> 이진 트리는 트리 중에서 가장 널리 쓰이는 트리다. 특히 이진 탐색 트리는 이진 트리의 일종으로 자식 노드가 최대 두 개인 노드들로 구성된 트리를 이진 트리라고 한다. 이진 탐색 트리는 **이진탐색과 연결리스트를 결합한 자료구조다**

- **이진 탐색 트리는 효율적인 검색, 삽입, 삭제를 가능하게 한다**
- 왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드
- 이진 탐색 트리의 순회는 **중위 순회** 방식으로 정렬된 순서를 읽을 수 있다
- 평균적으로 `O(log n)`의 시간복잡도를 가진다
- 모든 원소는 유일한 키를 가진다
<br>

### [유일한 키를 가진다?]
- 이진 탐색의 목적은 **검색**이다
- 중복이 많은 경우 검색 속도가 느려진다

### [트리와 이진트리]
- 이진 트리의 모든 노드는 차수(= 자식 노드)가 2 이하다
- 이진 트리는 노드를 하나도 갖지 않을 수 있다
- 이진 트리는 서브 트리 간에 순서가 존재한다 (왼쪽, 오른쪽 서브 트리를 구분한다)

### [이진 탐색과 연결 리스트]
- 이진탐색의 시간복잡도는 O(log n) / 연결 리스트의 시간 복잡도는 O(1)
- 이진 탐색은 삽입, 삭제가 불가하다 (탐색에 최적화된 알고리즘) / 연결 리스트는 O(n)의 시간이 걸린다
- 이 두 가지를 합해 장점을 모두 얻기 위하여 고안된 것이 `이진 탐색 트리`
<br>

### Q. 이진 탐색 트리의 시간 복잡도
<img width="600" alt="image" src="https://github.com/user-attachments/assets/415783f6-1933-435a-9fd2-b4e60a0d818f">

- 시간 복잡도 = `O(log n)`
- 트리의 높이 = log n
- 균등한 트ㅇ리의 경우 시간 복잡도가 O(n)이 된다
- 최악의 경우 트리가 한 쪽으로 치우치면 시간 복잡도가 O(n)이 된다


###### *편향된 트리(한 쪽으로 뻗어나감)는 시간 복잡도가 O(n)이 되기 때문에 트리를 사용할 이유가 없다. 이를 개선하는 트리가 AVL Tree, RedBlack Tree

###### *삽입, 검색, 삭제에 대한 시간 복잡도는 트리의 depth에 비례한다
<br>

### Q. 이진 탐색의 삽입
<img width="600" alt="image" src="https://github.com/user-attachments/assets/b0c16db7-89e3-49ad-977c-0d7b27964644">
<br>

### Q. 이진 탐색의 삭제
<img width="700" alt="image" src="https://github.com/user-attachments/assets/8a881bc0-faf3-48bf-b7e2-d5885f08fe5c">

- 삭제할 노드가 리프 노드인 경우
	- 바로 삭제 가능하다
- 삭제할 노드에 자식이 하나만 있는 경우
	- 삭제할 노드의 자식을 삭제할 노드의 부모에 직접 연결한다
- 삭제할 노드에 자식이 둘 있는 경우
	1. 삭제할 노드의 successor 노드를 찾는다 (전위순회에서 다음에 방문할 노드)
 	2. 삭제할 노드와 successor 노드의 값을 바꾼다
  	3. successor 노드를 삭제한다
<br>

### Q. 이진 탐색의 구현
<img width="500" src="https://github.com/user-attachments/assets/b8ef215b-a909-4f7a-870f-db25a9a7ae24">

```java
int binarySearch(int key, int low, int high) {
	int mid;

	while(low <= high) {
		mid = (low + high) / 2;

		if(key == arr[mid]) {
			return mid;
		} else if(key < arr[mid]) {
			high = mid - 1;
		} else {
			low = mid + 1;
		}
	}

	return -1; // 탐색 실패 
}
```

### Q. 그래프 vs 트리
> `그래프`<br>
> 노드와 간선으로 구성된 자료구조로 연결되어 있는 객체 간의 관계를 표현한다.<br>
> `트리`<br>
> 노드와 간선으로 구성된 자료구조로 그래프의 한 종류다. 두 노드 사이에 반드시 1개의 경로만을 가지며 사이클이 존재하지 않는 방향 그래프다.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/7191ff51-55e9-4c3f-999b-88707e6f7f34">

||그래프|트리|
|-|-----|--|
|방향성|방향 or 무방향|방향|
|사이클|순환, 비순환|비순환|
|루트노드|X|1개의 루트 존재|
|부모-자식|X|1개의 부모노드(루트 제외)|
|모델|네트워크|계층|
|간선 수|제한 없음|N-1개|
|순회|DFS, BFS|DFS, BFS, 전/중/후위 순회|
