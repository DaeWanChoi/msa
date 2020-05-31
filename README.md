# MSA moim project
microserver architecture 기반 moim project

## Service 구성
1. Eureka Server, Port:8761
2. API Gateway, Port: 8090
3. Authorization Server: 8095
4. Resource Server
   meet : 10000
   user : 10005

## swagger 접속
- localhost:10000/swagger-ui.html

## h2 접속
- Authorization Server → localhost:8095/h2-console, jdbc:h2:mem:testdb
- meet → localhost:10000/h2-console, jdbc:h2:mem:testdb

## Usage
1. access token 받는 방법
   - curl auth_id:auth_secret@localhost:8095/oauth/token -d grant_type=password -d client_id=auth_id -d scope=read -d username=cdssw@naver.com -d password=1234 -d common=common
2. refresh token으로 access token 얻는 방법
   - curl auth_id:auth_secret@localhost:8095/oauth/token -d grant_type=refresh_token -d scope=read -d refresh_token={refresh token}
3. api 호출 방법 (위에서 받은 accessToken을 적용)
   - curl localhost:8090/meet/1 -H "Authorization: Bearer {access_token}"
   - curl localhost:8090/user/1 -H "Authorization: Bearer {access_token}"

## Todo...
- <del>통합테스트 코드에 Profile/ActiveProfile 적용</del>
- Config server 추가
- Kafka 연동
- <del>CustomAuthenticationProvider 적용</del>
- scope 처리
- zipkin 적용
- docker, kubernetes 적용
- Hystrix Circuit Breaker 적용
