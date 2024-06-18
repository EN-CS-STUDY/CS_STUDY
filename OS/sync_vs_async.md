### 작성자 : 박민지
<br>

# Synchronous
### Q. 동기화란?
`" 여러 프로세스가 공유하는 자원들의 일관성을 유지하는 것 "`<br>
> 하나의 공유자원에 여러 프로세스가 동시에 접근할 때,<br>
> 프로세스들의 `순서`를 정해서 동기화를 이루고 공유 자원의 일관성을 유지한다<br>
> ex) 커피 주문을 받고 나올 때까지 기다리는 것처럼 순서에 맞춰 진행되지만, 여러가지 요청을 동시에 처리할 수 없다

#### [목적]
여러 프로세스가 서로 간섭하지 않고 공유 자원에 접근하여 데이터 불일치 가능성을 방지한다<br>
#### [특징]
- 요청과 결과가 동시에 일어난다 (blocking)
- 노드 사이의 작업 처리 단위를 동시에 맞춘다
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
> 둘 이상의 프로세스가 동일한 코드를 실행하거나,<br>
> 해당 조건에서 동일한 메모리 or 공유 자원에 액세스하려고 할 때<br>
> 접근이 어떤 순서에 따라 이루어졌는지에 따라 실행 결과가 달라지는 상황을 의미한다<br>
> (시스템이 해당 자원에 대한 접근을 제어하거나 동기화하지 않을 때 발생)
<br>

#### [발생 상황]
<img width="200" alt="image" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/665bf92a-972d-4874-be85-03d5294355ab">

- 공유자원 10에 대해 Process1과 Process2가 자원을 사용하려 한다
  - P1 -> P2 순서로 실행하는 경우 공유자원 = 9
  - P2 -> P1 순서로 실행하는 경우 공유자원 = 11
- 프로세스의 접근 순서에 따라 실행 결과가 달라진다
- 즉, 한 번에 하나의 프로세스만 리소스에 접근할 수 있도록 보장하기 위해 프로세스 동기화가 필요하다
<br>

#### [Critical Section]
```C
do{
    flag=1;
    while(flag); // (entry section)
        // critical section
    if (!flag) // (exit section)
        // remainder section
} while(true);
```

> 프로세스의 동기화 문제는 critical section으로부터 시작한다<br>
> 한 프로세스가 자신의 section에서 수행하는 동안 다른 프로세스는 해당 구역에 접근할 수 없어야 한다

- `entry section`
  - critical section 진입 허가 요청을 구현하는 부분
  - data inconsistency를 막기 위한 제약 조건 (critical section에 2개 이상의 process 접근X)
- `critical section`
  - 각 process별로 포함하고 있는 코드 집합
- `exit section`
  - critical section에서 나간다
  - 작업이 끝난 후 process가 나왔다는 것을 다른 process에게 알려주는 영역
- `remainder section`
  - critical section이 아닌 다른 section을 처리한다
  - data inconsistency가 발생하지 않는 영역
<br>

#### [발생 원인]
##### 1. 커널 작업 수행 중 인터럽트 발생
> `유저모드`<br>
> 사용자가 접근할 수 있는 영역을 제한적으로 두고 자원에 함부로 접근하지 못하는 코드를 말한다. 코드 작성, 프로세스 실행 동작이 가능하다<br>
> `커널모드`<br>
> 모든 자원(CPU, Memory..) 접근, 명령이 가능한 모드이다
>  
> ###### *프로세스간 주소 공간은 독립적이지만, 커널에 있는 자원을 여러 프로세스가 공유할 수 있기 때문에 주로 커널모드에서 발생한다
<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/ca35d189-0f75-4acf-9b11-e3c76fd618e6">

- 문제
  - 커널모드에서 데이터를 불러 작업을 수행하다 인터럽트가 발생하면 데이터를 조작하게 된다
- 해결
  - 커널모드에서 작업을 수행할 때, 인터럽트가 발생하지 않도록 설정해야 한다

##### 2. 프로세스가 시스템콜을 하여 커널모드로 진입해서 작업을 수행하던 중 Context Switch 발생
<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/a7bad76e-d4ab-4b65-80bb-bb033983f8a2">

- 문제
  - P1이 커널모드에서 작업(count값을 읽고 증가)하는 도중 contex switch가 발생하면, P2에서 같은 데이터를 조작하게 되면 P2의 작업은 반영되지 않는다
- 해결
  - 프로세스가 커널모드에서 작업하는 경우 CPU 제어권을 뺏기지 않도록 한다

##### 3. 멀티 프로세서에서 공유 메모리 내의 커널 데이터에 접근할 때
<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/07c3cc11-dc43-41c0-9532-7c1035a2e317">

- 문제
  - 멀티 프로세스 환경에서 2개의 CPU가 동시에 커널 내부의 공유 자원에 접근하는 경우
- 해결
  - 커널 내부에 있는 공유 데이터에 접근할 때마다 lock/unlock을 해서 해결한다

###### *Thread-safe
> 멀티 스레드 환경에서 여러 스레드가 동시에 하나의 공유자원에 접근할 대 의도대로 동작하는 것을 의미한다.<br>
> 이를 위해 Critical Section을 동기화 기법으로 잘 제어해줘야 한다

<br>

#### [해결 방법]
1. Mutual exclusion
> process가 critical section에서 실행중인 경우 다른 process의 접근을 막는다

2. Deadlock
> 

3. Starvation


<br>

---

### Asynchronous
#### 


### Sync vs Async
- 동기 방식
  - 설계가 매우 간단하고 직관적이다
  - 결과가 주어질때까지 대기해야 한다 (ready queue)
- 비동기 방식
  - 동기보다 복잡하다
  - 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 할 수 있다
  - 자원을 효율적으로 사용할 수 있다

##### 참고자료
https://www.geeksforgeeks.org/introduction-of-process-synchronization/
