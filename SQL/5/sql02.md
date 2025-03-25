# Window Function
**윈도우 함수(Window Function)**?       
일반적인 집계 함수(SUM, AVG 등)처럼 데이터를 요약하지만, 
행(row)을 하나로 합치지 않고 각 행마다 결과를 보여주는 함수.         
- 집계 함수는 그룹당 한 행만 출력 but      
- 윈도우 함수는 그룹을 유지하면서 모든 행을 보여줌          
- 윈도우 함수는 OVER() 구문과 함께 써야 함
### 대표적인 문법구조
`SELECT 
column1, column2, 
AGG_FUNC(column3) OVER (PARTITION BY column2 ORDER BY column1) AS alias_name
FROM table_name;`

### 예제 
<img width="484" alt="스크린샷 2025-03-24 오후 9 15 43" src="https://github.com/user-attachments/assets/7ace8de0-7f52-4bd1-b0c5-9a1903b96324" />

##### 전체 행의 profit 합계를 각 행에 출력
SELECT      
  year,        
  country,         
  product,          
  profit,         
  SUM(profit) OVER() AS total_profit         
FROM sales;         

-  OVER() 안에 아무 것도 없으면, 전체 데이터셋 기준으로 계산

##### 나라별로 profit 합계
SELECT        
  year,        
  country,        
  product,        
  profit,       
  SUM(profit) OVER(PARTITION BY country) AS country_profit        
FROM sales;     

- PARTITION BY는 grouping 기준을 정해주는 것
- 각 나라별로 나눠서 profit의 합을 계산해줌

##### 나라별로 각 행에 고유번호 매기기(row_number)
SELECT
  year,
  country,
  product,
  profit,
  ROW_NUMBER() OVER(PARTITION BY country ORDER BY year, product) AS row_num
FROM sales;
 
- row_number()는 같은 나라 안에서 정해진 순서대로 고유 번호를 매겨줌

### over 정리 
OVER (
  [PARTITION BY ...]  : 데이터를 나눌 기준       
  [ORDER BY ...]      : 파티션 내 정렬 기준          
  [ROWS BETWEEN ...]  : 프레임 범위 지정 (해당 행을 기준으로 어느 범위까지 계산할지 지정)          
)

###### over 가능 집계함수 
가능 집계함수        
: SUM(), AVG(), COUNT(), MAX(), MIN(), VAR_POP(), STDDEV(), ...    
윈도우 전용 함수(over 무조건!!!)        
: ROW_NUMBER(), RANK(), DENSE_RANK(), LAG(), LEAD(), NTILE(), ...        

---
# window fucntion    

### 1.ROW_NUMBER()     
파티션 안에서 각 행의 고유한 순번을 매김. 동순위 없음.     

SELECT      
  name,       
  score,         
  ROW_NUMBER() OVER(PARTITION BY class ORDER BY score DESC) AS row_num        
FROM student;        
- class 별로 점수 내림차순 정렬 후, 순위를 1부터 부여함
- 동점자가 있어도 무조건 다음 순번으로 넘어감

### 2.RANK()
동점자는 같은 순위를 부여하지만, 다음 순위는 건너뜀 (띄어쓰기 존재)      

SELECT       
  name,        
  score,       
  RANK() OVER(PARTITION BY class ORDER BY score DESC) AS rank       
FROM student;       

- 1등 2명 → 다음은 2등이 아닌 3위
- 즉, 순위 번호에 공백(gap)이 생김

### 3. DENSE_RANK()     
동점자는 같은 순위, 다음 순위는 건너뛰지 않음         

SELECT          
  name,        
  score,      
  DENSE_RANK() OVER(PARTITION BY class ORDER BY score DESC) AS dense_rank       
FROM student;       

- 1등 2명 → 다음은 바로 2등
- 순위가 조밀하게 이어짐 (no gap)

### 4.CUME_DIST()       
누적 상대 백분위. 현재 값 이하인 데이터의 비율          
0 ~ 1 사이의 값 반환       

SELECT       
  score,       
  CUME_DIST() OVER(ORDER BY score) AS cume_percent       
FROM student;       

- 낮은 점수일수록 값 작고, 높은 점수일수록 1에 가까움
- 현재 행보다 작거나 같은 데이터 개수 ÷ 전체 개수

### 5. PERCENT_RANK()
백분위 순위 (0 ~ 1). 현재 순위 기준 상대적 위치

SELECT        
  score,         
  PERCENT_RANK() OVER(ORDER BY score) AS percent_rank         
FROM student;         

- 공식: (RANK - 1) / (전체 행 수 - 1)
- 최솟값: 0, 최댓값: 1
- CUME_DIST와 비슷하지만 계산 방식 다름

### 6. NTILE(N)
데이터를 N개의 구간(bucket)으로 나누고, 각 행이 속한 구간 번호 반환       

SELECT         
  score,         
  NTILE(4) OVER(ORDER BY score) AS quartile         
FROM student;          

- 성적순으로 정렬 후 4개의 그룹으로 나눠서
- 각 그룹에 1~4 번호 부여

###  7. FIRST_VALUE(expr), LAST_VALUE(expr)
윈도우 프레임 내에서 첫 번째 값 / 마지막 값 반환      

SELECT        
  time,        
  val,         
  FIRST_VALUE(val) OVER w AS first_val,        
  LAST_VALUE(val) OVER w AS last_val         
FROM observation           
WINDOW w AS (          
  PARTITION BY sensor ORDER BY time           
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW           
);           

- 센서별로 시간순 정렬 후
- 현재까지의 첫 값, 마지막 값 추출
- w로 저장하고 over 에 불러옴!!
- WINDOW는 윈도우 함수에서 반복되는 PARTITION/ORDER 기준을 저장해두는 키워드

### 8. NTH_VALUE(expr, N)
윈도우 프레임 내에서 N번째 값을 반환 (없으면 NULL)     

SELECT        
  time,       
  NTH_VALUE(val, 3) OVER w AS third_val        
FROM observation         
WINDOW w AS (        
  PARTITION BY sensor ORDER BY time       
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW       
);         
- 프레임 내 3번째 값 추출
- 현재 행이 프레임의 3번째 이전이면 NULL 나옴

### 9. LAG(expr, N, default) / LEAD(expr, N, default)

LAG: N행 전의 값 / LEAD: N행 후의 값        
기본값 없으면 NULL, N 생략 시 1       

SELECT          
  time,         
  val,          
  LAG(val, 1, 0) OVER(ORDER BY time) AS prev_val,         
  LEAD(val, 1, 0) OVER(ORDER BY time) AS next_val       
FROM series;         

- 시간 기준으로 바로 앞/뒤 값 보여줌
- 첫 행의 LAG는 0, 마지막 행의 LEAD도 0 (기본값 설정)

--- 
# 집계 함수    
### 집계함수 + OVER() = 윈도우 함수      

**집계 함수(예: SUM, AVG, COUNT 등)**는 보통 GROUP BY와 함께 사용해서 집단별 하나의 결과만 반환함.      
하지만 OVER() 절을 붙이면 집계 함수도 각 행마다 결과를 반환하는 윈도우 함수처럼 동작함.        
=> 집계 + 윈도우 기능을 동시에 사용하는 것        
(DISTINCT 옵션은 대부분 쓸 수 없음)        

<img width="519" alt="스크린샷 2025-03-25 오후 3 24 55" src="https://github.com/user-attachments/assets/43d80c0b-f886-4a11-9bc8-378f0971f714" />

### 1. AVG()

SELECT       
  student_name,       
  test_score,         
  AVG(test_score) OVER(PARTITION BY student_name) AS avg_score        
FROM student;      

- 학생별로 그룹을 나누고, 각 행에 대해 평균 점수를 출력함
- AVG() + OVER() → 윈도우 함수처럼 작동

### 2. COUNT()

SELECT          
  student_name,         
  course,        
  COUNT(*) OVER(PARTITION BY student_name) AS course_count          
FROM student_course;        

- 학생별 수강 과목 수를 각 행에 표시
- COUNT(*) + OVER() → 집계이지만 그룹을 쪼개지 않고 전체 행에 결과 표시

### 3. SUM()

SELECT          
  product,         
  region,        
  profit,          
  SUM(profit) OVER(PARTITION BY region) AS region_profit       
FROM sales;        

- 지역(region)별로 나눠서 그 안에서 누적 profit 합계를 각 행마다 보여줌


