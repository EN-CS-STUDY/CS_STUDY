### 작성자 : 박민지

### TCP
---
#### Transmission Control Protocol (전송 제어 프로토콜)
- 애플리케이션 사이에서 안전하게 데이터를 통신하는 규약
- 4번째 계층인 전송 계층(Transport Layer)에서 사용
- `연결 지향 프로토콜`이기 때문에 TCP는 통신을 수행하기 전, 먼저 두 컴퓨터 간에 세션이 존재하는지 확인해야 함
<br>

### TCP HandShake
---
#### TCP 에서 연결 설정(connection establishment)은 `3-way handshake`를 통해 이루어진다
- 데이터의 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션(=연결)을 수립하는 과정을 의미한다
- 양쪽 모두 데이터 전송 준비가 완료되었음을 보장한다
- 총 3 단계로 이루어진다
  - connection setup (3-way handshake)
  - data transfer
  - connection termination (4-way-handshake)
- TCP 통신은 `양방향성 연결`이기 때문에 클라이언트의 요청, 서버의 허가, 클라이언트의 응답 과정까지 총 3번의 handshaking 과정이 필요하다
###### *3-way handshake는 가상의 연결 통로를 설정하는 것 뿐, 실제 데이터를 주고 받지는 않는다
###### *각 호스트는 독립적으로 자신의 cwnd를 관리하고, 데이터를 전송하는 과정에서 혼잡 제어 알고리즘을 통해 조절한다

<br>

#### [1 단계] 3-way handshake를 통해 동기화를 진행한다
<img width="500" alt="image" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/a826a5dd-a3be-4192-9373-c4afccdddecb">

> 1. Client > Server : TCP SYN<br>
>    (연결 요청) 클라이언트가 서버에게 SYN 패킷을 보냄 (sequence)
> 2. Server > Client : TCP SYN ACK<br>
>    (연결 허가) 서버가 SYN-ACK(sequence + 1)로 응답
> 3. Client > Server : TCP ACK<br>
>    (허가 응답) 클라이언트가 서버에게 ACK(squence + 1)를 보내면 연결이 이루어짐
>
>    ###### *SEQ: 송신 측에서 데이터를 세그먼트로 나눌 때 각 세그먼트의 시작 위치를 식별하기 위해 사용 (ACK)
>    ###### *ACK: 송신 측으로부터 받은 데이터의 바로 다음에 기대되는 데이터의 순서 번호
<br>

#### [2 단계] 데이터 통신
#### 네트워크에서는 데이터를 통신할 때 여러 조각으로 나누어 각 조각을 따로 전달한 뒤 합친다
> 1. 데이터 스트림에서 받은 데이터를 일정 단위로 분할한다
> 2. 분할된 데이터 단위에 TCP 헤더를 붙여서 `TCP 세그먼트`를 생성한다
> 3. TCP 세그먼트를 `IP 데이터그램`으로 변환한다
> 4. IP 데이터그램을 수신 애플리케이션에 보낸다
>    
>    ###### *TCP segment(TCP header + data): 데이터 전송 단위, 수신 애플리케이션에서 데이터를 안전하고 정확하게 원상복구하도록 함
>    ###### *IP datagram: 인터넷 통신에 사용되는 데이터 패킷
<br>

#### [3 단계] 4-way-handshake
- 안전하게 통신을 종료한다
<br>

### 신뢰성을 보장하는 방법
---
#### 1. 흐름 제어 (Flow Control)
- `데이터 양을 조절한다`
- 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
  - 수신측의 처리 능력을 초과하지 않도록 보장
- " receiver가 sender에게 현재 자신의 상태를 feedback 한다 "
  - 수신측은 자신의 버퍼 용량, 처리 속도를 송신측에 알려주어 송신측에서 데이터를 보내는 속도를 조절
  - overflow 상황에서 발생하는 데이터 손실을 방지하고 안정적인 연결을 수립

#### [Stop and Wait]
#### 매번 전송한 패킷에 대한 확인 응답을 받아야 그 다음 패킷을 전송할 수 있다
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/5a0518ea-3f44-4ef6-91f0-b549029615a9"/>

- 장점
  - 구현이 간단하고 오류 검출 및 복구가 쉽다
- 단점
  - 매번 한개의 세그먼트만 전송 가능하다 (전송 효율↓)
  - 중간에 신호가 유실되거나 데이터 전송이 지연되어서 time-out이 발생하게 되면 손실된 세그먼트를 재전송 해야 한다 (중복된 프레임 발생)

#### [Sliding Window]
#### 수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답(ACK) 없이 패킷을 전송할 수 있게 하여 데이터 흐름을 동적으로 조절한다
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/5c6c6ece-f202-4f15-bfdd-306e9e98b62a"/>

- 슬라이딩 윈도우 수행 과정
  - 1번에 대한 ACK가 수신측으로 부터 송신측에 전달되면서 2,3에 대한 ACK만 기다리면서 자리 하나가 남게 된다 (window-size = 3)
  - 자리가 하나가 남아 다음 프레임인 4를 전달할 수 있게 된다
- 장점
  - 송신측에서는 ACK 프레임을 수신하지 않더라도 여러 개의 프레임을 연속적으로 전송할 수 있다
  - 전송 속도가 빠르다
- 단점
  - 구현 복잡성 증가
  - 데이터 손실 시 재전송 해야할 데이터가 많을 수 있다
<br>

#### 2. 혼잡 제어 (Congestion Control)
- `네트워크 혼잡에 대처한다`
- 혼잡 == 네트워크 내에 패킷의 수가 과도하게 증가하는 현상
- 처리 속도보다 많은 양의 데이터를 너무 빠르게 전송하여 발생하는 네트워크 혼잡 현상
- 한 라우터에 데이터가 몰릴 경우 데이터를 모두 처리할 수 없게 되고 오버플로우로 데이터 손실이 발생할 수 있기 때문에 `데이터 전송 속도를 강제로 줄임` (송신측 제어)

#### 1) 혼잡 회피 방식(Congestion Avoidance)
#### [AIMD(Additive Increase / Multiplicative Decrease)]
#### 합 증가 - 곱 감소 알고리즘: 더해 나갈때는 증가시키고, 반으로 줄여 감소시킨다
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/58029ca9-4727-4603-8749-24a70ea030f3"/>

> 1. 송신자는 처음에 하나의 패킷을 전송한다
> 2. 전송된 패킷이 문제없이 도착하는 것을 확인 (= 네트워크의 혼잡 전)<br>
>    -> `윈도우 사이즈를 하나 증가`시켜 두개를 보낸다
> 3. 위의 방식으로 하나씩 윈도우 사이즈를 늘려나간다
> 4. 패킷이 전송되지 않거나 TIME_OUT 이 발생 (= 네트워크의 혼잡 상태)<br>
>    -> 늘려놨던 `윈도우 사이즈를 절반`으로 줄인다

- 장점
  - 윈도우 크기를 증가시켜 네트워크의 대역폭을 최대한 활용할 수 있다
  - 여러 사용자 간에 `공평한 대역폭 분배`를 유지한다 (시간이 흐르면 평형 상태로 수렴)
- 단점
  - 초기 네트워크의 높은 대역폭을 사용하지 못한다
  - 혼잡상황을 미리 감지하지 못해 혼잡이 발생한 후에야 대역폭을 조절한다 (대응 시간↑)
 
#### [Slow Start]
#### 초기 데이터 전송에서 `네트워크의 대역폭을 탐색`하고 `최대 전송 속도`를 결정하는 알고리즘
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/648820d9-bbb9-4fdb-96d0-b5a21ff7e30b"/>

###### *cwnd(congestion window): 현재 네트워크에서 안정적으로 전송될 수 있는 데이터의 양
###### *RTO(Retransmission Timeout): 재전송 타임아웃, 데이터 세그먼트가 송신되고 확인 응답을 받기까지 기다리는 시간

- 혼잡 회피 상태가 될 때 까지 cwnd 증가 방식을 지수 증가 방식으로 한다
- 시작부터 빠르게 윈도우를 증가 시키고 특정 시기가 오면 윈도우를 확 줄인다
- 이름과 다르게 `지수 형태로 빠르게 증가`한다
- 제약 없이 처음부터 최대로 전송하는 UDP보다 상대적으로 느리게 경로를 확인하며 전송 속도를 높인다

> 1. 패킷이 문제없이 도착하면 각각의 `ACK 패킷마다 window size를 1씩 늘려준다`
> 2. 한 주기가 지나면 window size가 2배로 된다 (전송속도가 지수 함수 꼴로 증가)
> 3. `혼잡 현상이 발생하면 window size를 1`로 떨어뜨린다
> 4. 혼잡 현상이 발생했던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가, 그 이후부터는 완만하게 1씩 증가시킨다

- 장점
  - 한번 혼잡 현상이 발생하고 나면 네트워크의 `수용량 예상이 가능`하다
- 단점
  - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없다 

#### [Fast Retransmit]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/0c055b3b-a9ed-48db-8d39-4aab3af68aa1"/>

> 1. 수신자에게 세그먼트로 분할된 내용들이 순서대로 도착하지 않은 경우, 순서대로(정상적으로) 마지막에 도착한 패킷의 다음 패킷 순번을 ACK에 보낸다
> 2. 중간에 패킷 하나가 손실되면 송신측에서는 순번이 중복된 ACK 패킷을 받게 된다
> 3. 2번이 `3번 중복(3 ACK Duplicated)되면 오류가 난 패킷을 재전송`하게 된다

- 장점
  - 설정한 time out이 지나지 않아도 `3번의 중복된 ACK패킷을 받게되면 해당 패킷을 바로 재전송` 할 수 있다 (`빠른 재전송`)
  - `패킷의 순서를 유지`해 보다 신뢰성있는 데이터 송수신을 할 수 있다
<br>

#### 2) 혼잡 제어 정책
> ###### - TCP 회피 방식들을 활용
> ###### - AIMD, Slow Start를 적절히 섞어 사용하되 네트워크 혼잡 발생시 대처 방식에 따라 나뉨

#### Tahoe vs Reno
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/529095e0-a253-4414-8760-a68163f49844">

#### [Tahoe]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/3ebca775-fcd3-46cf-829e-6bb9e3ef74aa">

> 1. TCP connection
> 2. cwnd = 1에서 시작 (네트워크 혼잡 상황 조사)
> 3. 이후 지수형태로 증가 (대역폭 활용)
> 4. loss 발생
>    - ssthresh = cwnd / 2로 설정
>    - `cwnd = 1`에서 다시 시작 (`slow start`)
> 6. cwnd가 지수형태로 증가하다가 `ssthresh값에 다시 도달하게 되면 1씩 증가`

- algorithm: slow start, congestion avoidance
- fast retransmit, 즉 time out 전에라도 3개의 중복된 ACK를 받으면 위의 혼잡 제어 방식 적용
*time out: 패킷 응답이 없음
*fast retransmit: time out 전 `3 duplicated ACK`, 패킷 전송은 문제가 없음

#### [Reno]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/0626d5ae-5335-4117-894b-17cf9b5681d8">

> 1. TCP 연결
> 2. cwnd = 1에서 시작 (이후 지수형태로 증가)
> 3. loss 발생
>    - time out의 경우
>      - Tahoe와 동일한 방식으로 동작
>    - fast retransmit의 경우
>      - ssthresh = cwnd / 2
>      - `cwnd /= 2`에서 다시 시작 -> `fast recovery`<br>
>        (: time out처럼 심각한 상황이 아니라고 판단)
> 4. 새로운 ACK가 도달할때까지 기다림
>    - 새로운 ACK가 도달하면 congestion avoidance 실행
>    - cwnd가 1씩 증가
<br>

#### 3. 오류 제어
- `데이터 유실, 잘못된 데이터 수신을 대처한다`
- (수신자) 훼손된 패킷, 중복 수신된 패킷 확인 후 폐기
- (수신자) 분실된 패킷이 도착할때까지 순서에 맞지 않는 세그먼트를 버퍼에 저장
- (송신자) 훼손되거나 손실된 패킷은 재전송을 통해 오류 복구

> - 검사합(Check Sum): 세그먼트내에 있는 검사합 필드를 통해 패킷이 훼손되었는지 확인한다.
> - 확인응답(Acknowledgement): 세그먼트의 수신을 알려주기 위해 확인 응답을 사용한다.
> - 타임아웃(Time-out): 송신 TCP는 연결당 재전송 타임아웃(RTO, retransmission time-out) 타이머를 사용하여 패킷의 재전송 시기 결정

#### [Go Back N]
<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/97e67b6f-5113-46ec-bea8-88ffc78562b9">

- cumulative ACK를 사용하여 윈도우를 관리하는 방식이다
- 정상적으로 전송된 가장 마지막 패킷에 대한 ACK를 다음 패킷을 받을 때마다 계속 전송한다
- 유실되거나 잘못된 패킷 이후 수신되는 모든 패킷을 버린다

<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/b372de36-360a-4b1f-89d6-8ff001319236">

1. 설정된 window size = 4, [0 1 2 3] 4개의 패킷을 차례로 전송한다
2. [0 1]에 대한 ACK를 확인한 후 다음 데이터인 [4 5]를 전송한다 (window = [2 3 4 5])
3. 2번 패킷에 대한 time out이 발생한다
4. [4 5]에 대한 ACK가 모두 버려지고, 2번 패킷부터 다시 재전송한다 ([2 3 4 5] 데이터 전송 시작)
