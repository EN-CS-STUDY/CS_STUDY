### 작성자 : 조준희

# Key

Key란? 검색이나 정렬 시 Tuple을 구분할 수 있는 기준이 되는 Attribute.

1. Candidate Key (후보키)

    Tuple을 유일하게 식별하기 위해 사용하는 속성들의 부분 집합. (기본키로 사용할 수 있는 속성들)

- 2가지 조건 만족시켜야함 (유일성, 최소성)
  - 유일성 : Key로 하나의 Tuple을 유일하게 식별할 수 있음
  - 최소성 : 꼭 필요한 속성으로만 구성

2. Primary Key (기본키)
- 후보키 중 선택한 Main Key
- Null 값을 가질 수 없음
- 동일한 값이 중복될 수 없음

3. Alternate Key (대체키)
- 기본 키가 아니지만 레코드를 고유하게 식별할 수 있는 키. 
- 데이터베이스에서 기본 키로 사용할 수 있는 후보키(Candidate Key) 중에서 선택된 대안으로 사용될 수 있습니다.


4. Super Key (슈퍼키)
- 유일성은 만족하지만, 최소성은 만족하지 못하는 키
- 슈퍼 키는 후보키나 기본 키를 포함할 수 있음.


5. Foreign Key (외래키)
- 다른 테이블의 기본 키를 참조하는 키로, 두 테이블 간의 관계를 설정하는 데 사용됨. 
- 부모 테이블의 기본 키를 자식 테이블의 외래 키로 설정하여 참조 관계를 유지함.


# 데이터베이스 키 종류

데이터베이스에서 키(key)는 테이블의 각 행을 고유하게 식별하거나, 테이블 간의 관계를 정의하는 중요한 요소입니다. 

키는 데이터의 무결성과 일관성을 유지하는 데 중요한 역할을 합니다. 이 문서에서는 데이터베이스에서 사용되는 주요 키의 종류와 그 예시를 설명합니다.

## 키 종류와 설명

| 키 종류                | 설명                                                                                     | 예시                                       |
|-----------------------|-----------------------------------------------------------------------------------------|--------------------------------------------|
| 기본 키 (Primary Key) | 각 행을 고유하게 식별하는 데 사용되는 하나 이상의 열(Column). 기본 키는 NULL 값을 가질 수 없음. | `Users` 테이블의 `user_id`, `Orders` 테이블의 `order_id` |
| 후보 키 (Candidate Key) | 기본 키로 선택될 수 있는 유일한 값의 속성 집합. 테이블 내에서 고유성을 가짐.                            | `Users` 테이블의 `email`, `username`        |
| 대체 키 (Alternate Key) | 기본 키를 제외한 후보 키. 후보 키 중 기본 키로 선택되지 않은 키.                                  | `Users` 테이블의 `email`, `username`        |
| 외래 키 (Foreign Key) | 다른 테이블의 기본 키를 참조하여 테이블 간의 관계를 정의. 참조 무결성을 유지.                            | `Orders` 테이블의 `customer_id` (참조: `Users.user_id`) |
| 슈퍼 키 (Super Key)     | 행을 고유하게 식별할 수 있는 하나 이상의 열의 집합. 기본 키와 후보 키를 포함.                            | `Users` 테이블의 `user_id`, `email`, `username` |

## 예시 테이블

### `Users` 테이블

| user_id (PK) | username (AK) | email (AK)      | full_name  |
|--------------|---------------|-----------------|------------|
| 1            | jdoe          | jdoe@example.com| John Doe   |
| 2            | asmith        | asmith@example.com| Alice Smith|
| 3            | bjones        | bjones@example.com| Bob Jones  |

- **기본 키 (Primary Key)**: `user_id`
- **후보 키 (Candidate Key)**: `email`, `username`
- **대체 키 (Alternate Key)**: `email`, `username`
- - **슈퍼 키 (Super Key)**: `user_id`, `email`, `username`, `full_name`의 조합

### `Orders` 테이블

| order_id (PK) | order_date | customer_id (FK) | product_id |
|---------------|------------|------------------|------------|
| 1001          | 2023-01-10 | 1                | 2001       |
| 1002          | 2023-01-11 | 2                | 2002       |
| 1003          | 2023-01-12 | 3                | 2003       |

- **기본 키 (Primary Key)**: `order_id`
- **외래 키 (Foreign Key)**: `customer_id` (참조: `Users.user_id`)
- - **슈퍼 키 (Super Key)**: `order_id`, `order_date`, `customer_id`, `product_id`의 조합


## 요약

- **기본 키 (Primary Key)**: 각 행을 고유하게 식별합니다. 예: `Users.user_id`, `Orders.order_id`
- **후보 키 (Candidate Key)**: 기본 키로 선택될 수 있는 유일한 값의 속성 집합입니다. 예: `Users.email`, `Users.username`
- **대체 키 (Alternate Key)**: 기본 키를 제외한 후보 키입니다. 예: `Users.email`, `Users.username`
- **외래 키 (Foreign Key)**: 다른 테이블의 기본 키를 참조하여 테이블 간의 관계를 정의합니다. 예: `Orders.customer_id`가 `Users.user_id`를 참조
- **슈퍼 키 (Super Key)**: 행을 고유하게 식별할 수 있는 하나 이상의 열의 집합입니다. 예: `Users.user_id`, `Users.email`, `Users.username`
- **기본 키 대체 키 (Surrogate Key)**: 데이터베이스에서 인위적으로 생성한 키입니다. 보통 숫자나 UUID로 구성됩니다. 예: `Users.user_id` (자동 증가)
