### 작성자 : 박민지
<br>

# Injection
> `DB를 비정상적으로 조작하는 기법`<br>
> 코드 인덱션의 한 기법으로 클라이언트 입력값을 조작하여 서버의 DB를 공격할 수 있는 공격 방식을 말한다.

- SQL 삽입, SQL 주입으로도 불린다
- **주로 사용자가 입력한 데이터를 서버 측에서 제대로 필터링/이스케이필 하지 못한 경우 발생한다**
<br>

### Q. 웹 페이지의 SQL
> SQL Injection은 일반적으로 사용자 이름, ID와 같은 입력을 요청할 때 발생한다.

##### - 사용자가 입력한 SQL
```
txtUserId = getRequestString("UserId"); 
txtSQL = "SELECT *
          FROM Users 
          WHERE UserId = " + txtUserId;
```
##### - 공격자가 주입한 SQL
```
SELECT *
FROM Users
WHERE UserId = 105 OR 1=1;
```

- 공격자가 UserId필드에 `105 OR 1=1`을 입력한다
- 해당 쿼리는 UserId="105"인 사용자 뿐만 아니라 **모든 사용자의 데이터를 반환**하게 된다
- 따라서 Users 테이블의 모든 정보를 조회하게 됨으로써 가장 먼저 만들어진 계정으로 로그인에 성공한다
- 보통 관리자 계정을 처음에 만들기 때문에 관리자 계정을 탈취한 공격자가 권한을 악용할 수 있다
<br>

### Q. SQL Injection 유형

#### 1. 인증 우회
로그인 폼(form)을 대상으로 공격을 수행한다. 정상적인 계정 정보가 없어도 로그인을 우회하여 인증 획득이 가능하다. 즉, Where 구문을 우회한다

[예제 1] - classic
```
SELECT * FROM Users WHERE userId='admin' -- AND password='';
```
- 의도적으로 주석을 삽입하여 Where 조건을 무력화한다
- 비밀번호 검증 없이 관리자 계정에 액세스할 수 있다

[예제 2] - blind
```
SELECT * FROM Users WHERE name='ming-taro' AND password='123' OR '1'='1';
```
- 공격자가 DB의 데이터를 직접 조회하지 않고(= 결과 확인X) 참/거짓의 결과를 통해 정보를 추출하는 방법이다
- **항상 참이되도록 구성하여 Where 조건을 무력화한다**
- 쿼리의 결과가 무조건 참이 되어 없는 계정과 잘못된 비밀번호라도 인증을 통과한다
<br>

##### [Where 1 = 1]
🧐 왜 사용하는 걸까?
```
SELECT * FROM A
WHERE
A.name is not null
AND a.gender='male'
```
위의 쿼리에서 이름이 null인 사람을 포함하는 것으로 수정하고 싶다면 어떻게 해야 할까?
```
SELECT * FROM A
WHERE
-- A.name is not null
-- AND
a.gender='male'
```
해당 조건을 주석처리하기위해 `AND`를 줄바꿈하여 수정해줘야 한다.<br>
하지만 `1=1`을 사용하게 되면 해당 조건 줄만 주석처리하여 해결할 수 있다.
```
SELECT * FROM A
WHERE 1=1
-- A.name is not null
AND a.gender='male'
```
**이처럼 `1=1`조건은 동적쿼리에서 쿼리의 가독성과 유지보수에 용이하다**
<br><br>

#### 2. 데이터 노출
- 시스템에서 발생하는 에러 메세지를 이용해 공격을 수행하는 방법이다
- 공격자는 악의적인 구문을 삽입하여 에러를 유발시킨다
- 조작된 쿼리가 실행되도록 하여 기업의 개인정보에 접근하여 데이터를 획득한다
- 데이터 값을 변경하거나 테이블을 삭제할 수도 있다
- ex) 해커가 GET 방식으로 동작하는 URL 쿼리 스트링을 추가해 에러를 발생시킨다
  - 매우 큰 파일을 업로드 시도 or 무한 반복 요청
  - 오류 발생시 DB 스키마를 유추할 수 있다
<br><br>

### Q. Injection 방어
#### 1. input 값을 받을 때 특수문자 여부를 검사한다
```java
public class InputValidation {
    private static final String EMAIL_PATTERN = 
        "^[A-Za-z0-9+_.-]+@([A-Za-z0-9-]+\\.)+[A-Za-z]{2,6}$";
    private Pattern pattern;
    private Matcher matcher;

    public InputValidation() {
        pattern = Pattern.compile(EMAIL_PATTERN);
    }

    public boolean validateEmail(String email) {
        matcher = pattern.matcher(email);
        return matcher.matches();
    }

    public static void main(String[] args) {
        InputValidation validator = new InputValidation();

        String email = "user@example.com";
        boolean isValid = validator.validateEmail(email);
    }
}
```
로그인하기 전에 검증 로직을 추가하여 미리 설정한 특수문자에 대한 요청을 막아내거나 데이터의 길이를 제한한다
<br><br>

#### 2. SQL 서버 오류 발생 시, 해당하는 에러 메세지를 감춘다
[원본 테이블]
```
CREATE TABLE Users (
    UserID INT PRIMARY KEY,
    UserName VARCHAR(100),
    UserEmail VARCHAR(100)
);
```
[뷰 생성 및 활용]
```
CREATE VIEW PublicUsers AS
SELECT UserName, UserEmail
FROM Users;

-- 사용자는 뷰를 통해서만 데이터를 조회한다
SELECT * FROM PublicUsers;
```
view를 활용하여 원본 DB 테이블에 대한 접근 권한을 높인다. 따라서 일반 사용자는 view로만 접근하여 에러를 볼 수 없도록 한다
<br><br>

#### 3. Prepared Statement 구문을 사용한다
```java
public class MyDB {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/yourdatabase";
        String user = "root";
        String password = "password";

        try (Connection con = DriverManager.getConnection(url, user, password)) {
            String query = "INSERT INTO Users (UserName, UserEmail) VALUES (?, ?)";

            try (PreparedStatement pst = con.prepareStatement(query)) {
                pst.setString(1, "John Doe");
                pst.setString(2, "john@example.com");

                pst.executeUpdate();
            }
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }
}
```
특수문자를 자동으로 escaping 해주기 때문에 서버에서 필터링 과정을 통해 공격을 방어한다

##### [preparestatement]
- SQL 쿼리를 실행하기 위한 기능을 제공하는 객체다
- <파라미터 위치에 대해 자리표시자로 '?'기호를 사용한다
- 동적으로 파라미터값을 설정할 수 있다
- DB에 안전하게 접근하고 유연한 쿼리를 작성할 수 있다
###### *statement: Connection 객체로부터 sql문을 DB에 전달하고 결과를 리턴 받는 객체</h6>
<br><br>
          
#### [참고]
https://velog.io/@k4minseung/DB-SQL-Injection-%EA%B3%B5%EA%B2%A9%EA%B3%BC-%EB%B0%A9%EC%96%B4-%EB%B0%A9%EB%B2%95
