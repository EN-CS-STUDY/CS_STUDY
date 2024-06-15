### 작성자 : 조준희

## OSI(Open Systems Interconnection) 7계층

<img src = "https://private-user-images.githubusercontent.com/48996701/339950830-be76c356-1fd8-4ca6-91b8-1d78b70891ef.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTg0MjMxNzYsIm5iZiI6MTcxODQyMjg3NiwicGF0aCI6Ii80ODk5NjcwMS8zMzk5NTA4MzAtYmU3NmMzNTYtMWZkOC00Y2E2LTkxYjgtMWQ3OGI3MDg5MWVmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA2MTUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNjE1VDAzNDExNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWNlODBhNjEzZjBiMDNhMGY4YWI4Y2U4ZDRhZWE3NjNiMjFjODZlODIwYTNjOTU1MDk5OGExODk2ODQ5NzRlMmImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.eHif-bEKSwoSJ10EsalL9MekybVe34ZSxtZWDxl8dq8"/>

### OSI 7계층을 나누는 이유
- OSI 7계층은 네트워크 통신을 7단계로 나눈 것이다.

<br/>

1. 물리 계층(Physical Layer)
   - 단지 데이터를 전기적인 신호로 변환해서 주고받는 기능을 진행하는 공간
   - 예시 : 리피터, 케이블, 허브 등

<br/>

2. 데이트 링크 계층(Data Link Layer)
   - 물리 계층에서 송수신되는 데이터의 오류와 흐름을 관리하는 계층
   - Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 에러검출, 재전송, 흐름제어를 진행한다.
   - 예시 : 브릿지, 스위치

<br/>

3. 네트워크 계층(Network Layer)
   - 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다.
   - 라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.
   - 예시 : 라우터, IP

<br/>

4. 전송 계층(Transport Layer)
   - TCP와 UDP 프로토콜을 통해 통신을 활성화한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다.
   - TCP : 신뢰성, 연결지향적 
   - UDP : 비신뢰성, 비연결성, 실시간성이 필요한 서비스에 사용
   - 예시 : TCP, UDP

<br/>

5. 세션 계층(Session Layer)
   - 데이터가 통신하기 위한 논리적 연결을 담당한다. 
   - TCP/IP 세션을 만들고 없애는 책임을 지니고 있다.
   - 예시 : API, Socket

<br/>

6. 표현 계층(Presentation Layer)
    - 데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 담당한다.
    - 파일 인코딩, 명령어를 포장, 압축, 암호화한다.
    - 예시 : JPEG, MPEG, SSL

<br/>

7. 응용 계층(Application Layer)
   - 최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.
   - 사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.
   - 예시 : HTTP, FTP, SMTP, DNS


## TCP/IP 4계층 모델

1. 네트워크 인터페이스 계층 (Network Interface Layer)
   - 데이터의 물리적 전송 및 연결 
   - OSI의 물리 계층과 데이터 링크 계층에 해당
   - 예시 : 이더넷, ARP

<br/>

2. 인터넷 계층 (Internet Layer):
   - 데이터 패킷이 목적지까지 이동할 경로를 결정.
   - OSI의 네트워크 계층에 해당.
   - 예시: IP, ICMP

<br/>

3. 전송 계층 (Transport Layer):
    - 데이터 전송의 신뢰성과 흐름을 제어.
    - OSI의 전송 계층에 해당.
    - 예시: TCP, UDP

<br/>

4. 응용 계층 (Application Layer):
   - 응용 프로그램이 네트워크 서비스를 이용.
   - OSI의 세션, 표현, 응용 계층에 해당.
   - 예시: HTTP, FTP, SMTP

## TCP/IP 4계층과의 비교

<img src="https://github.com/devSquad-study/2023-CS-Study/raw/main/Network/img/network_osi_7_layer_02.png"/>
<br/>
출처 : <a href = "https://github.com/devSquad-study/2023-CS-Study/blob/main/Network/network_osi_7_layer.md">Ready-For-Tech-Interview Readme</a>

### 공통점 
1. 계층별 역할
   - 캡슐화, 프로토콜 사용.
   - 계층간 역할을 정의.
2. 통신 역할
   - 다중화, 역다중화
   - 페이로드 전송기능

### 차이점
1. OSI 모델을 역할을 기반으로 각 계층을 구성하고, TCP/IP 모델을 프로토콜의 집합을 기반으로 구성된다.
2. 전체적인 통신전반에 대한 표준화 방식이 OSI 모델이라면 TCP/IP 모델은 데이터 전송에 특화되어있다.