### 작성자 : 박민지

# REST
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

# RESTful
> REST라는 아키텍쳐를 구현하는 웹 서비스를 나타내는 것으로 REST 원리를 따르는 시스템이다
