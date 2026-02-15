-----
### AWS CloudFormation의 주요 개념
-----
1. 템플릿 (Template) : JSON 또는 YAML 형식의 텍스트 파일로 AWS 인프라를 구성하는 리소스들의 청사진
   - 프로비전 할 리소스를 정의하고 리소스 간 관계 정의
   - 다양한 섹션들과 기능들로 구성
   - 재사용 가능

2. 논리적 리소스 : 템플릿을 기반으로 실제 프로비전되는 리소스를 대표하는 논리적 개념 (템플릿 + 사용자의 파라미터 / 프로비전 시점의 값들)
3. 스택(Stack) : CloudFormation으로 리소스를 프로비전할 때 관련된 리소스를 묶은 단위
   - 스택 ID와 이름으로 구분 : ID는 글로벌 고유 값 / 이름은 리전 단위 고유 값
   - 논리적 리소스의 구성을 실제 리소스로 반영 : 즉, 논리적 리소스가 변경되면 실제 리소스도 변경
   - 삭제 시 스택 단위로 삭제 가능하며, 스택 삭제 시 모든 리소스 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/37f92fde-b820-4bad-893f-23c782ff7472" />
<img src="https://github.com/user-attachments/assets/7bbd6f98-74da-4008-9caa-11c9288faa73" />
<img src="https://github.com/user-attachments/assets/b28838b8-8bd3-4e5f-b91e-7bfd16206d3b" />
<img src="https://github.com/user-attachments/assets/6f168204-a0cc-4ed2-961c-de05dc99a291" />
<img src="https://github.com/user-attachments/assets/7091d726-c3fd-47e2-863d-a785171b7fa5" />
</div>
