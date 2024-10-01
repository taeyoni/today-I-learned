# 코드
-- 1. 포켓몬 중에서 type2가 없는 포켓몬 수를 작성하는 쿼리   
-- null 은 is null 로 작성   
-- is not null 로 작성   
-- or 조건 => () or ()   
select    
count(id) As cnt   
from `basic.pokemon`    
where    
type2 is null    

-- 2.type2가 없는 포켓몬의 type1과 포켓몬 수를 알려주는 쿼리 작성      
-- 테이블 : pokemon     
-- 칼럼 : type1        
-- 조건 : type2가 없는 포켓몬      
-- 집계 : 포켓몬 수 => count       
-- 정렬 : type1의 포켓몬 수가 큰 순으로 정렬=>order by       
-- 집계함수는 group by와 같이 다님. 집계하는 기준이 없으면 count만 쓸 수 있으나, 집계하는 기준이 있다면 그 기준 칼럼을 group by에 써줘야 한다.       
select      
 type1,       
 count(id) As pokemon_cnt      
from `basic.pokemon`     
where       
type2 is null     
group by      
type1      
order by     
pokemon_cnt DESC    


-- 3. type2 상관없이 type1의 포켓몬 수를 알 수 있는 쿼리 작성    
-- 테이블 : pokemon    
-- 조건 : type2 상관없이 => 조건인가? 아닌가? => 아님    
-- 컬럼 : type1   
-- 집계 : 포켓몬 수 => count   
#select   
#type1,   
#count(id) as pokemon_cnt,   
#COUNT(DISTINCT id) as pokemon_cnt   
#from basic.pokemon    
#group by     
 type1    

-- 4.전설 여부에 따른 포켓몬 수를 알 수 있는 쿼리 작성   
-- 테이블 : pokemon   
-- 조건 : 전설여부 => 컬럼    
-- 컬럼 : 전설(is_legendary)    
-- 집계 : 포켓몬 수   
#select   
#is_legendary,    
#count(id) As pokemon_cnt    
#from `basic.pokemon`   
#group by   
#is_legendary    

-- 5. 동명 이인이 있는 이름은 무엇일까?   
--테이블 : 트레이너   
--조건 : 같은 이름이 2개 이상   
--컬럼 : 이름   
--집계 : count   
#select   
#name,    
#count(name) As trainer_cnt   
#from `basic.trainer`    
#group by    
#name    
--집계 후 조건 => having. from절의 테이블 조건 => where   
#having    
#trainer_cnt >= 2    
-- where : 원본데이터 from 절에 있는 데이터에 조건 설정   
-- having : group by와 함께 집계 결과에 조건 설정    

-- 7.trainer 테이블에서 "Iris", "Whitney", "Cynthia" 트레이너의 정보를 알 수 있는 쿼리를 작성   
-- 테이블 : trainer     
-- 조건 : 이름    
-- 컬럼 : *    
-- 집계 : 없음    
#select     
#*    
#from basic.trainer    
#where    
  #(name = "Iris")   
  #or (name = "Cynthia")    
  #or (name = "Whitney")    
  #or 조건 대신 => in   
  #name IN ("Iris", "Cynthis", "Whitney")       

--8. 전체 포켓몬 수는 얼마나 되나요?   
-- 테이블 : 포켓몬    
#select   
#count(id) AS pokemon_cnt   
#from basic.pokemon   

-- 9.세대별로 포켓몬 수가 얼마나 되는지 쿼리 작성   
#select   
#generation,   
#count(id) As pokemon_cnt   
#from basic.pokemon   
#group by   
#generation   

-- 10.type2가 존재하는 포켓몬 수는?    
--테이블 : 포켓몬   
--조건 : type2 존재 => is not null   
#select   
#count(id) As pokemon_cnt   
#from basic.pokemon   
#where   
#type2 is not null   

--11. type2가 있는 포켓몬 중에서 제일 많은 type1은?    
#select   
#count(id) As pokemon_cnt   
#from basic.pokemon   
#where   
#type2 is not null    
#group by    
#type1    
#order by    
#pokemon_cnt desc    
#limit 1    

--12. 단일 타입 포켓몬 중 많은 type1은 무엇일까?   
-- 조건 : 단일 타입 => 하나의 타입만 존재 => type2가 null(값이 없어야 한다)   
#select   
#type1,   
#count(id) As pokemon_cnt    
#from basic.pokemon   
#where   
#type2 is null   
#group by type1   
#order by    
#pokemon_cnt desc    


--13. 포켓몬의 이름에 "파"가 들어가는 포켓몬은?   
-- 조건 : name에 "파"   
-- 컬럼 : name    
-- 집계 : 없음   
#select    
#kor_name   
#from basic.pokemon   
#where   
  kor_name like "파%"   
-- %파 => 파로 끝남 / 파% => 파로 시작     

--14. 뱃지가 6개 이상인 트레이너 수는?   
-- 조건 : 뱃지 6개 이상    
-- 집계 : count   
#select   
#count(id) as trainer_cnt   
#from basic.trainer   
#where   
#badge_count >= 6   

-- 15. 트레이너가 보유한 포켓몬 수가 제일 많은 트레이너는?   
-- 테이블 : trainer_pokemon    
-- 조건 : 없음   
-- 칼럼 : trainer_id   
-- 합계 : 포켓몬 수 => count   
#select    
#trainer_id,   
#count(pokemon_id) As pokemon_cnt   
#from `basic.trainer_pokemon`   
#group by   
#trainer_id   

--16. 포켓몬을 많이 풀어준 트레이너는?    
--테이블 : pokemon_trainer    
--조건 : status = "Released"    
--칼럼 : trainer_id    
--집계 : count    
#select     
#trainer_id,    
#count(pokemon_id) As pokemon_cnt    
#from `basic.trainer_pokemon`    
#where    
#status = "Released"    
#group by     
#trainer_id    
#order by    
#pokemon_cnt    

--17. 트레이너 별로 풀어준 포켓몬의 비율이 20%가 넘는 포켓몬 트레이너는 누구일까요? 풀어준 포켓몬 비율 = (풀어준 포켓몬 수/전체 포켓몬 수)    
--테이블 : trainer_pokemon   
--조건 : 풀어준 포켓몬의 비율이 20%가 넘어야 한다   
--칼럼 : trainer    
--집계 : countif    
--countif(조건) : countif(컬럼 = "3")    
select     
trainer_id,   
countif(status = "Released") As released_cnt,    
count(pokemon_id) AS pokemon_cnt,   
countif(status = "Released")/count(pokemon_id) As released_ratio    
from basic.trainer_pokemon   
group by   
trainer_id   
having    
released_ratio >= 0.2    

<img width="708" alt="image" src="https://github.com/user-attachments/assets/92766784-8dc0-4565-85d7-03384774773c">

# 새로운 집계 함수
- group by all : 컬럼 명시 하지 않아도 됨
  select
   firstname as first_name,
   lastname as last_name,
   sum(pointscored) As total_points
  From playerstats
  Group by all
  (group by
   first_name,
   last_name)

# 3강(SQL 작성) 목차
- sql 쿼리 작성 흐름
- 쿼리 작성 템플릿, 생산성 도구(테이블, 조건, 컬럼, 집계)
- 오류를 디버깅 하는 방법

# SQL 쿼리 작성 흐름
<img width="702" alt="image" src="https://github.com/user-attachments/assets/3d0d314a-4548-423b-bd9b-d38b62951c6b">

# 쿼리 작성 템플릿과 생산성 도구 
- 템플릿
  #쿼리를 작성하는 목표, 확인할 지표 :    
  #쿼리 계산 방법 :    
  #데이터의 기간 :    
  #사용할 테이블 :    
  #join key :    
  #데이터 특징 :    

- 생산성 도구 : espanso 핵심로직   
   특정 단어가 감지되면 정의된 것으로 바꾼다.   
  <img width="697" alt="image" src="https://github.com/user-attachments/assets/6bf4cf9f-754b-4c15-9aed-6f616deb1550">



