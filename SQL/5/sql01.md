# 15.2.15.2 Comparisons Using Subqueries
non_subquery_operand comparison_operator (subquery)      
comparison operator : =  >  <  >=  <=  <>  !=  <=>     
ex) ... WHERE 'a' = (SELECT column1 FROM t1)

JOIN으로는 수행할 수 없는 서브쿼리를 사용한 일반적인 비교 사례
- t2 테이블에서 column2의 최댓값과 같은 값을 가지는 t1의 모든 행을 찾는 쿼리:     
SELECT * FROM t1        
  WHERE column1 = (SELECT MAX(column2) FROM t2);      
- t1 테이블에서 특정 열의 값이 두 번 등장하는 모든 행을 찾는 쿼리:     
SELECT * FROM t1 AS t      
  WHERE 2 = (SELECT COUNT(*) FROM t1 WHERE t1.id = t.id);     

1. 서브쿼리 vs 스칼라 값(단일값)
=> 서브쿼리는 반드시 하나의 값만 반환해야 함.     
옳은⭕️ 예제 :      
SELECT * FROM employees      
WHERE salary = (SELECT MAX(salary) FROM employees);     
틀린❌ 예제 :            
SELECT * FROM employees     
WHERE salary = (SELECT salary FROM employees);      
한가지 값만 반환하지 않는다!!

2. 서브쿼리 vs 행       
=> 서브쿼리는 비교 대상 행과 동일한 개수의 값을 반환해야 한다.
옳은⭕️ 예제 :          
SELECT * FROM employees        
WHERE (department_id, salary) = (SELECT department_id, MAX(salary) FROM employees GROUP BY department_id);         
틀린❌ 예제 :        
SELECT * FROM employees          
WHERE (department_id, salary) = (SELECT department_id FROM employees);
왼:2개, 오:1개로 틀림

# 15.2.15.3 Subqueries with ANY, IN, or SOME
operand comparison_operator ANY (subquery)      
operand IN (subquery)      
operand comparison_operator SOME (subquery)        
comparison operator : =  >  <  >=  <=  <>  !=  <=>        

### ANY 
비교 연산자와 함께 사용되며, **서브쿼리가 반환하는 값 중 하나라도 비교 조건을 만족하면 TRUE**를 반환.     
예제 : SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);        
- t1.s1 값이 t2.s1의 어떤 값보다 크다면(> ANY), 해당 s1 값을 반환.     
- 예를 들어, t2의 값이 (21,14,7)이라면, t1.s1이 7보다 크기만 해도 조건이 참(TRUE)이 됨.
- t2가 비어 있으면 조건은 FALSE, t2에 NULL만 존재하면 결과는 NULL이 됨.

### IN 
in 연산자는 = ANY와 동일한 의미를 가짐.     
SELECT s1 FROM t1 WHERE s1 = ANY (SELECT s1 FROM t2);         
SELECT s1 FROM t1 WHERE s1 IN    (SELECT s1 FROM t2);         
in은 표현식 리스트에 사용 가능       
=ANY는 표현식 리스트 사용 불가        

<> : **두 값이 다르면 TRUE, 같으면 FALSE**를 반환하는 연산자
NOT IN과 <>ANY는 다름.       
NOT IN과 <>ALL은 같음. =>t2의 어떤 값과도 일치하지 않는 값을 찾을 때 사용됨        

### SOME       
ANY와 SOME은 동일함!!!      
SELECT s1 FROM t1 WHERE s1 <> ANY  (SELECT s1 FROM t2);        
SELECT s1 FROM t1 WHERE s1 <> SOME (SELECT s1 FROM t2);         
<> ANY는 서브쿼리의 값 중 하나라도 다르면 TRUE(not “there is no b which is equal to a”)     

# 15.2.15.4 Subqueries with ALL     
operand comparison_operator ALL (subquery)       
비교 연산자와 함께 사용되며, 서브쿼리에서 반환된 모든 값과 비교하여 조건이 참(TRUE)인 경우에만 참이 된다.       
SELECT s1 FROM t1 WHERE s1 > ALL (SELECT s1 FROM t2);       
t1 containing (10)     
t2 : (-5,0,+5) => TRUE      
t2 : (12,6,NULL,-100) => FALSE     
t2 : (0,NULL,1) => unknown(null)    
t2 : empty 일때           
SELECT * FROM t1 WHERE 1 > ALL (SELECT s1 FROM t2); => TRUE     
SELECT * FROM t1 WHERE 1 > ALL (SELECT MAX(s1) FROM t2); => NULL      

# 15.2.15.6 Subqueries with EXISTS or NOT EXISTS     
If a subquery returns any rows at all, EXISTS subquery is TRUE, and NOT EXISTS subquery is FALSE. For example:        
SELECT column1 FROM t1 WHERE EXISTS (SELECT * FROM t2);       

### EXISTS (서브쿼리)      
**서브쿼리가 하나 이상의 행을 반환하면 TRUE**를 반환.     
서브쿼리가 비어 있으면 FALSE를 반환.        
### NOT EXISTS (서브쿼리)     
**서브쿼리가 하나도 반환되지 않으면 TRUE**를 반환.      
서브쿼리가 하나 이상의 행을 반환하면 FALSE를 반환.       


- What kind of store is present in one or more cities?
SELECT DISTINCT store_type FROM stores          
WHERE EXISTS (           
    SELECT * FROM cities_stores           
    WHERE cities_stores.store_type = stores.store_type          
);

- What kind of store is present in all cities?
WHERE NOT EXISTS (        
    SELECT * FROM cities        
    WHERE NOT EXISTS (        
        SELECT * FROM cities_stores       
        WHERE cities_stores.city = cities.city       
        AND cities_stores.store_type = stores.store_type       
    )       
);         
외부 NOT EXISTS 서브쿼리는 내부 서브쿼리를 뒤집어서 “모든 도시에 존재하는 store_type“을 찾는다.      

# 15.2.15.10 Subquery Errors     
1. 지원되지 않는 서브쿼리 문법       
ERROR 1235 (ER_NOT_SUPPORTED_YET)      
SQLSTATE = 42000       
Message = "This version of MySQL doesn't yet support        
'LIMIT & IN/ALL/ANY/SOME subquery'"
예제)        
SELECT * FROM t1 WHERE s1 IN (SELECT s2 FROM t2 ORDER BY s1 LIMIT 1)      
MySQL은 IN 서브쿼리에서 LIMIT을 허용하지 않음        

2. 서브쿼리에서 반환된 열 개수 오류
ERROR 1241 (ER_OPERAND_COL)
SQLSTATE = 21000
Message = "Operand should contain 1 column(s)"
예제)    
SELECT (SELECT column1, column2 FROM t2) FROM t1;
단일 열(scalar value)만 반환해야 하지만, 다음과 같이 두 개 이상의 열을 반환하려고 하면 오류가 발생.


3. 서브쿼리가 여러 개의 행을 반환하는 오류
ERROR 1242 (ER_SUBSELECT_NO_1_ROW)
SQLSTATE = 21000
Message = "Subquery returns more than 1 row"
예제)         
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2); (틀림)
SELECT * FROM t1 WHERE column1 = ANY (SELECT column1 FROM t2); (고치기완료!)

4. 같은 테이블을 업데이트하는 오류
ERROR 1093 (ER_UPDATE_TABLE_USED)      
SQLSTATE = HY000     
Message = "You can't specify target table 'x'      
for update in FROM clause"
예제)     
UPDATE t1 SET column2 = (SELECT MAX(column1) FROM t1);
서브쿼리에서 업데이트하려는 테이블을 동시에 조회하면 오류가 발생

## 문제풀이
<img width="1074" alt="Image" src="https://github.com/user-attachments/assets/16750a7c-eab3-4810-9672-e546d6473f3d" />     
<img width="1070" alt="Image" src="https://github.com/user-attachments/assets/52d31288-39cb-46c2-b58e-0cbc87c45c6d" />     


# 15.2.20 WITH (Common Table Expressions)
### 공통 테이블 표현식(CTE)란?      
	•	CTE(Common Table Expression)는 일시적인 결과 집합을 생성하여 동일한 SQL 문 내에서 여러 번 참조할 수 있도록 하는 기능
	•	WITH 절을 사용하여 정의되며, 주로 가독성을 높이고 서브쿼리를 단순화하는 데 활용

### CTE 기본 문법
WITH cte_name AS (서브쿼리)
SELECT * FROM cte_name;      
	•	cte_name은 CTE의 이름이며, 마치 가상의 테이블처럼 사용할 수 있다.        
	•	AS (서브쿼리) 부분에 데이터를 생성하는 SQL 문을 작성한다.       

예제)          
WITH cte1 AS (SELECT a, b FROM table1),     
     cte2 AS (SELECT c, d FROM table2)       
SELECT b, d FROM cte1 JOIN cte2      
WHERE cte1.a = cte2.c;         

WITH sales_summary AS (       
    SELECT category, SUM(sales) AS total_sales FROM sales GROUP BY category      
)     
SELECT * FROM sales_summary WHERE total_sales > 100000;     

### CTE의 주요 특징
- CTE는 동일한 SQL 문 내에서 여러 번 참조 가능    
- 다른 CTE를 참조하여 새로운 CTE를 생성 가능      
- SELECT, UPDATE, DELETE, INSERT 등 다양한 문장에서 사용 가능





 




 

  

    











