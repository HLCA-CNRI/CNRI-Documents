# Github Secret Key 설정하기

## 목차 
> - [토큰 설정하기](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/setting-secretkey.md#토큰-설정하기)  
> - [필요한 토큰 종류 및 발급 방법](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/setting-secretkey.md#필요한-토큰-종류-및-발급-방법)


<br/>

## 토큰 설정하기

개인 계정에  secret 키 설정이 필요하다. 

1. lyllia-client 를 `fork` 한 repo 접속
2. Setting > Security > Secrets > Actions
3. 우측 상단의 New repository secret 버튼 클릭 
4.  Name : 토큰 이름 , Secret : 실제 토큰 내용
    - 필요한 토큰들은 [필요한 토큰 종류 및 발급방법](https://github.com/HLCA-CNRI/CNRI-Documents/blob/main/Projects/Ciet/setting-secretkey.md#필요한-토큰-종류-및-발급-방법) 참고
5. 토큰 설정 완료 화면은 아래와 같아야 함. (토큰 순서는 무관)
![image](https://user-images.githubusercontent.com/77582221/189015394-87b2e22a-3548-41ad-ab1d-4d58a73580db.png)


<br/>


## 필요한 토큰 종류 및 발급 방법

### 토큰 종류

`VERCEL_TOKEN` , `GH_TOKEN` , `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`

### VERCEL_TOKEN  발급 방법

1. [vercel 사이트](https://vercel.com/dashboard) 접속
2. 상단 바의 Settings > 죄측 Nav바의 Tokens > Create  눌러서 토큰 생성


### GH_TOKEN 발급 방법

1. github  접속 > 프로필 이미지 클릭 > Settings 클릭 
2. 좌측 Nav 맨 아래의 Developer settings > Personal access tokens > Generate new token 
3. Note나 Expiration은 알아서 설정 
4. Select Scope
![image](https://user-images.githubusercontent.com/77582221/189014498-b39e7d60-e5e8-401c-96f8-bdbed01624cd.png)

5. github 토큰 발급 완료 


### VERCEL_ORG_ID, VERCEL_PROJECT_ID 토큰 발급 방법

1. fork했던 lyllia-client 열기 
2. 터미널에 `npm i -g vercel` 
3. 터미널에 `vercel` 입력 
4. `Link to existing project? [y/N]` 질문에 `N`  선택하면 알아서 프로젝트 연결됨.
5. 프로젝트 내부에 `.vercel`디렉토리 생성
6. .vercel 내부의   `project.json`에서  `orgId`는 `VERCEL_ORG_ID`로 , `projectId`는 `VERCEL_PROJECT_ID`로 사용하면 됨
