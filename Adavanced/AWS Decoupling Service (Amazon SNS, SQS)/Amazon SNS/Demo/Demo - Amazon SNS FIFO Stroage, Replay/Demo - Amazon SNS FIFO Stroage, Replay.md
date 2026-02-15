-----
### Demo - SNS FIFO Stroage / Replay
-----
1. SNS FIFO와 SQS 큐로 연결하는 Subscription 생성
   - DynamoDB에 요청을 저장하는 로직
   - 이메일로 보내는 로직

2. 데이터를 삭제하고 Replay로 DB에 데이터가 다시 생성되는 것 확인
3. S3에 데이터를 저장하는 로직을 담은 Subscription 추가 : 이후 Replay로 기존 로직과 Sync을 맞추는 부분 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/e1ca6cca-6591-4208-bcbc-df8636dc550a" />
<img src="https://github.com/user-attachments/assets/f258c608-5647-45e8-82c1-3b6752c1a555" />
<img src="https://github.com/user-attachments/assets/4e78f807-3d72-4522-860a-c1378cd6e590" />
<img src="https://github.com/user-attachments/assets/222ddee1-2c7b-4072-9eea-2a55b3a8efe3" />
<img src="https://github.com/user-attachments/assets/e59b9fea-dea4-4fbf-a839-362be2cc9718" />
<img src="https://github.com/user-attachments/assets/df978139-fbfd-409c-b60f-872dc1ec380c" />
</div>

4. demo-sns-fifo-backend
```
1. nodejs 18 이상 설치
2. npm install --global yarn
3. yarn
4. yarn deploy --aws-profile {자신의 aws 프로파일} -> awsclassroom
```
   - AWS 프로파일 : IAM - Admin 보안 자격 증명 - 액세스 키 만들기 - 기타 - 액세스 키 / 비밀 액세스 키 준비
   - AWS CLI 설치
```
aws configure --profile awsclassroom
액세스 키 / 비밀 액세스 키 입력
Default region name [None]: ap-northeast-2
```
   - serverless_template.yml에 이메일 주소 변경
```yml
frameworkVersion: "3"
service: demo-sns-fifo
app: backend
useDotenv: true
email: rubywave.lecture.sandbox@gmail.com // 변경
provider:
  name: aws
  runtime: nodejs20.x
  deploymentMethod: direct
  versionFunctions: false
  iam:
    role: DefaultRole
  httpApi:
    cors: true
  stage: ${env:STAGE, "dev"}
  tags:
    Service: ${self:service}
    Environment: ${env:STAGE, "dev"}
  stackTags:
    Service: ${self:service}
    Environment: ${env:STAGE, "dev"}
  region: ${opt:region, "ap-northeast-2"}
  stackName: ${self:service}-${env:STAGE, "dev"}-${env:VER, "1"}-serverless
  timeout: 29 #api gateway를 거칠 경우 lambda의 max
  environment:
    service: ${self:service}
    version: ${env:VER, "1"}
    stage: ${env:STAGE, "dev"}
    region: ${opt:region, "ap-northeast-2"}
    app: ${self:app}
    record_ddb_name:
      Ref: RecordDDBTable
    alarm_topic:
      Ref: AlarmTopic
    main_sns_fifo_topic:
      Ref: MainSNSFifoTopic
    s3bucket_name:
      Ref: S3Bucket
  deploymentBucket:
    name: ${aws:accountId}-serverless-deploys
    maxPreviousDeploymentArtifacts: 5
    blockPublicAccess: true
  deploymentPrefix: ${self:service}-${env:STAGE, "dev"}-${env:VER, "1"}-serviceBackend
plugins:
  - serverless-deployment-bucket
  - serverless-cloudformation-sub-variables

resources: # CloudFormation template syntax
  Resources:
    DefaultRole:
      Type: AWS::IAM::Role
      Properties:
        RoleName: ${self:service}-${env:STAGE, "dev"}-${env:VER, "1"}-LambdaExcutionRole
        AssumeRolePolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service:
                  - lambda.amazonaws.com
              Action: sts:AssumeRole
            - Effect: Allow
              Principal:
                Service:
                  - iam.amazonaws.com
              Action: sts:AssumeRole
        ManagedPolicyArns:
          - arn:aws:iam::aws:policy/AmazonSNSFullAccess
          - arn:aws:iam::aws:policy/AmazonS3FullAccess
          - arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
          - arn:aws:iam::aws:policy/AmazonSQSFullAccess
          - arn:aws:iam::aws:policy/CloudWatchFullAccess
        Policies:
          - PolicyName: myPolicyName
            PolicyDocument:
              Version: "2012-10-17"
              Statement:
                - Effect: Allow
                  Action:
                    - sts:AssumeRole
                  Resource: "*"

    AlarmTopic:
      Type: AWS::SNS::Topic
      Properties:
        Subscription:
          - Endpoint: ${self:email}
            Protocol: email
    AlarmTopicpolicy:
      Type: AWS::SNS::TopicPolicy
      Properties:
        PolicyDocument:
          Id: AlarmTopicpolicy
          Version: "2012-10-17"
          Statement:
            - Sid: state1
              Effect: Allow
              Principal:
                Service:
                  - lambda.amazonaws.com
              Action: sns:Publish
              Resource: "*"
        Topics:
          - Ref: AlarmTopic

    MainSNSFifoTopic:
      Type: "AWS::SNS::Topic"
      Properties:
        TopicName: "my-sns-fifo-topic.fifo"
        FifoTopic: true
        ContentBasedDeduplication: false

    RecordSQSFIFOQueue:
      Type: AWS::SQS::Queue
      Properties:

        QueueName: ${self:service}-${env:STAGE, "dev"}-record-${env:VER, "1"}
        VisibilityTimeout: 100

    RecordSQSFIFOQueuePolicy:
      Type: "AWS::SQS::QueuePolicy"
      Properties:
        Queues:
          - Ref: RecordSQSFIFOQueue
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service: sns.amazonaws.com
              Action: "sqs:SendMessage"
              Resource:
                Fn::GetAtt:
                  - RecordSQSFIFOQueue
                  - Arn
              Condition:
                ArnEquals:
                  aws:SourceArn:
                    Ref: MainSNSFifoTopic

    MainSNSFifoSubscription1:
      Type: "AWS::SNS::Subscription"
      Properties:
        TopicArn:
          Ref: MainSNSFifoTopic
        Protocol: "sqs"
        Endpoint:
          Fn::GetAtt:
            - RecordSQSFIFOQueue
            - Arn
    RecordDDBTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: ${self:service}-${env:STAGE, "dev"}-record-${env:VER, "1"}
        AttributeDefinitions:
          - AttributeName: order_id
            AttributeType: S
          - AttributeName: datetime
            AttributeType: S
          - AttributeName: type
            AttributeType: S
        KeySchema:
          - AttributeName: order_id
            KeyType: HASH
          - AttributeName: datetime
            KeyType: RANGE
        BillingMode: PAY_PER_REQUEST
        GlobalSecondaryIndexes:
          - IndexName: type-index
            KeySchema:
              - AttributeName: type
                KeyType: HASH

            Projection:
              ProjectionType: ALL

    AlarmSQSFIFOQueue:
      Type: AWS::SQS::Queue
      Properties:

        QueueName: ${self:service}-${env:STAGE, "dev"}-alarm-${env:VER, "1"}
        VisibilityTimeout: 100

    MainSNSFifoSubscription2:
      Type: "AWS::SNS::Subscription"
      Properties:
        TopicArn:
          Ref: MainSNSFifoTopic
        Protocol: "sqs"
        Endpoint:
          Fn::GetAtt:
            - AlarmSQSFIFOQueue
            - Arn

    AlarmSQSFIFOQueueTopicPolicy:
      Type: "AWS::SQS::QueuePolicy"
      Properties:
        Queues:
          - Ref: AlarmSQSFIFOQueue
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service: sns.amazonaws.com
              Action: "sqs:SendMessage"
              Resource:
                Fn::GetAtt:
                  - AlarmSQSFIFOQueue
                  - Arn
              Condition:
                ArnEquals:
                  aws:SourceArn:
                    Ref: MainSNSFifoTopic

    APISQSFIFOQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: ${self:service}-${env:STAGE, "dev"}-api-${env:VER, "1"}
        VisibilityTimeout: 100
    APISQSFIFOQueueTopicPolicy:
      Type: "AWS::SQS::QueuePolicy"
      Properties:
        Queues:
          - Ref: APISQSFIFOQueue
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service: sns.amazonaws.com
              Action: "sqs:SendMessage"
              Resource:
                Fn::GetAtt:
                  - APISQSFIFOQueue
                  - Arn
              Condition:
                ArnEquals:
                  aws:SourceArn:
                    Ref: MainSNSFifoTopic

    MainSNSFifoSubscription3:
      Type: "AWS::SNS::Subscription"
      Properties:
        TopicArn:
          Ref: MainSNSFifoTopic
        Protocol: "sqs"
        Endpoint:
          Fn::GetAtt:
            - APISQSFIFOQueue
            - Arn

    S3SQSFIFOQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: ${self:service}-${env:STAGE, "dev"}-s3-${env:VER, "1"}
        VisibilityTimeout: 100
        
    S3SQSFIFOQueuePolicy:
      Type: "AWS::SQS::QueuePolicy"
      Properties:
        Queues:
          - Ref: S3SQSFIFOQueue
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service: sns.amazonaws.com
              Action: "sqs:SendMessage"
              Resource:
                Fn::GetAtt:
                  - S3SQSFIFOQueue
                  - Arn
              Condition:
                ArnEquals:
                  aws:SourceArn:
                    Ref: MainSNSFifoTopic
    S3Bucket:
      Type: AWS::S3::Bucket
      DeletionPolicy: Delete
      Properties:
        BucketName: ${aws:accountId}-s3-record-${env:STAGE, "dev"}-${env:VER, "1"}

    EmptyS3LambdaExecutionRole:
      Type: AWS::IAM::Role
      Properties:
        AssumeRolePolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal:
                Service:
                  - lambda.amazonaws.com
              Action:
                - sts:AssumeRole
        Path: "/"
        Policies:
          - PolicyName: root
            PolicyDocument:
              Version: "2012-10-17"
              Statement:
                - Effect: Allow
                  Action:
                    - "s3:*"
                  Resource: "*"
                - Effect: Allow
                  Action:
                    - "logs:CreateLogGroup"
                    - "logs:CreateLogStream"
                    - "logs:PutLogEvents"
                  Resource: "*"
    EmptyS3BucketLambda:
      Type: "AWS::Lambda::Function"
      Properties:
        Handler: "index.handler"
        Role:
          Fn::GetAtt:
            - "EmptyS3LambdaExecutionRole"
            - "Arn"
        Runtime: "nodejs20.x"
        Timeout: 600
        Code:
          ZipFile: |
            const { S3Client, ListObjectsV2Command, DeleteObjectCommand } = require("@aws-sdk/client-s3");
            const https = require('https');

            exports.handler = async (event, context) => {
              const s3Client = new S3Client({ region: process.env.AWS_REGION });
              try {
                const bucketName = event['ResourceProperties']['BucketName'];
                
                if (event['RequestType'] === 'Delete') {
                  let continuationToken = null;
                  do {
                    const listParams = {
                      Bucket: bucketName,
                      ContinuationToken: continuationToken,
                    };
                    console.log(listParams);
                    const listedObjects = await s3Client.send(new ListObjectsV2Command(listParams));
                    console.log(JSON.stringify(listedObjects))
                    if (listedObjects.Contents&& listedObjects.Contents.length > 0) {
                      for (let i = 0; i < listedObjects.Contents.length; i++) {
                        const deleteParams = {
                          Bucket: bucketName,
                          Key: listedObjects.Contents[i].Key,
                        };
                        await s3Client.send(new DeleteObjectCommand(deleteParams));
                      }
                    }
                    continuationToken = listedObjects.NextContinuationToken;
                  } while (continuationToken);
                }
                
                await sendResponseCfn(event, context, "SUCCESS");
              } catch (error) {
                console.error(error);
                await sendResponseCfn(event, context, "FAILED");
              }
            };

            const sendResponseCfn = async (event, context, responseStatus) => {
              const responseBody = JSON.stringify({
                Status: responseStatus,
                Reason: 'See the details in CloudWatch Log Stream: ' + context.logStreamName,
                PhysicalResourceId: context.logStreamName,
                StackId: event['StackId'],
                RequestId: event['RequestId'],
                LogicalResourceId: event['LogicalResourceId'],
                Data: {},
              });

              const parsedUrl = new URL(event['ResponseURL']);
              const options = {
                hostname: parsedUrl.hostname,
                port: 443,
                path: parsedUrl.pathname + parsedUrl.search,
                method: 'PUT',
                headers: {
                  'content-type': '',
                  'content-length': responseBody.length
                }
              };

              return new Promise((resolve, reject) => {
                const request = https.request(options, (response) => {
                  response.setEncoding('utf8');
                  response.on('end', () => {
                    resolve();
                  });
                });

                request.on('error', (error) => {
                  console.error('Failed to send CloudFormation response:', error);
                  reject(error);
                });

                request.write(responseBody);
                request.end();
              });
            };
    LambdaUsedToCleanUpS3:
      Type: Custom::cleanupbucket
      Properties:
        ServiceToken:
          Fn::GetAtt:
            - EmptyS3BucketLambda
            - Arn
        BucketName:
          Ref: S3Bucket
custom:

functions:
```

   - Deploy 결과 : endpoint 생성
```
✔ Service deployed to stack demo-sns-fifo-dev-1-serverless (97s)

endpoint: POST - https://fsqlaew59a.execute-api.ap-northeast-2.amazonaws.com/dev/order               
functions:
  order_post: demo-sns-fifo_dev_1_order_post (3.9 MB)                                                
  sqs_processAlarm: demo-sns-fifo_dev_1_sqs_processAlarm (3.9 MB)
  sqs_processAPI: demo-sns-fifo_dev_1_sqs_processAPI (3.9 MB)
  sqs_processRecording: demo-sns-fifo_dev_1_sqs_processRecording (3.9 MB)
  sqs_processS3: demo-sns-fifo_dev_1_sqs_processS3 (3.9 MB)
```

5. demo-sns-fifo-frontend
```
1. nodejs 18 이상 설치
2. npm install --global yarn
3. yarn
4. .env파일 내에 REACT_APP_API_ADDRESS 내용을 backend 프로비전시 나온 주소로 교체(영상 참고) : 위 주소
4. yarn start
```
```
REACT_APP_API_ADDRESS='https://fsqlaew59a.execute-api.ap-northeast-2.amazonaws.com/dev/order'
```

6. Demo-SNS-FIFO
   - AWS Notification - Subscription Confirmation 에서 Confirm subscription
   - 이메일 작성 및 내용 작성 후 제출하면, 이메일 도착 (5개)
   - DynamoDB - 서울 리전 - 항목 탐색 - 주문한 내역 확인
     + 항목 선택 후 삭제 후 다시 주문
     + 아이템 삭제

7. SNS Replay
   - SNS - 주제 - my-sns-fifo-topic.fifo - 구독 3개 존재 (api-1 / dev-alarm-1 / dev-record-1)
   - 아카이브 정책 : 편집 - 아카이브 정책 활성화 (메세지 보존 기간 : 30일)
   - 주문 시작 : 케이크 2개, 옷 3개, tv 5개
   - 기록 전부 삭제 후 복구 : SNS - 데이터 레코드 담당 구독 - 재생 시작 : UTC 기준이므로 9시간 뒤로 설정
   - 모든 데이터 복구 확인
   - S3 저장
     + 주제 - my-sns-fifo-topic.fifo - 구독 생성 : SNS FIFO이므로, Amazon SQS 프로토콜 사용 / 엔드포인트 : arn:aws:sqs:ap-northeast-2:362454057659:demo-sns-fifo-dev-s3-1 (S3 버킷 중 362454057659-s3-record-dev-1에 저장되는 것을 연동)
     + 생성된 구독에서 재생 실시하면, S3에 데이터 저장 (arn:aws:sqs:ap-northeast-2:362454057659:demo-sns-fifo-dev-s3-1)
     + 번호 부여 : SNS FIFO는 시퀀스 번호 부여 (중복되지 않으므로 고유 아이디 생성 가능)
     + SQS.fifo가 아닌 SQS로 생성한 이유 : Deduplication을 위함 (삭제하고 다시 복구할 때, 처리한 애들로 판단해 미실행됨)

8. 정리
   - SNS : 주제 - my-sns-fifo-topic.fifo - 편집 - 아카이브 정책 비활성화
   - CloudFormation - demo-sns-fifo-dev-1-serverless - 삭제하면 프로비전했던 모든 리소스 한 번에 삭제
   - Access Key 삭제 (IAM - 사용자 - 보안 자격 증명 - 비활성화 및 삭제)
   
   
