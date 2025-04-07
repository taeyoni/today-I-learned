### 1️⃣ 주요 개념

- **CASE문 & 논리 연산자 활용**:
    - `CASE WHEN ... THEN ... ELSE ... END` → 다중 조건 분기
    - `IF(expr1, expr2, expr3)` → expr1이 참이면 expr2, 거짓이면 expr3 반환
    - `IFNULL(expr1, expr2)` → expr1이 NULL이 아니면 expr1, NULL이면 expr2 반환
    - `NULLIF(expr1, expr2)` → 두 값이 같으면 NULL, 다르면 expr1 반환
    - `COALESCE(expr1, expr2, ...)` → NULL이 아닌 첫 번째 값 반환 (※ 문서엔 없지만 실무 필수)


-- 2️⃣ 주석 달린 예제

-- CASE: 다중 조건 분기 처리
SELECT CASE 1
    WHEN 1 THEN 'one'       -- 1과 일치하므로 'one' 반환
    WHEN 2 THEN 'two'
    ELSE 'more'
END;

-- CASE WHEN: 조건 기반 처리
SELECT CASE
    WHEN 1 > 0 THEN 'true'  -- 조건이 참이므로 'true' 반환
    ELSE 'false'
END;

-- BINARY 비교 (대소문자 구분)
SELECT CASE BINARY 'B'
    WHEN 'a' THEN 1
    WHEN 'b' THEN 2   -- BINARY 때문에 소문자와 매칭 안됨 → NULL 반환
END;

-- IF: 조건 분기 함수
SELECT IF(1 > 2, 'yes', 'no');   -- 거짓이므로 'no' 반환
SELECT IF(1 < 2, 'yes', 'no');   -- 참이므로 'yes' 반환
SELECT IF(STRCMP('test','test1'), 'no', 'yes'); -- STRCMP 결과는 1 → 'no' 반환

-- IFNULL: NULL 처리 함수
SELECT IFNULL(NULL, '대체값');    -- NULL이므로 '대체값' 반환
SELECT IFNULL(100, '무시');        -- 100 반환 (NULL 아님)

-- NULLIF: 두 값이 같으면 NULL, 다르면 첫 번째 값
SELECT NULLIF(1, 1);      -- 같으므로 NULL 반환
SELECT NULLIF(1, 2);      -- 다르므로 1 반환

-- COALESCE: NULL이 아닌 첫 번째 값 반환 (실무 활용도 높음)
SELECT COALESCE(NULL, NULL, '값', '대체');  -- '값' 반환


-- 3️⃣ 한글 설명

-- ✅ CASE 문법
--    - CASE value WHEN 비교값 THEN 결과 … ELSE 결과 END
--    - 또는 CASE WHEN 조건 THEN 결과 … ELSE 결과 END
--    - 순차적으로 비교하여 첫 번째 일치하는 조건의 결과 반환
--    - ELSE 생략 시 아무 조건도 안 맞으면 NULL 반환

-- ✅ IF 함수
--    - IF(expr1, expr2, expr3)
--    - expr1이 TRUE(0이 아니고 NULL 아님)이면 expr2, 아니면 expr3 반환
--    - SELECT IF(1>2, 'yes', 'no') → 'no'
--    - 문자열이 포함되면 결과는 문자열

-- ✅ IFNULL 함수
--    - NULL 처리에 특화된 함수
--    - expr1이 NULL이 아니면 그대로 반환, NULL이면 expr2 반환
--    - 예: IFNULL(NULL, 0) → 0
--    - 연산에서 NULL 발생을 방지할 때 유용함

-- ✅ NULLIF 함수
--    - 두 값이 같으면 NULL, 다르면 첫 번째 값 반환
--    - IF(expr1 = expr2, NULL, expr1)과 동일한 동작
--    - 예: NULLIF(5, 5) → NULL / NULLIF(5, 3) → 5

-- ✅ COALESCE 함수 (실무 추천)
--    - 여러 개의 인자 중 NULL이 아닌 첫 번째 값을 반환
--    - 다수의 NULL 처리 시 가장 유연한 함수
--    - 예: COALESCE(NULL, NULL, 'A') → 'A'

-- ⚠️ 데이터 타입 처리 주의
--    - 반환 타입은 입력된 모든 인자의 타입을 고려해 결정됨
--    - 숫자, 문자열, 날짜 등 혼합 시 예상치 못한 형변환 발생 가능
--    - IF / CASE 결과가 정수 + 문자열이면 문자열로 변환될 수 있음
--    - IFNULL(1, 'test') → VARBINARY
--    - NULLIF(1, 2) → 1 (정수 타입 유지)

-- ⚠️ 기타 주의사항
--    - IF()와 IF 구문은 다름 (하나는 함수, 하나는 구문)
--    - CASE는 CASE문 (프로시저에서)와 CASE 연산자(SELECT에서)로 구분됨
--    - 비교 시 대소문자 구분을 위해 BINARY 명시 가능
--    - 컬럼 혼합 비교 시 COLLATION 에러 발생 가능 → 명시적 캐스팅으로 회피

-- 1️⃣ 주요 개념: 비교 함수와 연산자
-- MySQL에서 값을 비교할 때 사용되는 연산자와 함수들
-- 비교 결과는 일반적으로 TRUE(1), FALSE(0), 또는 NULL 반환
-- 문자, 숫자, 날짜 등 다양한 타입에 대해 동작하며, 자동 형변환이 일어날 수 있음

-- 주요 연산자/함수:
-- ● =, <>, !=, <, <=, >, >=, <=>
-- ● BETWEEN ... AND ... / NOT BETWEEN
-- ● IN(...) / NOT IN(...)
-- ● EXISTS(...) / NOT EXISTS(...)
-- ● IS NULL / IS NOT NULL / IS(expr)
-- ● COALESCE(), GREATEST(), LEAST(), NULLIF(), ISNULL(), STRCMP()

-- 2️⃣ 주석 달린 예제

-- = (동등 비교)
SELECT 1 = 1;          -- 1
SELECT '0.0' = 0;      -- 1 (자동 형변환 발생)

-- <=> (NULL-safe 비교)
SELECT NULL <=> NULL; -- 1
SELECT 1 <=> NULL;     -- 0
SELECT 1 = NULL;       -- NULL (주의)

-- <> 또는 != (같지 않음)
SELECT 'a' <> 'b';     -- 1

-- <, <=, >, >= (크기 비교)
SELECT 2 < 3;          -- 1
SELECT 3 >= 3;         -- 1

-- BETWEEN ... AND ... (범위 포함 여부)
SELECT 2 BETWEEN 1 AND 3;      -- 1
SELECT 'b' BETWEEN 'a' AND 'c'; -- 1 (문자 비교 가능)

-- NOT BETWEEN
SELECT 2 NOT BETWEEN 3 AND 5;  -- 1

-- IN (...)
SELECT 2 IN (1, 2, 3);         -- 1
SELECT 'a' IN (0);             -- 1 (암묵적 형변환으로 인해 TRUE)

-- NOT IN (...)
SELECT 2 NOT IN (1, 3);        -- 1

-- EXISTS
SELECT EXISTS (SELECT 1 FROM dual);  -- 1 (결과가 있으면 1)

-- NOT EXISTS
SELECT NOT EXISTS (SELECT 1 FROM dual WHERE 1 = 0); -- 1 (결과 없으면 1)

-- COALESCE (NULL 아닌 첫 번째 값 반환)
SELECT COALESCE(NULL, NULL, 5);   -- 5

-- GREATEST (최댓값)
SELECT GREATEST(1, 5, 3);         -- 5

-- LEAST (최솟값)
SELECT LEAST(1, 5, 3);            -- 1

-- NULLIF (두 값이 같으면 NULL 반환)
SELECT NULLIF(1, 1);              -- NULL
SELECT NULLIF(1, 2);              -- 1

-- IS NULL / IS NOT NULL
SELECT NULL IS NULL;              -- 1
SELECT 1 IS NOT NULL;             -- 1

-- IS(expr): boolean 비교
SELECT 1 IS TRUE;                 -- 1
SELECT 0 IS FALSE;                -- 1
SELECT NULL IS UNKNOWN;          -- 1

-- ISNULL(expr)
SELECT ISNULL(NULL);             -- 1
SELECT ISNULL(1/0);              -- 1 (0으로 나누면 NULL)

-- STRCMP(a, b): 문자열 비교
SELECT STRCMP('a','b');          -- -1 (a < b)
SELECT STRCMP('a','a');          -- 0
SELECT STRCMP('b','a');          -- 1

-- INTERVAL(expr, list...): expr이 어디 위치하는지 index 반환
SELECT INTERVAL(23, 1, 15, 17, 30); -- 3

-- 3️⃣ 한글 설명
-- ● =, !=, <, > 등은 숫자/문자 모두 비교 가능하며 자동 형변환 발생
-- ● <=> 는 NULL-safe 연산자: NULL도 비교 가능 (IS NOT DISTINCT FROM와 동일)
-- ● BETWEEN은 범위 포함 확인 / NOT BETWEEN은 반대
-- ● IN은 리스트 안에 포함되는지 여부, NULL 포함 시 주의 (결과가 NULL 될 수 있음)
-- ● EXISTS는 서브쿼리 결과 존재 여부 확인 / NOT EXISTS는 부존재 확인
-- ● COALESCE는 NULL 아닌 첫 번째 값 반환, NULL 방지 처리에 유용
-- ● GREATEST / LEAST는 다중 인자 중 최댓값 / 최솟값 반환 (NULL 포함 시 NULL)
-- ● NULLIF는 두 값이 같으면 NULL 반환 → 예외 처리에서 사용
-- ● ISNULL(expr) 은 expr이 NULL이면 1 반환 (SELECT ISNULL(NULL) → 1)
-- ● STRCMP는 문자열 비교를 숫자로 반환 (0: 같음, -1: a < b, 1: a > b)
-- ● INTERVAL은 정렬된 리스트에서 expr 위치 index 반환 → 범주형 구간 분리에 사용 가능

