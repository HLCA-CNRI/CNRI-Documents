# 배포 상태

- 클라이언트 : Vercel (안경호)
  - cis.cnrikorea.net
  - ! 배포용으로 AWS EC2 인스턴스 생성 필요
- 서버 : AWS EC2 인스턴스
  - cis-api(cis-api.cnrikorea.net) : 배포용
  - cis-api-test(cis-api-test.cnrikorea.net) : 테스트용
- 사용자 인증 : AWS EC2 인스턴스
  - Keycloak_Auth(auth.cnrikorea.net) : 사용자 인증 서버
