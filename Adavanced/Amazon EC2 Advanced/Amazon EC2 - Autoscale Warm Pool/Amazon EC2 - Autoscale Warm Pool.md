-----
### Autoscale Warm Fool
-----
1. Autoscale에서 확장에 필요한 인스턴스를 미리 준비하는 서비스
   - 인스턴스의 준비 기간이 매우 긴 상황에서 Autoscaling의 확장 시 인스턴스를 빠르게 준비 가능 (즉, 인스턴스 준비 기간이 길지 않을 경우 체감 효과는 미비)
   - 대표 사용 사례 : 볼륨 용량이 매우 커서 AMI → 인스턴스 준비 기간이 오래 걸리는 경우

2. Warm Pool : Autoscale 상황을 위해 미리 준비해둔 인스턴스의 Pool
   - 3가지 상태
     + Running : 인스턴스 비용을 포함한 모든 비용 지불
     + Stopped / Hibernate (지원하는 경우) : EBS 비용과 IP 비용
   - EC2 뿐만 아니라 EKS / ECS 등에서도 활용 가능

3. EC2의 생명 주기
<div align="center">
<img src="https://github.com/user-attachments/assets/7b502bf8-372c-4722-b373-87ff487a6226" />
</div>

4. Hibernate (최대 절전 모드)
   - 단순하게 인스턴스만 중지하는 것이 아니라 메모리의 상태 역시 데이터화해서 저장해둔 상태
   - 즉, 애플리케이션을 처음부터 시작하는 것이 아닌 중단 시점부터 재개 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/f4c40712-4ec0-48b4-972e-dc2f78542305" />
</div>

   - 일반 모드
<div align="center">
<img src="https://github.com/user-attachments/assets/14cf878b-cd68-4ebd-8a1f-c42a86b05110" />
</div>

   - Hibernate (최대 절전 모드)
<div align="center">
<img src="https://github.com/user-attachments/assets/04f1182f-7d8c-48fa-9d72-9721986ced31" />
<img src="https://github.com/user-attachments/assets/ada39f8f-8f05-4fa8-bd47-d1869759a939" />
</div>

5. 재활용 정책
   - 기본적으로 Scale-In 활동 시 인스턴스 삭제
   - 단, Reuse Policy를 설정하여 다시 Warm Pool로 복귀시켜 재사용 가능 (💡 Warm Pool 생성 할 때 설정을 제외하고 AWS CLI / SDK로만 가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/3fe36cd6-ecc9-4ce1-a393-6b24f25ebc01" />
<img src="https://github.com/user-attachments/assets/04d97726-1d2c-47cd-98f3-7fde57a322f7" />
<img src="https://github.com/user-attachments/assets/924df15d-4e6c-4617-aef0-c3e669105c74" />
<img src="https://github.com/user-attachments/assets/1c87b004-925c-40ac-848d-88125142c31a" />
<img src="https://github.com/user-attachments/assets/fcc0f574-f828-4e01-9f86-e35ba4960c2c" />
</div>

6. 크기
   - 기본적으로 크기는 Autoscale 그룹의 Max Capacity - Desired Capacity (예) Min / Desired / MAX = 1 / 3 / 8이면 Warm Pool 사이즈는 5)
   - MaxGroupPreparedCapacity를 별도로 설정하여 크기 고정 가능 (예) Desired / Max가 1000 / 1500이면 Warm Pool 크기보다 필요보다 많을 수 있음)

7. 플로우
   - Lifecycle Hook을 활용해 추가적 설정 가능
     + EC2_INSTANCE_LAUNCHING : Warm Pool 진입 시 / 실제 Live로 올라갈 때 2번 실행
     + EC2_INSTANCE_TERMINATING : 종료되거나 Warm Pool로 돌아갈 때 실행
   - 💡 Lifecycle Hook이 없다면, Warm Pool 진입 후 Userdata 수행 중 Stop / Hiberate 될 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/3d138743-9bfb-4f3b-8627-2c62fea9da4a" />
</div>

8. Demo
   - EC2 IAM 역할 생성 (Lifecycle Hook 종료 권한)
     + IAM - 역할 - 역할 생성 - EC2 - 역할 이름 : ec2-role-for-warmpool - 역할 생성
     + ec2-role-for-warmpool - 권한 추가 - 인라인 정책 생성 - JSON - 정책이름 : demo-allow
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "editor",
            "Effect": "Allow",
            "Action": "autoscaling:CompleteLifecycleAction",
            "Resource": "*"
        }
    ]
}
```
   - Launch Template 생성 : 일부로 2분 이상 긴 시간이 걸리는 Userdata로 초기화
     + EC2 - 시작 템플릿 - 시작 템플릿 - demo-my-warmpool-template - 인스턴스 : t2.micro / 키 페어 없이 진행 / 보안그룹 : default / IAM 인스턴스 프로파일 : ec2-role-for-warmpool / 유저 데이터 추가
```
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0
 
--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment;
 filename="cloud-config.txt"
 
#cloud-config
cloud_final_modules:
- [scripts-user, always]
--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"
 
#!/bin/bash
echo "Starting EC2 user data script..."
# Check if httpd is installed
if ! dnf list installed httpd &> /dev/null; then
  echo "httpd is not installed. Installing httpd..."
  dnf install httpd -y
else
  echo "httpd is already installed."
fi
# Check if httpd service is available and start it
if ! systemctl status httpd &> /dev/null; then
  echo "httpd service is not available. Waiting for 120 seconds..."
  sleep 120
fi
# Start the httpd service
echo "Starting httpd service..."
service httpd start
# Enable httpd service to start on boot
echo "Enabling httpd to start on boot..."
chkconfig httpd on
# Fetch instance metadata token and instance ID
echo "Fetching instance metadata..."
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
# Add the instance ID to the default web page
echo "Writing instance ID ($INSTANCE_ID) to /var/www/html/index.html..."
echo "$INSTANCE_ID" >> /var/www/html/index.html
# Notify Auto Scaling Group that lifecycle hook is complete
echo "Notifying Auto Scaling Group that instance initialization is complete..."
# Replace <LIFECYCLE-HOOK-NAME> and <AUTO-SCALING-GROUP-NAME> with the actual names
LIFECYCLE_HOOK_NAME="on-instance-start"
AUTO_SCALING_GROUP_NAME="demo-asg-warmpool"
# Fetch lifecycle action token from the instance metadata
aws autoscaling complete-lifecycle-action \
  --lifecycle-hook-name "$LIFECYCLE_HOOK_NAME" \
  --auto-scaling-group-name "$AUTO_SCALING_GROUP_NAME" \
  --lifecycle-action-result "CONTINUE" \
  --instance-id "$INSTANCE_ID"
echo "Lifecycle hook notification sent. EC2 user data script completed."
--//--
```

   - Autoscale Group 생성 / Warm Pool 생성
     + Autoscale 그룹 : demo-asg-warmpool / 시작 템플릿 : demo-my-warmpool-template / 가용 영역은 모두 선택 / 원하는 용량 0 / 최소 0, 최대 2 / 태그 : Name - Warmpool
     + 인스턴스 관리 - 수명 주기 후크 - 수명 주기 후크 생성 - on-instance-start / 하트비트 제한 시간 : 300초
      
   - Autoscale의 Scale-Out을 요청해서 인스턴스 준비 확인
     + Autoscale Group : demo-asg-warmpool - 편집 - 원하는 용량 : 1 - 업데이트
     + demo-asg-warmpool 웜풀 인스턴스 생성 - 웜풀 생성 - 웜풀 인스턴스 상테 : 중지됨 - 생성
     + EC2 인스턴스 2개 생성 (1개 : Autoscaling Group / 1개 : Warm Pool 초기화 시키기 위해 실행, 120초 이후 중지)
     + Autoscaling Group EC2 연결
```
sudo -s
cd /var/log
nano cloud-init-output.log (120초 대기)
```

   - Autoscaling Group에서 demo-asg-warmpool - 편집 - 원하는 용량 2개로 업데이트 (바로 실행됨을 알 수 있음) : 접속하면 바로 접속됨을 알 수 있음
   - 원하는 용량을 1로 감소 : 1개는 Warm Pool로 들어감 (Pending -> Stopped)
   - 리소스 정리 : Autoscaling Group 삭제
