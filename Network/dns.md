### 작성자 : 박민지
<br>

# DNS
> `Domain Name System`<br>
> **www.google.co.kr (host name)** 을 입력하면 쉽게 구글에 접속할 수 있다. 하지만 실제 웹서버에 접속하기 위해서는 이런 문자열 주소가 아니라 컴퓨터가 알아들을 수 있는 IP주소가 필요하다. 그래서 **DNS** 시스템을 도입해 우리가 사용하는 **문자열의 인터넷 주소를 IP주소로 변환**하여 컴퓨터가 처리할 수 있도록 한다.

*host name : 인터넷 호스트에 대한 하나의 식별자(사람의 입장)
*IP address : 컴퓨터에서 네트워크 장치들이 서로를 인식하고 통신하기 위해 사용하는 특수 번호

### Q. www.google.co.kr 을 검색하면 무슨 일이 일어날까?
[도메인 주소가 IP로 변환되는 과정]

<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/602f791d-c576-44ee-b07a-955f7240e724">
<br>

1. 사용자가 웹브라우저 검색창에 `www.google.co.kr`을 입력한다 
2. 도메인 주소에 상응하는 IP주소를 찾기 위해 `캐싱된 DNS 기록을 확인`한다
   - 브라우저 캐시 -> 로컬 캐시(OS 캐시) -> 라우터 캐시 -> ISP캐시 순으로 확인한다
   - 사용자가 해당 도메인을 검색하면 바로 제공 가능하다 (없는 경우 다음 단계 진행)
3. ISP의 DNS 서버는 IP주소를 찾기 위해 `DNS query`를 시작한다
   - DNS 쿼리는 IP주소를 찾을 때까지 반복한다 (recursive search)
4. DNS는 웹브라우저가 요청한 사이트의 `IP주소를 응답`한다
5. IP주소를 받으면 `TCP 통신`을 시작한다 (일반적)
   - 3-way-handshake
6. HTTP 프로토콜로 요청한다
   - TCP 연결에 성공하면 브라우저는 `GET요청`을 통해 www.google.co.kr의 웹페이지를 요청한다
7. `WAS와 DB`에서 우선 웹페이지 작업을 처리한다
   - 웹서버 혼자서 모든 로직을 처리하고 데이터를 관리하게되면 서버 과부하 위험이 있다
   - 서버의 일을 돕는 조력자의 역할이 바로 `WAS`
8. WAS에서의 작업 처리 결과를 웹서버로 전송한다
9. 웹서버는 웹브라우저에게 html 문서 결과를 응답한다
10. 웹브라우저는 html 컨텐츠를 출력하여 사용자에게 제공한다 

##### [2번 단계 추가 설명]
>과거에는 컴퓨터마다 host.txt 파일을 가지고 있어 모든 컴퓨터의 host name과 IP주소가 저장되어 있었다. 하지만 90년대 초반 web 서비스 사용자의 폭발적인 증가로 인터넷에 연결된 host 숫자가 크게 늘어났다. 이로인해 네트워크 트래픽이 증가하고 host name을 짓기 어려워지는 문제가 생겨나면서 `분산 DB`를 이용하게 되었다.
<br>


### Q. DNS 조회 과정
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/413bff35-6259-428b-b467-1ec3179ac59f">

> - 브라우저가 요청한 IP주소를 확인하는 과정이다
> - **DNS는 Domain name과 IP address를 매핑해주는 서버다**
> - DNS query : [`Root` DNS] -> [`.com` DNS] -> [`.naver` DNS] -> [`.www` DNS]

1. **Local DNS**에 요청한다
   - 'domain IP 주소 알고 있니?'
   - 캐시 미스가 발생하면 다음 단계 진행(DNS 서버 요청)
2. **Root DNS** 서버에 요청한다
   - 'TLD(top-level domain, 최상위 DNS server)가 알고있을거야'
3. **Authoritative DNS**(책임) 서버에 요청한다
   - 해당 IP주소를 알려준다
4. local dns는 받아온 정보를 사용자에게 알려준다 (최종)
5. 자신의 dns record에 저장하여 후에 같은 요청이 들어오면 신속한 처리가 가능하도록 한다

###### *DNS record : dns에 받은 요청을 어떻게 처리할 것인지에 대한 정보

##### [왜 재귀적으로 조회할까?]
- 도메인의 계층 구조에 따라 DNS 서버도 계층화 되어있기 때문이다
- 도메인의 가장 최상단(.com, .kr ...)을 담당하는 Root DNS 서버는 전 세계 13개 뿐이다

##### [DNS query 조회 이후]
- 웹 서버의 IP주소를 알게 되었다
- HTTP request를 위해 TCP 통신을 연결한다 (3-way-handshaking)
- TCP 연결 성공시 socket을 통해 HTTP request를 보내고, 이에 대한 response로 웹 페이지 정보를 받아 사용자의 PC로 출력한다
<br>

### Q. DNS 서버를 사용하는 이유
- IP 주소를 외울 필요 없이 기억하기 쉬운 domain name(문자 주소)을 사용할 수 있다
- 사용자가 새로운 IP주소를 기억할 필요 없이 웹사이트가 IP 주소를 변경할 수 있다
  - ex) www.naver.com 의 IP주소가 변경되어도 사용자는 똑같은 www.naver.com 주소로 서비스를 이용할 수 있다

*loopback IP : 본인 PC의 IP(localhost)로 DNS를 통하지 않고도 바로 자신의 PC 연결이 가능하다
