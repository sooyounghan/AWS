-----
### ALB 리스너와 규칙
-----
1. 리스너 (Listener) : 프로토콜과 포트를 기반으로 ALB로 들어오는 요청을 확인하는 주체 (ALB 별로 최소 하나 이상 리스너 필요)
2. 규칙 (Rule) : ALB가 요청을 분배하는 기준이 되는 규칙 (조건, 작업, 우선순위 구성)
<div align="center">
<img src="https://github.com/user-attachments/assets/ca4b616f-930b-4b67-9ec4-9ba77a309fb4" />
</div>

3. ALB 규칙
   - 조건 (모두 만족 필요) : 호스트 헤더 / 경로 / HTTP Method / Source IP / HTTP Header / QueryString
   - 작업 유형
     + 대상 그룹(최대 5개) 전달 : 가중치 (1 ~ 999)
     + URL Redirection (URI만 가능, 전체 URL에 대해서 가)
     + 고정 응답

   - 규칙 순위 (1 ~ 50,000)

4. 실습
   - EC2 접속 - 로드밸런싱 - 대상 그룹 - 대상 그룹 생성 : 대상 그룹 이름(demo-rule) / 프로토콜 : 포트 (HTTP / 80)
   - 로드 밸런싱 - 로드 밸런서 - 로드 밸런서 생성 - Application Load Balancer 생성 - 로드 밸런서 이름 : demo-rule / 가용 영역 및 서브넷 모두 선택
     + 리스너 및 라우팅 : 리스너 - HTTP (80) / 대상 그룹으로 전달 - demo-rule - 로드 밸런서 생성 완료
     + 리스너 추가 : HTTP (8080) / 고정 응답 반환
     + 리스너 편집 : 리스너 편집 - 규칙 추가 - 이름 : my-rule / 경로 : Source IP - /image 
