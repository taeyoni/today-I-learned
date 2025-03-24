# Window Function
**윈도우 함수(Window Function)**?       
일반적인 집계 함수(SUM, AVG 등)처럼 데이터를 요약하지만, 
행(row)을 하나로 합치지 않고 각 행마다 결과를 보여주는 함수.         
- 집계 함수는 그룹당 한 행만 출력 but      
- 윈도우 함수는 그룹을 유지하면서 모든 행을 보여줌          
- 윈도우 함수는 OVER() 구문과 함께 써야 함

'SELECT      
column1,      
column2,       
AGG_FUNC(column3) OVER (      
    PARTITION BY column2       
    ORDER BY column1      
    ) AS alias_name       
FROM table_name;       '


