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
+------+---------+------------+--------+
| year | country | product    | profit |
+------+---------+------------+--------+
| 2000 | Finland | Computer   |   1500 |
| 2000 | Finland | Phone      |    100 |
| 2001 | Finland | Phone      |     10 |
| 2000 | India   | Calculator |     75 |
| 2000 | India   | Calculator |     75 |
| 2000 | India   | Computer   |   1200 |
| 2000 | USA     | Calculator |     75 |
| 2000 | USA     | Computer   |   1500 |
| 2001 | USA     | Calculator |     50 |
| 2001 | USA     | Computer   |   1500 |
| 2001 | USA     | Computer   |   1200 |
| 2001 | USA     | TV         |    150 |
| 2001 | USA     | TV         |    100 |
+------+---------+------------+--------+



