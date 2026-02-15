-----
### 계층 구조
-----
1. Parameter Store의 Key를 계층 구조로 관리 가능 : Slash(/)를 기반으로 각 계층 구분
<div align="center">
<img src="https://github.com/user-attachments/assets/d377292c-6c57-43df-b140-2f1c4a9c4b90" />
</div>

   - 첫 글자가 /로 시작하지 않을 경우 : 게층 구조 없는 파라미터로 생성 가능

2. 활용
   - 계층 구조 단위로 조회 가능 (GetParametersByPath) (예) aws ssm get-parameters-by-path --path /myproject/prod/db)
<div align="center">
<img src="https://github.com/user-attachments/assets/b12a89f2-5c14-4ae3-bf9a-eae4587f7066" />
</div>

   - 계층 구조 단위 권한 부여 가능 (예) allow only "arn:aws:ssm:us-east-2:123456789012:parameter/myproject/dev")
