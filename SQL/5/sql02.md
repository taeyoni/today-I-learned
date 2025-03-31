# Join 
1. 정의
MySQL의 `JOIN` 절은 둘 이상의 테이블을 결합해 하나의 결과 집합을 생성하는 SQL 문법       
SELECT, DELETE, UPDATE에서 모두 사용 가능

### JOIN 유형 정의
- INNER JOIN: 두 테이블에서 조건이 일치하는 행만 반환
- LEFT JOIN: 왼쪽 테이블은 전부, 오른쪽 테이블은 매칭된 행만 반환 (없으면 NULL)
- RIGHT JOIN: 오른쪽 테이블은 전부, 왼쪽 테이블은 매칭된 행만 반환 (없으면 NULL)
- CROSS JOIN: 카티션 곱 (모든 조합)
- NATURAL JOIN: 양쪽 테이블에 공통된 컬럼으로 자동 조인하며 중복 컬럼은 제거됨
- STRAIGHT_JOIN: 옵티마이저가 조인 순서를 바꾸지 않고 왼쪽부터 오른쪽 순으로 읽도록 강제

### JOIN 문법 구조
- `JOIN ... ON`: 조인 조건을 직접 지정
- `JOIN ... USING (col1, col2)`: 동일한 컬럼 이름이 양 테이블에 있을 경우 사용
- `NATURAL JOIN`: 공통된 컬럼 자동으로 조인 (암시적 USING)
- `{ OJ ... }`: ODBC 호환을 위한 문법, 중괄호는 실제로 작성
- `JOIN`은 `,`보다 우선순위가 높다
- `t1, t2 JOIN t3 ON ...` 은 `(t1, (t2 JOIN t3))`로 해석됨
- 혼용 시 ON 절에서 참조 불가 컬럼 오류 발생 주의

### COALESCE와 중복 컬럼 처리
- USING이나 NATURAL JOIN을 사용할 경우, 중복된 컬럼은 COALESCE(col1, col2)로 통합됨
- 결과 테이블에서는 중복 열이 하나로 통합돼 나타남
- 예: a LEFT JOIN b USING (id)는 SELECT 시 COALESCE(a.id, b.id)만 반환

### 예시 

##### INNER JOIN 예제   
SELECT t1.name, t2.salary                         
FROM employee AS t1                           
INNER JOIN info AS t2                    
  ON t1.name = t2.name;              

--설명--
employee 테이블의 이름과 info 테이블의 급여 선택      
employee 테이블을 t1으로 별칭 설정     
info 테이블을 t2로 별칭 설정 후 INNER JOIN 수행      
name 컬럼을 기준으로 두 테이블 연결      

##### LEFT JOIN 예제
SELECT left_tbl.*                     
FROM left_tbl                        
LEFT JOIN right_tbl                  
  ON left_tbl.id = right_tbl.id      
WHERE right_tbl.id IS NULL;   

--설명--      
left_tbl의 모든 열 선택
기준이 되는 테이블
조인할 테이블
id 기준으로 연결
대응되는 값이 없는 left_tbl의 행만 선택

##### NATURAL JOIN 예제
SELECT * FROM t1 NATURAL JOIN t2;    

--설명-- 
두 테이블에 공통된 열로 자동 조인, 중복 열 제거됨

##### USING 절 예제
SELECT * FROM a LEFT JOIN b USING (c1, c2);   

--설명--      
a와 b 테이블에서 c1, c2를 기준으로 LEFT JOIN 수행

##### STRAIGHT_JOIN 예제
SELECT * FROM t1 STRAIGHT_JOIN t2 ON t1.id = t2.id;      

--설명-- 
조인 순서를 명시적으로 지정

###### ODBC 호환 JOIN 예제
SELECT left_tbl.*
FROM { OJ left_tbl LEFT OUTER JOIN right_tbl
       ON left_tbl.id = right_tbl.id }
WHERE right_tbl.id IS NULL;


        
1. NATURAL JOIN: 중복 컬럼 제거, COALESCE 적용       
2. USING: 명시적 COALESCE, 중복 제거        
3. ON: 모든 컬럼 유지, a.col과 b.col 모두 존재         

### 문제 풀기    
<img width="1103" alt="스크린샷 2025-03-31 오후 5 52 28" src="https://github.com/user-attachments/assets/6e665fdb-2b93-462a-8b73-5453c63b1f06" />

--
# GROUP BY
GROUP BY는 집계 함수와 함께 사용되어 데이터를 그룹화함     .

### 예시 

##### 예제 1
- 표준 SQL에서 유효한 형태        
SELECT o.custid, MAX(o.payment)         
FROM orders AS o, customers AS c         
WHERE o.custid = c.custid        
GROUP BY o.custid;

##### 예제 2
- GROUP BY에 비컬럼 표현식 사용        
SELECT id, FLOOR(value/100)       
FROM tbl_name      
GROUP BY id, FLOOR(value/100);              
- MySQL에서는 허용 (비컬럼 표현식)      

##### 예제 3 
- GROUP BY에 별칭 사용
SELECT id, FLOOR(value/100) AS val        
FROM tbl_name       
GROUP BY id, val;         

##### 예제 4
- 파생 테이블을 사용해 workaround
SELECT id, F, id + F         
FROM (          
  SELECT id, FLOOR(value/100) AS F        
  FROM tbl_name        
  GROUP BY id, FLOOR(value/100)       
) AS dt;

# HAVING 
- HAVING 절은 GROUP BY로 그룹화된 결과에 조건을 적용할 때 사용
- group by 적용 이후 사용!!!
- HAVING은 집계 함수 조건에도 사용 가능
- 그룹별 집계된 결과 중 원하는 조건의 결과만 필터링하기 위해서는 HAVING 절을 사용하여 필터 조건을 사용가능
- HAVING 절과 WHERE 절의 다른 점 : HAVING 절은 GROUP BY 절과 함께 사용해야 하며 집계 함수를 사용하여 조건절을 작성하거나 GROUP BY 컬럼만 조건절에 사용 가능 
- WHERE와 달리 집계 함수(예: COUNT, MAX, SUM 등)를 사용할 수 있다
- WHERE 절에서는 컬럼 별칭을 사용할 수 없으며, HAVING만 별칭 참조 가능
- HAVING은 LIMIT 바로 앞 단계에서 실행되며, 최종 결과를 클라이언트에 보내기 직전에 필터링된다

### 예시 

##### 예제 1 
- GROUP BY와 HAVING 함께 사용
SELECT user, MAX(salary)        
FROM users         
GROUP BY user          
HAVING MAX(salary) > 10;
- 각 사용자 그룹의 최대 salary가 10 초과인 경우만 필터링

##### 예제 2 
- SELECT에 별칭을 사용한 HAVING 조건
SELECT name, COUNT(name) AS c          
FROM orders        
GROUP BY name         
HAVING c = 1;
- 별칭 c를 사용하여 이름이 1번만 등장한 행 선택

##### 예제 3
- 잘못된 HAVING 사용 (WHERE로 처리해야 함)
잘못된 예:       
SELECT col_name FROM tbl_name HAVING col_name > 0;        
올바른 예:        
SELECT col_name FROM tbl_name WHERE col_name > 0;

--
<img width="1433" alt="스크린샷 2025-03-31 오후 6 42 04" src="https://github.com/user-attachments/assets/3382e425-f707-4a08-b8ae-7c09dbf4dcc7" />







