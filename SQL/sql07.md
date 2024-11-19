# 실수는 언제 발생할까?
 - 문법을 잘못 알고 있는 경우    
 - 데이터를 파악하지 않고 쿼리를 작성하는 경우      
 - 쿼리가 복잡한 경우

# 가독성있는 SQL 
1. 예약어는 대문자 작성
   - SELECT, FROM, WHERE 등
2. 컬럼이름은 snake_case/CamelCase로 일관성 있게
3. alias 명시적 vs 암시적
4. 왼쪽 정렬
5. 예약어, 컬럼은 한줄에 하나씩



# WITH 구문
- cte(common table expression)
- select 구문에 이름을 정해주는 것과 유사    
- 쿼리 내에서 반복적 사용 가능    
WITH 이름 AS  (    
SELECT     
.    
.    
.    
)

SELECT    
col1    
FROM 이름 <= 이렇게 활용함     

# PARTITION
- 데이터 정리 용도
- 장점
  1. 쿼리 성능 향상(파티션 설정해 스캔 범위 줄임)
  2. 데이터 관리 용이성(특정 일자 데이터 모두 변경시 파티션 활용)
  3. 비용(파티션 해당 데이터만 스캔해서 비용 줄일 수 있음)

# 데이터 검증 
- SQL 쿼리 후 얻은 결과가 예상과 일치하는 확인하는 과정
- 목적 : 분석 결과의 정확성, 신뢰성 확보
- 방법 : 1. 기대하는 예상 결과 정의     
        2. 쿼리 작성
        3. 1-2 일치하는지 확인
   
<img width="697" alt="image" src="https://github.com/user-attachments/assets/ccf1ed9e-6a61-4e73-8acf-cf9401b92c1d">

# 데이터 검증 시 자주 활용하는 SQL 쿼리    
1) COUNT(*) : 행 수를 확인. 의도한 데이터의 행 개수가 맞는가?
2) NOT NULL : 특정 컬럼에 NULL이 존재하는가? 필수 필드가 비어있지 않은가?
3) DISTINCT : 데이터의 고유값을 확인해 중복 여부 확인
4) IF문, CASE WHEN : 의도와 같다면 TRUE, 아니면 FALSE

<img width="592" alt="image" src="https://github.com/user-attachments/assets/2f5020be-ee08-46bf-8324-a0a018755777">



