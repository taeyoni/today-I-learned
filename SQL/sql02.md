# 데이터 탐색 : 조건과 추출 SELECT, WHERE
select   
col As new_name As : 별칭 짓기   
from Dataset.Table : 어떤 데이블에서 데이터를 확인할 것인가?   
where : 원하는 조건    
ex.col1 = 1    

ex)   
SELECT   
* : 모든 컬럼을 출력하겠다
FROM basic.pokemon
where
type1 = "Fire"

SELECT    
* EXCEPT(제외할 컬럼)

# 유의할 점
As "pokemon" ; 이름에 따음표 없이 기록해야함   
가독성 있는 Query   
 세미콜론 ; : Query가 끝났다는 뜻
 
# 핵심정리
SQL 쿼리 구조   
SELECT - FROM - WHERE

