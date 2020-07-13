# MSA moim project
microserver architecture 기반 moim project

## Service 구성
1. Eureka Server, Port:8761
2. API Gateway, Port: 8090
3. Authorization Server: 8095
4. Resource Server  
   meet : 10000  
   user : 10005

## 소스 초기화
sts를 처음 설치 하고 소스를 다운받기 전에 아래의 순서대로 진행한다.
1. settings.xml 생성
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
   <localRepository>C:\sts-4.6.2.RELEASE\repository</localRepository>
   <interactiveMode>true</interactiveMode>
   <offline>false</offline>
</settings>
```
2. repository 위치 변경
   - Window → Preferences → Maven → User Settings의 User Settings 위치를 C:\{sts경로}\settings\settings.xml로 설정 
3. lombok 설치
4. QueryDsl 미생성 문제로 SpringToolSuite4.ini 파일을 열고 -vmargs 위에 아래의 정보를 추가한다.
   ```
   -vm
   C:\Program Files\Java\jdk1.8.0_251\bin\javaw.exe 
   ```

## swagger 접속
- localhost:10000/swagger-ui.html
- 최초 server를 up하고 Load balance 등의 오류가 나며 v2/api-docs를 못 읽어오면 잠시 기다리고 eureka가 재로딩 하기를 기다렸다가 다시 조회해 본다

## h2 접속
- Authorization Server → localhost:8095/h2-console, jdbc:h2:mem:testdb
- meet → localhost:10000/h2-console, jdbc:h2:mem:testdb

## MariaDB 접속
- jdbc:mariadb://{주소}:{port}/{db명}
- Authorization 서버와 User service의 경우 user-dev DB를 공유한다.

## Usage
1. access token 받는 방법
   - curl auth_id:auth_secret@localhost:8095/oauth/token -d grant_type=password -d client_id=auth_id -d scope=read -d username=cdssw@naver.com -d password=1234 -d common=common
   - curl auth_id:auth_secret@localhost:8095/oauth/token -d grant_type=password -d client_id=auth_id -d scope="read write" -d username=cdssw@naver.com -d password=1234 -d common=common
2. refresh token으로 access token 얻는 방법
   - curl auth_id:auth_secret@localhost:8095/oauth/token -d grant_type=refresh_token -d scope=read -d refresh_token={refresh token}
3. api 호출 방법 (위에서 받은 accessToken을 적용)
   - curl localhost:8090/meet/1 -H "Authorization: Bearer {access_token}"
   - curl localhost:8090/user -H "Authorization: Bearer {access_token}"

### [MSA QnA Link](https://github.com/cdssw/msa/blob/master/MSA%20QnA.md).

## Todo...
- <del>통합테스트 코드에 Profile/ActiveProfile 적용</del>
- <del>Kafka 연동</del>
- <del>CustomAuthenticationProvider 적용</del>
- <del>scope 처리방법 (PreAuthorize("#oauth2.hasScope('write')") 으로 처리</del>
- <del>docker 적용</del>
- <del>Jenkins 처리</del>
- Config server 추가
- zipkin 적용
- Hystrix Circuit Breaker 적용
