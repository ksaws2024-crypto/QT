# KIE 2026 B2B 매칭 플랫폼 — Lovable 프롬프트

아래 내용을 Lovable 채팅창에 복사·붙여넣기 하세요.

---

## ▶ Lovable 입력 프롬프트 (전체 복사)

```
Build a full-stack admin dashboard for "KIE 2026 한국수입엑스포 B2B 매칭 플랫폼" using React + Supabase.

---

## 앱 개요
한국수입엑스포(KIE 2026) B2B 1:1 소싱 상담회 운영 관리 시스템.
행사 기간: 2026년 6월 23일(화) ~ 25일(목), COEX Hall B
상담 시간: 10:30~16:00 (25분 상담 + 5분 휴식, 점심 12:30~13:30 제외)

---

## Supabase 테이블 설계

### 1. suppliers (공급업체 - 해외 참가사)
- id: uuid (PK)
- company_name: text (회사명)
- country: text (국가명)
- country_flag: text (국기 이모지)
- category: text (식품·음료 / 뷰티·화장품 / 건강·기능식품 / 리빙·가구 / 주류·음료)
- booth_number: text (부스번호, 예: B-101)
- tagline: text (한줄 소개)
- description: text (상세 설명)
- products: text[] (주요 제품 배열)
- contact_email: text
- website: text
- is_active: boolean (노출 여부)
- created_at: timestamp

### 2. buyers (바이어 - 국내 기업)
- id: uuid (PK)
- name: text (담당자 성명)
- company: text (회사명)
- position: text (직책)
- email: text
- phone: text
- interest_categories: text[] (관심 카테고리)
- status: text (pending / approved / rejected)
- notes: text (내부 메모)
- registered_at: timestamp

### 3. bookings (상담 예약)
- id: uuid (PK)
- buyer_id: uuid (FK → buyers)
- supplier_id: uuid (FK → suppliers)
- day: text (2026-06-23 / 2026-06-24 / 2026-06-25)
- slot_id: text (s1~s9)
- slot_time: text (예: 10:30~11:00)
- status: text (confirmed / cancelled)
- created_at: timestamp
- UNIQUE(supplier_id, day, slot_id) — 같은 슬롯 중복 방지

---

## 어드민 페이지 구성 (사이드바 네비게이션)

### 1. 대시보드 (/)
- 통계 카드: 공급업체 수 / 바이어 수 / 총 예약 건수 / 승인 대기 바이어 수
- 일자별 예약 현황 차트 (6/23, 6/24, 6/25)
- 최근 예약 5건 목록
- 승인 대기 바이어 알림

### 2. 공급업체 관리 (/suppliers)
- 공급업체 목록 테이블 (검색, 카테고리 필터, 국가 필터)
- [공급업체 추가] 버튼 → 모달 폼:
  - 회사명, 국가, 국기이모지, 카테고리(셀렉트), 부스번호
  - 한줄소개, 상세설명(textarea)
  - 주요제품 (태그 입력, 엔터로 추가/삭제)
  - 연락처 이메일, 웹사이트
  - 노출여부 토글
- 행 클릭 → 수정 모달
- 삭제 버튼 (confirm 후)
- 각 업체별 예약 현황 보기 버튼

### 3. 바이어 관리 (/buyers)
- 바이어 목록 테이블
- 상태별 탭: 전체 / 승인대기 / 승인완료 / 거절
- [바이어 추가] 버튼 → 폼 (직접 등록)
- 승인/거절 버튼 (pending 상태일 때)
- 각 바이어의 예약 내역 보기
- CSV 내보내기 버튼

### 4. 예약 관리 (/bookings)
- 예약 목록 테이블 (바이어명, 공급업체, 날짜, 시간, 상태)
- 날짜 필터, 공급업체 필터, 상태 필터
- 예약 취소 버튼
- [예약 직접 등록] (관리자가 수동으로 추가)
- CSV 내보내기

### 5. 타임테이블 (/timetable)
- 날짜 탭: 6/23 | 6/24 | 6/25
- 각 날짜별 그리드 테이블:
  - 행 = 시간 슬롯 (9개 + 점심 구분선)
  - 열 = 공급업체 (모든 업체)
  - 셀 = 예약된 바이어 정보 표시 (회사명) 또는 빈칸
- 셀 클릭 → 해당 예약 상세 보기 / 수동 등록
- 색상 구분: 예약됨(파란), 빈슬롯(흰), 마감(회색)
- 전체 타임테이블 인쇄 버튼

### 6. 설정 (/settings)
- 행사 기본 정보 수정 (이름, 날짜, 장소)
- 슬롯 시간 설정 (상담 시간 분, 휴식 시간 분)
- 관리자 비밀번호 변경

---

## 기술 요구사항
- React + TypeScript
- Supabase (auth + database + realtime)
- Tailwind CSS
- shadcn/ui 컴포넌트
- React Query (데이터 fetching)
- 어드민 로그인 (Supabase auth, 이메일/비밀번호)
- 반응형 (데스크톱 우선, 태블릿 지원)
- 한국어 UI

---

## 디자인 가이드
- Primary color: #4A5CF0 (인디고)
- 폰트: Pretendard (Google Fonts 또는 CDN)
- 카드 스타일: 흰 배경, 미세한 그림자, 라운드 코너
- 사이드바: 다크 (남색 계열)
- 테이블: 줄무늬, 호버 강조
- 배지: 카테고리별 색상 구분
  - 식품·음료: 초록
  - 뷰티·화장품: 핑크
  - 건강·기능식품: 하늘
  - 리빙·가구: 황금
  - 주류·음료: 보라

---

## 슬롯 정의 (고정값)
| 슬롯ID | 시간 | 구분 |
|--------|------|------|
| s1 | 10:30~11:00 | 오전 |
| s2 | 11:00~11:30 | 오전 |
| s3 | 11:30~12:00 | 오전 |
| s4 | 12:00~12:30 | 오전 |
| — | 12:30~13:30 | 점심 |
| s5 | 13:30~14:00 | 오후 |
| s6 | 14:00~14:30 | 오후 |
| s7 | 14:30~15:00 | 오후 |
| s8 | 15:00~15:30 | 오후 |
| s9 | 15:30~16:00 | 오후 |
```

---

## 사용 방법

1. [lovable.dev](https://lovable.dev) 접속
2. **New Project** 클릭
3. 위 프롬프트 박스 내용을 **전체 복사** → 입력창에 붙여넣기
4. 생성 완료 후 **Supabase 연결** (Lovable 우측 패널 → Supabase 탭)
5. SQL 마이그레이션 자동 실행 (Lovable이 테이블 자동 생성)
6. 공급업체 데이터 입력 시작!

---

## 추가 요청 프롬프트 (생성 후 이어서 입력)

### 공급업체 초기 데이터 입력용:
```
Add a "데이터 초기화" button in Settings that seeds the suppliers table with these 12 companies: [기존 플랫폼의 12개 업체 데이터]
```

### 바이어 플랫폼과 연동:
```
Add a public API endpoint that the buyer-facing HTML page can call to:
1. GET /suppliers — return active suppliers list
2. POST /bookings — create a booking
3. GET /bookings?buyer_email=xxx — get buyer's bookings
Use Supabase Edge Functions.
```
