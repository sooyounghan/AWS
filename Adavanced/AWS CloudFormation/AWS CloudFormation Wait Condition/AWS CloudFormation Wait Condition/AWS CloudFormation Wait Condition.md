-----
### Wait Condition
-----
1. 리소스  프로비전 중 특정 주체가 로직을 수행 후 성공을 알릴 때까지 리소스 프로비전을 멈추는 구성 (예) EC2 인스턴스를 프로비전할 때, 다양한 설정 작업(웹 서버 설치 등) 이후 완료 처리)
2. N개의 성공 신호를 X초까지 대기 가능 (최대 12시간)
   - 실패 신호를 받으면 실패 처리
   - X초 안에 N개의 성공 신호를 받지 못하면 실패 처리 (Timeout)

3. 두 가지 방법
   - 별도의 AWS::CloudFomration::WaitCondition 리소스를 생성 : 다수의 리소스를 컨트롤 할 경우
   - 리소스의 CreationPolicy에서 설정 : EC2/AutoScale Group의 경우 추천
<div align="center">
<img src="https://github.com/user-attachments/assets/74853124-6a78-4def-86a1-790cd7f718dd" />
<img src="https://github.com/user-attachments/assets/25b7c8e3-8a2f-46c8-98c6-89f4c25169ce" />
<img src="https://github.com/user-attachments/assets/d77e2d0d-dbc3-42d7-8961-13f7fe9b128b" />
</div>
