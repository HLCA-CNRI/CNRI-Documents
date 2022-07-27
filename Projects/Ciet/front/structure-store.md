# Redux store structure

redux-toolkit을 이용하여 전역적으로 관리하는 state store의 구조를 정리함

## record

```ts
export type RecordType = {
  id: string;
  device: UserDevice;
  transport: UserTransport;
  carFuel: CarFuel | null;
  companyGo: boolean;
  meetingGo: boolean;
  meetingLocation: SigunguStateType | null;
  meetingTime: MeetingTime | null;
  companyBack: boolean | null;
  meal: Meal;
  overwork: boolean;
  createdAt: Date;
};
```

## recordList

```ts
export type RecordListType = {
  recordList: RecordType[];
};
```
