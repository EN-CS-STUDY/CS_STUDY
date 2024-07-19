## 작성자 : 홍영환

### 📚 테이블 네이밍

![Untitled](https://github.com/user-attachments/assets/8c4cbc2a-f1f9-403f-8800-f934e5ac76bc)

코드리뷰를 위해 보던 중 “college-lesson-prof-relations” 네이밍 발견. 강의와 교수님간 연관데이터가 들어가 있는 테이블인데, 테이블 이름은 어떻게 지어야 할까?

💡 **따옴표를 사용하지 않는다.**


- “CollegeRelation” 등 이름에 따옴표를 사용하면 SQL을 작성하기 어려워진다.
- 공백을 포함해서는 안된다.

💡 **이름은 소문자로 작성한다.**


- 대소문자가 혼합된 이름은 사용할 때 마다 큰따옴표를 묶어야 한다.
- JPA Entity 네이밍 에러의 예시이다.

[JPA Entity Table 대소문자 네이밍 에러](https://bbogle2.tistory.com/entry/JPA-Entity-Table-대소문자-네이밍-에러)

💡 **테이블 타입을 이름으로 작성하면 안된다.**


- 명사로 지어야하며 test, timestamp와 같은 데이터 타입의 이름을 사용하면 안된다.

💡 **단어 사이는 언더스코어( _ )로 구분한다. (Snake Case)**

- wordCount 대신 word_count를 사용한다.


💡 **약어를 사용하지 않는다.**
- mid_nm 대신에 middle_name 사용
- 대부분의 SQL 데이터베이스는 최소 30자로 된 이름을 지원한다.
- PostgreSQL은 식별자를 63자까지 지원한다.


💡 직관적인 **기술형**으로 작성한다.

- 오브젝트 명만 보고도 어떤 오브젝트인지 구분이 명확하도록 생성
- 되도록 쉬운 단어 선택
- 테이블 생성 예)
    
    `log 테이블 → log (x)어떤 로그를 담는 테이블인가?배송 → delivery_log (o)주문 → order_log (o)`


💡 동사는 **능동태**를 사용한다. 동명사는 허용


- `created_date (x)create_date (o)`


💡 **예약어는 피한다**


- user, locak, table 등을 이름으로 사용하지 않는다.
- 예약어를 이름에 사용하려면 따옴표를 붙여줘야 한다.

![Untitled](https://github.com/user-attachments/assets/517aa3ec-b4f5-4bee-8c60-951c2a2b1bdd)

이전 프로젝트를 진행하면서 ‘type’ , ‘range’ 네이밍을 사용했었는데 SQL 시스템에서 키워드로 예약된 단어여서 수정한 기억이 있다.

### ✅ 예시

```
•	SELECT
•	INSERT
•	UPDATE
•	DELETE
•	FROM
•	WHERE
•	JOIN
•	INNER
•	OUTER
•	LEFT
•	RIGHT
•	ON
•	CREATE
•	DROP
•	ALTER
•	TABLE
•	DATABASE
•	INDEX

```

### 📚 MySQL에서 따옴표와 백틱

> `따옴표(' or ") :  문자열 데이터 입력시 사용`
> 
- 따옴표 사용하는 경우
    - 문자열 데이터 입력시
    - 날짜 / 상수 입력시
    - 단, 홀따옴표 사용 안하는 경우 : 숫자, Bool, Null

> `백틱(`) : 객체 감쌀때 사용`
> 
- 백틱 사용해야 하는 경우 (구문 오류 발생하지 않기 위함)
    - 객체 감싸는 경우
    - 객체 이름에 공백이 같이 있는 경우
    - 예약어
    - 숫자로 시작하는 경우