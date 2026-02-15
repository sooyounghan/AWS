-----
### Amazon DynamoDB의 인덱스
-----
1. DynamoDB의 Query는 하나의 파티션키만 활용 가능
2. 테이블에 추가로 파티션 키와 정렬 키를 부여하여 Query가 가능하도록 설정 가능
   - 일종의 View와 같은 개념
   - 모든 속성을 포함하거나 일부 속성만 포함하도록 설정 가능
3. 두 가지 종류
   - Local Secondary Index : 기존의 테이블 구조에서 정렬키만 더 추가하는 인덱스
     + 파티션 내부에서 재정렬
     + RCU / WCU 등의 테이블 설정을 공유
     + 정렬키에 해당하는 값이 없다면 해당 항목은 제외
     + 테이블 생성 시 같이 생성 필요 : 즉, 테이블 생성 이후 추가 생성 불가능
     + 최대 5개
     + 새로 정렬된 정렬 키와 파티션 키가 반드시 유니크일 필요는 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/d13f007a-1407-446d-b2cb-e2032474ddd0" />
<img src="https://github.com/user-attachments/assets/36dbc057-eb18-4dff-b4e6-1bb0430156c9" />
<img src="https://github.com/user-attachments/assets/334f3a2b-923c-4edf-83b9-826a46fa6115" />
</div>

   - Global Secondary Index : 기존의 테이블 구조에서 새로운 파티션 키와 정렬 키 추가 가능
     + 추가로 파티션을 생성하여, 일종의 서브 테이블을 따로 만드는 개념으로 스토리지 용량 추가
     + 별도의 RCU / WCU 등의 테이블 설정
     + 언제든지 추가 가능
     + 최종 일관성 읽기만 가능 : 정확히는 GSI 자체가 업데이트할 때, 최종 일관성 쓰기로 업데이트 진행
     + 테이블 당 20개
     + 새로 생성된 정렬 키와 파티션 키가 반드시 유니크일 필요 없음
     + 선택적으로 포함할 속성들을 선택 가능 : 쿼리할 때, 해당 속성들의 크기를 합산해서 용량 계산에 반영
<div align="center">
<img src="https://github.com/user-attachments/assets/02e0c99d-5de4-4cd1-ad78-a611d734b189" />
<img src="https://github.com/user-attachments/assets/b2af87c5-e692-49f3-9d6f-aca2bae4789a" />
<img src="https://github.com/user-attachments/assets/5f20290c-a615-4397-a71b-3ec7ba59c9bd" />
</div>
