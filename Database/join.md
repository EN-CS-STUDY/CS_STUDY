### 작성자 : 박민지
<br>

# JOIN
> 둘 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법을 말한다<br>

###### *테이블을 연결하기 위해서는 적어도 하나의 칼럼을 공유해야 하며 이를 이용해 데이터 검색에 활용한다
<br>

### Q. JOIN을 사용하는 이유
> 데이터베이스에서 테이블을 분리하여 **데이터의 중복을 최소화**하고 **데이터의 일관성을 유지**하기 위함이다. 이를 통해 데이터를 효율적으로 검색하고 처리할 수 있다.
<br>

### Q. JOIN의 4가지 유형
##### [Student]
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/d6fe5c6a-d684-4634-a2f0-68c28c205af6">

##### [Student Course]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/9db4ea00-af4f-4bfb-b3ef-64f26e5725d5">

<br><br>

### 1. INNER JOIN
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/86988fc0-d112-452e-95a5-2d69029ed4b0">

```
SELECT StudentCourse.COURSE_ID, Student.NAME, Student.AGE
FROM Student
INNER JOIN StudentCourse
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```

- JOIN을 얘기할 때는 보통 INNER JOIN을 지칭한다
- 테이블간 교집합을 말한다
- 두 테이블에 모두 **지정한 열의 데이터**가 있어야 한다

##### [INNER JOIN에서 ON절 대신 WHERE절을 사용한다면?]
`ON` : join 전에 조건을 필터링한다
`WHERE` : join 후에 조건을 필터링한다

- INNER JOIN에서 on과 where은 같은 결과와 같은 퍼포먼스가 나온다
- 하지만 가독성과 OUTER JOIN으로의 수정이 용이하기 때문에 ON을 쓰는게 좋다
- 조건을 지정하지 않는 INNER JOIN은 CROSS JOIN처럼 동작한다
<br>

### 2. LEFT JOIN
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/9d7d4e6d-2397-481c-9888-fbe41581252b">

#### [SQL]
```
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student 
LEFT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```
#### [OUTPUT]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/844783c6-8e80-4ba6-b5b4-ba07d41ff9d8">

- LEFT (OUTER) JOIN
- 왼쪽 테이블의 모든 행을 반환하고, 오른쪽에 있는 테이블의 행과 일치한다
- 오른쪽 테이블과 일치하는 행이 없는 행은 null로 표기한다
<br>

### 3. RIGHT JOIN
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/428e5577-4e23-433e-bed9-5027bba550e8">

#### [SQL]
```
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
RIGHT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```
#### [OUTPUT]
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/6d836603-68c6-4e91-8c9d-da8e1a66c7e0">

- RIGHT (OUTER) JOIN
- 오른쪽에 있는 테이블의 모든 행을 반환하고, 왼쪽에 있는 테이블의 행과 일치한다
- 왼쪽 테이블과 일치하는 행이 없는 행은 null로 표기한다
<br>

### 4. FULL JOIN
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/e105bb4a-1339-499a-bfe0-dd7ea35385c7">

#### [SQL] - Oracle
```
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
FULL JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```
#### [SQL] - MySQL
```
SELECT *
FROM Student
LEFT JOIN StudentCourse

UNION

SELECT *
FROM Student
RIGHT JOIN StudentCourse;
```

#### [OUTPUT]
|NAME|COURSE_ID|
|----|---------|
|HARSH|1|
|PRATIK|2|
|RIYANKA|2|
|DEEP|3|
|SAPTARHI|1|
|DHANRAJ|NULL|
|ROHIT|NULL|
|NIRAJ|NULL|
|NULL|4|
|NULL|5|
|NULL|4|

- LEFT JOIN과 RIGHT JOIN의 결과를 결합하여 결과 집합을 만든다
- 두 테이블의 모든 행이 포함되면, 일치하는 행이 없는 행은 NULL로 표기한다
<br>

### 5. CROSS JOIN
<img width="300" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/5d3daf78-6d26-47fa-a7f7-9e1ec1eed5a8">

```
SELECT *
FROM Student
CROSS JOIN StudentCourse
```

- 카테시안 곱이라고도 한다
- 두 테이블 간의 가능한 모든 조합을 만들 수 있다
- 전체 행 개수 = Student 테이블 행의 개수 * StudentCourse 테이블 행의 개수
- 다른 join과 달리 **테이블 간의 일치 조건이 필요하지 않다**
- `WHERE` 조건을 지정하게 되면 INNER JOIN처럼 동작하게 된다

###### *카테시안곱은 join 쿼리의 where 조건절이 잘못되었거나 없는 경우 발생한다
<br>

### 6. SELF JOIN
<img width="200" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/fe8d1360-a75b-4ce5-adb8-325ef76e245d">

#### [customer table]
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/2ad6d8ad-a208-4dd5-b154-9cc746071cf9">

#### [SQL]
```
SELECT
cust.customer_id,
cust.firstname,
cust.lastname,
cust.birthdate,
cust.spouse_id,
spouse.firstname AS spouse_firstname,
spouse_lastname AS spouse_lastname
FROM customer AS cust
INNER JOIN customer AS spouse
ON cust.spouse_id = spouse.customer_id
```

#### [OUTPUT]
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/4ebf37c5-118f-40cb-b46c-0dc841d03e01">

> 고객테이블에서 1번의 John과 2번의 Mayer는 부부사이다. 하지만 해당 고객을 조회했을 때 배우자의 정보를 바로 알지 못한다. 이때 self join을 수행하여 customer table에 배우자의 성과 이름을 붙여주려 한다

- 자기 자신과 조인하므로 1개의 테이블을 사용한다
- **테이블의 이름을 꼭 지정해 주어야 한다**
  - 어떤 데이터를 어떤 테이블에서 가져올지 모르기 때문이다
  - SQL 내부적으로는 서로 다른 테이블 2개를 조인하는 것으로 인식한다
- inner, outer, cross 등 문제에 따라 다르게 수행할 수 있다
<br>

### Q. JOIN 수행 과정

### [Nested Loops Join]
<img width="500" src="https://github.com/EN-CS-STUDY/CS_STUDY/assets/100523178/33674419-9d19-473c-a815-7e157085aa9d">

[과정]
1. table A와 table B사 key를 기준으로 결합을 진행한다
2. table A의 첫 번째 행에서 출발해 table B의 모든 행을 스캔한다. 이때 결합 조건이 맞다면 값을 리턴한다
3. 첫 번째 행의 스캔이 끝나면 두 번째 행도 table B의 모든 행을 스캔한다
4. 2~3번 과정을 반복하다 table A의 마지막 행이 table B의 모든 행을 스캔하면 종료한다

[결과]
> 중첩 for문과 원리가 같다. 레코드 수를 R(A), R(B)라고 할 때 **실행시간은 R(A) * R(B)에 비례한다.**

[해결]
> - 구동 테이블이 작아야 한다
> - 내부 테이블의 결합키 필드에 인덱스가 존재한다
<br>

### Q. NoSQL에서의 JOIN
`RDBMS`
- 정해진 스키마에 따라 데이터를 저장한다
- 관계를 나타내기 위해 외래키를 사용하여 join이 가능하다<br>
`NoSQL`
- 스키마 없이 key-value 형태로 관리한다
- 테이블간 관계정의가 없기 때문에 join이 불가하다

#### [NoSQL]
- NoSQL에서는 `Reference` 방식을 통해 데이터간의 관계를 나타낸다
- Reference는 특정 문서가 다른 문서를 참조하는 방식으로 데이터간의 관계를 나타낸다
