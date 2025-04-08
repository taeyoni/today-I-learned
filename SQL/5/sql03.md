# CASE문, 비교 논리 연산자 

### case 
CASE: 다중 조건 분기 처리
SELECT CASE 1          
    WHEN 1 THEN 'one'                
    WHEN 2 THEN 'two'           
    ELSE 'more'           
END;        

### when
CASE WHEN: 조건 기반 처리       
SELECT CASE              
    WHEN 1 > 0 THEN 'true'  -- 조건이 참이므로 'true' 반환                 
    ELSE 'false'              
END;              

### binary
BINARY 비교 (대소문자 구분)         
SELECT CASE BINARY 'B'           
    WHEN 'a' THEN 1             
    WHEN 'b' THEN 2   -- BINARY 때문에 소문자와 매칭 안됨 → NULL 반환                
END;         

### if
-- IF: 조건 분기 함수           
SELECT IF(1 > 2, 'yes', 'no');   -- 거짓이므로 'no' 반환          
SELECT IF(1 < 2, 'yes', 'no');   -- 참이므로 'yes' 반환        
SELECT IF(STRCMP('test','test1'), 'no', 'yes'); -- 결과는 1 → 'no' 반환       

### ifnull
IFNULL: NULL 처리 함수
SELECT IFNULL(NULL, '대체값');   
SELECT IFNULL(100, '무시');                 

- NULL이므로 '대체값' 반환       
- 100 반환 (NULL 아님)

### nullif 
NULLIF: 두 값이 같으면 NULL, 다르면 첫 번째 값           
SELECT NULLIF(1, 1);      -- 같으므로 NULL 반환       
SELECT NULLIF(1, 2);      -- 다르므로 1 반환       

### coalesece          
COALESCE: NULL이 아닌 첫 번째 값 반환 (실무 활용도 높음)          
SELECT COALESCE(NULL, NULL, '값', '대체');               
- '값' 반환
- 여러 개의 인자 중 NULL이 아닌 첫 번째 값을 반환
- 다수의 NULL 처리 시 가장 유연한 함수
- 예: COALESCE(NULL, NULL, 'A') → 'A'

## 비교 함수와 연산자 

- 비교 결과는 일반적으로 TRUE(1), FALSE(0), 또는 NULL 반환
  
- 주요 연산자/함수:
  - =, <>, !=, <, <=, >, >=, <=>
  - IN은 리스트 안에 포함되는지 여부, NULL 포함 시 주의 (결과가 NULL 될 수 있음)
  - EXISTS는 서브쿼리 결과 존재 여부 확인 / NOT EXISTS는 부존재 확인
  - STRCMP는 문자열 비교를 숫자로 반환 (0: 같음, -1: a < b, 1: a > b)
  - INTERVAL은 정렬된 리스트에서 expr 위치 index 반환 → 범주형 구간 분리에 사용 가능


### EXISTS
SELECT EXISTS (SELECT 1 FROM dual);  

- 1 (결과가 있으면 1)


### NOT EXISTS
SELECT NOT EXISTS (SELECT 1 FROM dual WHERE 1 = 0); 

- 1 (결과 없으면 1)

### is 
IS(expr): boolean 비교      
SELECT 1 IS TRUE;                 -- 1         
SELECT 0 IS FALSE;                -- 1      
SELECT NULL IS UNKNOWN;          -- 1           

### isnull
ISNULL(expr)        
SELECT ISNULL(NULL);             -- 1          
SELECT ISNULL(1/0);              -- 1 (0으로 나누면 NULL)          

### strcmp
STRCMP(a, b): 문자열 비교
SELECT STRCMP('a','b');          -- -1 (a < b)        
SELECT STRCMP('a','a');          -- 0      
SELECT STRCMP('b','a');          -- 1          

### interval
INTERVAL(expr, list...): expr이 어디 위치하는지 index 반환           
SELECT INTERVAL(23, 1, 15, 17, 30); -- 3        

-- 문제 풀이 --     
<img width="512" alt="스크린샷 2025-04-08 오전 9 18 36" src="https://github.com/user-attachments/assets/f490ef33-bda9-4be4-bedd-c15d44cececd" />

<img width="614" alt="스크린샷 2025-04-08 오전 9 33 30" src="https://github.com/user-attachments/assets/b1acf78a-bb94-4d6f-b131-04eb30d88456" />
아스키 캐릭터가 뭔데!!!!!!!!!!!

''' 
/*
 #두변같음 Isosceles
#세변같음 Equilateral
#모두다름 Scalene
#삼각형아님 Not A Trinagle
*/

select case when A+B <= C or A+C <= B or B+C <= A then 'Not A Triangle'
    when A=B and B=C then 'Equilateral'
    when A=B or B=C or A=C then 'Isosceles'
    else 'Scalene' 
    end
from Triangles
''''
