-----
### AWS CodeDeploy
-----
1. AWS의 리소스 / On-Premise 리소스에 배포를 담당하는 서비스 : EC2 / ECS / Lambda 또는 On-Premise 등의 리소스에 배포
2. 거의 대부분의 컨텐츠 배포 지원 : 코드 / Lambda / Web / 패키지 / 미디어파일 등
3. 배포를 위한 소스로 S3, GitHub, Bitbucket 등 활용
4. AWS의 Autoscale / ELB와 연동 지원 (예) Autoscale에서 EC2 신규 프로비전 CodeDeploy로 배포 완료 후 내보내기)
5. 💡 CodeDeploy Agent 활용 (조건 : Outbound 443 Open / CodeDeploy 서비스 접근 가능)
6. 롤백 지원 : 배포에 실패하거나 CloudWatch 경보가 발생하는 경우
7. 배포 방식 (In-Place / Rolling)
   - 💡 기존에 배포된 리소스를 활용하는 배포 방식
     + In-Place : 기존에 배포된 리소스를 인프라 교체 없이 업데이트
     + Rolling : 기존에 배포된 리소스를 점진적으로 새로운 버전의 리소스로 교체
   - 특징
     + 한꺼번에 업데이트 하는 비율에 따라 All-At-Once / Half-At-A-Time / One-At-A-Time 등
     + 업데이트가 비교적 빠르고 추가 비용이 없음
     + 애플리케이션의 가용성에 영향 / 롤백이 어려움

   - In-Place Deployment
<div align="center">
<img src="https://github.com/user-attachments/assets/29b459d5-f4ce-4469-a06e-d32a354175f6" />
<img src="https://github.com/user-attachments/assets/a508182b-7703-4efd-b296-8b004aa2fa56" />
<img src="https://github.com/user-attachments/assets/9a049289-a465-471e-b15f-880bb2fc8e03" />
<img src="https://github.com/user-attachments/assets/a3480b1d-6a16-40e1-9d22-d0255605beef" />
<img src="https://github.com/user-attachments/assets/83aa49f3-33fa-4a73-8dde-1b243221955d" />
<img src="https://github.com/user-attachments/assets/1733f3de-96dc-499f-aac6-531e8cbb9d3c" />
<img src="https://github.com/user-attachments/assets/2ad9cb90-d803-4ddb-aad5-e370360b1c13" />
</div>

   - Rolling Deployment
<div align="center">
<img src="https://github.com/user-attachments/assets/05efcad1-6be6-43db-84df-acb6b6979737" />
<img src="https://github.com/user-attachments/assets/1d3cc54f-2c09-4696-8bdf-e33ce081c1fb" />
<img src="https://github.com/user-attachments/assets/01824019-6e90-450f-b6d8-2607721d4f7b" />
<img src="https://github.com/user-attachments/assets/a677c2b4-8c5b-4335-a413-bcc228149909" />
<img src="https://github.com/user-attachments/assets/73965756-3ab5-4085-8be1-a0e693c2aaa0" />
<img src="https://github.com/user-attachments/assets/123e8863-903a-42cf-8cfb-117f40f73c8b" />
<img src="https://github.com/user-attachments/assets/30271601-830d-4b42-8f06-080b6cec1ef8" />
</div>

8. 배포 방식 (Blue / Green)
   - 기존 리소스(Blue)와 완전히 별도로 새로운 리소스(Green)를 프로비전하고 트래픽을 교체하는 방식
   - 특징
     + Green 리소스를 충분히 테스트할 수 있으며, 롤백도 쉬워 안정적
     + 트래픽의 중단 최소화
     + 추가 비용
<div align="center">
<img src="https://github.com/user-attachments/assets/7c9ce601-2dc9-4741-8111-7e1dcf6f6656" />
<img src="https://github.com/user-attachments/assets/90d68410-87b7-43a7-aaa7-eaf2c5ac3457" />
<img wsrc="https://github.com/user-attachments/assets/0fc8c007-85cc-42ef-a855-e0dcadc1bd44" />
<img src="https://github.com/user-attachments/assets/3a618532-1500-4153-9e9d-99b8dcecbbab" />
</div>

9. 배포 방식 (Canary)
    - Blue / Green 배포에서 트래픽 전환을 점진적으로 하는 방식 : 일정 비율로 트래픽을 전환하여 예기치 못한 장애에 대비
    - 특징 : Blue / Green 보다 안정적이나 배포에 오랜 시간이 걸리고 복잡도 증가
<div align="center">
<img src="https://github.com/user-attachments/assets/93758a55-bf9d-4129-bf0b-32b0bfb6d30b" />
<img src="https://github.com/user-attachments/assets/eb1d96e4-a48e-466a-862e-0c410359bbf1" />
<img src="https://github.com/user-attachments/assets/bf426d21-a070-443b-8631-7a9c51f04e89" />
<img src="https://github.com/user-attachments/assets/b3c7fe74-aec9-4028-8373-84812654325b" />
<img src="https://github.com/user-attachments/assets/8392268b-9f65-4677-9322-e984cfa75a62" />
</div>

10. AWS CodeDeploy 배포 방식
    - EC2 / On-Premise : In-Place / Blue, Green
      + All-At-Once / Half-At-A-Time / One-At-A-Time
    - ECS / Lambda : Canary / Blue Grren
      + All-At-Once / X-Percent-Every-N-Minute

11. AppSpec.yml
    - CodeDeploy에서 수행할 내용을 정의한 문서 : yml 형식
    - 정의 가능한 방법
      + 배포 방법
      + 각 스테이지 별 수행 내용 정의
    - 구성 요소
<div align="center">
<img src="https://github.com/user-attachments/assets/d70c03ea-b70b-47fd-ab9c-72728ad063c4" />
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/ee43108b-02aa-4e34-ac65-8eb12ee5eb20" />
<img src="https://github.com/user-attachments/assets/eb15c258-3ded-41f2-8cfc-5071d00dfd86" />
</div>

12. 구성
    - Application : CodeDeploy의 구분 단위 (EC2 / On-Premise, Lambda, ECS 플랫폼 지원)
    - 배포 그룹 : 배포할 대상을 묶은 그룹
      + Application 종류에 따라 다른 구성 (예) EC2 / On-Premise Application 이라면 환경은 Autoscale Group, EC2 Instance, On-Premise 지원)
      + 별도로 트리거, 롤백, 경보 등의 설정 가능

13. 롤백 : 배포에 실패하거나 사용자가 원할 때 롤백 가능
    - 자동 롤백 : CloudWatch Alarm으로 트리거되어 특정 임계치가 넘어가면 롤백
    - 수동 롤백 : 수동으로 이전 버전으로 롤백
    
