-----
### CloudFormation Template
-----
1. JSON 또는 YAML 형식의 텍스트 파일로 AWS 인프라를 구성하는 리소스들의 청사진 : 가독성 측면에서 YAML 형식 추천 (추후 JSON과 상호 변환 가능)
2. 텍스트 파일
   - 에디터로 수정 가능
   - 소스 컨트롤 가능
   - LLM으로 생성 가능 (추천)

3. 다양한 섹션과 기능으로 구성
   - Resource 섹션은 필수이며, 나머지는 선택
   - 실질적으로 Resource, Parameter, Intrinstic Function 3가지 요소가 가장 기초 단위

4. 특수한 상황을 제외하고 스택을 생성하는 IAM 사용자의 권한을 활용하여 프로비전
