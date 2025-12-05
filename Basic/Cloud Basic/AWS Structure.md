-----
### AWS의 구조
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/e00585d5-70fe-458a-8282-88f2f2ccabdd" />
</div>

1. AWS의 리전(Region)
   - AWS의 서비스가 제공되는 서버의 물리적 위치
   - 전 세계에 흩어져 있으며, 큰 구분(동남아 / 유럽 / 북아메리카 등)으로 묶여 있음
   - 각 리전에는 고유 코드가 부여
     + 서울 Region : ap-northest-2
     + 미국 동부(버지니아 북부) Region : us-east-1
   - Region 별 가능한 서비스가 다름
   - Region 선택 시 고려할 점
     + 지연 속도
     + 법률(데이터, 서비스 제공 관련)
     + 사용 가능한 AWS 서비스

   - US-East-1 Region
     + 모든 AWS의 서비스가 최초로 서비스되는 Region
     + 기타 글로벌 서비스의 서비스 Region (예) 빌링, CloudFront 등)

2. 가용 영역 (Availability Zone)
   - Region의 하부 단위로, 하나의 Region은 3개 이상의 가용영역으로 구성되며, AZ라고 불림
   - 구성
     + 하나 이상 데이터 센터로 구성
     + AZ 간의 연결은 매우 빠른 전용 네트워크로 연결
     + 반드시 물리적으로 일정 거리 (몇 Km 이상) 떨어져 있음
       * 다만, 모든 AZ는 서로 100km 이내 거리에 위치
       * 여러 재해에 대한 대비 및 보안

   - 가용 영역의 위치 : 각 계정별로 AZ의 코드와 실제 위치는 다름
     + 예) 계정 Test1의 계정 AZ-A는 계정 Test2의 AZ-A와 다른 위치 (랜덤)
     + 보안 및 하나의 AZ 몰림 방지
<div align="center">
<img src="https://github.com/user-attachments/assets/7f11849b-84b5-42ef-91c6-6e3f10eb7eac" />
</div>

3. AWS의 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/b02df509-9608-4777-9cb1-6bb25f377ca0" />
</div>

4. 엣지 로케이션 (Edge Location)
<div align="center">
<img src="https://github.com/user-attachments/assets/b7ab5b4c-4d2b-48a9-9671-7fd440438719" />
</div>

   - AWS의 CloudFront(CDN)등의 여러 서비스들을 가장 빠른 속도로 제공(캐싱)하기 위한 거점
   - Global Accelerator와 유저를 연결하는 거점
   - 전 세계에 여러 장소에 흩어져 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/08a132d6-81c9-4834-bef9-538422bcc847" />
<img  src="https://github.com/user-attachments/assets/0011130f-60f3-42d7-b03c-a2c30c86d279" />
<img src="https://github.com/user-attachments/assets/88de5180-363a-4d3c-ac9a-bdf426a0d767" />
</div>

5. Global Service와 Region Service
   - 서비스의 종류 : AWS에는 서비스가 제공되는 지역 기반에 따라 글로벌 서비스와 리전 서비스로 불림
   - 글로벌 서비스 : 데이터 및 서비스를 전 세계의 모든 인프라가 공유 (예) IAM, Route53, WAF)
   - 지역 서비스 : 특정 리전을 기반으로 데이터 및 서비스 제공 (예) 대부분 서비스 - 리전 전체의 가용영역에서 서비스 또는 리전의 하나의 가용영역에서만 서비스)

6. AWS 서비스의 위치
   - Region
     + Region 전체 가용영역에서의 서비스
     + Region의 하나의 가용영역에서만 서비스

   - Global

7. ARN (Amazon Resource Names)
   - AWS 리소스에 부여되는 고유 ID
   - 형식 : "arn:[partition]:[service]:[region]:[account_id]:[resource_type]/resource_name_{qualifier}"
<div align="center">
<img src="https://github.com/user-attachments/assets/7d72468e-53ba-4d4e-af04-f593eaf3fda3" />
</div>

8. 정리 - AWS의 구성
   - AWS는 여러 Region과 3개 이상 가용 영역, Edge Location으로 구성
   - Region : AWS가 제공되는 서버의 물리적 위치
   - 가용영역 : 하나의 리전 안에 세 개 이상 가용영역이 있으며, 하나 이상의 데이터센터로 구성
   - Edge Location : AWS의 여러 서비스를 빠르게 제공하기 위한 거점 (캐싱)
   - AWS의 서비스는 글로벌 서비스와 리전 서비스로 구분
   - AWS의 모든 리소스는 ARN이 부여됨
   
