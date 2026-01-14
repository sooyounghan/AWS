-----
### Amazon S3
-----
1. 객체 스토리지 서비스 : 파일 보관만 가능 (↔ Block Storage Service (EBS), Network File Storage (EFS)) - 애플리케이션 설치 불가능
2. 글로벌 서비스 (단, 데이터는 Region에 저장)
3. 무제한 용량 (하나의 객체는 0 Byte ~ 5 TB의 용량)
4. Bucket
   - S3의 저장공간을 구분하는 단위
   - 디렉토리 / 폴더와 같은 개념
   - 버킷 이름은 전 세계에서 고유 값 : Region에 관계 없이 중복된 이름이 존재할 수 없음

5. S3 객체의 구성
   - Owner : 소유자
   - Key : 파일의 이름
   - Value : 파일의 데이터
   - Version Id : 파일의 버전 아이디
   - Metadata : 파일의 정보를 담은 데이터
   - ACL : 파일의 권한을 담은 데이터
   - Torrents : 토렌트 공유를 위한 데이터

4. S3의 내구성
   - 최소 3개의 가용 영역(AZ)에 데이터를 분산 저장 (Standard의 경우)
   - 99.99999999999% 내구성
     + 0.00000000001%의 확률로 파일을 잃어버릴 수 있음
   - 99.99% SLA 가용성 (스토리지 클래스에 따라 다름)

5. 보안 설정
   - S3 모든 버킷은 새로 생성 시 기본적으로 Private (비공개) : 따로 설정을 통해 불특정 다수에게 공개 가능 (예) 웹 호스팅)
   - 보안 설정은 객체 단위와 버킷 단위로 구성
     + Bucket Policy : 버킷 단위
     + ACL (Access Control List) : 객체 단위
   - MFA를 활용해 객체 삭제 방지 가능
   - Versioning을 통해 파일 관리 가능
   - 액세스 로그 생성 및 전송 가능 : 다른 버킷 혹은 다른 계정으로 전송 가능

6. 비용 종류
   - S3 데이터 보관 비용(GB 당 0.023 USD)
   - S3 데이터 요청 비용(PUT, COPY, POST, LIST 1,000개 당 0.005 USD)
   - 데이터 전송 비용 (GB당 0.09 USD)
   - 기타 : 암호화 / 스토리지 관리 / 복제 등

7. Demo - S3 버킷 생성, 업로드, EC2 유저 데이터 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/5caf1995-a0ed-444d-96ef-d54ecd525639" />
</div>

   - S3 버킷 생성
     + 버킷 만들기 - demo-my-s3-bucket-{계정 ID}
     + ACL 비활성화 (기본값)
   
   - 파일 업로드
     + 업로드 - 파일 추가 - 해당 파일 업로드

   - S3 접근 권한을 가진 EC2 IAM 역할 생성
     + 역할 - 역할 생성 - EC2 - AmazonS3FullAccess 선택 - demo-ec2-role-s3-fullacess
   - EC2 시작 구성 생성 : 유저데이터에서 S3 파일을 가져와 적용하도록 설정
     + demo-s3-test
     + 키 페어 없이 계속 진행
     + 기존 보안 그룹 선택 : default (인바운드, 아웃바운드 모두 개방)
     + 고급 세부 설정
       * IAM 인스턴스 프로파일 : demo-ec2-role-s3-fullacess
       * 사용자 데이터
```
#!/bin/bash
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
aws s3 cp s3://[버킷명]/index.html /var/www/html  --region ap-northeast-2
```

   - 리소스 정리 : EC2 인스턴스 종료 / S3 버킷 종료 (버킷 비우기 먼저 실시)
