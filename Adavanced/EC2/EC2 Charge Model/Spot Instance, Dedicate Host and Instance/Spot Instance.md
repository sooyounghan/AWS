-----
### 스팟 인스턴스
-----
1. AWS에서 보유중인 남는 인스턴스를 저렴한 가격으로 제공 : 가용영역 별, 인스턴스 유형 별 다른 풀로 관리
2. 최대 90%까지 절약 가능
   - 가격은 상황에 따라 변동 (단, 항상 가격은 On-Demand 이하 보장)
   - 선택적으로 최대 가격 지정 가능 (to On-Demand)

3. 단, 인스턴스가 언제 종료될지 예측 불가능 (Spot Instance Interruption)
   - 인스턴스만 종료됨 (혹은 중지 / 종료 / Hibernate 중 선택)
   - 인스턴스가 종료되는 경우
     + AWS가 인스턴스의 요청이 늘어날 때
     + AWS의 남는 인스턴스 풀이 모자를 때
   - 2분 전에 종료 알람 (안오는 경우가 존재)
<div align="center">
<img src="https://github.com/user-attachments/assets/41aac427-36dc-4ba0-ac46-a89ef99554c4" />
<img src="https://github.com/user-attachments/assets/f09a48bb-8ea5-40b0-8678-4b2cfd87d811" />
<img src="https://github.com/user-attachments/assets/cc496093-79d7-4e3d-8209-2dd8614d15ed" />
<img src="https://github.com/user-attachments/assets/936a2890-e584-4569-bfa2-6fa10da6a160" />
<img src="https://github.com/user-attachments/assets/d198dc5c-9f3e-436d-801d-a4139afc0b2f" />
</div>

4. 전용 호스트 (Dedicated Host)
<div align="center">
<img src="https://github.com/user-attachments/assets/00af97ca-69f3-4a53-a0c7-aecb9bc0bc4d" />
<img src="https://github.com/user-attachments/assets/8c8cd070-449e-43e0-9a26-968dc47f89a9" />
<img src="https://github.com/user-attachments/assets/0c1fb4f5-49ab-4906-9e6f-ee76a27bda38" />
</div>

   - Amazon EC2에서 마이크로소프트 및 오라클 같은 공급업체의 적격 소프트웨어 라이센스를 사용할 수 있으므로, 고객이 자사 보유 라이센스를 활용하는 유연성과 비용 효율성을 보장받으면서 AWS의 복원력, 간편성 및 탄력성을 활용할 수 있음
   - Amazon EC2 전용 호스트는 고객에게 전용으로 제공되는 물리적 서버로, 회사 규정 준수 요건을 해결하는데 유용
   - 💡 물리적으로 호스트 단위로 격리된 서버에서 EC2 실행
   - 전용 호스트(서버)를 전세 내서 빌려쓰는 개념
     + 인스턴스 재부팅 시 확보한 호스트에서 다시 동작
     + 인스턴스 배치 컨트롤 가능

   - 호스트 단위 빌링
     + 패밀리 선택 후, 호스트 당 요금 지불
     + 하나의 호스트 안에 인스턴스 숫자와 관계 없이 요금 지불
   - 내 라이선스를 직접 활용 가능 (Bring Your Own License 가능) : AWS License Manager
<div align="center">
<img src="https://github.com/user-attachments/assets/55e413c4-8a5e-43b0-96f6-9ab4f91848e1" />
<img src="https://github.com/user-attachments/assets/b200bbc5-e790-482b-ad08-48357317a9b5" />
<img src="https://github.com/user-attachments/assets/6361b47a-e354-4ed7-b26e-f7c8dcb6708d" />
</div>

5. 전용 인스턴스 (Dedicated Instance)
   - 단일 고객 전용 하드웨어의 VPC에서 실행되는 Amazon EC2 인스턴스
   - 전용 인스턴스는 호스트 하드웨어 수준에서 다른 게정 AWS 계정에 속하는 인스턴스로부터 물리적으로 격리됨
   - 💡 물리적으로 인스턴스 단위로 격리된 서버에서 EC2 실행
   - 한 게정에서 전용 호스트를 잠시 빌려서 사용
     + 인스턴스 재부팅 시 다른 전용 호스트에서 인스턴스가 동작할 가능성 존재
     + 내 계정의 전용 인스턴스가 아닌 인스턴스도 섞여 들어갈 수 있음
     + 인스턴스 배치 컨트롤 불가능
   - 인스턴스 단위 빌링
     + 전용 요금(Dedicated Instance의 숫자와 관계 없이, 단 하나라도 사용 시 부과) : 시간당 $2
     + 시간 당 인스턴스 사용료 : 약 10% 더 비싼 요금
   - 내 라이선스를 직접 활용 불가능 (Bring Your Own License 불가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/c2c1d725-4705-4c33-bb44-454f5071a625" />
</div>

6. 전용 호스트 vs 인스턴스
<div align="center">
<img src="https://github.com/user-attachments/assets/a2f56d5f-100f-4982-a6a4-1a64eb8f89ed" />
</div>

   - 전용 인스턴스
     + 인스턴스 간 간섭을 배제하거나 보안 규정 등 이유로 사용
     + 기본요금 + 사용 인스턴스 당 비용 지불

   - 전용 호스트
     + 라이센스 이슈와 인스턴스 간 Latency를 줄이기 위해 사용 : 자신의 라이센스 활용 가능
     + 호스트 비용 지불
       * 이후, 호스트를 어떻게 이용하는지는 마음대로 가능
       * 인스턴스 개수 제한 없음 (정해진 한도 내)

7. EC2 요금
   - 요금 순서 : 스팟 인스턴스 < 예약 인스턴스 < On-Demand < 전용 호스트
   - EC2 요금 모델은 EBS와는 별도 : EBS는 사용한 만큼만 지불
   - 기타 데이터 통신 등의 비용은 별도 청구 (참고 : AWS는 AWS 외부로 나가는 트래픽에 대해서만 요금 부과)
