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



