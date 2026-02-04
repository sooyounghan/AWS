-----
### CloudWatch Demo - 지표 수집 / 알람
-----
1. Custom Metric (EC2 Memory 모니터링)
   - EC2에 웹 서버 설치
   - CloudWatch Agent 설치 및 설정 : 로그 수집 / 커스텀 지표 (메모리 / 디스크 수집 용도)
   - 이후 Custom Metric 확인
   - 수집된 Access Log를 지표 필터로 지표화
   - 수집된 지표 기반 알람 설정 : 404 에러가 분당 N(5)번 이상 발생시 이메일로 전송

2. IAM 생성
   - 역할 생성 : EC2 - CloudWatchFullAccessV2, AmazonEC2ReadOnlyAccess - demo-ec2-role-for-cloudwatch

3. EC2 생성
   - demo-cloudwatch
   - 키 페어 필요 없음
   - default 보안 그룹
   - 고급 세부 정보 : IAM은  demo-ec2-role-for-cloudwatch

4. EC2 설정
```
sudo -s
dnf install httpd -y
service httpd start
chkconfig httpd on
sudo mkdir /var/log/www/
sudo mkdir /var/log/www/error
sudo mkdir /var/log/www/access
curl -L -O http://public.rwlecture.com/awsclassroom_lecture/basic_monitoring/EnablePHPCloudwatchlog.json
curl -L -O http://public.rwlecture.com/awsclassroom_lecture/basic_monitoring/httpd.conf
cp httpd.conf /etc/httpd/conf (y입력)
service httpd restart

sudo dnf install amazon-cloudwatch-agent -y
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/home/ec2-user/EnablePHPCloudwatchlog.json
sudo systemctl start amazon-cloudwatch-agent
```

5. CloudWatch : 로그 - Log Management - apache/access로 로그 확인
   - 지표 - 모든 지표 : CWAgent - ImageId, InstanceId, InstanceType, device, fstype, path / ImageId, InstanceId, InstanceType

6. 알람 설정
   - 404 에러 발생 시 알람 아키텍쳐
   - apache/access - 작업 - 지표 필터 생성 - 패턴 { $.status= "404" } 및 로그 데이터 지정 후 패턴 테스트
   - 필터 이름 : demo-404 / 지표 네임스페이스 : demo-404 / 지표 이름 : demo-404-metric / 지표 값 : 1
   - 지표 - 모든 지표 - demo-404 - 그래프로 표시된 지표 - 기간 : 1분 / 통계 : 합계
   - 경보 - 모든 경보 - 경보 생성 - 지표 선택 - demo-404 - 통계 : 합계 / 기간 : 1분 / 임계값 : 5 / 경보 상태 / 새 주제 생성
   - demo-my-404-alarm
   - 이메일 Subscription
   - 경보가 발생하면 이메일로 전송

7. 리소스 정리 : 경보 삭제 / EC2 삭제 / CloudWatch - 로그 그룹 삭제 (apache)
