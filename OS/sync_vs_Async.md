### 작성자 : 박민지
<br>

# Synchronous
### Q. 동기화란?
`" 여러 프로세스가 공유하는 자원들의 일관성을 유지하는 것 "`<br>
> 하나의 공유자원에 여러 프로세스가 동시에 접근할 때,<br>
> 프로세스들의 `순서`를 정해서 동기화를 이루고 공유 자원의 일관성을 유지한다

#### [목적]
여러 프로세스가 서로 간섭하지 않고 공유 자원에 접근하여 데이터 불일치 가능성을 방지한다<br>
#### [기술]
  - semaphores
  - monitors
  - critical sections (임계 구역)
#### [프로세스]
- Independent Process
  - 프로세스의 실행이 다른 프로세스에 영향을 주지 않음
- Cooperative Process
  - 시스템에서 실행되는 다른 프로세스에 영향을 주거나 받을 수 있음

### Q. 프로세스의 race condition
`" 두 개의 스레드가 하나의 자원을 놓고 서로 사용하려고 경쟁하는 상황 "`
> 둘 이상의 프로세스가 동일한 코드를 실행하거나, 해당 조건에서 동일한 메모리 or 공유 자원에 액세스하려고 할 때<br>
> 접근이 어떤 순서에 따라 이루어졌는지에 따라 실행 결과가 달라지는 상황을 의미한다

#### [발생 상황]
<img width="200" alt="image" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/665bf92a-972d-4874-be85-03d5294355ab">

- 공유자원 10에 대해 Process1과 Process2가 자원을 사용하려 한다
  - P1 -> P2 순서로 실행하는 경우 공유자원 = 9
  - P2 -> P1 순서로 실행하는 경우 공유자원 = 11
- 프로세스의 접근 순서에 따라 실행 결과가 달라진다


#### [제어 문제]
1. Mutual exclusion


2. Deadlock


3. starVation


<br>

---

### Asynchronous
#### 


##### 참고자료
https://www.geeksforgeeks.org/introduction-of-process-synchronization/
