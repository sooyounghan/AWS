-----
### S3 버전 관리
-----
1. 객체의 생성, 업데이트, 삭제의 모든 단계 저장 : 삭제 시에는 실제 객체 삭제 대신 삭제 마커 추가
2. 버킷 단위로 활성화 필요 (기본적으로 비활성화)
   - 중지 가능 (단, 비활성화는 불가능)
   - 한 번 버전 관리를 시작하면 비활성화 불가능 (버킷 삭제 후 재생성으로 해결 가능)
3. 수명 주기 관리와 연동 가능
4. MFA 인증 후 삭제 기능을 통해 보안 강화 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/cbc89e14-84d5-4733-9a35-1e37cbc556cf" />
<img src="https://github.com/user-attachments/assets/63eda804-8d8f-4a0c-9679-a0461fbe5670" />
<img src="https://github.com/user-attachments/assets/2f4ea2da-1456-434b-b283-ad6b11d74b7b" />
<img src="https://github.com/user-attachments/assets/f85f1672-040e-4361-9891-306b2e4ed39d" />
<img src="https://github.com/user-attachments/assets/ad849836-1182-4c0c-8d04-185b98d6ef88" />
<img src="https://github.com/user-attachments/assets/e968312b-db5b-4f9e-a8ad-86fd7b91c315" />
<img src="https://github.com/user-attachments/assets/3203aa2e-504f-42e1-9a0d-0a187733ea56" />
<img src="https://github.com/user-attachments/assets/9cddd794-9caf-451a-a1b3-732215859b7f" />
</div>

5. 유의사항
   - 중지 가능 (단, 비활성화는 불가능)
   - 한 번 버전 관리를 시작하면 비활성화 불가능 (버킷 삭제 후 재생성으로 해결 가능)
   - 모든 버전에 대해 비용 발생 : 10GB 객체의 버전이 5개 있다면, 총 50 GB에 대해 비용 발생
