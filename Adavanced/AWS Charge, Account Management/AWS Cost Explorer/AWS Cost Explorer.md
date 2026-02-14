-----
### AWS Cost Explorer
-----
1. 시간에 따른 AWS 비용과 사용량을 시각화, 이해 및 관리할 수 있는 손쉬운 인터페이스 제공
<div align="center">
<img src="https://github.com/user-attachments/assets/18f4f5aa-2eaa-4b20-9bbb-661c56fd53ba" />
<img src="https://github.com/user-attachments/assets/cabeb54d-3d2f-4387-a6bd-c9082064b9ea" />
</div>

2. AWS 서비스의 비용 및 사용량을 분석하고 시각적으로 확인할 수 있는 서비스 : 시간별, 서비스별, 계정별 비용 보고서의 생성 및 다운로드 가능
3. 최대 13개월까지 확인 가능
4. 별도로 API 호출을 활용하여 데이터 불러오기 가능
5. 처음 콘솔 UI로 접속할 때 자동적으로 활성화
   - 이후 비활성화는 불가능
   - 활성화시점부터 최근 13개월의 비용을 기반으로 앞으로 12개월의 비용 예측을 계산

6. AWS Cost Explorer의 활성화
   - 처음 콘솔 UI로 접속할 때 자동적으로 활성화
   - 활성화 시 Cost Anomaly Detection 활성화
     + Cost Anomaly Detection : 머신러닝 기반 비용관련 이상 탐지
     + $100 이상 AND 40% 이상 비용 범위 안에서 이상 비용이 발생할 경우 알림

7. Organizations와 Cost Explorer
   - Organizations에 속한 Member 계정도 Cost Explorer 활용 가능 : 단, 관리 게정에서 허용 필요
   - 새로 Organization에 가입한 경우 이전 데이터 활용 불가능
     + Organizations에 떠난 경우에도 Organizations 안에 있을 시기의 데이터 활용 불가능
     + 복귀 시 다시 액세스 가능
   - 💡 대부분 MSP 관리 환경에서는 Cost Explorer 사용 불가능

8. AWS Cost Explorer의 구분
   - 날짜 범위, 세분성(일별, 월별, 시간별(추가 요금)), 그룹화(차원)
   - 이 외에 다양한 필터 (서비스 리전, 사용량 유형 등) : Include, Exclude 선택 가능
   - 주요 그룹화 추천 : 사용량 유형, 리소스, 서비스

-----
### Demo
-----
1. 우측 상단 - 결제 및 비용 관리 - Cost Explorer : 시각화 된 화면 표시
2. 요금 유형 : include, exclude (크레딧 제외)
3. 날짜 범위 : 지정 가능 (미래 지정도 가능 : 머신 러닝을 통해 예측 가능)
4. 세분성 : 일별, 주별 등 분할 가능
5. 차원
   - 사용량 유형 : 어디서 비용이 발생하는지 알 수 있음 (서비스 지정 가능)
   - 리소스 : 활성화 필요 (14일만 사용 가능)
     + 기본 설정 및 설정 - 비용 관리 기본 설정 - Cost Explorer
     + 기록 데이터 활성화, 일별 세부 수준 리소스 수준 데이터 활성화 (서비스 : All)
   - 리전 : 리전별 확인 가능
