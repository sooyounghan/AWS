-----
### Consumer
-----
1. 권한이 있는 상태에서 주기적으로 API로 Queue에 메세지를 요청(Poll)해서 처리 : 역시 메세지를 받아오고 삭제하는 권한 필요 (sqs:ReceiveMessage / sqs:DeleteMessage)
2. 기본적인 Workflow : Poll → 처리 → 삭제
3. Visibility Timeout 안에 메세지를 받아 처리하고 완료 시 삭제
   - 삭제하지 않으면 다시 Queue Visibility Timeout 이후 Queue로 반환
   - 혹은 Visibility Timeout을 늘려서 추가 시간 확보 후 처리

4. 두 가지 Polling 방법 : Short Polling / Long Polling
   - Short Polling (기본) : 메세지 요청 시 Queue 일부를 검색 해 가능한 메세지를 빠르게 찾아 전달 (메세지를 못 찾아도 응답을 바로 전달)
   - Long Polling : 메세지 요청 시 Queue 전체를 검색하여 적어도 하나의 메세지를 찾아 전달 (메세지가 없다면 찾을 때까지 기다리거나, 혹은 Timeout을 넘는다면 그 때 응답 전달)
     + 1 ~ 20초까지 대기 시간 설정 가능
     + 장점
       * False Empty Response 방지
       * 요청 횟수 줄이기
   - Batch 처리 가능 : 한 번에 최대 10개까지 메세지 요청 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/38cb55b2-30c0-4b1f-8035-a96da15ac68a" />
<img src="https://github.com/user-attachments/assets/c80c996a-cd6e-4c02-a0ff-7b50ce06e252" />
<img src="https://github.com/user-attachments/assets/240a3803-b072-46f6-bc90-800595c7fa23" />
<img src="https://github.com/user-attachments/assets/c3db4ede-024f-40b5-b382-35dfcf488f7e" />
<img src="https://github.com/user-attachments/assets/d7c87dab-6be0-48c4-ba60-6cde6d64565f" />
</div>

-----
### Visibility Timeout
-----
1. 메세지 요청 이후 다른 주체가 메세지를 요청할 수 없는 기간
   - 일종의 Lock
   - 해당 기간 동안 다른 주체가 메세지 요청 불가능

2. 기본 30초 / 최소 0초 / 최대 12시간
3. Visibility Timeout 기간을 넘어서는 처리(삭제) 되지 않는 메세지는 자동으로 다른 주체에게 열림 (요청가능)
   - 메세지를 처리하기 위한 충분한 시간이나 오류 / 에러 상황에 적절하게 대응할 수 있는 시간 필요
<div align="center">
<img src="https://github.com/user-attachments/assets/6fe9ae72-fd92-4d4c-b944-5065a3be382a" />
</div>

4. 기본적으로 Queue 단위 이나 메세지 단위로 설정 가능 : 즉, 필요하다면, Visibility Timeout 수정 가능
<div align="center">
<img alt="image" src="https://github.com/user-attachments/assets/386ce062-e5cc-4289-9da2-b69fbf060d87" />
</div>

5. 주의 : 💡 Visibility Timeout이 만료된다면 Queue 맨 뒤로 돌어감
