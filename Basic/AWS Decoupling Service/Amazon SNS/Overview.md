-----
### Amazon SNS (Simple Notification Service)
-----
1. 애플리케이션 간 (A2A) 및 애플리케이션과 사용자 간(A2P) 통신 모두를 위한 완전관리형 메시징 서비스
2. Pub / Sub 기반 매세징 서비스 : 💡 하나의 메세지를 여러 서비스에서 처리
3. 주요 사용 사례
   - 하나의 메세지를 다수의 주체에서 처리하고 싶을 때 (Fan-Out)
   - 서비스에서 서비스로 메세지를 전달하고 싶을 때
   - 메세지를 임시로 저장하고 있거나 재생하여 동일한 로직을 다시 처리하고 싶을 때(FIFO)

4. 사용 방식
   - 하나의 토픽을 여러 주체가 구독
   - 토픽에 전달된 내용을 구독한 모든 주체가 받아 처리 : 하나의 메세지를 여러 주체에서 처리
   - 다양한 프로토콜 및 서비스 지원(이메일, HTTP(S), SQS, SMS, Lambda)
   - 메세지 리플레이 기능 제공(FIFO)
<div align="center">
<img src="https://github.com/user-attachments/assets/3acdc145-986d-4a4d-945d-197dc66eb4f2" />
<img src="https://github.com/user-attachments/assets/2cc2dce9-f895-42a0-b39b-98fa9fd1273f" />
<img src="https://github.com/user-attachments/assets/8290689d-5874-4c70-b1ef-fdac9ed800f7" />
</div>

5. 정리
<div align="center">
<img src="https://github.com/user-attachments/assets/a4c19102-d616-4984-8afb-b8e27ace515d" />
</div>

6. CloudWatch 메트릭 기반 이벤트 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/14794acc-1304-4164-bcde-ffdc56fa1fcb" />
</div>

7. Large 이상 EC2 자동 정지 / 기록
<div align="center">
<img src="https://github.com/user-attachments/assets/483dde25-fdf3-493a-8f87-147fdddffdd0" />
</div>

8. Public 서비스 : 즉, AWS 외부에서 인터넷을 통해 사용 가능
9. Serveless 서비스로 분류 : 고가용성이 자체적으로 확보되어 있음
10. 요청 횟수, 메세지 크기 등 과금
