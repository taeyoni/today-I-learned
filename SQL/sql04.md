# 오류를 디버깅 하는 법 
- syntax error
  구글/챗지피티/공식문서 등
 - ex) select list must not be empty at [10:1]
   > select from 사이 컬럼이 있어야함
 - ex) number of arguments does not match for aggregate function COUNT
   > count(id, kor_name) count 함수에는 하나의 컬럼만 들어가야 함.
 - ex) select list expression references column type1 a which is neither grouped nor aggregated
   > group by에 적절한 컬럼 명시X
 - ex) expected end of input but got keyword SELECT
   > 쿼리 끝나는 부분에 ; 작성, select 근처 확인하기
 - ex) expected end of input but got keyword where at [5:1]
   > limit 이 where 앞에 있을 때.. limit 위치 바꿔주기
- ex) expected ")" but got end of script at [8:11]
   > 괄호가 안닫힌 경우

# 데이터 탐색 : 변환
- SELECT 문에서 데이터 변환 가능
- WHERE의 조건문에서도 가능
- 데이터 타입에 따라 다양한 함수
  > 데이터 타입 : 숫자(1,2,3), 문자("나"), 시간날짜(2024-01-01), 부울(참/거짓)
<img width="641" alt="image" src="https://github.com/user-attachments/assets/ac31dce8-20b1-4a86-b29b-d64dce317523">

# cast 
 - 자료 타입을 변경하는 함수
    - 숫자1 > 문자1로    
      select   
      cast(1 AS STRING)
 - SAFE_CAST
      변환 실패시 null로 변환
   
# 문자열(STRING) 함수 
- 





  


