-----
### CloudFront gRPC 지원
-----
1. gRPC : Cloud Native Computing Foundation에서 만든 RPC 기반 오픈소스 API 시스템
   - RPC(Remote Procedure Call) : 다른 주체에 함수 혹은 기능을 직접 호출하는 프로토콜
   - 단순하게 서버의 데이터를 가져오거나 업데이트하는 REST와 다르게 직접 서버 함수 호출 가능

2. HTTP/2, Protobuf 기반 양방향 스트리밍 방식
3. 주요 적용 사례
   - 마이크로 서비스 아키텍쳐 서비스 간 통신
   - 실시간 데이터 전송 (채팅 / IoT 데이터 스트리밍)
   - 서버 클라이언트 아키텍쳐에서 양방향 통신이 필요한 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/602bed14-ab4f-46e4-a5b6-8a53fe902782" />
</div>

