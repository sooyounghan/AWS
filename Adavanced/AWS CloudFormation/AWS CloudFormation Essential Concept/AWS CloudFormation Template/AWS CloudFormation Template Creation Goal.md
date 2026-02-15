-----
### 탬플릿 생성 시 목표
-----
1. 최대한 외부 도움 없이 스스로 동작할 수 있을 것(Portability) (예) EC2 AMI 기본 값으로 프로비전 하는 리전의 Amazon Linux 2023 ID 가져오기)
2. 파라미터를 통해 커스터마이징 가능하도록 설정하여 범용성을 확보할 것
   - 예) 하나의 템플릿에 Prod / Dev 옵션에 따라 다르게 프로비전
   - 예) EC2 인스턴스 타입 등을 입력 받아 다양한 상황에 일괄적으로 사용할 수 있을 것

3. 가능한 독립적으로 만들 것 : 추후 다른 스택에서 템플릿을 참조할 수 있도록 모듈화 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/93b834a0-dd73-4515-82dd-b7c67c6064a6" />
</div>
