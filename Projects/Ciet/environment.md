# CIET 첫 환경 설정 
ciet (새 이름) == lyllia (옛날 이름) 

## 목차 
> - [Repo clone](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#repo-clone)  
> - [환경 변수 설정](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)  
> - [Lyllia-client repo 세부 세팅](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#lyllia-client-repo-%EC%84%B8%EB%B6%80-%EC%84%B8%ED%8C%85)  
> - [Ciet 서버(lyllia-api) 실행](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#ciet-%EC%84%9C%EB%B2%84lyllia-api-%EC%8B%A4%ED%96%89)  
> - [Keyclock 서버 연결](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#keyclock-%EC%84%9C%EB%B2%84-%EC%97%B0%EA%B2%B0)
> - [Swagger](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#swagger)
> - [CNRI 회사 계정 등록하기](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#cnri-%ED%9A%8C%EC%82%AC-%EA%B3%84%EC%A0%95-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0)
> - [CIET 서비스 가입&로그인 후 회사 계정으로 변경하기](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#ciet-%EC%84%9C%EB%B9%84%EC%8A%A4-%EA%B0%80%EC%9E%85%EB%A1%9C%EA%B7%B8%EC%9D%B8-%ED%9B%84-%ED%9A%8C%EC%82%AC-%EA%B3%84%EC%A0%95%EC%9C%BC%EB%A1%9C-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0)

<br/>

## Repo clone

`lyilla-client` , `lyilla-api`, `keycloak` 

git clone한 뒤, upstream 설정까지 하기

<br/>

## 환경 변수 설정

`lyilla-client` , `lyilla-api` 의 루트 디렉토리에 환경 변수 파일 `env.dev` 각각 삽입


>⚠️ **주의 1**
>
>vscode에서 파일 복붙 하고 난 뒤, `env.dev` 파일명 앞에 . 추가 
>
>ex) `.env.dev`
>
>⇒ gitignore 적용하기 위함 


>⚠️ **주의 2**
>
>`lyllia-client`의 `.env.dev`에서 `NEXT_PUBLIC_AUTH_ISSUER` 부분을 아래와 같이 바꿔주기 
>
> ```
> NEXT_PUBLIC_AUTH_ISSUER=http://localhost:8080/auth/realms/{NEXT_PUBLIC_AUTH_REALM}
> ```

<br/>

## Lyllia-client repo 세부 세팅

`Node js` 설치 - 설치 확인 명령어 `node -v`

- 만약 설치했는데  node 없다고 하면 재부팅

`npm install -g yarn` 

그 후,

yarn 설치 시 

`corepack enable` ← 오류나도 무시

`yarn set version berry` 해주기

`yarn install` 후, 

`yarn start:dev`로 실행 가능 

<br/>


## Ciet 서버(lyllia-api) 실행

Docker - 이미지 pull 및 실행 으로 서버 실행  

1. wsl2 설치 
2. Docker설치 
3. 아래 명령어 실행 ({}부분에는 괄호 없이 {안에 있는 내용}에 해당하는 것만 적어주면된다.)
``` bash
docker run --name lyllia -e POSTGRES_PASSWORD=code1234 -e POSTGRES_USER=cnri -p 5432:5432 -d seahahn/postgres-cnri:latest
docker exec -it {lyllia-api의 env.dev에 적혀있는 DATABASE_NAME} bash 
psql -U {lyllia-api의 env.dev에 적혀있는 DATABASE_USER} postgres 
CREATE DATABASE lyllia; <- 세미콜론 필수
```

5. [keyclock 서버 연결](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#keyclock-%EC%84%9C%EB%B2%84-%EC%97%B0%EA%B2%B0)  ← 회원 계정 정보(아이디, 비밀번호 등)을 관리하는 서버 
6. `lyllia-api` 실행 
    
    vscode로 lyllia-api 디렉토리 열어서 터미널에 실행   
    
    `npm install -g yarn pnpm`
    
    `pnpm install`
    
    `pnpm start:dev` 
    
6. 실행되는지 확인했으면, 서버 중단하고 `pnpm seed:dev` 실행하여 데이터 받아오기 (시도, 시군구 등의 기본 데이터를 뿌려주는 과정) 
7. 다시 서버 실행
8. [Swagger 사용 단계로 이동](https://github.com/HLCA-CNRI/CNRI-Documents/edit/main/Projects/Ciet/environment.md#swagger)

<br/>

## Keyclock 서버 연결

1. vs code 로 keyclock clone 한 디렉토리 열기 
2. 포트 설정 변경 : `docker-compose.yml` 파일에서 포트 내용 아래처럼 변경- services 하위에 있는 port 만   
    ![1](https://user-images.githubusercontent.com/77582221/188625592-0da1f731-d3a1-4042-a061-1ccf743d31df.png)

3. vs code의 terminal에 명령어 입력 - `docker-compose up -d --build` : keyclock 서버 실행됨.
4. [localhost:8080/](http://localhost:8080/auth/)으로 이동
5. 첫번째 박스 클릭 - admin 계정으로 로그인 :  
    - `keyclock` repo에  `docker-compose.yml`  파일에 id, 비번 적혀있음. (Ctrl+f: `KEYCLOAK_ADMIN` ,  `KEYCLOAK_ADMIN_PASSWORD`)
    - 
    > ⚠️ **주의**
    >
    > `docker-compose.yml` 에 있는 `KEYCLOAK_USER` ,  `KEYCLOAK_PASSWORD` 값과 , 
    > `lyllia-api`의 `.env.dev` 에  있는 KEYCLOAK_ADMIN_ID 와 KEYCLOAK_ADMIN_PASSWORD 가 동일한지 확인. 
    >
    > 만약 동일하지 않다면, 
    > `lyllia-api`의 `.env.dev` 에 있는 id, pw 값을 실제 keyclock에 로그인된 id, pw 값으로 맞추기  


6. 좌측 nav 바에서 Add Realm 클릭 
    - Lyllia-test를 생성해야함 (생성 해야 아래 사진 처럼 바뀜)   
    - 생성할 때 json 파일 넣기   
    - (`lyllia-test-realm.json` 따로 저장해둠)  
<div align="center">
  <img src="https://user-images.githubusercontent.com/77582221/188625730-d593046a-a91c-4681-9712-efa77b5e09fb.png" width="20%"/>
</div>
  

7. client admin 계정 생성
    - Manage - Users - Add user - email 입력하여 생성
    - 생성된 화면의 상단 Credentials → 비밀번호&비밀번호 확인&Temporary off → set pw
    - 다시 내 계정 상단 Groups → admin → join (없으면 role, group 생성 후 그룹 추가 ([keyclock 서버 연결](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#keyclock-%EC%84%9C%EB%B2%84-%EC%97%B0%EA%B2%B0)의 11, 12번)
    
8. keycloak secret key
    - Client → `lyllia-client` → edit → Credentials → secret key 복사
    - lyllia-client, lyllia-api의 `.env.dev`에 붙여넣기
  
   ``` bash
   // client
    NEXT_PUBLIC_AUTH_CLIENT_SECRET= 여기에 붙여넣기  

   // api
    KEYCLOAK_CLIENT_SECRET= 여기에 붙여넣기 
   ```
 
9. realm setting > Login : 아래와 동일하게 맞추기 
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188626026-b88077bf-6804-4a8e-bc74-8f41a7a23ef6.png" width="80%"/>
</div> 


10. Realm Settings > Tokens : Access Token 주기 수정(Refresh token 보다 짧아야함)   
    원래 30분~1시간 정도가 적절한데 테스트 편하게 하기 위함. 
    
    <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188626026-b88077bf-6804-4a8e-bc74-8f41a7a23ef6.png" width="80%"/>
</div> 

11. Roles : 아래 형광펜 role 들 없으면 Add Role해서 등록

 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627001-4402cbee-8656-4733-bf2a-175878f5ea14.png" width="80%"/>
</div> 


12 .Groups : 얘도 없으면 추가 

 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627037-e2726772-5831-4bd4-a648-96bf42c33da5.png" width="80%"/>
</div>
  
  추가 후 각 group명 더블 클릭  
  
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627094-f6259b72-18a3-4411-9dab-40669f07f65e.png" width="80%"/>
</div>

  Role Mapping에서 11번에서 생성한 role 이름과 같은 걸로 mapping 해주기



13. redis 
    
   redis 서버 실행 - 아무 터미널에다 치면 됨.
    
   ``` bash 
    docker pull redis
    docker run --name my_redis -p 6379:6379 -d redis
   ```

14. 이제 [lyilla-api 실행 단계](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/environment.md#ciet-%EC%84%9C%EB%B2%84lyllia-api-%EC%8B%A4%ED%96%89)로 back


<br/>

## Swagger

api확인 및 테스트 하는 곳 

1. lyllia-api 에서 서버 실행 후 [http://localhost:3001/docs/](http://localhost:3001/docs/) 로 접속
2. Authorize 클릭 
 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627581-9d541e4b-d8b9-475f-a292-f79ff1793297.png" width="80%"/>
</div>



3. 로그인 - id : lyllia-client , pw :  lyllia-client의 secret key
 <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627894-af47888f-f362-4f09-9226-04993464f840.png" width="80%"/>
</div>

<br/>

## CNRI 회사 계정 등록하기 

1. Swagger 접속 
2.  Admin Company section에 있는 POST /admin/companies 클릭 
3.  Try it out 클릭
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188627987-ed046faf-b39e-4019-b887-d2f518598ed0.png" width="80%"/>
</div>
  
  
  요 상태 그대로 execute 
  
  <div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188628218-8427d6af-7dbb-4d99-b0aa-bd223d2bc9c3.png" width="80%"/>
</div>
  
 
  
<br/>

## CIET 서비스 가입&로그인 후 회사 계정으로 변경하기

1. CIET 에서 회원가입 
2. DBeaver 설치 + 실행 
3. Select Database  - PostgreSQL 선택 
4. 형광펜 부분  `lyllia-api` 의 `env.dev` 보고 설정
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188628282-f87f636f-5f82-493c-a69e-19645fb24588.png" width="80%"/>
</div>
<div align="center">
    
`env.dev`

  | Database | DATABASE_NAME |
  | --- | --- |
  | Port | DATABASE_PORT |
  |  userName | DATABASE_USER |
  | Password | DATABASE_PASSWORD |
</div>
  
5. user 테이블 들어가서 role : user_company로 변경
<div margin="3px" align="center"> 
    <img src="https://user-images.githubusercontent.com/77582221/188628527-070ee680-7f1c-4f1c-b725-82bfe945c135.png" width="80%"/>
</div>

6. [http://localhost:8080](http://localhost:8080) 접속 → 왼쪽 nav 에서 Users  선택 → view all users  → user_company로 전환한 계정 선택 → Groups 
   ![image](https://user-images.githubusercontent.com/77582221/194007286-b760db25-5ca1-4b1c-93c5-e93bab7e539d.png)

   
    
    위와 같이 변경

     
