## 스키마

**테이블, 뷰, 프로시저, 트리거 등을 포함한 하나의 단위이다.**

ex) 엑셀로 따지면 하나의 스프레드시트 파일.

### **개념적 스키마**

- 데이터베이스의 고수준의 개념적 모델을 나타낸다.
- 데이터베이스에 저장되는 주요 개념과 이들 간의 관계를 정의합니다.
    - 손으로 슥슥 그려도 모델링의 구조를 나타낸다면 그 것 또한 개념적 스키마라고 부를 수 있습니다.

### **논리적 스키마**

- 개념적 스키마를 기반으로 한 보다 더 구체적인 데이터베이스다.
- 테이블, 열, 데이터 유형, 제약 조건 등을 정의한다.
    - 테이블의 컬럼과 타입 등을 자세히 써서 실제 테이블의 구조를 나타내 줄 수 있는 ***ER Diagram** 이라면 논리적 스키마라고 부를 수 있습니다.
        - *ER Diagram = Entity-Relationship Diagram

### **물리적 스키마**

- 논리적 스키마를 실제 데이터베이스 관리 시스템(DBMS)에서 구현한 것이다.
- 데이터가 실제로 저장되는 방법, 인덱스, 파티셔닝 등을 포함한다.
- MySQL 데이터베이스에서 테이블을 생성하고, 데이터베이스 서버에 저장되어 실제로 데이터 관리가 된다.

<br>

## 테이블

**테이블은 데이터베이스에서 데이터를 구조적으로 저장하는 기본 단위이다.**

- 행과 열로 구성되어있으며, 행은 `레코드(record)`, 열은 `필드(field)` 또는 `속성(attribute)`라고 한다.
- 기본 키(Primary Key):
    - 각 레코드를 고유하게 식별하는 하나의 열 또는 여러 열의 조합이다.
- 외래 키(Foreign Key):
    - 다른 테이블의 기본 키를 참조하는 열로, 테이블 간의 관계를 설정하는 데 사용된다.

<br>

## 인덱스

**어느 위치에 저장되어 있는지 알게 해주는 정보를 인덱스라고 한다.**

### **기술적 정의**

1. **용어정의**: RDBMS에서의 인덱스(index)는 데이터베이스에서 데이터 검색 속도를 향상시키기 위해 사용되는 데이터 구조이다. 특정 컬럼(들)에 대해 인덱스를 생성함으로써, 해당 컬럼을 기준으로 레코드를 빠르게 찾을 수 있게 해준다.
2. **사용이유**: 인덱스는 대용량의 데이터 속에서 특정 데이터를 빠르게 찾아야 할 때 매우 유용하다. 인덱스를 사용하지 않을 경우, 데이터베이스는 모든 데이터를 처음부터 끝까지 검색하는 풀 스캔(full scan)을 수행해야 하며, 이는 많은 시간을 소모할 수 있다.
3. **사용방법**: 인덱스는 일반적으로 테이블의 특정 컬럼에 생성되며, 이 컬럼의 값들을 기준으로 정렬된 상태로 유지된다. 사용자는 SQL 쿼리에서 **`CREATE INDEX`** 명령어를 사용하여 인덱스를 생성할 수 있다. 예를 들어, 사용자 이름을 기준으로 빠른 검색을 원한다면 사용자 이름 컬럼에 인덱스를 생성할 수 있다.
4. **언제 쓸 때 좋은지**: 항상 쓰시고 오히려 안써야 할 이유가 없다. 보통 개발을 진행하면 SELECT 쿼리가 전체 프로젝트에서 90%~95%를 차지 한다. 만약 SELECT 는 없고 INSERT 만 존재한다면 인덱스는 필요 없겠지만 그럴 경우는 없다고 보면 된다.
    1. 참고> UPDATE랑 DELETE 쿼리는 실행 되는 시점에 ROW 를 검색하는 행동을 한다. 따라서 기본키가 있어야한다.

<br>

## 쿼리

### **create (생성)**

ex)

```sql
-- 'students' 테이블에 새 학생 데이터 추가
INSERT INTO students (name, birthdate)
VALUES ('강동원', '1990-05-20'); -- 이름과 생년월일을 지정하여 학생 추가
```

`INSERT INTO`

**Multi-line Insert** 

- 한 번의 INSERT 문으로 여러 행의 데이터를 동시에 삽입 가능.

```sql
INSERT INTO students (name, birthdate)
VALUES 
    ('강동원', '1990-05-20'),
    ('송강호', '1967-01-17'),
    ('전도연', '1973-02-11');
```

### **read (읽기)**

ex)

```sql
-- 'students' 테이블에서 특정 학생의 정보 조회
SELECT * FROM students WHERE student_id = 1; -- 학생 ID가 1인 학생의 정보를 조회
```

`SELECT FROM WHERE`

- `SELECT` 에 `*` 사용하면 욕먹는다. 불필요한 데이터까지 가져오기 때문.

**SELECT 의 더 자세한 사용법**

- 특정 열 선택: **`SELECT 열1, 열2 FROM 테이블명;`**
- 조건부 선택: **`SELECT * FROM 테이블명 WHERE 조건;`**
- 정렬: **`SELECT * FROM 테이블명 ORDER BY 열 [ASC|DESC];`**
    - `ORDER BY` : 조회 결과를 특정 열 기준으로 정렬한다.
    - `ASC` : 오름차순
    - `DESC` : 내림차순
- 그룹화: **`SELECT 열, COUNT(*) FROM 테이블명 GROUP BY 열;`**
    - `GROUP BY` : 같은 값끼리 묶는다.
    - `COUNT(*)` : 행(row)의 개수를 센다.
- 중복 제거: **`SELECT DISTINCT 열 FROM 테이블명;`**

```sql
-- 'students' 테이블에서 이름과 생년월일만 선택
SELECT name, birthdate FROM students;

-- 'students' 테이블에서 1990년 이후 출생한 학생 선택
SELECT * FROM students WHERE birthdate >= '1990-01-01';

-- 'students' 테이블에서 학생들을 이름 순으로 정렬
SELECT * FROM students ORDER BY name ASC;

-- 'enrollments' 테이블에서 각 학생별 수강 과목 수 계산
SELECT student_id, COUNT(*) as course_count 
FROM enrollments 
GROUP BY student_id;

-- 'students' 테이블에서 중복되지 않는 생년월일 목록
SELECT DISTINCT birthdate FROM students;
```

### **update (업데이트)**

ex)

```sql
-- 'students' 테이블에서 특정 학생의 이름 업데이트
UPDATE students
SET name = '원빈'
WHERE student_id = 1; -- 학생 ID가 1인 학생의 이름을 '원빈'으로 변경
```

`UPDATE SET WHERE` 

### **delete (삭제)**

ex)

```sql
-- 'students' 테이블에서 특정 학생 데이터 삭제
DELETE FROM students
WHERE student_id = 1; -- 학생 ID가 1인 학생 데이터 삭제
```

`DELETE FROM WHERE` 

`WHERE` **없으면 테이블 내 모든 데이터가 삭제된다. 주의하자.**

<br>

## JOIN

**JOIN**은 데이터베이스에서 여러 테이블의 데이터를 결합하여 하나의 결과 집합으로 만드는 SQL 연산이다.

### **JOIN을 사용하는 이유**

1. **데이터 결합**: 데이터베이스에서 데이터는 여러 테이블에 나누어 저장된다. JOIN은 이러한 여러 테이블의 데이터를 결합하여 유의미한 정보를 생성한다.
2. **관계 표현**: 관계형 데이터베이스에서 테이블 간의 관계를 표현하고, 이러한 관계를 기반으로 데이터를 조회한다. 예를 들어, 학생과 그들이 등록한 수업, 고객과 그들의 주문 등.
3. **효율성**: 한 번의 쿼리로 여러 테이블의 관련 데이터를 함께 조회할 수 있어 효율적이다. 여러 개의 별도 쿼리를 실행하는 것보다 성능이 향상된다.
4. **유지보수성**: 데이터가 여러 테이블에 분산되어 저장되기 때문에, 테이블을 개별적으로 관리하고 유지보수할 수 있다. 필요한 경우 JOIN을 통해 데이터를 통합 조회한다.

### 기본적인 사용 방법

```sql
SELECT A.*, B.*
FROM 테이블A AS A
JOIN 테이블B AS B ON A.기준열 = B.기준열;
```

### 종류

- **JOIN (INNER JOIN)**
    - 두 개 이상의 테이블을 결합하여 데이터를 조회하는 연산
    
    ```sql
    SELECT students.name, courses.course_name
    FROM students
    INNER JOIN courses ON students.student_id = courses.student_id;
    -- or (INNER JOIN / JOIN 둘 다 쿼리 실행결과가 같음)
    JOIN courses ON students.student_id = courses.student_id;
    ```
    
    - **결과**: 두 테이블에서 **`student_id`**가 일치하는 레코드만 반환한다.
    
- **LEFT JOIN (LEFT OUTER JOIN)**
    - 왼쪽 테이블의 모든 레코드와 오른쪽 테이블의 일치하는 레코드를 반환한다. 일치하지 않는 오른쪽 테이블의 레코드는 NULL로 표시된다.
    
    ```sql
    SELECT A.*, B.*
    FROM 테이블A AS A
    LEFT JOIN 테이블B AS B ON A.기준열 = B.기준열;
    ```
    
    ```sql
    SELECT students.name, courses.course_name
    FROM students
    LEFT JOIN courses ON students.student_id = courses.student_id;
    ```
    
    - **결과**: **`students`** 테이블의 모든 레코드를 반환하며, **`courses`** 테이블과 일치하지 않는 레코드는 NULL로 표시한다.
    
- **RIGHT JOIN (RIGHT OUTER JOIN)**
    - 오른쪽 테이블의 모든 레코드와 왼쪽 테이블의 일치하는 레코드를 반환한다. 일치하지 않는 왼쪽 테이블의 레코드는 NULL로 표시된다.
    
    ```sql
    SELECT students.name, courses.course_name
    FROM students
    RIGHT JOIN courses ON students.student_id = courses.student_id;
    ```
    
    - **결과**: **`courses`** 테이블의 모든 레코드를 반환하며, **`students`** 테이블과 일치하지 않는 레코드는 NULL로 표시한다.
    
    **LEFT JOIN과 같은 쿼리인데 보통은 LEFT JOIN만 사용한다.**
    
- **FULL JOIN (FULL OUTER JOIN)**
    - 양쪽 테이블의 모든 레코드를 반환합니다. 일치하지 않는 레코드는 NULL로 표시된다.
- **CROSS JOIN**
    - 조회 결과값의 모든 경우의 수를 포함한다.

<br>

## **SELECT 쿼리의 논리적 실행 순서**

### **실행 순서 정리**

1. **FROM**: 데이터 소스 지정
    - 테이블이나 뷰와 같은 데이터 소스를 선택합니다.
        
        ```sql
        FROM 테이블명
        ```
        
2. **JOIN**: 테이블 결합
    - 다른 테이블과의 결합을 수행합니다.
        
        ```sql
        JOIN 다른테이블명 ON 조건
        ```
        
3. **WHERE**: 조건 필터링
    - 결합된 테이블에서 조건에 맞는 레코드만 필터링합니다.
        
        ```sql
        WHERE 조건
        ```
        
4. **GROUP BY**: 그룹화
    - 특정 컬럼을 기준으로 레코드를 그룹화합니다.
        
        ```sql
        GROUP BY 컬럼명
        ```
        
5. **HAVING**: 그룹화된 결과 필터링
    - **`GROUP BY`** 절에 의해 그룹화된 결과에 대한 조건을 지정합니다.
        
        ```sql
        HAVING 조건
        ```
        
6. **SELECT**: 출력할 컬럼 선택
    - 최종적으로 출력할 컬럼을 선택합니다.
        
        ```sql
        SELECT 컬럼명
        ```
        
7. **DISTINCT**: 중복 제거
    - 결과에서 중복된 레코드를 제거합니다.
        
        ```sql
        SELECT DISTINCT 컬럼명
        ```
        
8. **ORDER BY**: 정렬
    - 결과를 특정 컬럼을 기준으로 정렬합니다.
        
        ```sql
        ORDER BY 컬럼명
        ```
        
9. **LIMIT**: 반환할 레코드 수 제한
    - 결과에서 반환할 레코드의 수를 제한합니다.

<br>

### 실행 순서 요약 정리

<img width="2532" height="968" alt="Image" src="https://github.com/user-attachments/assets/cf859302-1b4c-4262-bff5-0556c7f4d3bd" />

<br>
<br>

## pymysql

**pymysql**은 Python에서 MariaDB/MySQL 데이터베이스에 접속하기 위한 라이브러리이다.

### **pycharm에서 DB 연결 테스트**

```python
import pymysql

conn = pymysql.connect(
    host='서버주소',
    user='사용자명',
    password='비밀번호',
    db='데이터베이스명',
    charset='utf8mb4'
)

print("연결 성공!")
conn.close()
```

### 직접 테스트 해보기

```python
import pymysql

# 데이터베이스 연결
conn = pymysql.connect(
    host='서버주소',
    user='사용자명',
    password='비밀번호',
    db='데이터베이스명',
    charset='utf8mb4'
)

# 커서 생성 및 쿼리 실행
# demo 스키마의 샘플 데이터를 조회합니다
cursor = conn.cursor()
cursor.execute("SELECT * FROM demo.users")

# 결과 출력
for row in cursor.fetchall():
    print(row)

# 연결 종료
cursor.close()
conn.close()
```

<br>

### 데이터베이스 연결 흐름

<img width="2400" height="1276" alt="Image" src="https://github.com/user-attachments/assets/ff9af8eb-f4c6-4d67-96e9-e0a4ef2d3e71" />
