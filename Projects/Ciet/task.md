# Ciet Tasks

Ciet 프로젝트 개발 작업 필요한 내용 정리

- 범례 :
  - ~~완료 사항~~
  - 미완료 사항
  - _참고 사항_
  - **논의 혹은 관찰 필요 사항**

## Client

### 비밀번호 찾기 페이지

- ~~임시 비밀번호 발송 후 화면(success) UI 수정 필요~~

### 마이페이지

- ~~상단 누적 배출량 표시 -> 계산식 개선 필요(전체 데이터에 대한 평균)~~

### 질문 페이지

- **마이페이지로 이동 가능한 버튼(or else) - 논의 필요**

### 결과 페이지

- 최상단
  - ~~우측 아이콘(개인/회사)~~ & 로고(일단 자리만 확보) -> ~~회사 화면 구현 필요~~ (로고는 후순위)
- 공유하기 버튼 기능(후순위)
  ~~소셜미디어 아이콘~~
  ~~공유하기 팝업~~
  SNS 공유 기능

### 회사 관리자 페이지

- 목록
  - **우상단 회사 계정 아이콘 기능 추가 - 논의 필요**
- ~~데이터 입력 영역~~
  - ~~인풋 라벨 수정(휘발유(내연기관) / 휘발유(하이브리드)) & 단위 수정(휘발유 L, 전기 kWh, 수소 kg)~~
  - ~~배출 계수 수정(휘발유(하이브리드) = 휘발유(내연기관))~~
  - ~~카테고리별 배출량 테이블 정렬(탄소배출량 우측에 붙을 수 있게)~~
  - ~~인풋 단위 너비 크기 늘리기(정렬 목적)~~
- ~~데이터 결과 영역~~
  - ~~우하단 총 배출량 표기~~

### PWA

- Web Push
- Service Worker
- Manifest
- Add to Home screen(Icon)
- Splash Image

## Server

- 테스트용 더미 계정 및 더미 데이터 생성
  - 생성할 더미 계정 수: 365
  - 계정별 개인 기록 데이터 생성: 최근 3~6개월
  - 회사 관리자 주간/월간 데이터 생성: 최근 3~6개월
- API 캐싱
- 과부하 테스트