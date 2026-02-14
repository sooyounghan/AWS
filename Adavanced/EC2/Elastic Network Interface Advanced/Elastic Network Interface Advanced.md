-----
### Elastic Network Interface 고급
-----
1. Elastic Network Interface : 탄력적 네트워크 인터페이스는 VPC에서 가상 네트워크 카드를 나타내는 논리적 네트워크 구성 요소
2. Amazon EC2의 구성
<div align="center">
<img src="https://github.com/user-attachments/assets/e7340835-ed48-486d-b2f1-5e15b6320e6f" />
<img src="https://github.com/user-attachments/assets/a2d2689f-f1fe-49d5-9f89-0d05270d0382" />
</div>

3. ENI (Elastic Network Interface)
   - EC2의 가상의 LAN 카드
     + IP Address와 MAC Address 보유
     + ENI 하나 당 Private IP + 하나의 Public IP (Optional)
     + 필요에 따라서 한 개 이상 Private IP 부여 가능

   - EC2는 반드시 하나 이상의 ENI가 연결되어 있음
     + 제일 처음 EC2를 생성할 때 Primary ENI가 생성되어 연결됨
     + 추가로 ENI 연결 가능 : 즉, EC2는 한 개 이상 ENI 보유 가능
     + 추가적인 ENI의 경우, EC2와 같은 가용영역(AZ)이면 다른 서브넷에도 설정 가능

   - 💡 실질적으로 EC2의 서브넷 위치, 보안그룹 등 외부와 관련된 연결은 ENI 단위에서 결정

4. ENI의 활용
<div align="center">
<img src="https://github.com/user-attachments/assets/a1d84b22-34c3-48d5-b842-a8748b3a4d18" />
<img src="https://github.com/user-attachments/assets/862d09ef-597e-4e84-89e2-e3e6414447dc" />
<img src="https://github.com/user-attachments/assets/7b1550d4-d2e1-4af3-bc3a-9fe66516a265" />
</div>

5. 다중 ENI 아키텍쳐
   - 하나의 EC2 인스턴스에 여러 ENI 연동 가능
   - 사용 사례
     + ENI 교체를 통한 배포 / 업데이트
     + 관리를 위해 하나의 EC2 인스턴스에 다양한 접근 경로 설정
     + MAC Address에 종속된 라이센스 프로그램을 다양한 EC2에서 사용
   - 동시에 연동 가능한 ENI 숫자는 EC2 타입과 크기에 따라 다름
<div align="center">
<img src="https://github.com/user-attachments/assets/ff67d3ba-1c3a-4c0b-aedf-caf2e9367481" />
</div>

6. ENI Switching
<div align="center">
<img src="https://github.com/user-attachments/assets/4edf155f-7642-473d-8a9d-5c2b67455d2e" />
<img src="https://github.com/user-attachments/assets/f7c56a34-a3da-4ca5-9848-cd4055885a70" />
</div>

7. ENI와 보안 그룹
   - 보안 그룹 적용은 ENI 단위
     + 즉, 하나의 EC2 인스턴스에 다양한 보안 그룹으로 구성된 경로를 적용 가능
     + 예) Subnet A에서는 80번만 허용, Subnet B에서는 22번만 허용
   - 참고로 NACL은 Subnet 단위
<div align="center">
<img src="https://github.com/user-attachments/assets/d9e79136-d3bf-4655-a080-8541b2078ae2" />
</div>

8. 다양한 경로 설정
<div align="center">
<img src="https://github.com/user-attachments/assets/bbe9d47b-0788-47f5-9b0e-341f6d3b03d6" />
</div>

9. EC2와 Public IP
   - EC2 Public IP는 ENI와 Public IP ↔ Private IP 1:1 매칭 (NAT)
     + 이 레코드는 Elastic IP로 고정하지 않는 이상 영구적 레코드가 아님
     + EC2의 중지 → 재부팅 시, Private IP는 변경되지 않으나, Public IP는 변경
   - 인터넷에서 Public IP로 통신이 전달되면 IGW가 Static NAT을 통해 변환 후 전달
   - 즉, EC2의 OS는 절대로 Public IP를 알 수 없음
     + 즉, 어떤 문제도 EC2 내부에서 Public IP를 설정해서 풀 수 없음
     + Private IP의 경우 OS에서 확인 가능
   - EC2를 생성할 때 만들어지는 Primary ENI가 아닌 경우에 Public IP를 부여하려면 Elastic IP 활용 필요
   - EC2의 Public 통신
<div align="center">
<img src="https://github.com/user-attachments/assets/64237aa7-39bd-493d-9659-7b05e69adaa6" />
</div>

   - EC2의 중지 / 재시작
<div align="center">
<img src="https://github.com/user-attachments/assets/b6c6d8c8-2b24-40c7-9f37-8368b4479fdb" />
<img src="https://github.com/user-attachments/assets/3aa74307-9326-4365-8180-5e6a1bf705b5" />
<img src="https://github.com/user-attachments/assets/e1d5c8cb-780d-4ddf-a952-0b46229a1f78" />
</div>

   - Elastic IP
<div align="center">
<img src="https://github.com/user-attachments/assets/27aaa87d-a4ab-4219-ad58-ca1012113460" />
<img src="https://github.com/user-attachments/assets/6c148e6c-a6c6-415c-8954-df13a60307d4" />
<img src="https://github.com/user-attachments/assets/6e640d3e-7878-4b12-92c8-b03352761a51" />
<img src="https://github.com/user-attachments/assets/fc7e5435-0f7a-4439-bb8a-8ea66bce4f06" />
</div>

10. Source / Destination Check
    - ENI는 기본적으로 자신이 발생시켰거나, 자신이 대상이 아닌 트래픽은 무시
    - 단, 설정에 따라 해제 가능 : NAT Instance 등 자신을 위한 트래픽이 아닌 다른 대상에게 중계해주는 경우 해제 필요
    - ENI 단위
<div align="center">
<img src="https://github.com/user-attachments/assets/379058d8-bc66-4e09-b0e4-5faf5a1f998b" />
<img src="https://github.com/user-attachments/assets/d6ee7c83-0b1a-4048-b5f1-22055b394667" />
</div>

11. ENA vs EFA
    - ENA(Elastic Network Adapter) : EC2의 네트워킹 속도를 최대 100Gbps까지 향상 가능
      + 낮은 Latency, 높은 I/O
      + 지원하는 EC2 인스턴스만 사용 가능(Nitro-based)
<div align="center">
<img src="https://github.com/user-attachments/assets/c7c65283-d744-4402-b9cf-6cde9a4e2470" />
</div>

   - EFA (Elastic Fabric Adapter) : 주로 AI, ML, HPC 등을 위한 퍼포먼스를 지원하는 어댑터
     + 엄청나게 낮은 Latency와 높은 Throughput 지원
     + 주로 매우 높은 사양의 EC2 인스턴스만 지원 (최소 12xlarge, 보통 24xlarge 이상)

-----
### Demo - Multiple ENI
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/aed991fd-6c4f-4cc3-a342-9190388a962b" />
</div>

1. EC2 - 보안 그룹
   + Demo-MY-WEB - 인바운드 규칙 - 유형 : HTTP / 소스 : 0.0.0.0/0 - 태그 : Name / Demo-MY-WEB
   + Demo-MY-SSH - 인바운드 규칙 - 유형 : SSH / 소스 : 0.0.0.0/0 - 태그 : Name / Demo-MY-SSH
   + EC2 인스턴스 생성 : Demo-MY-ENI-TEST / 키 페어 생성 / 기존 보안 그룹 선택 : Demo-MY-SSH / 사용자 데이터
```
#!/bin/bash
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
echo "$INSTANCE_ID" >> /var/www/html/index.html
```

2. EC2 가용 영역 확인
3. 탄력적 IP 생성 : 탄력적 주소 할당
4. 네트워크 인터페이스 - 네트워크 인터페이스 생성 - Demo-WEB-WEB - 서브넷 : 가용 영역 서브넷 설정
   - 인터페이스 유형 : ENA
   - 보안 그룹 : Demo-WEB-ENI
   - 태그 : Name, Demo-WEB-ENI
   - Demo-MY-SSH 네트워크 인터페이스 Name 태그 이름 Demo-SSH-ENI 변경

5. 탄력적 IP - 작업 - 탄력적 IP 주소 연결 - 네트워크 인터페이스 - Demo-WEB-ENI
6. 네트워크 인터페이스 - Demo-WEB-ENI 선택 - 작업 - 연결 - VPC 선택, 생성된 EC2 선택
7. 웹 서버 동작 확인 : Demo-WEB-ENI Public IPv4 주소로 접속 확인
   - SSH 접속 : Demo-SSH-ENI Public IPv4 주소로 접속 확인 (MobaXTerm)
   - Session - SSH - Demo-SSH-ENI Public IPv4, ec2-user, 키 페어 연결

8. 리소스 정리 : EC2 정리 / 탄력적 IP 연결 해제 / 네트워크 인터페이스 삭제 (분리 후 삭제)
