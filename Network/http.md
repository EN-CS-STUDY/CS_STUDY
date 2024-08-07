### 작성자 : 박민지
<br>

# HTTP
> - HTML과 같은 문서(HyperText)를 전송(Transfer)하기 위한 프로토콜(Protocol)
> - application layer
> - 기본 포트인 80번 포트에서 대기한다
> - 클라이언트가 TCP 80포트를 사용해 연결하면 서버의 요청에 응답하며 정보를 전송한다
<br>

### Q. HTTP 동작 방식
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/c5de2b2d-311a-4821-b741-7ecb43e426ce">

- client: 서버에게 요청을 보내는 리소스 사용자
- server: 클라이언트의 요청에 대한 응답을 제공하는 리소스 관리자
<br>

### HTTP Keep-Alive
- HTTP/1.0에서는 클라이언트가 서버에 요청을 보내면 서버는 응답을 보내고 연결을 끊음
- 이런 connectionless 방식은 매번 연결을 새로 맺어야 하므로 많은 시간과 자원이 소모되기 때문에 keep-alive가 있음
- 주요특징
  - TCP 연결 설정 및 해제에 드는 비용을 줄일 수 있음
  - TCP 연결을 매번 새로 시작하면, 연결을 설정하고 해제하는데 필요한 핸드셰이크와 헤더 데이터가 오버헤드를 유발 -> 이를 효과적으로 관리
  - TCP 연결의 초기 지연을 피할 수 있습니다. 연결을 유지하기 때문에, 데이터 전송 시작 시점의 지연이 줄어듦

### Q. HTTP의 보안 취약점 
#### 1. 도청이 가능하다
> 로그인/회원가입과 같이 비밀번호를 입력하는 중요한 경우에 HTTP 평문으로 통신한다면 어떨까? 로그인 패킷을 수집하는 것만으로도 통신을 도청해서 비밀번호를 그대로 볼 수 있다. 즉, 메세지의 의미를 쉽게 파악할 수 있다
-  http는 자신을 암호화하는 기능은 없다
- **텍스트 기반의 평문**으로 통신하기 때문에 네트워크에서 정보 탈취, 변조 위험이 있다
- 평문(암호화X)으로 통신하는 경우 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다
###### *평문: 암호화 되어있지 않고, 추가 처리 없이 사람이나 컴퓨터가 읽을 수 있는 모든 정보


#### 2. 위장이 가능하다
> IP주소나 포트에서 해당 웹서버에 대한 액세스 제한이 없기 때문에 요청이 오면 상대가 누구든 무언가의 응답을 반환한다.
- **통신 상대를 확인하지 않기 때문**에 위장된(허가되지 않은) 상대와 통신할 수 있다
- cf) HTTPS는 CA인증서를 통해 인증된 상대와 통신이 가능하다

##### ※문제점
1. request를 보낸 곳의 웹서버가 원래 의도한 response를 보내야 하는 웹서버인지 확인할 수 없다
2. response를 반환한 곳의 클라이언트가 원래 의도한 request를 보낸 클라이언트인지 확인할 수 없다
3. 통신중인 상대가 접근이 허가된 상대인지 알 수 없다
4. 어디에서 누가 request를 보냈는지 알 수 없다
5. 의미없는 request도 수신한다 (-> Dos공격을 방지할 수 없다)

#### 3. 변조가 가능하다
> 요청/응답이 발신된 후 상대가 수신하는 사이에 누군가에 의해 변조되더라도 이 사실을 알 수 없다. 즉, 공격자가 도중에 요청/응답을 빼앗아 변조할 수 있다.
- **완전성을 보장/증명하지 않는다**
- cf) HTTPS는 메세지 인증 코드(MAC), 전자 서명을 통해 변조를 방지한다
###### *완전성: 정보의 정확성 (서버 또는 클라이언트에서 수신한 내용이 송신측이 보낸 내용과 일치한다)

#### :loudspeaker: HTTP에는 암호화 구조가 없지만 SSL or TLS 프로토콜을 조합하여 통신 내용을 암호화할 수 있다
<br>

### Q. HTTP 보안을 위한 필요조건
1. 기밀성 : 데이터를 클라이언트와 서버끼리만 확인할 수 있도록 데이터를 암호화한다
2. 메시지 무결성 : 통신내용이 전송 도중 변경되지 않아야 한다
3. 종단점 인증 : 서버의 신분을 확인할 수 있는 인증 과정을 거쳐야 한다
#### :loudspeaker: 위의 조건을 만족시키는 프로토콜이 바로 `HTTPS`
<br>

# HTTPS
> - HyperText Transfer Protocol Secure
> - 인터넷 상에서 정보를 암호화하는 SSL프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약
> - **HTTP의 보안 취약점을 해결하기 위한 프로토콜 (HTTP + SSL)**
<br>

### Q. HTTPS는 새로운 프로토콜인가?
> - HTTP 통신 소켓을 SSL or TLS 프로토콜로 대체하는 것 뿐이다
> - HTTP는 TCP와 직접 통신했지만, HTTPS에서는 HTTP가 SSL과 통신하고 SSL이 TCP와 통신한다<br>
>   (layer를 하나 더 둔 것)
> - HTTP와 TCP는 다른 계층에서 동작하는 프로토콜로 HTTP는 TCP를 이용해 데이터를 전송하고, TCP는 데이터의 신뢰성과 정확성을 보장하는 관계다
###### * TCP - transfort layer / HTTP - application layer
<br>

### Q. HTTPS 통신 흐름
<img src="https://user-images.githubusercontent.com/33534771/75338777-a1d9bf80-58d2-11ea-9754-809110475c89.png" width="400"/>

1. 애플리케이션 서버(A)를 만드는 기업은 HTTPS적용을 위해 **공개키와 개인키를 만든다**
2. 신뢰할 수 있는 **CA기업을 선택**하고, **공개키 관리를 부탁**하며 계약한다
3. 계약이 완료된 CA기업은 CA기업만의 공개키와 개인키가 있다
4. **인증서**(CA기업명, 서버(A)의 공개키, 공개키 암호화 방법)를 만들고 CA기업의 개인키로 암호화해서 서버(A)에 제공한다
5. **서버(A)는 암호화된 인증서를 갖는다.** 이제 서버(A)는 서버의 공개키로 암호화된 HTTPS요청이 아닌 다른 요청이 오게 되면 암호화된 인증서를 클라이언트에게 건네준다
6. 만약 **클라이언트가 main.html파일을 요청**한다면, **HTTPS 요청이 아니기 때문에 클라이언트는 서버로부터 인증서**(CA기업이 서버(A)의 정보를 CA기업의 개인키로 암호화한)를 받게 된다
7. 세계적으로 신뢰할 수 있는 CA기업의 공개키는 브라우저가 이미 알고 있다. 때문에 브라우저는 CA기업 리스트를 탐색하며 인증서에 적힌 **CA기업이름을 찾아 공개키로 인증서를 해독한다** (즉, 신뢰할 수 있는 인증서인지 확인한다)
8. 해독한 뒤, 브라우저는 사이트 정보와 **서버(A)의 공개키를 얻는다**
9. **서버(A)와 통신할 때 서버의 공개키로 암호화해서 request를 날린다**

###### *비대칭키: 암복호화 시점에 서로 다른 키를 사용한다. (공개키 암/복호화 <-> 개인키 복/암호화)
<br>

### Q. HTTPS를 적용하면 100% 안전한 것일까?
- HTTPS를 지원한다고 해서 무조건 안전한 것은 아니다
- 신뢰할 수 없는 CA기업을 통해 인증서를 발급받을 수도 있다. 이때 브라우저는 알림을 띄운다(ex 안전하지 않은 사이트)
- 웹서버가 루트 권한을 탈취당한 경우 기밀 데이터 열람 권한이 넘어갈 수 있다 (중간자 공격)
<br>

### Q. SSL과 TLS Protocol

#### [SSL]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/60c51b0d-7b70-457f-8edb-705c0ee86207">

- Secure Socket Layer, 암호화 기반 인터넷 보안 프로토콜
- 대칭키 암호화, 공개키 암호화 방식을 모두 사용한다
- CA(Certificate Authority)라고 불리는 서드 파티로부터 서버와 클라이언트의 인증에 사용된다
- handshake를 통해 인증이 이루어지며 데이터에 디지털 서명을 통해 의도적으로 조작 여부를 확인한다
#### [TLS]
- Transport Layer Security
- TLS는 SSL의 업데이트 버전이다 (명칭만 다르다고 볼 수 있음)
<br>

### Q. SSL과 TLS의 차이점은 무엇인가?
- 현대의 HTTPS는 TLS를 기반으로 한 기술이다
- SSL과 TLS는 사실상 동일한 원리로 동작한다
- SSL은 1996년 3.0ver 이후 업데이트되지 않아 사라질 것으로 여겨지며 TLS를 권장하고 있다
<br>

### Q. SSL(TLS) Handshake
> HTTP에서 클라이언트와 서버간 통신 전, SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/352e5bfc-21d0-4cae-a9f0-4d26f972f651">

- 파란색 패킷(syn - syn ack - ack)
  - SSL handshake에 앞서 연결을 생성하기 위한 과정
  - HTTPS가 TCP 기반의 프로토콜이기 때문이다
- 노란색 패킷(SSL handshake)
  - 암호화 알고리즘(Cipher Suite)결정
  - 데이터를 암호화할 대칭키(비밀키) 전달

※진행 순서
1. 클라이언트가 서버에 'client hello' 메세지를 담아 보낸다
   - 암호화된 정보: 버전, 알고리즘, 압축 방식
2. 서버는 클라이언트가 보낸 암호 알고리즘과 압축 방식을 받는다
   - 'server hello(+ sessionID, CA인증서)'메세지로 응답한다
   - CA인증서: 공개키 정보가 담겨있다
3. `클라이언트`는 서버가 보낸 CA인증서가 유효한지 확인한다
4. CA인증서의 신뢰성이 확보되면 서버의 공개키로 대칭키를 생성한다
   - 앞으로의 통신에서 메세지 암호화에 사용한다
5. `서버`는 클라이언트의 인증서를 확인한 후 개인키로 복호화해 마스터키를 생성한다
6. `클라이언트`는 handshake 과정이 완료되었다는 finished 메세지를 서버에 보낸다
7. `서버`도 finished 메세지를 대칭키로 암호화해 보낸다
8. `클라이언트`는 해당 메세지를 대칭키로 복호화해 서로 통신이 가능한 사용자라는 것을 인치하고 앞으로 해당 대칭키로 데이터를 주고받는다.

*참고자료
https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TLS%20HandShake.md
*****

### [면접 질문]
### Q. HTTP vs HTTPS
<img width="400" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/5167ffd2-c364-4b26-8349-2393615b2182">

<br><br>

### Q. HTTP를 사용하는 REST API 서버에게 HTTPS를 사용하게 하기 위해서는 어떠한 절차를 거쳐야 할까?
1. SSL/TLS 인증서를 획득한다
   - 인증된 CA에서 발급받아야 한다
2. 웹 서버 설정을 변경한다
   - HTTPS를 활성화하고 사용할 포트(80 -> 443)를 지정한다
3. API 엔드포인트를 수정한다
   - HTTP접속시 HTTPS로 리다이렉트 하도록 설정해준다
<br>

### Q. HTTPS Handshake 과정에서는 왜 인증서를 사용하는 것 일까?
> handshake 과정에서 인증서는 관련된 당사자의 신뢰성 확립을 위해 중요한 역할을 한다

1. 보안 및 신원 확인 : 통신에 참여하는 서버(or 클라이언트)의 신원을 확인한다
2. 암호화 키 교환
3. 중간자 공격 방지 : 적절한 인증서 없이는 악의적인 행위자가 서버로 가장해 통신을 가로챌 수 있다. 따라서 인증서를 사용함으로써 의도한 서버와의 직접 통신을 보장해 공격을 방지한다
