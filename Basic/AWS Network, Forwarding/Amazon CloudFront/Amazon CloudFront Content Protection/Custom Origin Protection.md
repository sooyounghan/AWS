-----
### Cusstom Origin 보호
-----
1. 방법 1 : Custom Header 활용 (CloudFront에서 Header 생성 : Origin에서 해당 Header가 없으면 거부)
2. 방법 2 : Origin에서 CloudFront IP를 제외한 모든 트래픽을 차단
<div align="center">
<img src="https://github.com/user-attachments/assets/6ab39fe7-64a9-4ee0-a8eb-b78cc37deca5" />
<img src="https://github.com/user-attachments/assets/675406bc-ccde-4f27-a3b6-1ce721246e81" />
</div>

-----
### Demo
-----
1. EC2 새성
2. CloudFront Distribution 생성
3. 보안그룹 설정을 통한 Origin 보호
