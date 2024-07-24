### 작성자 : 박민지

# REST
<img height="300" src="https://github.com/user-attachments/assets/7687ed6f-7218-4fbe-9b4a-622cef292233">

> Representational State Transfer의 약자로 `자원을 이름으로 구분`하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다

- REST는 네트워크상에서 클라이언트와 서버 사이의 통신 방식 중 하나다
- HTTP URI를 통해 자원을 명시한다
- HTTP Method(POST, GET, PUT, DELETE, PATCH)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용한다
<br>

#### [CRUD Operation]
- Create : 데이터 생성 (POST)
- Read : 데이터 조회 (GET)
- Update : 데이터 수정 (PUT, PATCH)
- Delete : 데이터 삭제 (DELETE)

###### *put은 전체 내용, patch는 부분 내용 수정을 의미한다
<br>

### Q. REST 구성 요소

1. **자원(Resource) : HTTP URI**
    - 모든 자원(이미지, 텍스트, DB..)에는 고유 ID가 존재하고 자원은 서버에 존재한다
2. **자원에 대한 행위 : HTTP Method**
    - HTTP 프로토콜의 Method를 그대로 사용한다
3. **행위의 내용(Representations) : HTTP Message Pay Load**
    - 클라이언트가 자원의 상태에 대한 조작을 요청하면 서버는 적절한 응답(Representations)를 보낸다

#### <ins>자원(Resource)의 표현(Representations)에 의한 상태 전달(state transfer)을 뜻한다</ins>
<br>

#### [URL과 URI]
`URI`
- Uniform Resource Identifier = **통합 자원 식별자**
    - Uniform : 자원을 식별하는 통일된 방식
    - Resource : URI로 식별 가능한 모든 종류의 자원
    - Identifier : 다른 항목과 구분하기 위해 필요한 정보
- URI는 **인터넷 상의 자원 자체를 식별**하는 고유한 문자열 시퀀스다


`URL`
- Uniform Resource Locator
- 네트워크상에서 **통합 자원의 위치를 나타내기 위한 규악**
- **식별자 + 위치**

###### *URI와 URL의 차이
- elancer.co.kr = URI
- https://elancer.co.kr = URL
- https://hstory0208.tistory.com/category/12 = URL을 포함한 URI

<br>

### Q. HTTP API와 REST API
#### HTTP API
- HTTP 형태로 웹과 통신하는 방법
- GET, POST, Message body
- HTTP를 사용해서 서로 정해둔 스펙으로 데이터를 주고 받으며 통신한다

#### REST API
- REST 아키텍쳐 스타일로 설계된 API
  - 제약 조건의 집합
- HTTP API에 4가지 제약 조건을 추가하며, 이를 완벽하게 지키면서 개발하는 것을 RESTful API
  - <details>
    <summary>자원의 식별</summary>
    <div markdown="1">
    접근하고자 하는 자원을 명시하고 그 자원을 식별할 수 있는 변하지 않는 식별자가 필요하다<br>
    <img width="500" src="https://github.com/user-attachments/assets/4bf78b9b-029f-4ec5-8102-fc1d319abd99"><br>
    △ 잘못된 예시 / rest를 지킨 예시 (변하지 않는 식별자 = id)
    </div>
    </details> 
  - <details>
    <summary>표현을 통한 자원에 대한 조작</summary>
    <div markdown="1">
      HTTP 메소드(표현)을 통해 HTTP 메세지에 해당 자원에 대해 어떤 조작을 하는지 명시한다<br>
      <img width="500" src="https://github.com/user-attachments/assets/28f2bf2d-afdb-4879-a127-080f57624b8b">
    </div>
    </details>
  - <details>
    <summary>자기서술적 메세지</summary>
    <div markdown="1">
        <ul>
            <li>각 메세지는 자신을 어떻게 처리해야 하는지에 대한 충분한 정보를 포함해야 한다</li>
            <li>서버나 클라이언트가 변경되더라도 오고가는 메세지는 언제나 해석 가능해야 한다</li>
        </ul>
    </div>
    </details>
  - <details>
    <summary>애플리케이션 상태의 엔진으로서의 하이퍼미디어</summary>
    <div markdown="1">
    <ul>
        <li>REST에서 애플리케이션 상태 엔진으로서의 하이퍼미디어 = 웹상의 리소스들이 가지는 링크를 따라 가는 것</li>
        <li>HTML처럼 하이퍼링크가 추가되어 다음에 어떤 API를 호출해야 하는지 해당 링크를 통해 받을 수 있어야 한다</li>
        <li>클라이언트는 하이퍼미디어 링크를 통해 애플리케이션의 상태를 전환할 수 있어야 한다</li>
    </ul>
    </div>
    </details>

#### 결론
- HTTP와 REST API는 거의 같은 의미로 사용된다
- 엄격하게 위의 내용을 모두 지켜야 REST API라고 할 수 있다

<br>

### Q. REST의 장단점
`" 웹의 장점을 최대한 활용할 수 있는 Client와 Server간 통신 방식 중 하나 "`

[장점]
- <details>
    <summary>HTTP 프로토콜 인프라를 그대로 사용하기 때문에 REST API 사용을 위한 별도의 인프라 구축이 필요없다</summary>
    <div markdown="1">
        웹에서 널리 사용되는 기술과 호환되며 기존 웹 인프라를 활용할 수 있다
    </div>
  </details>
- <details>
    <summary> HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다</summary>
    <div markdown="1">
        JSON, XML 같은 표준 포맷을 사용하여 데이터를 교환한다
    </div>
  </details>
- <details>
    <summary>RESPT API 메세지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다</summary>
    <div markdown="1">
        URI로 자원을 식별하고 HTTP Method로 해당 자원에 대한 행위를 정의하기 때문에 API 구조를 쉽게 이해할 수 있다
    </div>
  </details>
- <details>
    <summary>여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화 한다</summary>
    <div markdown="1">
        REST의 설계 원칙을 따르면, 시스템의 확장성과 유연성을 높이고 유지보수와 확장성을 쉽게 할 수 있다
    </div>
  </details>
- <details>
    <summary>서버와 클라이언트 역할을 명확하게 분리한다</summary>
    <div markdown="1">
        <ul>
            <li>클라이언트는 온전히 사용자와 관련된 처리를 담당한다</li>
            <li>서버는 해당 요청을 처리해 클라이언트에게 응답을 전달하는 역할만 담당한다</li>
            <li>클라이언트와 서버가 개발할 내용이 명확하게 구분되고 의존성이 줄어든다</li>
        </ul>
    </div>
  </details>

[단점]
- <details>
    <summary>표준자체가 존재하지 않아 정의가 필요하다</summary>
    <div markdown="1">
        <ul>
            <li>REST의 원칙과 제약 조건이 존재하지만, 특정한 구현에 대한 엄격한 표준이 존재하지 않다. 즉 공식적인 API 가이드라인이 없다</li>
            <li>REST가 지양하는 아키텍처 요구사항을 모두 만족할 경우를 RESTful이라고 한다</li>
            <li>ex) 자원 모델링 : '사용자'에 대한 자원을 표현할 때 '/users'를 사용할지 '/user_profiles'를 사용할지는 개발자의 판단에 따른다</li>
            <li>ex) 응답 상태 코드 : 사용자 요청이 잘못된 경우 '400 - Bad Request'를 사용할지, 더 구체적으로 '422 - Unprocessable Entity'를 사용할지 정의되어 있지 않다</li>
        </ul>
    </div>
  </details>
- 사용할 수 있는 메소드가 4개 뿐이다
- HTTP 메소드 형태가 제한적이다
- 구형 브라우저에서는 호환이 되지 않는다

<br>

### Q. REST API 설계
1. URI는 명사, 소문자를 사용한다
2. 마지막에 슬래시(/)를 포함하지 않는다
3. 언더바 대신 하이픈을 사용한다
4. 파일 확장자는 URI에 포함하지 않는다
```
❌ http://dev.tistory.com/users/photo.jpg
```
5. 행위를 포함하지 않는다
   - 행위는 Method를 사용하여 전달한다
```
❌ POST http://dev.tistory.com/users/post/1
⭕ http:///dev.tistory.com/users/1
```

<br>

### Q. REST API 원칙
> REST는 특정 기술이나 플랫폼에 얽매이지 않는 독립적인 언어이고 API를 빌드하는 방법을 정확히 지정하지 않는다. 하지만 6가지 아키텍처 제약 조건에 따라 유효한 REST API라고 부른다

#### 1. 인터페이스 일관성 (Uniform)
- URI로 지정한 리소스 종류와 상관없이 동일한 API 메소드를 갖는다
- 리소스가 JSON, XML, 음악파일 등 무엇이든 처리가 일관된 형태로 요청된다

#### 2. Stateless
- 각 요청 간 클라이언트 콘텍스트가 서버에 저장되지 않는다
- API는 단순히 들어오는 요청만 처리하기 때문에 서버에서 붎필요한 정보를 관리하지 않음으로써 구현이 단순해진다

#### 3. Cacheable
- HTTP 메서드를 사용하기 때문에 웹 인프라를 그대로 사용할 수 있다
- 동일한 URI에 대한 요청을 여러 번 했을 경우 URI 리소스를 매번 서버로 요청하지 않고 클라이언트의 HTTP 캐시에서 미리 가져온 정보를 반환한다

#### 4. Self-descriptivness
- 자체 표현 구조
- 주석처럼 부연 설명 없이 REST API의 메시지만 보고도 이해 가능하도록 설계한다

#### 5. Client-Server
- 서버는 API 제공, 클라이언트는 컨텍스트(세션, 로그인 정보..)를 직접 관리하여 역활이 명확히 구분된다
- 서버에서 개발할 내용이 명확해지고 의존성이 줄어든다

#### 6 Layered System
- 서버는 다중 계층으로 구성될 수 있어 로드 밸런싱, 암호화 계층을 추가해 구조상 유연성을 둘 수 있다
- proxy, gateway 등 네트워크 기반의 중간 매체를 사용할수 있다

###### *API Gateway : API 서버 앞단에서 모든 API 서버의 엔드포인트를 단일화 해주는 서버 (단일 진입점)
<br>

# RESTful
> REST라는 아키텍쳐를 구현하는 웹 서비스를 나타내는 것으로 REST 원리를 따르는 시스템이다

### Q. RESTful API 클라이언트 요청 구성 요소

#### [고유 리소스 식별자]
- 서버는 고유한 리소스 식별자로 각 리소스를 식별한다
- URL을 사용하여 리소스를 식별하고 리소스에 대한 경로를 지정한다
- URL은 요청 엔드포인트라고도 하며 클라이언트가 요구하는 사항을 서버에 명확히 지정한다

#### [메서드]
> HTTP 메서드는 리소스에 수행해야 하는 작업을 서버에 알려준다

1. GET
- **서버의 지정된 URL에 있는 리소스에 액세스한다**
- GET요청을 캐싱하고 RESTful API 요청에 파라미터를 넣어 전송하여 전송 전에 데이터를 필터링하도록 서버에 지시한다

2. POST
- **서버에 데이터를 전송한다**
- 요청과 함께 데이터 표현이 포함된다
- 동일한 POST 요청을 여러 번 전송하면 동일한 리소스를 여러 번 생성하는 부작용이 있다

3. PUT
- 서버의 기존 리소스를 업데이트한다
- POST와 달리 RESTful 웹 서비스에서 동일한 PUT 요청을 여러 번 전송해도 결과는 동일하다

4. DELETE
- 리소스를 제거한다
- DELETE 요청으로 서버 상태를 변경할 수 있지만, 사용자에게 적절한 인증이 없으면 요청은 실패한다

#### [HTTP 헤더]
> 요청 헤더는 클라이언트와 서버간에 교환되는 메타데이터다. 요청, 응답형식, 요청 상태 등의 정보를 제공한다

1. 데이터
- HTTP 메서드가 성공적으로 작동하기 위한 데이터가 포함될 수 있다

2. 파라미터
- URL 세부 정보를 지정하는 경로 파라미터
- 리소스에 대한 추가 정보를 요청하는 쿼리 파라미터
- 클라이언트를 빠르게 인증하는 쿠키 파라미터
<br>

### Q. 서버 응답 구성 요소

#### 1. 상태 표시줄
- 2XX : 성공
    - 200 : 일반 성공 응답
    - 201 : POST 메서드 성공 응답
- 3XX : URL 리디렉션
- 4XX, 5XX : 오류
    - 400 : 서버가 처리할 수 없는 잘못된 요청 (클라이언트의 요청이 부적절함)
    - 404 : 리소스를 찾을 수 없음

#### 2. 메시지 본문
- 응답 본문에는 리소스 표현을 포함한다
- 클라이언트는 데이터 작성 방식을 일반 텍스트로 정의하는 XML, JSON 형식으로 정보를 요청할 수 있다
- ex) {"name": "John", "age": 30}

#### 3. 헤더
- 서버, 인코딩, 날짜, 콘텐츠 유형 등의 정보를 포함한다

---------
### Q1. REST에서 지원하는 HTTP 메서드는 무엇입니까?
### Q2. PUT과 POST의 차이점은 무엇입니까?
### Q3. RESTful 웹 서비스의 단점은 무엇입니까?
1. 사용할 수 있는 메서드가 4가지 밖에 없다
- GET, POST, PUT, DELETE
 
2. 분산 환경에는 부적합하다
- point-to-point 통신 모델을 가정하기 때문에 둘 이상으로 상호작용하는 분산환경에 유용하지 않다

3. HTTP 통신 모델만 지원한다
- 자원을 HTTP URI로 포현하고 HTTP 프로토콜을 통해 요청과 응답을 정의한다

4. 보안, 정책 등 명확한 표준이 존재하지 않으며 RESTful을 완전히 만족하는 API를 만들기는 매우 까다롭다
