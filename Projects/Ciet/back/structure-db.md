# DB 구조 - Ciet

## Schema

- user
  - id: uuid
  - sso_id: string // keycloak 서버의 user id
  - role: **enum** // USER, USER_COMPANY, ADMIN, GUEST
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
  - meeting_time: **enum nullable** // 미팅 시간대(오전-오전, 오전-오후, 오후-오후)
  - meal: **enum** // 식사 방식(식당, 배달 등)
  - overwork: boolean // 야근 여부
  - emission_total: double precision // 배출량 총합
  - emission_commute: double precision // 배출량 출퇴근
  - emission_work: double precision // 배출량 근무
  - emission_overwork: double precision // 배출량 야근
  - emission_meal: double precision // 배출량 식사
  - emission_meeting: double precision // 외부 배출량 미팅
  - emission_trash: double precision // 배출량 쓰레기
  - user_id: FK user // 기록 저장한 사용자 id
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- meeting
  - id: uuid
  - company_back: boolean // 미팅 후 회사 복귀 여부
  - meeting_order: int // 미팅 순서
  - location: FK sigungu // 외부 미팅 O -> 미팅 장소(시군구)
  - record_id: FK record // 외부 미팅에 연결된 개인 기록(record)
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- companyRecord // 회사 주간 기록 데이터
  - id: uuid
    // 회사 건물 운영
  - buildingElectricity: double precision // 전기 사용량(단위: kWh)
  - buildingGas: double precision // 가스 사용량(단위: MJ)
    // 법인 차량 운영
  - corp_car_gasoline: double precision // 휘발유 사용량(단위: L)
  - corp_car_diesel: double precision // 경유 사용량(단위: L)
  - corp_car_lpg: double precision // LPG 사용량(단위: L)
  - corp_car_hybrid: double precision // 하이브리드 사용량(단위: L)
  - corp_car_electricity: double precision // 전기 사용량(단위: kWh)
  - corp_car_hydrogen: double precision // 수소 사용량(단위: kg)
    // 공유 차량 운영
  - share_car_gasoline: double precision // 휘발유 사용량(단위: L)
  - share_car_diesel: double precision // 경유 사용량(단위: L)
  - share_car_lpg: double precision // LPG 사용량(단위: L)
  - share_car_hybrid: double precision // 하이브리드 사용량(단위: L)
  - share_car_electricity: double precision // 전기 사용량(단위: kWh)
  - share_car_hydrogen: double precision // 수소 사용량(단위: kg)
    // 배출량 결과
  - emission_total: double precision // 회사 배출량(단위: tCO2e)
  - emission_building_electricity: double precision // 회사 운영(전기) 배출량(단위: tCO2e)
  - emission_building_gas: double precision // 회사 운영(가스) 배출량(단위: tCO2e)
  - emission_corp_car: double precision // 법인 차량 운영 배출량(단위: tCO2e)
  - emission_share_car: double precision // 공유 차량 운영 배출량(단위: tCO2e)
    // 에너지 사용량 결과
  - energy_usage_total: double precision // 회사 에너지 사용량(단위: GJ)
  - energy_usage_building_electricity: double precision // 회사 운영(전기) 에너지 사용량(단위: GJ)
  - energy_usage_building_gas: double precision // 회사 운영(가스) 에너지 사용량(단위: GJ)
  - energy_usage_corp_car: double precision // 법인 차량 운영 에너지 사용량(단위: GJ)
  - energy_usage_share_car: double precision // 공유 차량 운영 에너지 사용량(단위: GJ)
    // 기타 사항
  - sales_amount: numeric // 회사 매출액(단위: 억)
  - company: FK company
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime

## Enums

- user_role
  - user // 개인 사용자
  - user_company // 회사 사용자
  - admin // 관리자
  - guest // 게스트(임시)
- user_device
  - computer // 데스크탑
  - laptop
  - smartphone
  - tablet
- user_transport
  - car // 자가용
  - taxi // 택시
  - bus // 버스
  - subway // 지하철
  - motorcycle // 오토바이
  - elec_kickboard // 전동 킥보드
  - human_power // 도보/자전거
- car_fuel
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
- waste
  - food_waste // 음식물 쓰레기
  - package_waste // 포장 쓰레기
- company_record_period

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
  findAllByDate(date: Date): Promise<RecordModel[]>;
  findAllWithPagination(skip: number, take: number): Promise<RecordModel[]>;
  // Read One
  findById(id: string): Promise<RecordModel>;
  findByDate(date: Date): Promise<RecordModel>;
  // Read by UserId
  findByUserIdAndDate(id: string, start: Date, end: Date): Promise<RecordModel>;
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
  getAverageByUserId(id: string, start: Date, end: Date): Promise<number>;
  getAverageByCompanyId(id: string, start: Date, end: Date): Promise<number>;
  getTotalAverageByUserId(id: string): Promise<number>; // 전체 데이터에 대한 평균
  getTotalAverageByCompanyId(id: string): Promise<number>; // 전체 데이터에 대한 평균
  getSumByUserId(id: string, start: Date, end: Date): Promise<CategorySumType>; // 기간 내 총합 구할 때 사용
  getSumByCompanyId(id: string, start: Date, end: Date): Promise<CategorySumType>; // 기간 내 배출량 총합 구할 때 사용
  getSumOfEachCategoryByCompanyId(
    id: string,
    start: Date,
    end: Date
  ): Promise<DetailedCategorySumType>; // 기간 내 각 카테고리 항목별 배출량 합계 구할 때 사용
  getSumsByCompanyId(id: string, start: Date, end: Date): Promise<CategorySumType[]>; // 일자별 합계 구할 때 사용
  // 특정 회사의 기간 내 임직원 record 수 구할 때 사용
  countByCompanyId(id: string, start: Date, end: Date): Promise<number>;

  // For copied data
  getUsersWhoMadeRecordByDate(
    start: Date,
    end: Date
  ): Promise<{user_id: string; created_date: Date}[]>; // 특정 날짜에 레코드를 생성한 사용자를 조회
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
  findByPeriod(date: Date, period: CompanyRecordPeriod): Promise<CompanyRecordModel>;
  // Read by CompanyId
  findAllByCompanyId(id: string): Promise<CompanyRecordModel[]>;
  findAllByCompanyIdWithPagination(
    id: string,
    skip: number,
    take: number
  ): Promise<CompanyRecordModel[]>;
  findAllByCompanyIdBetweenDates(id: string, start: Date, end: Date): Promise<CompanyRecordModel[]>;
  findAllByCompanyIdAndPeriodBetweenDates(
    id: string,
    start: Date,
    end: Date,
    period: CompanyRecordPeriod
  ): Promise<CompanyRecordModel[]>;
  // Update
  update(id: string, record: CompanyRecordModel): Promise<CompanyRecordModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  // Delete Hard
  deleteHardById(id: string): Promise<DeleteResult>;
  ```
