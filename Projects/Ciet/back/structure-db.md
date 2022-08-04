# DB 구조 - Ciet

## Schema

- user
  - id: uuid
  - sso_id: string // keycloak 서버의 user id
  - role: **enum** // USER, ADMIN, GUEST
  - email: string
  - name: string
  - phone: string // 폰 번호. 010-0000-0000 형식.
  - device: **enum** // 주 사용 전자기기
  - transport: **enum** // 주요 교통 수단
  - fuel: **enum nullable** // (transport 자가용인 경우) 사용 연료
  - company_id: FK company
  - residence_id: FK sigungu
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- company
  - id: uuid
  - company_name: string
  - mail_domain: string // 회사 이메일 도메인
  - coordinate_x: double precision // 회사 좌표 x
  - coordinate_y: double precision // 회사 좌표 y
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- sido // 시도
  - id: int // 행정구역 법정동코드
  - name: string
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- sigungu // 시군구
  - id: int // 행정구역 법정동코드
  - name: string
  - sido_id: FK sido // 시군구가 속해 있는 시도의 id
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
  <!-- - dong // 읍면동
  - id: int
  - name: string
  - sigungu_id: FK sigungu
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime -->
- coordinate // 행정구역 중심점 좌표
  - id: int // 행정구역 법정동코드
  - coordinate_x: double precision // 행정구역 중심점 좌표 x
  - coordinate_y: double precision // 행정구역 중심점 좌표 y
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- record // 개인 사용자 일일 기록 데이터
  - id: uuid
  - transport: **enum** // 기록 저장 당시 주요 교통 수단
  - company_go: boolean // 회사 출근 여부
  - meeting_go: boolean // 외부 미팅 여부
  - meeting_location: FK sigungu **nullable** // 외부 미팅 O -> 미팅 장소(시군구)
  - meeting_time: **enum nullable** // 미팅 시간대(오전-오전, 오전-오후, 오후-오후)
  - company_back: boolean **nullable** // 미팅 후 회사 복귀 여부
  - meal: **enum** // 식사 방식(식당, 배달 등)
  - overwork: boolean // 야근 여부
  - emissionTotal: double precision // 배출량 총합
  - emissionCommute: double precision // 배출량 출퇴근
  - emissionWork: double precision // 배출량 근무
  - emissionOverwork: double precision // 배출량 야근
  - emissionMeal: double precision // 배출량 식사
  - emissionMeeting: double precision // 외부 배출량 미팅
  - emissionTrash: double precision // 배출량 쓰레기
  - user_id: FK user // 기록 저장한 사용자 id
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- companyRecord // 회사 주간 기록 데이터
  - id: uuid
    // 회사 건물 운영
  - buildingElectricity: double precision // 전기 사용량
  - buildingGas: double precision // 가스 사용량
    // 법인 차량 운영
  - corpCarGasoline: double precision // 휘발유 사용량
  - corpCarDiesel: double precision // 경유 사용량
  - corpCarLpg: double precision // LPG 사용량
  - corpCarElectricity: double precision // 전기 사용량
  - corpCarHydrogen: double precision // 수소 사용량
  - corpCarCng: double precision // CNG 사용량
    // 공유 차량 운영
  - shareCarGasoline: double precision // 휘발유 사용량
  - shareCarDiesel: double precision // 경유 사용량
  - shareCarLpg: double precision // LPG 사용량
  - shareCarElectricity: double precision // 전기 사용량
  - shareCarHydrogen: double precision // 수소 사용량
  - shareCarCng: double precision // CNG 사용량
    // 배출량 결과
  - emissionTotal: double precision // 회사 배출량
  - emissionBuildingElectricity: double precision // 회사 운영(전기) 배출량
  - emissionBuildingGas: double precision // 회사 운영(가스) 배출량
  - emissionCorpCar: double precision // 법인 차량 운영 배출량
  - emissionShareCar: double precision // 공유 차량 운영 배출량
    // 기타 사항
  - company: FK company
  - createdAt: datetime
  - updatedAt: datetime
  - deletedAt: datetime

## Enums

- role: enum
  - user // 개인 사용자
  - user_company // 회사 사용자
  - admin // 관리자
  - guest // 게스트(임시)
- device: enum
  - computer
  - laptop
  - smartphone
  - tablet
- transport: enum
  - car // 자가용
  - taxi // 택시
  - bus // 버스
  - subway // 지하철
  - motorcycle // 오토바이
  - elec_kickboard // 전동 킥보드
  - human_power // 도보/자전거
- fuel: enum nullable
  - gasoline // 휘발유
  - diesel // 경유
  - lpg // LPG
  - hybrid // 하이브리드
  - electricity // 전기
  - hydrogen // 수소
- meeting_time
  - m_m // 오전-오전
  - m_a // 오전-오후
  - a_a // 오후-오후
- meal
  - restaurant // 식당
  - delivery // 배달
  - convenience_food // 편의점
  - packed_lunch // 도시락
  - none // None

## Repository

- UserRepository
  ```ts
  // Create
  insert(user: UserModel): Promise<UserModel>;
  // Read
  findAll(): Promise<UserModel[]>;
  findById(id: string): Promise<UserModel>;
  findByEmail(email: string): Promise<UserModel>;
  findIdBySsoId(ssoId: string): Promise<string>;
  // Update
  update(id: string, user: UserModel): Promise<UserModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  deleteHardById(id: string): Promise<DeleteResult>;
  ```
- RecordRepository
  ```ts
  // Create
  insert(record: RecordModel): Promise<RecordModel>;
  // Read All
  findAll(): Promise<RecordModel[]>;
  findAllWithPagination(skip: number, take: number): Promise<RecordModel[]>;
  // Read One
  findById(id: string): Promise<RecordModel>;
  findByDate(date: Date): Promise<RecordModel>;
  // Read by UserId
  findAllByUserId(id: string): Promise<RecordModel[]>;
  findAllByUserIdWithPagination(id: string, skip: number, take: number): Promise<RecordModel[]>;
  findAllByUserIdBetweenDates(id: string, start: Date, end: Date): Promise<RecordModel[]>;
  // Read by CompanyId
  findAllByCompanyId(id: string): Promise<RecordModel[]>;
  findAllByCompanyIdWithPagination(id: string, skip: number, take: number): Promise<RecordModel[]>;
  findAllByCompanyIdBetweenDates(id: string, start: Date, end: Date): Promise<RecordModel[]>;
  // Update
  update(id: string, record: RecordModel): Promise<RecordModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  // Delete Hard
  deleteHardById(id: string): Promise<DeleteResult>;
  // For Calculation
  getAverageByCompanyId(id: string, start: Date, end: Date): Promise<number>;
  ```
- CompanyRepository
  ```ts
  // Create
  insert(company: CompanyModel): Promise<CompanyModel>;
  // Read
  findAll(): Promise<CompanyModel[]>;
  findById(id: string): Promise<CompanyModel>;
  findByName(name: string): Promise<CompanyModel>;
  findByMailDomain(mailDomain: string): Promise<CompanyModel>;
  // Update
  update(id: string, company: CompanyModel): Promise<CompanyModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  // Delete Hard
  deleteHardById(id: string): Promise<DeleteResult>;
  ```
- SidoRepository
  ```ts
  // Create
  insert(company: SidoModel): Promise<SidoModel>;
  // Read
  findAll(): Promise<SidoModel[]>;
  findById(id: number): Promise<SidoModel>;
  findByName(name: string): Promise<SidoModel>;
  // Update
  update(id: number, company: SidoModel): Promise<SidoModel>;
  // Delete
  deleteById(id: number): Promise<DeleteResult>;
  ```
- SigunguRepository
  ```ts
  // Create
  insert(company: SigunguModel): Promise<SigunguModel>;
  // Read
  findAll(): Promise<SigunguModel[]>;
  findAllBySidoId(id: number): Promise<SigunguModel[]>;
  findById(id: number): Promise<SigunguModel>;
  findByName(name: string): Promise<SigunguModel>;
  // Update
  update(id: number, company: SigunguModel): Promise<SigunguModel>;
  // Delete
  deleteById(id: number): Promise<DeleteResult>;
  ```
- CoordinateRepository
  ```ts
  // Create
  insert(company: CoordinateModel): Promise<CoordinateModel>;
  // Read
  findAll(): Promise<CoordinateModel[]>;
  findById(id: number): Promise<CoordinateModel>;
  // Update
  update(id: number, company: CoordinateModel): Promise<CoordinateModel>;
  // Delete
  deleteById(id: number): Promise<DeleteResult>;
  ```
- CompanyRecordRepository
  ```ts
  // Create
  insert(record: CompanyRecordModel): Promise<CompanyRecordModel>;
  // Read All
  findAll(): Promise<CompanyRecordModel[]>;
  findAllWithPagination(skip: number, take: number): Promise<CompanyRecordModel[]>;
  // Read One
  findById(id: string): Promise<CompanyRecordModel>;
  findByDate(date: Date): Promise<CompanyRecordModel>;
  // Read by CompanyId
  findAllByCompanyId(id: string): Promise<CompanyRecordModel[]>;
  findAllByCompanyIdWithPagination(
    id: string,
    skip: number,
    take: number
  ): Promise<CompanyRecordModel[]>;
  findAllByCompanyIdBetweenDates(id: string, start: Date, end: Date): Promise<CompanyRecordModel[]>;
  // Update
  update(id: string, record: CompanyRecordModel): Promise<CompanyRecordModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  // Delete Hard
  deleteHardById(id: string): Promise<DeleteResult>;
  ```
