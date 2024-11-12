# join
- 서로 다른 테이블을 연결   
- 연결할 수 있는 key(공통으로 갖는 열)   

-정규화? : 중복을 최소화하게 데이터를 구조화   

# join의 종류
- inner join : 두 테이블의 공통 요소만 연결   
- left/right join : 왼쪽/오른쪽 테이블 기준으로 연결    
- full outer join : 양쪽 기준으로 연결    
- cross join : 두 테이블의 각각의 요소를 곱하기

  <img width="725" alt="image" src="https://github.com/user-attachments/assets/34f4b743-d52b-4058-b110-d713040607b3">

# join 쿼리 작성
1. 테이블 확인
2. 기준 테이블 정의
3. join key 찾기

# self join 문법
select   
  A.col1,
  A.col2,
  B.col11,
  B.col12
from table1 AS A
left join table2 AS B
ON A.key = B.key 

<img width="715" alt="image" src="https://github.com/user-attachments/assets/1e0616e9-db6b-4eb0-87a2-deeb7d39e49c">

# join의 사용법
- 교집합 : inner
- 모두 다 : cross
- left : 기준이 되는 table을 왼쪽에

 <img width="486" alt="image" src="https://github.com/user-attachments/assets/c37195ae-161b-4b3f-913c-36de0a41a67f">











 

