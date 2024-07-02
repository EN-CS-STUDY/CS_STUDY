### 작성자 : 조준희

# 힙(Heap)

<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/8444c9d0-36bc-4f8a-8164-6f8d34efaf68" alt = "heap"/>

## 힙이란?
- Tree의 형식을 취하고 있으며 Tree 중에서도 배열을 기반으로 한 (Complete Binary Tree)완전 이진 트리이다.
- 최대값 및 최소값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전 이진 트리이다.
- 배열에 트리의 값을 넣어줄 때는 0이 아닌 1번째부터 루트 노드가 시작된다. 이유는 노드의 고유 번호와 index를 일치시켜 혼동을 줄이기 위함이다.
- 중복된 값을 허용. (이진 탐색 트리는 중복 값을 허용하지 않음.)
- 힙의 종류로는 최대힙과 최소힙이 존재한다.
- 최대힙 : 각 노드의 값이 자식 노드의 값보다 크거나 같은 Complete Binary Tree를 말한다.
- 최소힙은 최대힙의 반대이다.
- 위 최대, 최소힙 예시 이미지를 보면 Root Node에 있는 값이 제일 극단이므로, 각 루트 노드 값인 32, 14 탐색하는데 소요되는 연산의 시간 복잡도는 O(1)이다.
- Complete Binary Tree이기 때문에 배열을 이용해 관리할 수 있으며, 인덱스를 통한 Random Access가 가능하다.
- Index 번호는 노드 개수가 n개일 때, i번째 노드에 대하여 왼쪽 자식은 ix2, 오른쪽 자식은 ix2+1가 된다.

## 개념
### 힙은 자료구조의 일종으로 우선 순위 큐를 위해서 만들어졌다.

#### 우선 순위 큐 
- 우선순위의 개념을 큐에 도입한 자료구조.
- 데이터들이 우선순위를 가지고 있으며, 우선순위가 높은 데이터가 큐에서 먼저 빠져 나간다.

#### 언제 사용?
- 시뮬레이션 시스템, 작업 스케줄링, 수치해석 계산.
- 우선 순위 큐는 배열, 연결 리스트, 힙으로 구현한다. (힙으로 구현하는 게 가장 효율적이다.)

#### 시간 복잡도
- 삽입 : O(logN)
- 삭제 : O(logN)
- 스택 : LIFO, 큐 : FIFO


## 힙 구현
### 힙 예시 
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/1be18871-69f6-496c-a18c-b18b85f5c0bf" alt = "heap1"/>

### 힙 삽입
1. 힙에 새로운 요소가 삽입되면, 우선 새로운 노드를 힙의 마지막 노드에 삽입한다 
2. 새로운 노드를 부모 노드들과 교환한다

#### 1. 가장 말단 노드에 새로운 요소를 삽입
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/5bb02903-e57f-4489-8459-0988f94f1117" alt = "heap2"/>

#### 2. 부모 노드와 비교하여 값이 자식 노드가 더 크다면 교환하고 작다면 해당 부분에서 정지한다
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/fa3e7e88-b8ff-4d6e-a120-a5c3faaf9790" alt = "heap3"/>

#### 3. 다시 자신의 부모 노드와 비교를 수행, 크다면 교환, 작다면 해당 부분에서 정지
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/1d12428f-d16d-4f16-85b8-2eb88955b652" alt = "heap4"/>

#### 4. 교환한 노드에서 다시 큰 값을 가진 자식과 위치를 교환하며 최대 힙 구조가 유지되면서 삽입과정 완료
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/95808f06-4954-45dd-8f67-409f1411d490" alt = "heap5"/>


### 힙의 삭제
1. 최대 힙에서 최댓값은 루트 노드이므로 루트 노드가 삭제된다(최대 힙에서 삭제 연산은 최댓값 요소를 삭제하는 것)
2. 삭제된 루트 노드에는 힙의 마지막 노드를 가져옴
3. 힙을 재구성

#### 1. 루트 노드를 삭제하고 마지막 원소를 루트로 이동
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/69dc5a6d-1029-4c3b-a943-b06db606f67c" alt = "heap6"/>

#### 2. 큰 값을 가진 자식과 위치를 교환
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/5bb02903-e57f-4489-8459-0988f94f1117" alt = "heap7"/>

#### 3. 교환한 노드에서 다시 큰 값을 가진 자식과 위치 교환
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/ad8534d2-2b8c-49ab-ad91-6a28cbb64745" alt = "heap8"/>

#### 4. 교환한 노드에서 다시 큰 값을 가진 자식과 위치를 교환하며 힙 구조 완성
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/2f11778e-9b85-4e0c-becc-c5d245adbd4a" alt = "heap9"/>


### 힙이 아닌 배열을 힙으로 만들기(Build heap)
heapify의 경우 기본적으로 힙을 만족하는 경우에서 삽입 또는 삭제가 발생할 때 O(logN)의 시간복잡도로 다시 힙을 만드는 과정이다

Build heap은 이와 다르게 아예 힙의 조건을 만족하지 않는 배열을 힙으로 만드는 과정이다

여러 번의 heapify 과정을 거치기에 결과적으로 O(NlogN)의 시간복잡도를 가진다

아래 그림을 통한 예시로 자세히 살펴보자
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/e7b1e5cf-fc00-4c30-9dc8-acc1a93f4f23" alt = "heap10"/>
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/7ed36a19-8c97-4ceb-af76-ecb2b2c979a4" alt = "heap11"/>
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/82e49785-7a92-4a97-ad4b-30053126b6e3" alt = "heap12"/>
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/0b065256-ccb4-4055-92b5-e1e3895be275" alt = "heap13"/>
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/6d6055f5-cd62-4ca4-b006-3e592813ff5a" alt = "heap14"/>

### 면접 예상 질문
- 힙 구조에 대해서 설명해주세요
- 힙의 삽입 과정을 설명해주세요