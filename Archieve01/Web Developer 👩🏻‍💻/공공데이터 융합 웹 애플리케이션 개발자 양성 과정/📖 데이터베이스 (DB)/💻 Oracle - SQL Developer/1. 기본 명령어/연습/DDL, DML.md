## Oracle → SQL Developer → systemdb → ex1
```sql
--스키마:system
--ex1 테이블
CREATE TABLE ex1 (
    column1 CHAR(10), --고정길이 문자(10byte)
    column2 VARCHAR2(10) --가변길이 문자(10byte)
);

--데이터 추가
INSERT INTO ex1(column1, column2) VALUES ('abc', 'abc');
INSERT INTO ex1 VALUES ('당산', '당산');

--데이터 조회
SELECT column1, LENGTH(column1), column2, LENGTH(column2) FROM ex1;

--트랜잭션 : COMMIT, ROLLBACK
COMMIT;
```
## ## Oracle → SQL Developer → systemdb → ex2
```sql
--테이블 생성(CREATE)
CREATE TABLE ex2 (
    col_date DATE, --날짜 자료형(시스템의 현재 날짜)
    col_timestamp TIMESTAMP --날짜와 시간 자료형
);

--현재 날짜 삽입(INSERT)
INSERT INTO ex2 VALUES ( SYSDATE, SYSTIMESTAMP );
INSERT INTO ex2(hire_date) VALUES ('2023-09-01');

COMMIT;

SELECT * FROM ex2;

--테이블 변경(ALTER) - 입사일 컬럼 추가
ALTER TABLE ex2 ADD hire_date VARCHAR2(20);

--테이블 삭제(DROP)
DROP TABLE ex2;
```
## Oracle → SQL Developer → systemdb → department
- 참조 - [[1. 부서와 사원]]
```sql
--부서와 사원 테이블 생성
CREATE TABLE department(
    deptid NUMBER PRIMARY KEY, --기본키
    deptname VARCHAR2(20) NOT NULL, --NULL불허
    location VARCHAR2(20) NOT NULL
);

--자료 입력
INSERT INTO department(deptid, deptname, location) VALUES (10, '전산팀', '서울');
INSERT INTO department(deptid, deptname, location) VALUES (20, '총무팀', '인천');
INSERT INTO department(deptid, deptname, location) VALUES (30, '마케팅팀', '성남');

COMMIT;

/*자료 검색*/
--모든 칼럼 검색('*' 사용)
SELECT * FROM department;

--특정한 데이터(행:로우) 검색
--부서이름이 전산팀인 row(레코드) 검색
SELECT * FROM department WHERE deptname = '전산팀';

--자료 수정(부서이름 변경 관리팀 -> 경영팀)
UPDATE department SET deptname = '경영팀'
WHERE deptid = 20;

DROP TABLE DEPARTMENT;

ROLLBACK; --COMMIT 이전에 실행되면 취소됨

--삭제 이상(지식이 참조하고 있으므로 삭제 안됨)
--자료 삭제(부서번호가 30번인 마케팅팀 삭제)
DELETE FROM department 
WHERE deptid = 30;

COMMIT;
```
## Oracle → SQL Developer → systemdb → employee
- 참조 - [[1. 부서와 사원]]
```sql
--사원 테이블 생성
CREATE TABLE employee(
    empid NUMBER(3),
    empname VARCHAR2(20) NOT NULL,
    age NUMBER(3),
    deptid NUMBER(3),
    PRIMARY KEY(empid),
    FOREIGN KEY(deptid) REFERENCES department(deptid) --외래키
);

--사원 자료 추가
INSERT INTO employee(empid, empname, age, deptid) 
VALUES (101, '이강인', 23, 10);
INSERT INTO employee(empid, empname, age, deptid) 
VALUES (102, '손흥민', 32, 10); 
INSERT INTO employee(empid, empname, deptid) 
VALUES (103, '황희찬', 20);
INSERT INTO employee(empid, empname, age, deptid) 
VALUES (104, '김민재', 28, 10); --부모코드가 없어서 외래키 제약조건 위배

--정보 출력
--1. 사원의 모든 정보 출력 (모든 데이터)
SELECT * FROM employee;

--2. 사원 이름과 나이 컬람 출력 (특정 칼람)
SELECT empname, age FROM employee;

--3. 사원이름이 손흥민인 데이터 출력 (특정 데이터)
SELECT * FROM employee
WHERE empname = '손흥민';

--나이가 30이상인 사원을 검색
SELECT * FROM employee
WHERE age >= 30 ;

--부서번호가 20인 사원 검색
SELECT * FROM employee
WHERE deptid = 20 ;

--나이가 입력되지 않은 사원 검색
SELECT * FROM employee
WHERE age IS NULL;

--문자열 검색(사원이름에서 '민'을 포함하는 사원 검색)
SELECT * FROM employee
WHERE empname LIKE '%민%';

--문자열 검색(사원이름에서 '민'을 포함하고, 나이가 30살 이상인 사원 검색)
SELECT * FROM employee
WHERE empname LIKE '%민%' AND age >= 30;

--문자열 검색(사원이름에서 '민'을 포함하거나 나이가 저장되지 않은 사원 검색)
SELECT * FROM employee
WHERE empname LIKE '%민%' OR age IS NULL;

COMMIT;

DROP TABLE EMPLOYEE;
```

