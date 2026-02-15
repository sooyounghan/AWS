-----
### Demo
-----
1. DynamoDB 테이블 생성 : demo-ddb-table
   - 파티션 키 : pk
   - 정렬 키 : 나
   - 테이블 설정 - 설정 사용자 지정
     + 테이블 클래스 선택 : Standard / Standard-IA
     + 용량 모드 : On-Demand / Provisioned (On-Demand 선택)
     + 읽기 / 쓰기 용량 - AutoScaling 지정 가능
     + 월 처리량 지정 가능
     + 💡 보조 인덱스 : 로컬 인덱스 (테이블 생성 즉시 가능) / 글로벌 인덱스 생성 가능
       * 로컬 인덱스 : 정렬 키 (sk2) / 인덱스 이름(sk2-index) / 속성 프로젝션 : All
     + 예상 읽기 / 쓰기 비용

2. 항목 탐색 - 항목 생성
   - pk1, sk1 / 새 속성 : 문자열 (sk2 - 11111) / type : movie / name : avengers
   - 항목 선택 후 작업 - 항목 복제 - pk2, sk1 / sk2 - 22222 / type : movie / name : batman
   - 항목 선택 후 작업 - 항목 복제 - pk2, sk2 / sk2 - 22222 / type : movie / name : superman
   - 항목 선택 후 작업 - 항목 복제 - pk2, sk3 / sk3 - 22222 / type : movie / name : thor

3. 인덱스 이용 검색 : 쿼리
   - 파티션 키 : pk1
   - 쿼리 - 테이블 또는 인덱스 선택 : 인덱스 - sk2-index / sk2 : 22222 조회

4. 인덱스 변경
   - 테이블 - 작업 - 업데이트 설정 - 인덱스 - 글로벌 인덱스 생성
     + 파티션 키 : type
     + 정렬 키 : sk2
     + 인덱스 이름 : type-sk2-index
     + 인덱스 용량 : Global Table 기준 용량 모드를 따라감 (이 외 다른 것 설정 가능)
     + 속성 프로젝션 : All

5. 글로벌 보조 인덱스를 통한 검색 : 쿼리
   - 인덱스 : type-sk2-index 선택
   - type : movie
   - sk2 : 22222

6. 리소스 정리 : 테이블 삭제
