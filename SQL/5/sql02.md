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


