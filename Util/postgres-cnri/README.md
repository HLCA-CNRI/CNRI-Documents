# postgres-cnri

- Docker postgres image에 한글 설정을 포함한 image 생성을 위한 Dockerfile
- 기존의 official postgres image의 경우 기본 Collation이 en_US.utf8로 설정되어 있음
- 또한 Collation을 ko_KR.utf8로 설정하려 해도 image 내에 기본 포함되어 있지 않아 불가능함
- 이로 인해 이 image를 사용하여 DB 생성 후 데이터 query 시 한글 정렬이 필요할 때 제대로 정렬이 되지 않음
- 이를 해결하기 위해 image 생성 시 Collation에 ko_KR.utf8을 추가하여 DB에도 적용할 수 있도록 만듦
- image 이름: seahahn/postgres-cnri
  - 아래와 같이 이미지 받아서 container 만들면 됨
  - docker pull seahahn/postgres-cnri:latest

<참고 자료>

- [공식 PostgreSQL Docker 이미지에 한글 적용하기](https://www.bearpooh.com/136)
- [2. Docker 이미지 만들고 배포하기](https://dreamsea77.tistory.com/329)
