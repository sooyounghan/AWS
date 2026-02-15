-----
### Resource Attribute
-----
1. 리소스 자체 공통적인 속성 정의
2. CreationPolicy : 리소스 생성 시 특정 조건을 만족해야 완료 처리할 수 있도록 설정 (예) EC2에서 웹 서버가 설치되어야 EC2 생성 완료)
3. DeletionPolicy : 리소스 삭제 정책
   - Delete : 스택 삭제 시 같이 삭제 (몇 리소스를 제외하고 기본 옵션)
   - Retain : 스택 삭제 시 해당 리소스는 삭제하지 않고 보존
     + 💡 주의 : 리소스 생성에 실패하여 롤백할 때도 보존됨
     + RetaionExceptOnCreate : 리소스 처음 생성 할 때를 제외하고 보존 (즉, 처음 생성 시 실패한다면 삭제)
   - Snapshot (지원 시) : 스냅샷을 지원하는 리소스 (EC2, RDS 등)의 경우 삭제 시 스냅샷을 생성하고 삭제 (RDS, EBS, RedShift, ElasticCache 등)
<div align="center">
<img src="https://github.com/user-attachments/assets/9ddb8ba6-4573-4369-9624-22bece4fa17a" />
</div>

4. DependsOn : 특정 리소스가 생성된 이후 해당 리소스 생성을 시작하도록 설정 (예) 반드시 RDS가 생성된 이후 EC2를 생성을 시작해서 서버가 설정되도록 구성)
   - 💡 참고 : !Ref, !GetAtt, !Sub로 묶인 경우 : 암시적으로 참조하는 리소스를 생성 후 해당 리소스 생성
   - 즉, 해당 참조가 없는 상태에서 리소스 생성 순서를 제어하기 위해 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/6c2e9578-8e8a-4541-b110-de80c9fc9e92" />
<img src="https://github.com/user-attachments/assets/864aa0e0-a86d-4b01-a07c-59a766502696" />
</div>

5. Metadata : 리소스의 추가적 정보 제공
6. UpdatePolicy : 리소스 업데이트 시 동작 방식 정의 (예) Autoscale의 경우 업데이트 시 인스턴스 업데이트 방식 정의(Replace, Rolling, Schedule))
   - 예) ElasticCache의 샤드 업데이트 방식 정의

7. UpdateReplacePolicy : 리소스의 업데이트 시 기존 리소스의 교체 방식 정의 (예) EC2 인스턴스, RDS 업데이트 시 업데이트 시 기존 EBS 볼륨의 스냅샷을 만들 것인지 여부 등을 정의)
<div align="center">
<img =src="https://github.com/user-attachments/assets/149c65eb-3e5f-43b2-a611-91152c3be79f" />
</div>

