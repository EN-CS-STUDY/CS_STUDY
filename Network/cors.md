### CORS란?

`Cross-Origin-Resource-Sharing` 의 줄임말이다. 해석하면 교차 출처 자원 공유이다. CORS란, 웹 페이지가 다른 도메인의 리소스에 접근할 수 있도록 하는 방식이며, 보안상의 이유로 웹 브라우저는 같은 출처에서만 리소스를 불러올 수 있는 동일 출처 정책(Same-Origin Policy)를 따른다.

Origin이란 출처를 의미하며, Protocol + Host + Port를 합친 것을 말한다. Origin이 같으면 CORS에러는 발생하지 않는다. 개발자도구에서 location.origin을 치면 해당 사이트의 origin을 확인할 수 있다. HTTP는 포트번호가 80번, HTTPS는 포트번호가 443번으로 정해져 있어 생략해도 된다.

![1](https://github.com/user-attachments/assets/e9068cc1-d872-450d-b753-9b730e73d318)

브라우저간의 데이터를 주고받는 과정에서, 도메인 이름이 서로 다른 사이트간의 API요청을 할 때, 공유를 설정하지 않았다면 CORS에러가 발생한다.

프론트엔드 서버를 만들어서 백엔드 서버와 통신할 때 주로 CORS 에러를 마주친다. 오리진은 프로토콜과 호스트 이름, 포트의 조합을 말한다. 예를들어 http://test.com:3306/test 에서 오리진 즉, 출처는 http://test.com:3306을 뜻한다.

브라우저 환경에서만 적용되는데 일반적으로 브라우저는 스크립트에서 보안을 위해서 교차 출처 HTTP 요청을 제안한다. 리엑트와 스프링을 localhost로서 실행하면 각각 따로 설정을 하지 않는 이상 3000포트와 8080포트를 사용할 것이다. 포트번호가 다르기 때문에 아래와 같은 CORS에러가 나타나는 것이다.

![2](https://github.com/user-attachments/assets/db36e04a-28a1-4174-8049-e64322ebe15a)

즉, 백엔드에서는 자신의 출처(8080포트)에서가 아닌 다른 오리진(3000포트)을 허용하지 않기 때문에 위와 같은 에러가 발생한 것이다.

**cf) 동일 출처 정책이란?**

-동일한 출처에서의 리소스만 상호 작용이 가능하다는 것을 의미하는 정책이다. 이런 정책 때문에 기본적으로 웹 애플리케이션은 자신과 출처가 동일한 리소스만 호출할 수 있다.

### 어떻게 교차 출처 리소스에 접근할 수 있을까?

프론트엔드 단에서 서버와 통신하기 위해 사용하는 Fetch API나 XMLHttpRequest는 동일 출처 정책에 기반하여 작동하기 때문에 타 출처의 리소스를 호출할 수 없다.

하지만 이런 제약과는 다르게 개발을 하다보면 서로 다른 출처에서 리소스를 가져와서 사용하는 것이 흔하다. 이런 경우는 SOP를 위반하는 것이지만, CORS 정책을 지켜 리소스 요청을 보내면 예외적으로 타 출처 리소스에 접근할 수 있다.

---

### CORS의 동작 과정

기본적으로 웹 클라이언트 어플리케이션이 다른 출처의 리소스를 요청할 때는 HTTP프로토콜을 사용해서 요청을 보내게 되고, 이때 브라우저는 요청 헤더에 Origin이라는 필드에 요청을 보내는 출처를 함께 담는다.

Origin: https://google.com과 같이 출처를 담아서 서버로 보내면, 서버는 응답 헤더의 Access-Control-Allow-Origin이라는 값에 이 리소스를 접근하는 것이 허용된 출처 목록을 담아준다.

이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Orign과 서버가 보내준 응답의 Access-Control-Allow-Origin을 비교한다.

만약 허용되지 않은 Origin이면 CORS정책 위반 이슈가 발생한다. 하지만 서버의 응답은 200ok가 온다. CORS의 동작방식은 크게 3가지가 있다.

### 1. Simple Request

단순히 다른 출처의 리소스를 호출할 때 사용하는 요청이다.

요청과정은 아래와 같다.

- https://bong.com에서 https://boongs.com의 컨텐츠를 호출하기 위해 브라우저는 boongs 서버에게 요청을 보낸다.
- 이 때 브라우저에는 `Origin: [https://boong.com`과](https://boong.com과) 같이 리소스를 요청한 출처를 명시해 놓은 Origin 헤더를 같이 보낸다.
- 서버는 해당 요청에 대한 응답과 함께 `Access-Control-Allow-Origin:*` 값을 같이 보낸다
- 하지만 이 방법은 Authorization이나 Content-Type헤더가 필요한 경우에 불가능해 대부분의 경우에서 사용하기 힘들다.

### 2. Preflighted Request

Simple Request와는 다르게 OPTIONS메서드를 통해 다른 출처의 리소스로 예비 요청을 보내 실제 요청을 전송하기에 안전한지 판별하는 방법이다.

- https://boong.com에서 https://boongs.com을 신뢰할 수 있는지 판별하기 위해 브라우저는 boong 서버에게 Preflight Request를 보낸다.
- 다음과 같이 실제 요청을 보낼 때 어떤 메서드를 사용할지를 명시한다.

```json
OPTIONS https://domain-b.com/style.css

...

Origin: https://domain-a.com
Access-Control-Request-Method: POST
```

- 서버는 해당 요청에 대한 응답으로 Preflight Response를 보낸다.
- 브라우저는 CORS정책 위반 여부를 검사하고 위반하지 않았다면 본 요청을 보낸다.

### CORS 오류 해결방법

1. 스프링부트를 통한 해결법

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    private static final String DEVELOP_FRONT_ADDRESS = "http://localhost:3000";

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins(DEVELOP_FRONT_ADDRESS)
            .allowedMethods("GET", "POST", "PUT", "DELETE")
            .exposedHeaders("location")
            .allowedHeaders("*")
            .allowCredentials(true);
    }
}
```

핵심 부분은 addCorsMapping 부분이다. registry.addMapping()로 백엔드의 어느 주소를 허용할지(위의 코드에서는 "/\*\*"로 설정하여 모든 uri를 허용), allowedOrigins()를 통해서 어느 주소에 권한을 줄지(위의 코드에서는 프론트 서버), allowedMethods()를 통해 어느 http method를, expesedHeaders()를 통해 헤더에 어떤 것을 내보낼지, allowdHeaders()를 통해 어떤 헤더들을 받아들이는 것을 허용할지 등을 확인한다. 그리고 마지막으로 allowCredentials()를 true로 설정하여 CORS 문제를 해결할 수 있다.

정리하면 서버에서 응답헤더에 특정 헤더를 포함하는 방식으로 해결할 수 있다.

1. Access-Control-Allow-Origin: 특정 브라우저가 리소스에 접근이 가능하도록 허용
2. Access-Control-Allow-Method: 특정 HTTP Method만 리소스에 접근이 가능하도록 허용
3. Access-Control-Expose-Headers: 자바스크립트에서 헤더에 접근할 수 있도록 허용
4. credentials: 쿠키 등의 인증 정보를 전달

---

프론트에서는 프록시 서버를 만들어서 해결할 수 있다.

![3](https://github.com/user-attachments/assets/afc0900d-b302-4982-b79c-4a50cb16aa48)

다음과 같이 코드를 작성하면 CORS정책에 의해 막힌다.

따라서

![4](https://github.com/user-attachments/assets/b4161b31-ab4c-4507-8686-9b91e508ba57)

다음과 같이 프록시 서버를 이용해 프론트엔드 서버에서 요청되는 오리진을 백엔드 서버에 맞게 3000포트를 8080포트로 바꿔줘서 CORS문제를 해결한다.
