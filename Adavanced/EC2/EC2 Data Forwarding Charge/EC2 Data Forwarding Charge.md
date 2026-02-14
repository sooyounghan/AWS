-----
### AWS의 데이터 전송 비용
-----
1. 다양한 주체(On-Premise, 인터넷, EC2, AWS 서비스)에서 서로 데이터를 주고받을 때 발생하는 요금 정리
2. 공통 원칙
   - AWS로 들어가는 데이터 비용은 무료
   - 같은 가용영역 내부 통신은 무료
3. 설명 내용 중 세금은 제외
4. US-East 리전 기준 (리전 별 다를 수 있음)
5. 같은 리전
   - Internet Gateway를 통해 VPC에서 AWS Public Service (Amazon DynamoDB / S3 / CloudWatch)에 데이터 전송 시 비용 없음
   - 단, EC2 Instance에서 NAT Gateway를 통해 Internet Gateway에 접근 : $0.045 / Hour + $0.045 / GB

6. 다른 리전
   - Internet Gateway를 통해 VPC에서 AWS Public Service (Amazon DynamoDB / S3 / CloudWatch)에 데이터 전송 시 비용 : $0.02?/GB
   - 단, EC2 Instance에서 NAT Gateway를 통해 Internet Gateway에 접근 : $0.045 / Hour + $0.045 / GB

7. AWS 워크로드 간 데아터 요금
   - Amazon RDS (Primary → Secondary (서로 다른 가용 영역)) : Cross-AZ BackUp 비용은 무료 (서비스에서 자체 지원)
   - 이 외에는 비용 발생 (EC2 → 다른 가용 영역의 RDS, EC2 → 다른 가용 영역의 EC2 등의 Cross-AZ : $0.01 in/out)
   - 같은 가용 영역 간에는 무료
   - 같은 리전, 다른 VPC, 다른 AZ
     + 같은 가용 영역 주체끼리 통신 (VPC Peering) : 무료
     + 다른 가용 영역 주체끼리 통신 (VPC Peering) : 0.01 in/out
     + VPC Peering 자체는 무료

8. AWS 서비스 데이터 요금 (Transit Gateway)
   - $0.02 / GB
   - VPC 연결 당 1시간 기준 : $0.05/VPC 연결(1시간)
   - VPC 2개가 필요하므로 $0.1

9. AWS 서비스 데이터 요금 (다른 리전) : 리전 마다 금액이 다름 ($0.02?/GB)
   - VPC Peering : Inter-Region 비용 발생
   - 리전 별로 다름

10. AWS 서비스 데이터 요금 (On-Premise)
    - Direct Conenct : 시간 당 요금 / 처리량 당 요금 청구
    - Site to Site VPN : $0.05/hour, $0.09/GB

-----
### ALB와 NLB의 데이터 요금 정리
-----
1. 다양한 주체와 ALB / NLB 간 데이터 전송 요금 정리 : 기본적인 AWS의 비용 정책 확인 가능
2. 세금 제외
3. ALB Data 요금 정리
   - ALB에서 같은 VPC안의 트래픽은 In/Out 모두 무료
   - 다른 가용영역이라하더라도, 같은 VPC라면 무료
   - Internal VPC
     + 즉, 같은 VPC에 있으므로 AZ 상관없이 클라이언트 ↔ ALB ↔ 대상 모두 데이터 In / Out 비용 무료
     + 단, Public IP를 활용해 전송할 경우 $0.01/GB In/Out 발생 : 클라이언트와 ALB 모두 Public IP를 사용하므로 $0.01/GB In/Out X 2
   - Cross VPC
     + VPC가 다른 경우, 같은 가용 영역 : 같은 가용 영역이므로 무료
     + VPC가 같은 경우, 같은 가용 영역 : 무료
     + VPC를 넘어가더라도 같은 AZ에 있으면 데이터 In / Out 비용 무료
     + AZ가 다르다면, $0.01 GB In / Out X 2
   - Internet Facing
     + AWS 외부 인터넷에서 오는 트래픽은 무료
     + 내보내는 트래픽은 $0.09 / GB
   - Cross Region
     + Inter-Region 비용 발생
     + Region Out 트래픽 비용 적용 : 보통 $0.02/GB 지만 리전 별 상이
     + 예) AP-Northeast-2 → US-East : $0.08/GB

4. NLB Data 요금 정리
   - 💡 AZ간 통신 비용 : $0.01 / GB
   - ALB와 달리 AZ 간 통신 비용 발생
   - 그 외에 ALB Data 요금 발새과 모두 동일
   
