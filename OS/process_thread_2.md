작성자: 신지훈

## 멀티 스레드(Multi thread)

스레드는 한 프로세스 내에서 실행되는 동작의 단위이다. 각 스레드는 속해있는 프로세스의 **스택 메모리**를 제외한 나머지 메모리 영역을 공유할 수 있다.

멀티스레드란 하나의 프로세스가 동시에 여러개의 일을 수행할 수 있도록 해주는 것이다. 즉 하나의 프로세스(실행이 된 하나의 프로그램)에서 여러 작업을 병렬로 처리하기 위해 멀티스레드를 사용한다.

멀티스레드에서는 한 프로세스 내에서 여러 개의 스레드가 있고, 각 스레드들은 스택 메모리를 제외한 나머지 영역(Code, Data, Heap)영역을 공유한다.

cf) Stack 영역?

함수 호출 시 전달되는 인자, 함수의 return address, 함수 내 지역변수등을 저장하기 위한 메모리영역이다.

![Untitled](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/88e4ffd8-b083-4b98-95c1-0ab5d76a1a89)

- **스레드가 독립적으로 가지고 있는 부분**
  - **program counter (→ 실행 흐름)**
  - register set
  - stack space
- **스레드가 동료 스레드와 공유하는 부분 (=task)**
  - code
  - data
  - heap

### Stack memory & PC register

스레드가 **함수를 호출**하기 위해서는 인자 전달, Return Address 저장, 함수 내 지역변수 저장 등을 위한 독립적인 stack memory 공간을 필요로 한다. 결과적으로 스레드는 프로세스로부터 Stack memory 영역은 독립적으로 할당받고, Code, Data, Heap 영역은 공유하는 형태를 가지게 된다.

또한 멀티스레드에서는 각각의 스레드마다 PC register를 가지고 있어야한다. 그 이유는 한 프로세스내에서도 스레드끼리 context switch가 일어나는데, PC register에 code address가 저장되어 있어야 이어서 실행을 할 수 있기 때문이다.

![Untitled1](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/025b5360-aba5-4e61-ad52-89798b3f8b0d)

멀티스레드 환경에서 스레드 사이의 메모리 공유

### 🏅프로세스와 스레드의 차이점

운영체제는 프로세스마다 독립된 메모리 영역을 Code/Data/Stack/Heap의 형식으로 할당한다. 각각 독립된 메모리 영역을 할당해주기 때문에 **프로세스는 다른 프로세스의 변수나 자료에 접근할 수 없다.**

![Untitled2](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/ab3f0716-70ef-471f-ba30-143877fcc7a1)

이와 다르게, 스레드는 메모리를 서로 공유할 수 있다. 자세히 말하자면 프로세스가 할당받은 메모리 영역 내에서 Stack 형식으로 할당된 메모리 영역은 따로 할당받고, 나머지 **Code/Data/Heap 형식으로 할당된 메모리 영역을 공**유한다. 따라서, **각각의 스레드는 별도의 스택을 가지고 있지만 힙 메모리는 서로 읽고 쓸 수 있게 된다.**

![Untitled3](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/34101beb-2251-4c9b-8f66-4385585aae46)

정리하자면, **프로세스는 운영체제로부터 별도의 메모리 영역을 할당** 받고

**스레드는 Stack 을 제외한 Code/Data/Heap 부분은 공유해 서로 읽고 쓸 수 있게 된다**. **(공유자원을 가진다.)**

📌**이런 방식으로 메모리는 공유하는 이유는?**

스레드는 “흐름의 단위로" CPU 의 입장에서 최소 작업 단위가 된다. 반면 운영체제는 이렇게 작은 단위까지 직접 작업하지 않기 때문에 운영체제의 관점에서는 프로세스가 최소 작업단위가 된다.

여기서 중요한 점은 하나의 프로세스는 하나 이상의 스레드를 가진다는 점이다. 따라서, 운영체제 관점에서는 프로세스가 최소 작업 단위인데, 이 때문에 같은 프로세스 소속의 스레드끼리 메모리를 공유하지 않을 수 없다.

---

위의 사진은 **멀티 스레드**를 나타내는 구조도이다.

![Untitled4](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/6e838a73-e391-45fa-817c-815d9c5623b8)

**멀티스레드**는 하나의 프로세스가 여러 작업을 여러 스레드를 사용해 동시에 처리하는 것을 의미한다.

![Untitled5](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/78f36410-76cf-4b5b-846f-15bf161fd699)

유튜브를 예시로 봤을때, 댓글을 작성하면서 렌더링이 되면서 동영상이 재생된다. 이 모든 작업들이 한 프로세스 내에서 **독립적으로 실행**되고 있는 기능들이다.

### ☑️ multi process vs multi thread

두 방법 모두 동시에 여러 작업을 수행한다는 점에서 유사하다. 적용할 시스템에 따라서 두 방법의 장단점을 고려해 적합한 방식을 선택해야한다.

메모리 구분이 필요할 때는 multi process가 유리하다. 반면에 context switching이 자주 일어나고 데이터 공유가 빈번한 경우, 그리고 자원을 효율적으로 사용해야 하는 경우에는 multi thread를 사용하는 것이 유리하다.

![Untitled6](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/3b609cfe-8d41-4584-8b43-5e60068cabb6)

첫번째 사진은 멀티 프로세스이다. 하나씩 돌아가면서 PC register를 가리키면서 context switch가 발생하게 된다. 이 때의 context switch는 상당히 느리다. 왜냐하면 바꿀 때 마다 프로세스를 시작하기때문에 세팅해야할 값들이 많기 때문이다.

두번째 사진은 멀티스레드이다. 하나의 메모리영역밖에 없고 그 안에는 stack3개와 code3개로 나뉜다. 메모리 영역을 공유하고있기 때문에 멀티프로세스보다 context switch가 상당히 빠르다.

멀티프로세스대신 멀티스레드로 구현할 경우, 메모리 공간과 시스템 자원 소모가 줄어들게 된다. 하지만 멀티스레드를 사용할 때는 스레드간 자원을 공유하기 때문에 **동기화문제**가 발생할 수 있기 때문에 이를 고려한 프로그램 설계가 필요하다.

> **동기화문제란?** 서로 다른 스레드가 메모리 영역을 공유하기 때문에 여러 스레드가 동일한 자원에 동시에 접근하여 엉뚱한 값을 읽거나 수정하는 문제

또한 process간의 통신(IPC)보다 thread간의 통신 비용이 적기 때문에 통신으로 인한 오버헤드가 적다.

|               | 메모리 사용 / CPU 시간           | Context switching | 안정성 |
| ------------- | -------------------------------- | ----------------- | ------ |
| multi process | 많은 메모리 공간 / CPU 시간 차지 | 느림              | 높음   |
| multi thread  | 적은 메모리 공간 / CPU 시간 차지 | 빠름              | 낮음   |

📌**핵심정리**

- 멀티스레드는 멀티프로세스보다 적은 메모리 공간을 차지하고 context switching이 더 빠르다.
- 멀티프로세스는 멀티스레드보다 많은 메모리공간과 CPU 시간을 차지한다.
- 멀티스레드는 동기화문제와 하나의 스레드 장애로 전체 스레드가 종료될 위험이 있다.
- 멀티프로세스는 하나의 프로세스가 죽더라도 다른 프로세스에 영향을 주지 않아 안정성이 높다.

📍**핵심정리**

멀티스레드가 멀티 프로세스보다 좋은 점은?

- 멀티프로세스를 이용하던 작업을 멀티 스레드로 구현할 경우, 메모리 공간과 시스템 자원 소모가 줄어들게 된다. 또한 프로세스를 생성하고 자원을 할당하는 등의 system call을 생략할 수 있기 때문에 자원을 효율적으로 관리할 수 있다. 뿐만 아니라 context switching 시 캐시 메모리를 초기화할 필요가 없어서 속도가 빠르다.
- 데이터를 주고 받을 때를 비교해보면, 프로세스간의 통신(IPC)보다 멀티스레드간의 통신 비용이 적기 때문에 통신으로 인한 오버헤드가 적다. (통신시 별도의 자원을 이용하지 않고, 프로세스에 할당된 Heap영역등을 이용하여 데이터를 주고받기 때문이다.)

멀티스레드가 멀티프로세스보다 안좋은 점은?

- 스레드간의 자원 공유 시 동기화문제가 발생할 수 있어서 프로그램 설계 시 주의가 필요하고, 하나의 스레드에 문제가 생기면 프로세스 내의 다른 스레드에도 문제가 생길 수 있다.

### 멀티프로세스 환경에서 프로세스간의 데이터는 어떻게 주고받는가?

원칙적으로 프로세스는 독립적인 주소 공간을 갖기 때문에, 다른 프로세스의 주소 공간을 참조할 수 없다. 하지만 경우에 따라 운영체제는 프로세스간의 자원 접근을 위한 매커니즘인 프로세스 간 통신(IPC, Inter Process Comunication)를 제공한다.

프로세스 간 통신(IPC)방법으로는 파이프, 파일, 소켓, 공유메모리등이 있다.

멀티스레드와 다르게 프로세스끼리는 데이터를 공유하지 않고 있어 데이터를 주고받기 위한 방법이 IPC인 것이다. 크게 공유메모리 방식과 메시지 전달방식으로 나뉘는데 이 둘의 장단점과 차이점이 중요하다.

### IPC(Inter Process Communication)란?

프로세스는 각자 자신만의 독립적인 주소공간을 가지는데, 다른 프로세스가 이 주소공간을 참조하는 것을 허용하지 않는다. 그렇기 때문에 다른 프로세스와 데이터를 주고받을 수 없다. 이를 해결하고자 운영체제는 IPC기법을 통해 프로세스들 간에 통신을 가능하게 해준다.

### 공유 메모리(shared memory)

공유 메모리 방식에서는 프로세스들이 주소 공간의 일부를 공유한다. 공유한 메로리 영역에 읽기/쓰기를 통해서 통신을 수행한다.

프로세스가 공유 메모리 할당을 커널에 요청하면 커널은 해당 프로세스에 메모리 공간을 할당해준다.

공유메모리 영역이 구축된 이후에는 모든 접근이 일반적인 메모리 접근으로 취급되기 때문에 더이상 커널의 도움없이도 각 프로세스들이 해당 메모리 영역에 접근할 수 있다. 따라서 커널의 관여 없이 데이터를 통신할 수 있기 때문에 **IPC속도가 빠르다**는 장점이 있다.

공유 메모리 방식은 프로세스간의 통신을 수월하게 하지만 동시에 같은 메모리 위치에 접근하게 되면 **일관성문제가** 발생할 수 있다.

이에 대해서는 커널이 관여하지 않기 때문에 프로세스들끼리 직접 공유메모리 접근에 대한 동기화 문제를 책임져야한다.

![1](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/b348fdef-b1df-4471-a5ef-ab5170b4fcd2)

### 메시지 전달(message passing)

메시지 전달 방법은 통상 system call을 사용하여 구현된다. kernel을 통해 send(message)와 receive(message)라는 두 가지 연산을 제공받는다. 예를 들면, process1이 kernel로 message를 보내면 kernel이 process2에게 message를 보내주는 방식으로 동작한다.

메모리 공유보다는 속도가 느리지만, 충돌을 회피할 필요가 없기 때문에 적은양의 데이터를 교환하는 데 유용하다. 또, 구현하기가 쉽다는 장점이 있다.

대표적인 예시로는 pipe, socket, message queue등이 있다.

![2](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/84c16737-6c0d-41d3-9a3b-79b05de3de0f)

📌**핵심정리**

공유메모리와 메시지 전달 모델의 장단점

- 공유 메모리 모델은 초기에 공유 메모리 할당을 제외하면 커널의 관여 없이 통신할 수 있기 때문에 속도가 빠른 장점이 있다.
- 하지만 여러 프로세스가 동시에 접근하는 문제가 발생할 수 있어서 별도의 동기화 과정이 필요하다는 단점도 존재한다.
- 메시지 전달 모델은 커널을 통해서 데이터를 주고받기 대문에 통신속도가 느리다.
- 커널에서 제어를 해주기 때문에 안전하며 커널이 동기화를 제공해준다는 장점이 있다.

이 뒤에는 멀티프로세스와 멀티스레드 환경에서의 동기화 문제 해결법
