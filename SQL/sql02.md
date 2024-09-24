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
SELECT - FROM(테이블) - WHERE<조건문>
<img width="697" alt="스크린샷 2024-09-23 오후 2 10 31" src="https://github.com/user-attachments/assets/543952be-efb3-4ec5-9000-4dbc348a4b45">

# 집계 : GROUP BY
같은 값끼리 모아서 그룹화한다.    
특정 컬럼을 기준으로 모으면서 다른 컬럼에서는 집계 가능(max, mean, min)   
EX) "타입" 기준으로 그룹화해서 "평균 공격력" 집계하기   
selelct   
type1,*(집계할 컬럼 명시하기)*         
AVG(attack) As avg_attack   
From Pokemon   
GROUP BY   
type1   

SELECT
 *generation*,
 count (id) as cnt
From basic.pokemon
GROUP BY
 *generation*
 

 **DISTINCT**
 여러 값 중에 Unique한 것만 보고싶은 경우 사용      
 distinct는 중복을 제거하는 것.
 ex) count(distinct 컬럼) 

# 조건 설정
**WHERE**
Table에 바로 조건을 설정하고 싶은 경우 사용   
Raw data인 테이블 데이터에서 조건 설정

**HAVING**
GROUP BY한 후 조건을 설정하고 싶은 경우 사용  

# 서브쿼리
select문 안에 존재하는 select 쿼리   
From 절에 또 다른 SELECT 문을 넣을 수 있음    
괄호로 묶어서 사용    
서브 쿼리를 작성하고, 서브 쿼리 바깥에서 WHERE 조건 설정하는 것   
=서브 쿼리에서 HAVING으로 하는 것. 

# 정렬하기 : ORDER BY
SELECT   
col   
FROM    
ORDER BY <컬럼><순서>   
순서 : DESC(내림차순), OSC(오름차순 - 보통 디폴트)   
order by는 쿼리 맨마지막에, 중간에 작성할 필요 없음

# 출력 개수 제한하기 : LIMIT   
쿼리문의 결과 row를 제한하고 싶은 경우 사용   
쿼리문의 제일 마지막에 작성   
ex) limit 10

# 정리 
집계하고 싶은 경우 : GROUP BY + 집계함수(AVG,MAX ..)   
고윳값을 알고 싶은 경우 : DISTINCT   
조건을 설정하고 싶은 경우 : WHERE(바로)/HAVING(그룹화후)      
정렬하고 싶은 경우 : ORDER BY   
출력 개수를 제한하고 싶은 경우 : LIMIT   








 
 






