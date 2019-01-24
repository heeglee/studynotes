# 2. 관계 데이터 모델

## 무결성 제약조건

### Key

- Super Key
  - Candidate Key
    - Primary Key
    - Alternate Key
    - Artifical(Surrogate) Key
- Foreign Key
  - 다른 relation의 Primary key를 참조
  - Null/Not unique 허용
  - Refer하는 값(PK) 변경 -> Foreign Key 값도 자동으로 변경
  - FK가 PK의 일부가 될 수 있음
  - 자기 자신의 PK를 참조 가능



### 무결성 제약 조건

- 참조되던 table의 data가 삭제되는 경우
  - RESTRICTED: 삭제 자체가 거부
  - CASCADE: 참조하던 data도 삭제
  - DEFAULT: 설정된 default 값으로 변경
  - NULL: NULL로 변경
- 수정: (삭제 + 삽입) 동시에.



# 3. SQL 기초

## DML - SELECT

- SELECT
  - DISTINCT: 중복값 제거
  - FROM TABLENAME [JOIN ...]
  - WHERE
    - BETWEEN
    - LIKE
  - GROUP BY + [HAVING]



## DDL

```plsql
CREATE TABLE Orders (
    custid NUMBER(2) REFERENCES Customer(custid),	-- FOREIGN KEY
    ...
);

-- OR --

CREATE TABLE Orders (
	custid NUMBER(2),
	...
    FOREIGN KEY custid REFERENCES Customer(custid)
    ON DELETE {CASCADE | SET NULL}
)
```

```plsql
ALTER TABLE tablename (
	[ADD col_name type]
	[DROP COLUMN col_name]
	[MODIFY col_name type [NULL | NOT NULL]]
	[ADD PRIMARY KEY(col_name)]
	[[ADD | DROP] constraint]
)
```





# 4. SQL 고급

## 내장 함수

- 숫자 206p / 문자 208p / 날짜 211p
- NVL(attr, value): 속성 값이 null이면 value로 대체



## 부속질의 subquery

- SELECT (SCALAR) FROM (INLINE) WHERE (NESTED)



## View

- 실제 테이블처럼 사용 가능한 가상의 테이블
- 테이블의 모든 연산 사용 가능
- I/U/D는 자제

```plsql
CREATE OR REPLACE VIEW view_name [(col_name [,...n])]
AS SELECT ( ... )
```



## INDEX

### Index

- 한 개 이상의 속성을 이용해 선택
- 빠른 검색 + 효율적 record 접근
- 속성 + 데이터 위치만 가짐
- 데이터 수정, 삭제 등 변경 시 index도 재구성해야 함



### 종류

- B-Tree Index
  - CREATE INDEX t_ix ON T(key1);
- Index Organized Table
  - CREATE TABLE T (...) ORGANIZATION INDEX;
  - 키 값 정렬과 동시에 leaf node에 실제 데이터가 같이 저장되는 테이블
- Bitmap Index
  - CREATE BITMAP INDEX bt_ix ON T(col);
  - 하나의 entry가 여러 row를 가리킬 수 있음
- Function-Based Index
  - CREATE INDEX fbi_ix ON T(FUNC(name));
  - 함수 결과를 저장



### 생성

- WHERE 절 / JOIN에 자주 사용되는 속성이어야 함

- Table 당 4~5개

- 속성의 선택도 낮을 때 유리(속성의 모든 값이 다를 때)

- 속성이 가공되지 않는 경우에 사용

  ```plsql
  -- REVERSE: 역순
  -- UNIQUE: 속성 값에 대해 중복이 없는 유일한 INDEX
  CREATE [REVERSE] [UNIQUE] INDEX idx_name
  ON tbl_name (col_name [ASC | DESC], ...);
  ```

  ```plsql
  -- REBUILD: 단편화된 기존 INDEX 삭제 + 재생성
  ALTER [REVERSE] [UNIQUE] INDEX idx_name
  [ON {ONLY} tbl_name (col_name, ...)] REBUILD;
  ```

  ```
  DROP INDEX idx_name;
  ```

  





