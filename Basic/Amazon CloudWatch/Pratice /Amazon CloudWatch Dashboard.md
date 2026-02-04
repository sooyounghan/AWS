-----
###  Amazon CloudWatch Dashboard 실습
-----
1. 순서
   - CloudWatch Dashboard에 AutoScaling Group의 다양한 지표표시
   - Custom Dashboard 로직을 통해 직접 인스턴스 시작 / 재부팅 / 종료 및 AutoScaling의 Capacity 변경
   - 만든 대시 보드 공유

2. 알아둘 점
   - CloudFormation을 통해 예제 프로비전 예정 (CloudFormation : 코드로 인프라를 관리하는 IaaC (Infrastrcuture as a Code) 서비스
   - Lambda를 활용한 커스텀 로직 처리 (Lambda : AWS의 Serverless 컴퓨팅 서비스로, 간단한 로직을 서버 없이 처리)
<div align="center">
<img src="https://github.com/user-attachments/assets/fc756bc8-3d27-4f60-a140-e2e1a31cb4d7" />
</div>
