# GROUP_CONCAT 

**GROUP_CONCAT()**는 하나의 그룹에 속한 값들을 문자열로 연결하여 반환하는 **집계 함수(Aggregate Function)**      
특정 그룹의 값들을 하나의 문자열로 연결      
ex) "87,90,82"처럼 연결

- ORDER BY : 연결 순서를 지정한다. 예: ORDER BY test_score DESC → 높은 점수부터 연결.
- SEPARATOR : 기본적으로 값들은 ,로 구분되지만, SEPARATOR ' '를 쓰면 공백으로 연결하거나 '/'처럼 임의의 구분자를 사용할 수 있다.


### 예제 1: 학생 이름별로 시험 점수를 콤마로 이어서 출력
SELECT student_name,       
       GROUP_CONCAT(test_score)       
FROM student         
GROUP BY student_name;       


### 예제 2: 중복 제거 + 점수 높은 순 정렬 + 공백으로 구분
SELECT student_name,       
       GROUP_CONCAT(DISTINCT test_score        
                    ORDER BY test_score DESC       
                    SEPARATOR ' ')        
FROM student            
GROUP BY student_name;       

<img width="1053" alt="스크린샷 2025-05-05 오후 11 51 50" src="https://github.com/user-attachments/assets/c4a1182e-0696-4404-885c-5278f29767db" />
easy - 🤭


# with_recursive    
WITH RECURSIVE는 자기 자신을 참조할 수 있는 임시 테이블(CTE)을 정의하여,
재귀적으로 데이터를 생성하거나 계층 구조를 탐색할 수 있게 해주는 문법

주로 사용되는 용도:
- 숫자/날짜 시퀀스 생성
- 계층적 조직도 구성
- 그래프 탐색 (ex. 트리 구조, 네트워크 연결)

종료 조건을 반드시 명시해야 무한루프 방지       
컬럼 개수 및 순서는 anchor와 recursive 부분이 일치해야 함       


### 예제1
숫자 1부터 5까지 재귀적으로 생성           
WITH RECURSIVE cte (n) AS (              
    SELECT 1                #anchor: 시작값 1               
    UNION ALL               
    SELECT n + 1 FROM cte   #recursion: n에 1씩 더하기             
    WHERE n < 5             #종료 조건              
)              
SELECT * FROM cte;          

### 예제 
CEO → 직원까지의 경로를 path 컬럼으로 연결       
WITH RECURSIVE employee_paths (id, name, path) AS (         
    SELECT id, name, CAST(id AS CHAR(200))         
    FROM employees           
    WHERE manager_id IS NULL     #CEO 찾기        
               
    UNION ALL            
                    
    SELECT e.id, e.name, CONCAT(ep.path, ',', e.id)          
    FROM employee_paths ep              
    JOIN employees e ON ep.id = e.manager_id              
)           
SELECT * FROM employee_paths ORDER BY path;          






