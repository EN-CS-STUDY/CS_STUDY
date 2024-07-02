### 작성자 : 박민지
<br>

# JOIN
> 둘 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법을 말한다<br>

###### *테이블을 연결하기 위해서는 적어도 하나의 칼럼을 공유해야 하며 이를 이용해 데이터 검색에 활용한다
<br>

### Q. JOIN을 사용하는 이유
> 데이터베이스에서 테이블을 분리하여 **데이터의 중복을 최소화**하고 **데이터의 일관성을 유지**하기 위함이다. 이를 통해 데이터를 효율적으로 검색하고 처리할 수 있다.
<br>

### Q. JOIN의 3가지 유형
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

### Q. NoSQL에서의 JOIN
- NoSQL에서는 Reference 방식을 통해 데이터간의 관계를 나타낸다
