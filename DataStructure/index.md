### 작성자 : 조준희

### Index
- Index란?
  - 인덱스란 추가적인 쓰기 작업과 저장 공간을 활용하여 데이터베이스 테이블의 검색 속도를 향상시키기 위한 자료구조
  
<br/>

- Index 의미
  - RDBMS에서 검색속도를 높이기 사용하는 하나의 기술
  - Index는 Table의 특정 컬럼을 색인화하여 데이터를 조회할 때 색인화된 index파일에서의 검색을 통해 검색 속도를 향상시킴

<br/>

- Index의 원리
  - Index를 해당 컬럼에 주게 되면 초기 TABLE생성시 만들어진 MYD,MYI,FRM 3개의 파일중에서 MYI에 해당 컬럼을 색인화 하여 저장함.
    - .FRM : 테이블 구조에 대한 정보를 저장
    - .MYD : 실제 데이터를 저장
    - .MYI : Index를 저장
  - 이 때 사용자가 Index가 사용하는 쿼리를 사용시 해당 테이블이 아닌 색인화된 파일인 MYI를 통해 검색함
  - 만약 Index를 사용하지 않은 쿼리라면 full scan을 통하여 해당 값을 찾음

<br/>

- Index의 장점
  - 키 값을 기초로 하여 테이블에서 검색과 정렬 속도를 향상시킴
  - 전반적인 시스템의 부하를 줄일 수 있다.

<br/>


- Index의 단점
  - 인덱스를 관리하기 위해 DB의 약 10%에 해당하는 저장공간이 필요함.
  - 인덱스가 많으면 데이터 삽입, 업데이트, 삭제 시 인덱스를 갱신해야 하므로 쓰기 성능이 저하될 수 있음.
  - 데이터 변경이 잦을 경우 Index를 재작성해야 하므로 성능에 악영향을 끼친다.


<br/>


- Index의 목적
  - 인덱스의 목적은 해당 RDBMS의 검색 속도를 높이는데 있음.
  - SELECT 쿼리의 WHERE절이나 JOIN 예약어를 사용했을때만 인덱스를 사용되며 SELECT 쿼리의 검색 속도를 빠르게 하는데 목적을 두고 있음.
  - DELETE,INSERT,UPDATE 쿼리에는 해당 사항없으며 Index사용 시 느려질 수 있음.

<br/>

- Index를 생성하면 좋은 경우
  - WHERE절에 자주 사용되는 컬럼 -> 해당 컬럼을 기준으로 데이터를 찾기 때문
  - 외래키가 사용되는 컬럼 -> JOIN시 해당 컬럼을 기준으로 데이터를 찾기 때문
  - JOIN에 자주 사용되는 컬럼 -> JOIN시 해당 컬럼을 기준으로 데이터를 찾기 때문
  - 수정 빈도가 낮을 때 (DML문이 별로 없을 때)
  - 카디널리티가 낮을 때 (중복도가 낮을 때)

<br/>

- Index 생성을 피해야 하는 경우
  - 데이터의 중복도가 높은 컬럼 (Cardinality가 낮은 컬럼,  ex : 성별)
    - 인덱스가 많은 행을 동일하게 참조하므로, 선택도가 낮아 인덱스가 검색 속도를 크게 개선하지 못함.
    - 불필요한 인덱스 유지 비용이 발생
  - DML이 자주 일어나는 컬럼
    - 인덱스는 데이터 변경 시마다 갱신되어야 하므로, 삽입, 수정, 삭제 작업이 빈번한 경우 성능 저하를 유발
      - INSERT
        - 새로운 Block을 할당 받은 후, Key를 옮기는 작업을 수행한다.
        - 데이터를 추가할 때 Index를 split하는 경우가 발생 -> 블로킹(대기하는 시간) 발생
      - DELETE
        - Table에서 data가 delete 되는 경우 : Data가 지워지고, 다른 Data가 그 공간을 사용 가능하다.
        - Index에서 Data가 delete 되는 경우 : Data가 지워지지 않고, 사용 안 됨 표시만 해둔다.(테이블 공간은 유지)
      - UPDATE
        - 테이블에 UPDATE가 발생할 경우 인덱스에서는 DELETE가 먼저 발생한 후 새로운 작업의 insert 작업이 발생 -> 더 많은 부하가 생김

<br/>

- Index 자료구조
  - B-tree
    - Balanced Tree 라는 특징이 있으며 자식 노드를 둘 이상 가질 수 없음 → 탐색 연산에 있어 O(log N)의 시간복잡도를 갖는다.
    - 모든 노드들에 대해 값을 저장하고 있으며 포인터 역할을 동반
  - B+tree
    - 값을 리프노드에만 저장하며 리프노드는 서로 Linkedlist로 연결되어있음 → Hash와 달리 부등호문 연산에 대해 효과적
    - 값은 리프노드에서만 찾을 수 있음 → 리프노드를 제외한 나머지 노드는 포인터 역할만 함
  - Hash 자료구조
    - 일반적인 경우 탐색, 삽입, 삭제 연산에 대해 O(1)의 시간 복잡도를 갖는다.
    - 최악의 경우 해시 충돌이 발생하는 것으로 탐색, 삽입, 삭제 연산에 대해 O(N)의 시간복잡도를 갖는다.
    - 해시는 등호(=) 연산에만 특화되어 있어 부등호 연산(>, <)이 자주 사용되는 데이터베이스 검색에는 해시 테이블이 적합하지 않다.

<br/>

[참고자료]
- https://lalwr.blogspot.com/2016/02/db-index.html
- https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/%5BDB%5D%20Index.md
- https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Database#index
- https://github.com/devSquad-study/2023-CS-Study/blob/main/DB/db_index.md
- [B-tree 시뮬레이터](https://www.cs.usfca.edu/~galles/visualization/BTree.html)
- [B+tree 시뮬레이터](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
