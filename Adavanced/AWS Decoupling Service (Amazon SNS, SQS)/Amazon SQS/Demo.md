-----
### Demo - SQS를 활용한 EC2 기반 이미지 리사이징
-----
1. 예제 시나리오
   - 이미지를 업로드하면 썸네일 생성
   - 시연 내용 : SQS 활용, 간단한 CI / CD

2. 순서
<div align="center">
<img src="https://github.com/user-attachments/assets/6690def8-7b63-429a-b964-c803c5d36716" />
<img src="https://github.com/user-attachments/assets/ca913f80-320f-4967-a5af-e04cebd023d6" />
</div>

   - SQS 생성 및 S3과 연동
     + SQS 생성 : 대기열 생성 - 표준 - demo-image-resize-queue / 표시 제한 시간 : 30초 / 액세스 정책 : 고급 - 다음 정보 입력
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "SQS:SendMessage",
      "Resource": "*",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "{s3-arm}"
        }
      }
    }
  ]
}
```

   - S3에 생성 (소스 코드 / 이미지 업로드) : demo-image-resize-sqs-{계정ID}
     + 속성 - 이벤트 알림 - 이벤트 알림 생성 - demo-image-resize / images/ / 모든 객체 생성 이벤트 / SQS 대기열 후 SQS 선택
     + 폴더 생성 : images 디렉토리 생성 후 SQS 확인 (메세지 전송 및 수신 - 메세지 폴링 / 삭제도 가능)
     + SQS URL 복사 후 아래 입력, .env 파일에 저장 후, src 디렉토리 / .env / package.json 압축 후 S3에 업로드
```
STAGE=dev
VER=1
QUEUE_URL={SQS URL}
```

   - 시작 템플릿 생성 : 유저데이터로 S3에서 소스 코드를 받아오고 빌드 후 애플리케이션 시작
     + IAM 역할 생성 : EC2 / AmazonSQSFullAccess, AmazonS3FullAccess, AmazonEC2FullAccess / demo-ec2-role-for-image-resize
     + demo-my-sqs-image-resize / Amazon Linux / t2.micro / 키 페어 X / 기존 보안 그룹 (default) / 위 IAM 역할 지정 / 사용자 데이터 입력
     + Auto Scaling Group : demo-image-resize-asg / 시작 템플릿 위 지정 / 서브넷 모두 선택 / 원하는 용량 : 1, 최대 용량 : 2 / 태그 : Name, Demo-SQS-Image-Resize
```
#!/bin/bash
curl --silent --location https://rpm.nodesource.com/setup_20.x | bash -
dnf -y install nodejs
cd /home/ec2-user
aws s3 cp s3://{버킷명}/demo-sqs-ec2-image-resize.zip /home/ec2-user  --region {리전명}
unzip demo-sqs-ec2-image-resize.zip
npm install
npm install forever -g
forever start -o out.log -e err.log src/server.js
```

   - EC2가 시작되면서 애플리케이션에서 메세지 폴링 및 이미지 리사이징 시작 및 확인
     + S3에 (images/) 약 10개의 이미지 첨부 후, resized/ 디렉토리로 이미지 사이즈 크기 변경 확인
     + EC2에서 확인
```
sudo -s
dir
tail -f out.log
```
```
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400 - 복사본 (7).jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400 - 복사본 (7).jpg에 업로드 하였습니다
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400.jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400.jpg에 업로드 하였습니다
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400 - 복사본 (2).jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400 - 복사본 (2).jpg에 업로드 하였습니다
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400 - 복사본 (6).jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400 - 복사본 (6).jpg에 업로드 하였습니다
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400.jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400.jpg에 업로드 하였습니다
width: 200 height: 200
demo-image-resize-sqs-362454057659/images/zSN42WxK_400x400 - 복사본 (2).jpg 이미지를 리사이징 하여 demo-image-resize-sqs-362454057659/resized/images/zSN42WxK_400x400 - 복사본 (2).jpg에 업로드 하였습니다
...
```

3. 리소스 정리
   - S3 - resized/ 디렉토리 영구 삭제 / Auto-Scaling 최소 : 0, 최대 : 0으로 변경
   
