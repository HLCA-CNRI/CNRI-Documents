# DB 구조 - Lyllia

## Schema

- user
  - id: uuid
  - sso_id: string
  - role: **enum**
  - email: string
  - name: string
  - phone: string
  - device: **enum**
  - transport: **enum**
  - fuel: **enum nullable**
  - company_id: FK company
  - residence_id: FK sigungu
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- company
  - id: uuid
  - name: string
  - coordinate_x: double precision // 회사 좌표 x
  - coordinate_y: double precision // 회사 좌표 y
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- sido // 시도
  - id: int
  - name: string
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- sigungu // 시군구
  - id: int
  - name: string
  - sido_id: FK sido
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
  - id: int
  - coordinate_x: double precision // 행정구역 중심점 좌표 x
  - coordinate_y: double precision // 행정구역 중심점 좌표 y
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime
- record
  - id: uuid
  - transport: **enum**
  - company_go: boolean
  - meeting_go: boolean
  - meeting_location: FK sigungu **nullable**
  - meeting_time: **enum nullable**
  - company_back: boolean **nullable**
  - meal: **enum**
  - overwork: boolean
  - user_id: FK user
  - created_at: datetime
  - updated_at: datetime
  - deleted_at: datetime

## Enums

- role: enum
  - user // 일반 사용자
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
  - convenience_food // 간편식
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
  // findByNameAndPhoneAndCompany(
  //   userName: string,
  //   phoneNumber: string,
  //   company: string
  // ): Promise<UserModel>;
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
  // Read by UserId
  findAllByUserId(id: string): Promise<RecordModel[]>;
  findAllByUserIdWithPagination(id: string, skip: number, take: number): Promise<RecordModel[]>;
  // Read by CompanyId
  findAllByCompanyId(id: string): Promise<RecordModel[]>;
  findAllByCompanyIdWithPagination(id: string, skip: number, take: number): Promise<RecordModel[]>;
  // Update
  update(id: string, record: RecordModel): Promise<RecordModel>;
  // Delete
  deleteById(id: string): Promise<DeleteResult>;
  // Delete Hard
  deleteHardById(id: string): Promise<DeleteResult>;
  ```
- CompanyRepository (SidoRepository, SigunguRepository, CoordinateRepository도 형식 동일)
  ```ts
  // Create
  insert(company: CompanyModel): Promise<CompanyModel>;
  // Read
  findAll(): Promise<CompanyModel[]>;
  findById(id: number): Promise<CompanyModel>;
  // Update
  update(id: number, company: CompanyModel): Promise<CompanyModel>;
  // Delete
  deleteById(id: number): Promise<DeleteResult>;
  ```
