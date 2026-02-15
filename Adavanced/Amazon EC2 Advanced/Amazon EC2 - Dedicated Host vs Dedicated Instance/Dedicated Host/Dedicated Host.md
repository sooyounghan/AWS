-----
### Dedicated Host
-----
1. Amazon EC2 전용 호스트를 사용하면 Amazon EC2에서 Microsoft 및 Oracle 같은 공급업체의 적격 소프트웨어 라이센스를 사용할 수 있으므로, 고객이 자사의 보유 라이센스를 활용하는 유연성과 비용 효율성을 보장받음
2. AWS의 복원력, 간편성 및 탄력성 활용 가능
3. 고객에게 전용으로 제공되는 물리적 서버로, 회사 규정 준수 요건을 해결하는데 유용
4. 즉, 물리적으로 호스트 단위로 격리된 서버를 빌려서 그 안에서 EC2 실행
5. 전용 호스트(서버)를 빌려 쓰는 게념
   - 인스턴스 재부팅 시 확보한 호스트에서 다시 동작
   - 인스턴스 배치 컨트롤 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/989115f9-4a67-4149-8fb9-af30fd334771" />
<img src="https://github.com/user-attachments/assets/a92e320f-ce0f-466e-b011-2a8ce2e13811" />
<img src="https://github.com/user-attachments/assets/04ed999f-eaac-4a2f-9d25-a029e9df3b6d" />
</div>

6. 호스트 단위 빌링
   - 패밀리 선택 후, 호스트 당으로 요금 지불
   - 하나의 호스트 안 인스턴스 숫자와 관계 없이 요금 지불

7. 내 라이센스를 직접 활용 가능 (Bring Your Own License 가능) : AWS License Manager 등 활용
