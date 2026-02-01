-----
### Aurora Serverless
-----
1. Amazon Aurora의 On-Demand Auto-Scaling 구성
2. 애플리케이션 요구 사항을 기반으로 자동으로 시작 및 종료하고, 용량을 확장 및 축소
3. 이를 사용하면 데이터베이스 용량을 관리하지 않고도, 클라우드에서 데이터베이스 실행 가능
4. Aurora의 Serverless 버전
   - 즉, 인스턴스를 미리 프로비전하거나 관리할 필요가 없음
   - t2.micro / t2.medium 등의 인스턴스 타입 선택 역시 불필요
5. V1과 V2 존재 : V1은 2024-12-31 지원 종료
<div align="center">
<img src="https://github.com/user-attachments/assets/39220705-1b8c-4a99-b57c-f4baa90de10a" />
</div>

6. 주요 장점
   - 초당 몇 만건의 트랜잭션 수준에서 0 트래픽 까지 연결 / 트랜잭션의 장애 없이 스케일 가능 : 컴퓨팅 파워 / 스토리지 자동 스케일링
   - 사용한 만큼만 비용 지불
<div align="center">
<img src="https://github.com/user-attachments/assets/a4d6964f-57c1-4456-a6bf-5226407ea88f" />
<img src="https://github.com/user-attachments/assets/2146f406-cc6d-4c9f-a190-f6e8541e5336" />
div>

7. 주요 사용 사례 : 개발용 리소스, Read Replica, Fail-Over 대비 리소스 등

-----
### ACU (Aurora Capacity Unit)
-----
1. Aurora Serverless의 Scale 단위
2. 1 ACU = 약 2GB RAM, CPU, 네트워크
3. 최소 / 최대 ACU 설정 가능 : 최소 0 ~ 최대 256 (버전별 차이 존재)
4. 요금 역시 ACU 소모량에 따라 과금
5. 버전별 ACU
<div align="center">
</div>

<img width="638" height="143" alt="image" src="https://github.com/user-attachments/assets/9ef3439c-2c95-4396-9287-b31c8e5cadb2" />

-----
### Aurora Serverless 유휴(Auto Pause) 기능
-----
1. 사용하지 않는 기간(= 트랜잭션이 없거나 연결이 없는 기간)에는 ACU를 0으로 유지
   - 컴퓨팅 리소스에 대한 비용 절감
   - 💡 단, 스토리지 및 기타 비용은 부과
   - MySQL 3.08+, PostgreSQL 13, 15+, 14.12+, 15.7+, 16.3+ 만 지원

2. Cold Start 발생 : 즉, 유휴 상태에서 다시 연결 / 트랜잭션을 처리하기 전 대기시간 발생 (약 15초 ~)
3. 비활성 후 일시 정지 시간 설정 가능 : 트래픽 / 연결이 없는 상태에서 0 ACU로 전환대기 전 대기시간 (5분 ~ 24시간)
4. 유휴 / 재시작 시, AWS RDS 이벤트 발생 : EventBridge로 확인 가능
   - 기타 instance.log에서 중지 이유 혹은 중지할 수 없는 이유 등 대해서 리포트
<div align="center">
<img src="https://github.com/user-attachments/assets/4c3829e0-8ccc-4068-bde3-166bb5e50787" />
<img src="https://github.com/user-attachments/assets/99131d80-b104-46f3-814e-9f27b7f296dc" />
</div>

-----
### Demo - Aurora Serverless V2 Auto Pause 기능
-----
1. 5분동안 트래픽 / 트랜잭션이 없는 경우 ACU를 0으로 낮추어 유휴 상태로 진입
2. 이후, 트래픽 발생 시 다시 0.5+ ACU 상태로 진입

-----
### 알아둘 점
-----
1. Amazon Aurora의 몇몇 기능 사용 불가능 (예) Aurora DB Stream / Backtrack 등)
2. 단위 시간 당 비용은 기존 Aurora에 비해 일반적으로 비싼 편 : Saving Plan(RDS도 불가능) / Reserved Instance 등 할인 헤택 적용 불가
3. 생각보다 ACU는 민감하게 변동
   - 유휴 상태에서 단순한 로그 조회로도 ACU가 다시 올라갈 수 있음
   - 혹은 Workbench 등 툴로 연결을 지속해둔 경우 유휴되지 않을 수 있음
  
