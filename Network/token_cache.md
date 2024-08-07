### 2. 토큰이란?

암호화된 데이터로, 서버와 클라이언트 간 인증 및 권한 관리에 사용된다.

그러면 토큰을 왜 사용할까?

세션은 서버에 저장된다. 서버는 수많은 사용자의 세션을 저장하고 각각의 사용자에게 요청이 들어올 때마다 세션ID 확인작업을 한다. 문제는 세션ID를 저장하는 저장소 공간이 한정되어있다. 따라서 많은 사람들이 이용하면 이용할수록 서버에 과부하가 걸려 속도가 느려진다.

이를 위해 서버의 부담을 덜어주기 위해서 등장한 것이 세션/쿠키보다 보안성도 높고 서버에게도 효율적인 방식인 토큰이다.

토큰은 사용자의 인증정보를 암호화시킨것으로, 일종의 출입증 역할이다. 사용자가 최초 로그인 시 서버는 세션ID대신 용량차지가 적은 토큰을 발급해주고, 이후 사용자는 이 토큰을 가지고 다른 웹사이트들로 출입할 수 있게 된다.

토큰은 위조 방지장치가 있는 지폐와 같이 특정 수학적 원리가 적용되어 있어서 오직 서버만이 유효한 토큰을 발행할 수 있다.

**그러나 토큰 기반 인증시스템은?**

stateless 이 한단어로 정의할 수 있다. 상태유지를 하지 않기에 유저의 인증 정보를 서버나 세션에 담아두지 않는다.

따라서 서버의 부담이 감소하고, 보안성이 높다. 또한 여러 플랫폼이나 도메인에서 적용이 가능하다.

대표적인 토큰 인증 기반 세스템은 **jwt와 Oauth**가 있다.

### 2-1. 토큰은 어디에 담는가?

위에서 말했듯이 토큰은 유저 인증을 위한 효과적인 도구이다. 따라서 **유저를 인증할 수 있는 어떤 값을 토큰에 담아야한다**.

이때 HTTP의 Authorization header에 담는다.

![1](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/af2a2280-3a23-43e2-bec2-774a9858f53f)

header안에 authorization 키의 value값으로 인증 스키마를 담는다는 뜻이다. 이때, 인증 스키마는 2가지로 이루어진다.

- type
  - 인증 방식, 대표적으로 **Bearer**
- credential
  - 인증 정보, 즉 우리가 보내고자 하는 유저의 인증 정보, 대표적으로 **access-token**

### 2-2. 토큰 인증 동작방식

![2](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/adbc0a62-9a41-43cd-85d7-2f4ea9b572be)

1. 클라이언트 로그인
2. 서버는 사용자를 확인하고 acess token을 발급
3. 발급 후 사용자에게 응답
4. 사용자는 access token을 받아 쿠키와 같은 곳에 저장
5. 인증이 필요한 요청마다 토큰을 헤더에 실어서 전송
6. 서버는 해당 토큰을 검증, 적절한 토큰인 경우 사용자에게 알맞는 데이터를 전송

### 2-3. 토큰 기반 인증 방식 장단점

- 장점
  - 세션/쿠키와 달리 별도의 저장소 관리가 필요없고 **검증만** 하면 된다
  - **토큰 자체에 정보가 포**함되어 있어 네트워킹 상태를 유지하지 않아도 가능
  - ouath와 같은 서비스들고 토큰 기반으로 진행되기 때문에 관련 기능을 **확장하기 쉽다**
- 단점
  - 토큰의 경우도 세션 ID처럼 해커인지 사용자인지 구분할 수 없다.(따라서 HTTPS프로토콜을 사용하고 refresh token을 사용해 토큰의 유효기간을 짧게 한다)
  - 토큰의 길이가 길어 인증요청이 많아질수록 네트워크 부하가 심해질 수 있다.
  - 여러기기에서의 로그인을 제한하기 위해 서버는 강제로 기기로그아웃을 시킬 수 있어야하지만 토큰 방식에서는 불가능하다.
  - 특정 토큰을 강제로 만료시키기 어렵다.
  - 따라서 맨위의 단점처럼 refreshToken을 사용해 만료시간을 조절하는 것이 중요하다.

### 2-4. 토큰의 종류

1. JWT
2. OAuth
3. SAML
4. OIDC

### 3. JWT란?

JWT는 Json Web Token는 Json 포맷을 통해 사용자에 대한 속성을 저장하는 web token이다. 정보가 디지털서명되어 있기에 신뢰할 수 있다.

![3](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/9fd13c1a-0232-4c2c-9876-2d2c1dbdb63e)

위의 사진과 같이 Header, Payload, Signature 3가지로 구분이 가능하다.

토큰을 만들기 위해서는이 3가지 요소인 Header, Payload, Verify Signature가 필요하다.

- Header
  - 위 3가지 정보를 암호화할 방식(alg), 타입(type) 등
- Payload
  - 서버에 보낼 데이터. 일반적으로 유저의 고유 ID값, 유효기간. 사용자의 정보가 이곳에 저장되어 있기 때문에 서버 자원에 접근하지 않더라도 인증이나 인가가 가능해진다.
- Signature
  - Base64방식으로 인코딩한 Header, Payload 그리고 SECRET KEY를 더한 암호화

Header, Payload는 인코딩될뿐(16진수로 변경), 따로 암호화 되지 않는다. 따라서 JWT토큰에서는 Header, Payload는 누구나 디코딩하여 확인할 수 있다.(Payload에 비밀번호 같은 민감한 정보가 포함 될시 노출될 수도 있다.)

하지만 Signature는 SECRET KEY를 알지 못하면 복호화할 수 없다.

### 3-1. JWT의 장/단점

- 장점
  - JSON형식을 사용하기 때문에 크기가 작아 전달에 용이
  - header와 payload를 통해 signature를 생성하므로 데이터 위변조를 막을 수 있음
  - refresh token을 통해 토큰 탈취의 위험성 방지 가능
- 단점
  - 정보가 많아질수록 길이가 증가해 네트워크에 부하
  - payload에 담기는 데이터들은 암호화가 아닌 인코딩과정이기에 중요한 정보를 담을 수 없음
  - 서버자원을 아얘 사용하지 않는 것이 아니다. refresh token의 무효화나 access token이 만료가 되었을 때 갱신을 하기 위해 서버자원을 사용한다. 하지만 매 인증이나 인가를 요청해 서버 자원을 사용하는 session보다는 사용양이 훨씬 적다.
  - 토큰은 클라이언트가 가지고 있기에 탈취를 당하게 될 경우 보안에 취약해진다.

**Refresh Token**

Access Token(JWT)를 통한 인증 방식의 문제는 제 3자에게 탈취당할 경우 보안에 취약하다는 점이다. 유효기간이 짧은 Token의 경우 그만큼 사용자는 로그인을 자주해서 새롭게 Token을 발급받아야 하므로 불편하다. 그러나 유효기간을 늘리자면, 토큰을 탈취당했을 때 보안에 더 취약해진다. "그러면 유효기간을 짧게 하면서 더 좋은 방법은 없을까?" 라는 고민에 의해 탄생하게 된 것이 Refresh Token이다.

Refresh Token은 Access Token과 똑같은 형태의 JWT이다. 처음에 로그인을 완료 했을때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 발급해주는 열쇠가 된다.

Access Token은 탈취 당하면 정보가 유출되는건 동일하다. 다만 유효기간이 짧기에 조금더 안전하다는 뜻이다. Refresh Token의 유효기간이 만료 됐다면, 사용자는 새로 로그인 해야한다. Refresh Token도 탈취될 가능성이 있기때문에 적절한 유효기간 설정이 필요하다.(보통 2주)

![4](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/c5914505-75dd-4c26-8bba-3db9a4699ffd)

Q.

### 1. 캐시란?

데이터를 빠르게 저장하고 불러오기 위해 사용되는 임시 저장소이다.

![5](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/cdeccc44-9d42-4683-978a-44d858be339a)

### 1-1. 캐시의 장점

사용자들은 다양한 플랫폼을 통해 텍스트, 이미지, 동영상 등 대량의 데이터를 서버로부터 전송받는다. 카톡보다 유튜브가 배터리와 데이터 소모량이 빠른 것처럼, 데이터 전송에는 시간뿐만 아니라 비용도 소모된다.

그러나 한번 전송 받은 데이터는 저장해놨다가 다시 사용할 때 꺼내 쓴다면 반복적으로 요청할 필요가 없다. 이렇든 콘텐츠를 반복적으로 사용할 때 데이터 사용량을 줄이면서 동시에 빠르게 이용할 수 있는 방법이 캐시이다.

캐시에 데이터를 미리 복사해놓으면, 계산이나 접근 시간없이 더 빠른 속도로 데이터에 접근이 가능하다. 따라서 **반복적으로 사용하는 컨텐츠를 빠르게 이용**할 수 있고 **데이터 사용량도 줄일 수 있다.**

흔히 접할 수 있는 캐시는 브라우저 캐시이다. 캐시 덕분에 사용자는 같은 사이트를 다시 방문하거나 시청하던 동영상을 멈추고 다시 시청할 때 로딩없이 콘텐츠를 이용할 수 있다.

### 1-2. 캐시의 단점

캐시는 비싸다. 메모리 저장공간은 속도가 빠를수록 용량이 작고, 가격이 높다.

따라서 저용량이라면 조금씩 빠르게 사용할 때 용이한 방법이다.

### 1-3. 쿠키/세션와 캐시의 차이

사실 정보를 저장하여 재활용 하는 같은 일을 수행한다. 하지만 **쿠키/세션은 사용자의 인증을 돕는 것이 주요 목적**이고, **캐시는 데이터의 전송량을 줄이고 서비스 이용 속도를 높이는 것이 목적**이다.

---

다음시간 - 캐싱의 종류, redism CDN
