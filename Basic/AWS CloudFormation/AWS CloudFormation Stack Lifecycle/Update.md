-----
### 스택의 생명 주기 - 업데이트
-----
1. 기존 템플릿 기반으로 파라미터만 변경 또는 새로운 템플릿 기반 업데이트 (S3 경로를 지정 / 직접 템플릿 업로드(S3 업로드) 또는 Git에서 동기화)
2. 가능한 설정 : 생성과 거의 동일
3. Stack Policy : 스택 리소스의 불필요한 업데이트를 방지하기 위한 일종의 보호 장치
   - 💡 설정 시, 명시적으로 허용한 리소스를 제외하고는 업데이트 불가능
   - 💡 한 번 설정 시 삭제 불가능
   - 주요 사용 사례 : 특정 리소스만 업데이트 허용 / 특정 리소스 삭제 방지 (스택 삭제는 가능)

4. 변경 세트로 변경 내용에 대해 미리 확인 가능 : 단, 변경 성공 여부를 보장하진 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/793d9906-2f93-417c-b87d-3f1290af7e22" />
<img src="https://github.com/user-attachments/assets/2bccf81c-a7ce-4897-9265-5ee47b5f4faf" />
<img src="https://github.com/user-attachments/assets/368c8b9b-6f3a-4831-be1d-d3cea220bc47" />
</div>
