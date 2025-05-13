# REGEXP_LIKE()
: 문자열이 정규 표현식 패턴과 일치하는지 여부를 Boolean으로 반환

### 예제 

✅'abc'가 'a'로 시작하는가? → 1 반환       
SELECT REGEXP_LIKE('abc', '^a')        

✅대소문자 무시 (기본값) → 1 반환        
SELECT REGEXP_LIKE('abc', 'ABC')        

✅대소문자 구분(c) → 0 반환           
SELECT REGEXP_LIKE('abc', 'ABC', 'c');        

# REGEXP_REPLACE()
: 정규 표현식에 매칭되는 부분 문자열을 치환

### 예제 

✅ 'a b c'에서 b를 X로 치환 → 'a X c'       
SELECT REGEXP_REPLACE('a b c', 'b', 'X');  

✅ 세 번째 매칭되는 [a-z]+ 부분만 'X'로 치환 → 'abc def X'      
SELECT REGEXP_REPLACE('abc def ghi', '[a-z]+', 'X', 1, 3);

✅ 설명
	•	세 번째 인자는 치환할 문자열
	•	네 번째 인자 pos: 시작 위치 (1부터 시작)
	•	다섯 번째 인자 occurrence: 몇 번째 일치를 치환할지 (0이면 전체)
	•	여섯 번째 인자 match_type: REGEXP_LIKE()와 동일한 옵션 사용 가능

# REGEXP_SUBSTR()  
: 정규 표현식에 매칭되는 부분 문자열을 추출        

### 예제     

✅ 첫 번째 매칭: 'abc'        
SELECT REGEXP_SUBSTR('abc def ghi', '[a-z]+');

✅ 세 번째 매칭되는 부분: 'ghi'        
SELECT REGEXP_SUBSTR('abc def ghi', '[a-z]+', 1, 3);         

<img width="661" alt="스크린샷 2025-05-13 오후 8 51 05" src="https://github.com/user-attachments/assets/2ece4ef1-4aa9-4823-8e8b-57d9b5f77a38" />
<img width="733" alt="스크린샷 2025-05-13 오후 8 51 11" src="https://github.com/user-attachments/assets/29927ee4-f9ce-4afe-b1e1-7818fda3e275" />

문제..
<img width="1054" alt="스크린샷 2025-05-13 오후 8 49 44" src="https://github.com/user-attachments/assets/26207af9-8419-4bdb-ad72-0b2b5d1deead" />

# 비트 연산자 

### & (비트 AND)
: 두 숫자 또는 동일 길이의 바이너리 문자열에서 각 자리의 비트가 모두 1이면 1, 아니면 0.

✅ 11101 & 01111 = 01101 → 결과: 13
SELECT 29 & 15;        
SELECT HEX(_binary X'FF' & b'11110000');          
=> 결과: 'F0'      

### | (비트 OR)        
: 두 숫자 또는 바이너리 문자열에서 각 자리 중 하나라도 1이면 1, 아니면 0.      

✅ 11101 | 01111 = 11111 → 결과: 31
SELECT 29 | 15;                       
SELECT HEX(_binary X'40' | X'01');            
=> 결과: '41' (ASCII A)        
  
### ^ (비트 XOR)
: 두 숫자 또는 바이너리 문자열에서 서로 다른 비트일 때만 1, 같으면 0.        
✅ 1011 ^ 0011 = 1000 → 결과: 8
SELECT 11 ^ 3;                  
SELECT HEX(X'FEDC' ^ X'1111');         
=> 결과: 'EFCD'        

### << (Left Shift)       
: 비트를 왼쪽으로 이동. 오른쪽은 0으로 채움.
1비트 이동 시 2배 증가 효과.         

✅ 0001 → 0100 → 결과: 4          
SELECT 1 << 2;                  
SELECT HEX(X'00FF00FF00FF' << 8);            
=> 결과: 'FF00FF00FF00'

### >> (Right Shift)    
: 비트를 오른쪽으로 이동. 왼쪽은 0으로 채움.
1비트 이동 시 2로 나눈 효과.       

✅ 100 → 001 → 결과: 1
SELECT 4 >> 2;                            
SELECT HEX(X'00FF00FF00FF' >> 8);       
=> 결과: '0000FF00FF00'


<img width="1046" alt="스크린샷 2025-05-13 오후 9 10 50" src="https://github.com/user-attachments/assets/fcf3d91f-163b-4737-a9ec-62f36078db77" />



