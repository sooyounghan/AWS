-----
### Amazon DynamoDB
-----
1. 어떤 규모에서도 10밀리초 미만 성능을 제공하는 키-값 문서 데이터베이스
2. 완전 관리형의 내구성이 뛰어난 다중 리전 / 다중 마스터 데이터베이스로서, 인터넷 규모 애플리케이션을 위한 보안 / 백업 및 복원 / In-Memory Caching 기능을 기본적 제공
3. AWS에서 제공하는 NoSQL 데이터베이스 서비스
   - RDS처럼 데이터베이스를 서비스로 제공
   - 백업 / 암호화 등 다양한 기능 지원

4. Key-Value / NoSQL 데이터 모델
   - 스키마가 존재하지 않아 데이터 형식 자유로움
   - 데이터를 단순히 키-값으로 정의
   - 키를 고유한 식별자로 사용하는 키-값 쌍의 집합으로 데이터 저장

5. Serverless 서비스
   - 사용한 만큼만 비용 지불 가능 : True On-Demand
   - 고가용성 / 장애 내구성을 아키텍쳐 단위에서 확보
   - Event-Driven 아키텍쳐 활용 가능 : 데이터베이스의 내용 변화 시 Event 생성 가능

6. 특징
   - NoSQL DB
     + 관계형 데이터베이스에서 수행해야 하는 작업(JOIN 등) 수행 불가능
     + SQL 사용 불가능 : SDK / API / CLU로만 접근 가능하며, SQL 사용 불가능
   - 요금은 WCU(Write Capacity Unit), RCU(Read Capacity Unit) + 저장 데이터만큼 과금
   - 저장 공간 미리 설정 불필요

7. 구조
   - 속성(Attribute) : Key-Value로 구성된 최소 단위 데이터
     + 단순한 값 혹은 집합(Set) 구성 가능
     + 타입 지정 필요 (예) String, Integer)

   - 항목(Item) : 여러 속성의 집합 (각 항목은 최대 40KB (모든 항목 명 / 항목 값 포함))
   - 테이블 : 다양한 아이템의 집합
     + 하나의 테이블에는 반드시 하나의 파티션 키(Partition Key 또는 Hash Key)를 위한 속성 지정 필요
     + 선택적으로 정렬 키(Sort Key 또는 Range Key)를 위한 속성 지정

   - 키 : 각 테이블 별로 파티션 키 / 정렬 키 지정 가능 (파티션 키 단독 혹은 파티션 키 + 정렬 키 조합으로 Unique 키 구성)
<div align="center">
<img src="https://github.com/user-attachments/assets/1bb0b900-3fec-4521-94cd-23b115518b52" />
<img src="https://github.com/user-attachments/assets/0bad27dd-080f-4c91-9824-7f6129153af8" />
<img src="https://github.com/user-attachments/assets/2a63aa16-5383-4612-9bbc-226184f066a3" />
<img src="https://github.com/user-attachments/assets/6f58f066-6483-44d0-8a46-7ded5d2baa52" />
<img src="https://github.com/user-attachments/assets/29e251a5-2e1b-4a8a-ae06-a5c682019de6" />
</div>

8. 작업
   - 읽기
     + Scan : 테이블의 전체 작업을 읽어오는 작업
     + Query : 주어진 파티션 키에 해당하는 항목을 조회하는 작업 (추가적으로 정렬키를 통해 조회한 항목들에서 필터링 가능)
     + GetItem : 기본 키 (파티션 키 / 정렬 키)로 하나의 항목을 가져오는 작업
     + BatchGetItem : 최대 100개의 항목을 가져오는 작업 (GetItem의 집합)
<div align="center">
<img src="https://github.com/user-attachments/assets/b9f9c363-0ceb-4a59-a54f-f3b7c406387d" />
<img src="https://github.com/user-attachments/assets/f22f24fd-02f1-4e0e-80b6-5039c0e73acf" />
<img src="https://github.com/user-attachments/assets/a323fc03-a575-4fea-8d7c-07f7593fc3dd" />
<img src="https://github.com/user-attachments/assets/938380b9-3113-4e56-90f8-edd9c00bfff3" />
</div>

   - 쓰기
     + PutItem : 항목 하나 (파티션 키 / 정렬 키로 구분)를 쓰는 작업 (만약 해당 키가 존재한다면, 기존 항목 갱신 (중복 아닌 속성 제거)
     + UpdateItem : 항목 하나를 수정하는 작업 (기존 속성 그대로 유지)
     + DeleteItem : 특정 항목을 삭제하는 작업
<div align="center">
<img src="https://github.com/user-attachments/assets/c8ee15f6-8aee-496a-a950-e35db92118e0" />
<img src="https://github.com/user-attachments/assets/2cbca1e7-3f59-40ed-abcf-87f761c138d8" />
<img src="https://github.com/user-attachments/assets/feec1845-4c3f-4dd4-b0b0-496f2157fccd" />
<img src="https://github.com/user-attachments/assets/c5b676ce-b758-406d-8e3a-5fb0a1c9c828" />
<img src="https://github.com/user-attachments/assets/43adcf02-600c-4d82-9404-f09b85ea0800" />
</div>

   - 결과
<div align="center">
<img src="https://github.com/user-attachments/assets/40968b74-6130-44d3-b335-58882833b394" />
<img src="https://github.com/user-attachments/assets/2cc4e450-16af-469e-b2d1-d03beceec441" />
<img src="https://github.com/user-attachments/assets/4c421641-9e0b-402a-a81d-0f4fd81917ac" />
</div>

9. Demo
   - DynamoDB - 대시보드 - 테이블 생성 - demo-test-table
     + 파티션 키 : partition_key (문자열)
     + 정렬 키 : sort_key (숫자)

   - 항목 탐색 - 테이블 선택 - 항목 생성
     + partition_key : test
     + sort_key : 1
     + 속성 추가 : 문자열 (region : ap-northeast-2)
     + 속성 추가 : 숫자 (account_id : 계정ID)

   - 새로운 항목 생성
     + partition_key : test2
     + sort_key : 2
     + 속성 추가 : 문자열 (provider : aws)
     + 속성 추가 : 숫자 (age : 15)
     + JSON 뷰로 확인 가능

   - 항목 생성
     + partition_key : test1
     + sort_key : 1
     + 속성 추가 : 문자열 (country : kr)
     + 속성 추가 : 문자열 (south : ture)
     + 생성 불가 : partition_key, sort_key 존재 - test2로 변경하면 생성

   - 항목 스캔 또는 쿼리 - 스캔 : 아이템 모두 확인 가능
   - 항목 스캔 또는 쿼리 - 쿼리 : 특정 키 검색 (파티션 키만 입력해서 검색 가능 / 정렬 키만 입력 가능 / 둘 다 가능)
   - 업데이트 (test, test2)
     + 작업 - 항목 편집
     + CloudShell에 명령어 입력
       * 값이 변경 (Update / 기본 속성 없어짐) [putItem]
       * 값이 변경 (기존 항목 그대로 유지) [updateItem]
```
aws dynamodb put-item \
    --table-name demo-test-table \
    --item '{
        "partition_key": {"S": "test"},
        "sort_key": {"N": "1"},
        "value": {"S": "abcd1234"}
    }'
```
```
aws dynamodb update-item \
    --table-name demo-test-table \
    --key '{"partition_key": {"S": "test"}, "sort_key": {"N": "1"}}' \
    --update-expression "SET #attrName = :attrValue, #testValue = :testVal" \
    --expression-attribute-names '{"#attrName": "newvalue", "#testValue": "testvalue"}' \
    --expression-attribute-values '{":attrValue": {"S": "abcd123456"}, ":testVal": {"S": "5432abcd"}}' \
    --return-values ALL_NEW
```

   - 항목 삭제 가능
   - 테이블 삭제 후 리소스 정리
