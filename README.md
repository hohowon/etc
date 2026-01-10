# etc

# Next.js + Spring Boot 프로젝트 규약 및 전반 흐름

## 1. 프로젝트 개요

- **프로젝트 형태**: 4인 팀 협업 웹 서비스
- **Frontend**: Next.js (App Router)
- **Backend**: Spring Boot (REST API)
- **DB**: PostgreSQL
- **Server**: AWS

---

## 2. 전체 아키텍처 흐름

- [ Client (Browser) ]
- ↓
- [ Next.js (Frontend) ]
- ↓ REST API (JSON)
- [ Spring Boot (Backend) ]
- ↓
- [ Database (PostgreSQL) ]


- Next.js: UI 렌더링 및 API 호출 담당
- Spring Boot: 비즈니스 로직 및 DB 처리 담당
- 모든 데이터 교환은 JSON 기반 REST API 사용

---

## 3. Git & 브랜치 전략

### 3.1 브랜치 구성

- **MASTER**: 배포 가능한 안정 브랜치
- **develop**: 개발 통합 브랜치
- **ST**: ST용 브랜치
- **LT**: LT용 브랜치
- **feature/{기능명}**: 기능 단위 개발 브랜치 (UT 수행)
- **hotfix/{이슈명}**: 긴급 수정 브랜치 (UT 수행)

---

### 3.2 작업 흐름

1. develop 브랜치에서 feature 브랜치 생성
2. 기능 개발 후 Pull Request 생성
3. 개발 브랜치에서 UT 수행
4. 코드 리뷰 1명 이상 승인 (선택)
5. UT 브랜치 → LT 브랜치 머지 후 LT 수행
6. LT 브랜치 → ST 브랜치 머지 후 ST 수행
7. ST 완료 후 develop 병합

---

### 3.3 민감 정보 관리

- API 키, DB 비밀번호 등 민감 정보 하드코딩 금지
- `.env`, `application.properties` 등 환경 변수로 관리
- `.gitignore`에 민감 정보 파일 포함
- 공개 저장소에 민감 정보 업로드 금지
- 필요 시 AWS Secrets Manager, GitHub Secrets 사용

---

## 4. 커밋 메시지 규칙

### 커밋 타입

- **feat**: 신규 기능
- **fix**: 버그 수정
- **refactor**: 리팩토링
- **docs**: 문서 수정
- **chore**: 설정/빌드 관련

### 예시


- Next.js: UI 렌더링 및 API 호출 담당
- Spring Boot: 비즈니스 로직 및 DB 처리 담당
- 모든 데이터 교환은 JSON 기반 REST API 사용

---

## 3. Git & 브랜치 전략

### 3.1 브랜치 구성

- **MASTER**: 배포 가능한 안정 브랜치
- **develop**: 개발 통합 브랜치
- **ST**: ST용 브랜치
- **LT**: LT용 브랜치
- **feature/{기능명}**: 기능 단위 개발 브랜치 (UT 수행)
- **hotfix/{이슈명}**: 긴급 수정 브랜치 (UT 수행)

---

### 3.2 작업 흐름

1. develop 브랜치에서 feature 브랜치 생성
2. 기능 개발 후 Pull Request 생성
3. 개발 브랜치에서 UT 수행
4. 코드 리뷰 1명 이상 승인 (선택)
5. UT 브랜치 → LT 브랜치 머지 후 LT 수행
6. LT 브랜치 → ST 브랜치 머지 후 ST 수행
7. ST 완료 후 develop 병합

---

### 3.3 민감 정보 관리

- API 키, DB 비밀번호 등 민감 정보 하드코딩 금지
- `.env`, `application.properties` 등 환경 변수로 관리
- `.gitignore`에 민감 정보 파일 포함
- 공개 저장소에 민감 정보 업로드 금지
- 필요 시 AWS Secrets Manager, GitHub Secrets 사용

---

## 4. 커밋 메시지 규칙

### 커밋 타입

- **feat**: 신규 기능
- **fix**: 버그 수정
- **refactor**: 리팩토링
- **docs**: 문서 수정
- **chore**: 설정/빌드 관련

### 예시



