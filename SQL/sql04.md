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
<img width="641" alt="image" src="https://github.com/user-attachments/assets/3176cbb2-4853-4439-8c02-c1d99497df72">
- concat 
  > string이나 숫자를 넣을 때 데이터에 직접 넣어준 것 => from 없어도 실행 가능
- split
  > split(문자열_원본, 나눌 기준이 되는 문자)
- replace 
  > replace(문자열_원본, 찾을 단어, 바꿀 단어)
- trim
  > trim(문자열_원본, 자를 단어)
- upper 
  > upper(문자열 원본) 소문자를 대문자로 변환     
   
 # 날짜 및 시간 데이터
 ![image](https://github.com/user-attachments/assets/033bd3f2-3bfc-4125-9e7d-762756f05dd1)
- 시간
  > created at, updated at 
# 시간 데이터 
- date : date만 표시하는 데이터 (2023-12-31)
- datetime : date와 time까지 표시하는 데이터, timezone 정보 없음, (2023-12-31 14:00:00)
- time : 날짜와 무관하게 시간만 표시하는 데이터 (23:59:59)
- time zone
  > 










  


