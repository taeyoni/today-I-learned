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
![image](https://github.com/user-attachments/assets/d9641597-9e72-4c43-88ca-2c7c771c50ed)

- 시간 데이터 다루기 
<img width="698" alt="스크린샷 2024-10-08 오후 12 24 00" src="https://github.com/user-attachments/assets/aed6fda3-28a2-43cd-8335-6859f50ac81a">
<img width="691" alt="image" src="https://github.com/user-attachments/assets/027e6e78-4226-49d0-9888-d1628171cf9b">

- timestamp 와 datetime 비교
  <img width="701" alt="image" src="https://github.com/user-attachments/assets/4a9b8a6d-2fae-48ba-a101-cfde789b9c4a">

# DATETIME 함수 - CURRENT_DATETIME
CURRENT_DATETIME([time_zone]) : 현재 DATETIME 출력     
> select
    current_date() AS current_date,    
    current_date("Asia/Seoul") AS asia_date,   
    current_datetime() AS current_datetime, <= 타임존 X  
    current_datetime("Aisa/Seoul") AS current_datetime_asia; <= 타임존o

# DATETIME 함수 - EXTRACT 
DATETIME에서 특정 부분만 추출하고 싶은 경우   
> select
    extract(date from datetime "2024-0102 14:00:00") AS date,
    extract(year from datetime "2024-0102 14:00:00") AS date,
    extract(month from datetime "2024-0102 14:00:00") AS date,
    extract(day from datetime "2024-0102 14:00:00") AS date,
    extract(hour from datetime "2024-0102 14:00:00") AS date,
    extract(minute from datetime "2024-0102 14:00:00") AS date,
- 요일 추출    
  extract(dayofweek from datetime_col)
  한 주의 첫날이 일요일인 [1,7] 범위의 값을 반환
  
# DATETIME 함수 - DATETIME_TRUNC
DATE와 HOUR만 남기고 싶은 경우 => 시간 자르기    
ex)datetime_trunc(datetime_col, HOUR)   
  2024-0102 14:42:13을 HOUR로 자르면 2024-01-02 14:00:00   

# DATETIME 함수 - PARSE_DATETIME
문자열로 저장된 DATETIME을 DATETIME 타입으로 바꾸고 싶은 경우   
PARSE_DATETIME('문자열의 형태', 'DATETIME 문자열') AS datetime   
ex) select    
      parse_datetime('%Y-%m-%d %H:%M:%S', '2024-01-01 12:35:35') AS parse_datetime    
# DATETIME 함수 - FORMAT_DATETIME   
DATETIME 타입 데이터를 특정 형태의 문자열 데이터로 변환하고 싶은 경우   
ex) select   
     format_datetime("%c", DATETIME "2024-01-11 12:35:35") AS formatted;

# DATETIME 함수 - LAST_DAY   
마지막 날을 알고 싶은 경우 : 자동으로 월의 마지막 값을 계산해서 특정 연산을 할 경우   
ex) last_day(datetime) : 월의 마지막 값을 반환 
<img width="607" alt="image" src="https://github.com/user-attachments/assets/19fa716d-ce95-4dad-89b4-4b2b997d223f">

# DATETIME 함수 - DATETIME_DIFF
두 DATETIME의 차이를 알고 싶은 경우    
datetime_diff(첫 datetime, 두번째 datetime, 궁금한차이(day, month, week)   







  


