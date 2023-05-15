# CIET 첫 환경 설정

## 목차

> - [Repo clone]()
> - [환경 변수 설정]()
> - [carcle-client repo 세부 세팅]()
> - [Carcle 서버(carcle-api) 실행]()
> - [Keyclock 서버 연결]()
> - [Swagger]()
> - [CNRI 회사 계정 등록하기]()
> - [Carcle 서비스 가입 & 로그인 후 회사 계정으로 변경하기]()

<br/>

## Repo clone

`carcle-client` , `carcle-api`, `keycloak`

git clone한 뒤, upstream 설정까지 하기

<br/>

## 환경 변수 설정

`carcle-client` , `carcle-api` 의 루트 디렉토리에 환경 변수 파일 `env.dev` 각각 삽입

> ⚠️ **주의 1**
>
> vscode에서 파일 복붙 하고 난 뒤, `env.dev` 파일명 앞에 . 추가
>
> ex) `.env.dev`
>
> ⇒ gitignore 적용하기 위함

<br/>

## carcle-client repo 세부 세팅

`Node js` 설치 - 설치 확인 명령어 `node -v`

- 만약 설치했는데 node 없다고 하면 재부팅

`npm install -g yarn`

그 후,

yarn 설치 시

`yarn set version berry` 해주기

`yarn install` 후,

`yarn start:dev`로 실행 가능

<br/>

## Carcle 서버(carcle-api) 실행

Docker - 이미지 pull 및 실행 으로 서버 실행

1. wsl2 설치
2. Docker설치
3. 아래 명령어 실행 ({}부분에는 괄호 없이 {안에 있는 내용}에 해당하는 것만 적어주면된다.)

```bash
docker run --name {컨테이너 이름} -e POSTGRES_PASSWORD=code1234 -e POSTGRES_USER=cnri -p 5432:5432 -d seahahn/postgres-cnri:latest
docker exec -it {컨테이너 이름} bash
psql -U {carcle-api의 env.dev에 적혀있는 DATABASE_USER} postgres
CREATE DATABASE {데이터베이스 이름}; <- 세미콜론 필수
```

추천 이름

- 컨테이너 이름 : carcle
- 데이터베이스 이름: carcle-api의 `.env.dev`에 `DATABASE_NAME`으로 적혀있는 이름

5. [keyclock 서버 연결](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Carcle/environment.md#keyclock-%EC%84%9C%EB%B2%84-%EC%97%B0%EA%B2%B0) ← 회원 계정 정보(아이디, 비밀번호 등)을 관리하는 서버
6. `carcle-api` 실행

   vscode로 carcle-api 디렉토리 열어서 터미널에 실행

   `npm install -g yarn pnpm`

   `pnpm install`

   `pnpm start:dev`

7. 실행되는지 확인했으면, 서버 중단하고 `pnpm migrate:run` 실행하여 데이터베이스 테이블 구조 설정
8. 실행되는지 확인했으면, 서버 중단하고 `pnpm seed:dev` 실행하여 기본 데이터 받아오기 (ProcessInput 등..)
9. 다시 서버 실행
10. [Swagger 사용 단계로 이동](https://github.com/HLCA-CNRI/CNRI-Documents/edit/main/Projects/Carcle/environment.md#swagger)

<br/>

## Keyclock 서버 연결

1.  vs code 로 keyclock clone 한 디렉토리 열기
2.  포트 설정 변경 : `docker-compose.yml` 파일에서 포트 내용 아래처럼 변경- services 하위에 있는 port 만  
    ![1](https://user-images.githubusercontent.com/77582221/188625592-0da1f731-d3a1-4042-a061-1ccf743d31df.png)

3.  vs code의 terminal에 명령어 입력 - `docker-compose up -d --build` : keyclock 서버 실행됨.
4.  [localhost:8080/](http://localhost:8080/auth/)으로 이동
5.  첫번째 박스 클릭 - admin 계정으로 로그인 :

    - `keyclock` repo에 `docker-compose.yml` 파일에 id, 비번 적혀있음. (Ctrl+f: `KEYCLOAK_ADMIN` , `KEYCLOAK_ADMIN_PASSWORD`)
    - > ⚠️ **주의**
      >
      > `docker-compose.yml` 에 있는 `KEYCLOAK_USER` , `KEYCLOAK_PASSWORD` 값과 ,
      > `lyllia-api`의 `.env.dev` 에 있는 KEYCLOAK_ADMIN_ID 와 KEYCLOAK_ADMIN_PASSWORD 가 동일한지 확인.
      >
      > 만약 동일하지 않다면,
      > `carcle-api`의 `.env.dev` 에 있는 id, pw 값을 실제 keyclock에 로그인된 id, pw 값으로 맞추기

6.  좌측 nav 바에서 Add Realm 클릭 - Carcle-test를 생성해야함 (생성 해야 아래 사진 처럼 바뀜)

- 생성할 때 `Select file` 버튼 클릭한 뒤`carcle-test-keycloak-setting.json` 파일 넣기
- <div align="center">
    <img src="https://user-images.githubusercontent.com/77582221/188625730-d593046a-a91c-4681-9712-efa77b5e09fb.png" width="20%"/>
  </div>

7.  client admin 계정 생성
    - Manage - Users - Add user - email 입력하여 생성
    - 생성된 화면의 상단 Credentials → 비밀번호 & 비밀번호 확인 & Temporary off → set pw
    - 다시 내 계정 상단 Groups → admin → join (없으면 role, group 생성 후 그룹 추가 ([keyclock 서버 연결](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#keyclock-%EC%84%9C%EB%B2%84-%EC%97%B0%EA%B2%B0)의 11, 12번)
8.  keycloak secret key

    - Client → `carcle-client` → edit → Credentials → secret key 복사
    - carcle-api의 `.env.dev`에 붙여넣기

    ```bash
    // api
     KEYCLOAK_CLIENT_SECRET= 여기에 붙여넣기
    ```

9.  realm setting > Login : 아래와 동일하게 맞추기
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188626026-b88077bf-6804-4a8e-bc74-8f41a7a23ef6.png" width="80%"/>
</div>

10. 이제 [carcle-api 실행 단계]()로 back

<br/>

## Swagger

api확인 및 테스트 하는 곳

1. carcle-api 에서 서버 실행 후 [http://localhost:3001/docs/](http://localhost:3001/docs/) 로 접속
2. Authorize 클릭
 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627581-9d541e4b-d8b9-475f-a292-f79ff1793297.png" width="80%"/>
</div>

3. 로그인 - id : carcle-client , pw : carcle-client의 secret key
 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627894-af47888f-f362-4f09-9226-04993464f840.png" width="80%"/>
</div>

<br/>

## 회사 계정, 사용자 계정 생성하기

1. Swagger 접속
2. Admin Company section에 있는 POST /admin/companies 클릭
3. Try it out 클릭
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627987-ed046faf-b39e-4019-b887-d2f518598ed0.png" width="80%"/>
</div>

요 상태 그대로 execute

  <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188628218-8427d6af-7dbb-4d99-b0aa-bd223d2bc9c3.png" width="80%"/>
</div>
  
4. response에서 company_id 복사하기 
5. Swagger에서 User Users 하위의 [post] /user/users 클릭
6. Try it out 클릭
7. `company_id:` 부분에 4번에서 복사한 회사 아이디 붙여넣기
8. `code`에 원하는 코드(6자리), `password`에 원하는 비밀번호 입력 후 실행 
 
  
<br/>

## DBeaver이용하여 데이터베이스 확인하기

1. DBeaver 설치 + 실행
2. Select Database - PostgreSQL 선택
3. 형광펜 부분 `carcle-api` 의 `.env.dev` 보고 설정
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188628282-f87f636f-5f82-493c-a69e-19645fb24588.png" width="80%"/>
</div>
<div align="center">

`.env.dev`

| Database | DATABASE_NAME     |
| -------- | ----------------- |
| Port     | DATABASE_PORT     |
| userName | DATABASE_USER     |
| Password | DATABASE_PASSWORD |

</div>
