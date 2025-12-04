# 05. MySQL 실행계획 실습

예제 테이블 : https://github.com/datacharmer/test_db

## 1. 인덱스

### 1-1. 인덱스 없이 사원 조회

```sql
EXPLAIN
SELECT * FROM employees
WHERE hire_date >= '2000-01-01';
```

### 1-2. 인덱스 생성 후 조회

```sql
CREATE INDEX idx_emp_hire_date ON employees(hire_date);

EXPLAIN
SELECT * FROM employees
WHERE hire_date >= '2000-01-01';
```

### 1-3. WHERE 조건에 함수 사용

```sql
EXPLAIN
SELECT * FROM employees
WHERE YEAR(hire_date) = 2000;
```

⇒ 컬럼을 가공해버려서 인덱스를 활용하지 못함!!

### 1-4. 그러면 어떻게 하는 것이 좋나

```sql
EXPLAIN
SELECT * FROM employees
WHERE hire_date BETWEEN '2000-01-01' AND '2000-12-31';
```

⇒ 이런 식으로 범위 검색 형태로 나누는 것이 좋음. 그러면 인덱스 활용함

### 마무리. 인덱스 제거

```sql
DROP INDEX idx_emp_hire_date ON employees;
```

## 2. 정렬 없애기

### 2-1. 인덱스가 없는 상태에서 조회

```sql
EXPLAIN
SELECT * FROM employees
WHERE first_name = 'Georgi'
ORDER BY last_name;
```

### 2-2. 인덱스 생성 후 조회

```sql
CREATE INDEX idx_emp_first_name ON employees(first_name);

EXPLAIN
SELECT * FROM employees
WHERE first_name = 'Georgi'
ORDER BY last_name;
```

⇒ 검색 자체는 빨라지나 extra 에서 usingFileSort(정렬 작업)는 사라지지 않음. 즉 정렬을 수행함

### 2-3. 복합 인덱스 생성 후 조회

```sql
DROP INDEX idx_emp_first_name ON employees;
CREATE INDEX idx_emp_name_order ON employees(first_name, last_name);

EXPLAIN
SELECT * FROM employees
WHERE first_name = 'Georgi'
ORDER BY last_name;
```

⇒  extra 에서 usingFileSort가 사라지는 것을 볼 수 있음. 정렬 작업 불필요

### 마무리. 인덱스 제거

```sql
DROP INDEX idx_emp_name_order ON employees;
```

## 3. 암시적 형변환

### 3-1. 인덱스 생성 후 시작

```sql
CREATE INDEX idx_emp_last_name ON employees(last_name);
```

### 3-2. varchar을 int 형으로 조회

```sql
EXPLAIN
SELECT * FROM employees
WHERE last_name = 10000;
```

⇒ 인덱스를 활용하지 않고 풀 스캔을 해버림

### 3-3. varchar 형태로 맞춘다면?

```sql
EXPLAIN
SELECT * FROM employees
WHERE last_name = '10000';
```

⇒ 인덱스를 활용하는 모습을 볼 수 있음

### 마무리. 인덱스 제거

```sql
DROP INDEX idx_emp_last_name ON employees;
```

## 4. 테이블의 조인 순서 비효율

### 4-1. 큰 테이블 먼저 읽는다면

```sql
EXPLAIN
SELECT STRAIGHT_JOIN e.first_name, e.last_name, dm.from_date
FROM  employees e
JOIN dept_manager dm ON e.emp_no = dm.emp_no;
```

⇒  STRAIGHT_JOIN 으로 순서를 강제해보았다. 이러면 읽는 row에서도 굉장히 큰 차이가 발생하고 분석을 통해 cost를 비교해봐도 굉장히 큰 차이가 있다.

### 4-2. 작은 테이블이 먼저라면?

```sql
EXPLAIN
SELECT e.first_name, e.last_name, dm.from_date
FROM dept_manager dm
JOIN employees e ON dm.emp_no = e.emp_no;
```

⇒ 대부분은 옵티마이저가 더 작은 테이블을 driving table로 삼는다

## 5. 조인 키에 인덱스 없음

### 5-0. 제약 조건 없는 테이블 생성

```sql
DROP TABLE IF EXISTS dept_emp_no_idx;

CREATE TABLE dept_emp_no_idx 
SELECT * FROM dept_emp;

SELECT COUNT(*) FROM dept_emp_no_idx;
```

### 5-1. 테이블 조회

```sql
EXPLAIN
SELECT e.first_name, de.dept_no
FROM employees e
JOIN dept_emp_no_idx de ON e.emp_no = de.emp_no
WHERE e.emp_no < 10100;
```

⇒ 풀 스캔을 해버린다.

### 5-2. 인덱스 생성 후에 다시 조회

```sql
CREATE INDEX idx_de_emp_no ON dept_emp_no_idx(emp_no);

EXPLAIN
SELECT e.first_name, de.dept_no
FROM employees e
JOIN dept_emp_no_idx de ON e.emp_no = de.emp_no
WHERE e.emp_no < 10100;
```

⇒ 인덱스를 사용해서 rows 가 굉장히 줄어든 걸 볼 수 있다.

### 마무리. 인덱스 제거

```sql
drop index idx_de_emp_no on dept_emp_no_idx;
```

## 6. SELECT * 로 인해 커버링 인덱스 미적용

### 1. 인덱스 생성

```sql
CREATE INDEX idx_emp_hire_date ON employees(hire_date);

EXPLAIN analyze
SELECT * FROM employees WHERE hire_date >= '2000-01-01';

EXPLAIN analyze
SELECT emp_no, hire_date FROM employees WHERE hire_date >= '2000-01-01';
```

### 2. *로 사원 조회

```sql
EXPLAIN
SELECT * FROM employees WHERE hire_date >= '2000-01-01';
```

### 3. 필요한 컬럼만 지정해서 조회

```sql
EXPLAIN
SELECT emp_no, hire_date FROM employees WHERE hire_date >= '2000-01-01';
```

⇒ extra에서 보면 using index, index condition으로 차이가 나는 것을 볼 수 있다. 이것은 인덱스를 사용하긴 하나 더 필요한 정보를 얻기 위해 디스크에 왔다갔다 했다는 소리!  왜 현업에서 *을 쓰지 말라고 하는 지 알 수 있다.

### 마무리. 인덱스 제거

```sql
drop INDEX idx_emp_hire_date ON employees;
```