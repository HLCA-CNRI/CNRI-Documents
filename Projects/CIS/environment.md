# CIS 환경

## 첫 환경설정

1. CIS repo clone
   - client
   - api
   - keycloak
2. keycloak 서버 세팅
   - keycloak 디렉토리 루트 경로 이동
   - docker-compose up --build -d
   - localhost:8080 접속 -> 관리자 콘솔(좌측 메뉴) 클릭 -> docker-compose.yml에 설정한 ADMIN 정보로 로그인
   - realm 만들기: 좌상단 Add Realm -> Realm 설정 파일(cis-test) import & create -> cis-test Realm 생성(cis-client Client도 함께 생성됨)
   - Client admin 계정 생성: 좌측 Manage - Users -> Add user -> 관리자 목적으로 사용할 본인 이메일 입력(나머지 생략 가능) -> 상단 Credentials -> 비밀번호 설정(Temporary=False로 설정하기) -> Groups에서 admin Join하기
   - 좌측 Clients -> cis-client Edit -> Credentials -> Secret 따로 기록해두기(클라이언트와 API 서버의 .env에 넣어야 함)
3. DB 세팅
   - postgres DB container 생성하기
   ```
   docker run --name cnri-cis -e POSTGRES_USER=cnri -e POSTGRES_PASSWORD=code1234 -p 5432:5432 -d postgres
   ```
   - DB 생성하기
   ```
   docker exec -it cnri-cis bash
   psql -U cnri
   ```
   ```sql
   <!-- 저 우측 끝에 세미콜론 안 붙이면 명령어 안 끝난 것으로 간주하여 실행 안 되니까 꼭 붙이길 바람! -->
   CREATE DATABASE cnri;
   ```
4. .env 세팅(관리자에게 문의 필요)
   - client
   - api
5. client & api 서버 실행
   - client: localhost:3000
   - api: localhost:3001
     - api 서버 첫 실행 시 DB에 자동으로 seeding 진행함
6. 로컬 테스트용 계정 생성
   - localhost:3001/docs -> Swagger 접속
   - 우상단 Authorize -> 하단 Authorize -> 2번에서 생성한 Client admin user 정보로 로그인
   - 카테고리 중 Admin User -> POST /admin/user -> Try it out -> 원하는 대로 JSON 데이터 입력 후 Execute -> return 201 되면 성공
   - 이후 생성한 계정 정보로 localhost:3000에서 로그인 가능

- 주의사항
  - keycloak Realm 설정 export 시 해당 Realm 들어간 후 좌측 하단의 Export로 하기(전부 ON 시키기)
    - Add Realm에서 이때 export된 파일을 불러옴(Clients까지 포함)
