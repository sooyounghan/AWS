-----
### IAM JSON 정책 구성 요소
-----
1. Version : JSON 정책 버전으로, "2012-10-17"로 설정 권장 (최신)
2. Statement : 아래 내용을 담은 논리적 단위
   - Sid (Optional) : Statement ID
   - Effect : Allow 또는 Deny
   - Principal : 주로 리소스 기반 정책에서 '누가'에 해당하는 값 (예) S3 버킷 정책에서 '누가' 이 파일에 접근할 수 있는가에 대한 ARN)
   - Action : Allow 또는 Deny 할 행동
   - Resource : Action의 대상이 되는 리소스
   - Condition (Optional) : 이 정책이 적용되는 조건
<div align="center">
<img src="https://github.com/user-attachments/assets/580857b5-0da9-4edc-9bb5-64861ac298dd" />
</div>
