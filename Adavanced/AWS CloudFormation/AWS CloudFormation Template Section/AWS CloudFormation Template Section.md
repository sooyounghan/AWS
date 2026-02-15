-----
### CloudFormation 템플릿의 Section
-----
1. Format Version (Optional) : 템플릿 버전 명시
2. Description (Optional) : 템플릿에 대한 설명
3. Metadata (Optional) : 템플릿에 대한 추가 데이터 명시
4. Parameters (Optional) : 템플릿을 프로비전 하는 시점에 논리적 리소스에 전달하는 값을 정의
5. Rules (Optional) : 파라미터를 검증하는 정책 명시
6. Mappings (Optional) : 추가적으로 지정한 변수 Map으로 파라미터처럼 활용
7. Conditions (Optional) : 템플릿 프로비전 시 조건을 사용할 때 활용
8. Transform (Optional) : 템플릿에서 사용하는 매크로 등을 정의
9. Resources (필수) : 템플릿에서 프로비전할 리소스 정의
10. Outputs (Optional) : 템플릿 프로비전 이후 추가적으로 명시할 내용 정의 (추후 다른 스택에서 참조 가능)
