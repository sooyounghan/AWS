-----
### Mapping 섹션
-----
1. CloudFormation에서 미리 Map으로 데이터를 정의할 수 있는 섹션 : 프로비전하는 상황에 따라 알맞은 값을 선택할 수 있도록 미리 데이터 저장
2. Fn::FindInMap Instrisic Function으로 활용 : Fn::FindInMap: [ MapName, TopLevelKey, SecondLevelKey ] / !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]
3. 주요 사용 사례
   - 리전별 AMI 선택
   - Route 53 Hosted Zone 선택 등
<div align="center">
<img src="https://github.com/user-attachments/assets/8c79b516-25f4-43ee-8c9c-c44a020180b8" />
</div>
