### 작성자 : 조준희

# Stack 
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/32b808da-76b6-40a2-b5d8-97c108b4af9a" alt="stack"/>

## 스택이란?
- 스택은 Last In First Out(LIFO, 후입선출)의 원리를 따르는 자료구조이다.
- 예를 들어 프링글스를 생각해보자, 프링글스 뚜껑을 열고 먹으려면 제일 위에 있는 것부터 먹어야 한다. 이것이 스택의 원리이다.
- 스택은 데이터를 쌓아 올린 형태의 자료구조를 뜻한다.
- 데이터를 한 방향으로만 쌓을 수 있으며 삽입/삭제/조회 등의 연산은 최상층에서만 가능하다.
- 시간복잡도는 제일 말단에서 연산이 이루어지므로 당연히 O(1)이다.

## 스택을 구성하는 주요 함수
- push() : 스택에 데이터를 삽입하는 함수
  - 스택이 가득 차있으면 Overflow가 발생한다.
- pop() : 스택에서 데이터를 삭제하는 함수
  - 스택이 비어있으면 Underflow가 발생한다.
- isEmpty() : 스택이 비어있는지 확인하는 함수
  - 스택이 비어있는지 유무에 관해 boolean 값을 반환한다.
- isFull() : 스택이 가득 차있는지 확인하는 함수
  - 스택이 비어있는지 유무에 관해 boolean 값을 반환한다.
- peek() : 스택에서 데이터를 조회하는 함수


# Queue 

<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/ca4a4309-ef31-45fb-a6c9-2d03daa98d9a" alt = "queue"/>

## 큐란?
- 큐는 First In First Out(FIFO, 선입선출)의 원리를 따르는 자료구조이다.
- 예를 들어 식당 예약줄을 생각해보자. 제일 먼저 줄을 선 사람이 먼저 들어가는 것을 생각할 수 있다. 이것이 큐의 원리이다.
- 큐는 데이터를 한 방향으로만 삽입하고 다른 방향으로만 삭제할 수 있는 자료구조이다.
- Front(앞)에서 데이터의 조회/삭제가 이루어지고 Rear(뒤)에서 삽입 연산을 수행한다.
- 시간복잡도는 제일 말단에서 연산이 이루어지므로 당연히 O(1)이다.

## 큐를 구성하는 주요 함수
- enqueue() : 큐에 데이터를 삽입하는 함수
  - Rear(뒤)에서 데이터 삽입을 수행한다.(위 이미지의 오른쪽) 
  - 큐가 가득 차있으면 Overflow가 발생한다.
- dequeue() : 큐에서 데이터를 삭제하는 함수
  - Front(앞)에서 데이터 삭제를 수행한다.(위 이미지의 왼쪽)
  - 큐가 비어있으면 Underflow가 발생한다.
- isEmpty() : 큐가 비어있는지 확인하는 함수
  - 큐가 비어있는지 유무에 관해 boolean 값을 반환한다.
- isFull() : 큐가 가득 차있는지 확인하는 함수
  - 큐가 비어있는지 유무에 관해 boolean 값을 반환한다.
- peek() : 큐에서 데이터를 조회하는 함수

## 스택 2개로 큐를 구현하는 방법
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/d2507a78-6107-4013-a7de-7ceca7ca187f" alt = "stack_to_queue"/>

구현 순서

1. inBox 에 데이터를 push(삽입)한다. - A,B
2. inBox 에 있는 데이터를 pop(추출) 하여 outBox 에 push(삽입) 한다. - B,A
3. outBox 에 있는 데이터를 pop(추출) 한다. - A,B 순으로 출력됨

즉, inBox에 삽입한 데이터를 pop으로 빼고 그 데이터를 outBox에 push해서 만들어진 스택을 pop하면 큐가 구현된다.

참고자료: https://creatordev.tistory.com/83


## 큐 2개로 스택을 구현하는 방법

구현 순서
### 1. 메인큐에 값을 넣는다.(put) ex) 1→ 2→ 3 → 4
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/c9db23ec-05b7-4ba5-8a9e-c371bf641db1" alt = "queue_to_stack1"/>

<br/>

### 2. 메인 큐에 1개의 원소가 남을 때까지 get 한 값을 임시 큐에 put 한다.
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/3b76b3c0-8afd-4207-994e-e5438356d753" alt = "queue_to_stack2"/>

<br/>


### 3. 마지막 남은 원소는 result 변수에 저장한다.
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/f1cd3714-0d36-4af6-b7f3-ab847f764d36" alt = "queue_to_stack3"/>

<br/>


### 4. 임시 큐에 있는 원소들을 메인 큐로 모두 이동시킨다. (get → put)
<img src = "https://github.com/EN-CS-STUDY/CS_STUDY/assets/48996701/57228080-03a6-4ac8-9aa8-a2551e45846d" alt = "queue_to_stack4"/>

### 5. result 값을 리턴한다.

참고자로 : https://ryu-e.tistory.com/96