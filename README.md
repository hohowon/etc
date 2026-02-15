Next.js + Spring Boot 프로젝트 규약 및 전반 흐름
1. 프로젝트 개요

프로젝트 형태: 4인 팀 협업 웹 서비스

프론트엔드: Next.js (App Router)

백엔드: Spring Boot (REST API)

DB: PostgreSQL

서버: AWS

2. 전체 아키텍처 흐름
[ Client (Browser) ]
        ↓
[ Next.js (Frontend) ]
        ↓  REST API (JSON)
[ Spring Boot (Backend) ]
        ↓
[ Database (PostgreSQL) ]


Next.js는 UI 렌더링 + API 호출 담당

Spring Boot는 비즈니스 로직 + DB 처리 담당

모든 데이터 교환은 JSON 기반 REST API로 통신

3. Git & 브랜치 전략
3.1 브랜치 구성

MASTER : 배포 가능한 안정 브랜치

develop : 개발 통합 브랜치

ST : ST용 브런치

LT : LT용 브런치

feature/{기능명} : 기능 단위 개발 브랜치 (이 브런치에서 UT실시할것)

hotfix/{이슈명} : 긴급 수정 (이 브런치에서 UT실시할것)

3.2 작업 흐름

develop에서 개발브랜치 생성

기능 개발 후 Pull Request 생성

개발 브런치에서 UT실시

코드 리뷰 1명 이상 승인 필수 //선택

UT브런치 작업물을 LT브런치 머지후 LT실시

LT브런치 작업물을 ST브런치에 머지후 ST실시

3.3 민감 정보 관리

코드에 API 키, DB 비밀번호 등 민감 정보 절대 하드코딩 금지

.env 파일 또는 application.properties 등 환경 변수로 관리

.gitignore에 민감 정보 파일 포함하여 Git에 커밋되지 않도록 설정

공개 저장소(GitHub 등)에 민감 정보 절대 업로드 금지

필요 시 AWS Secrets Manager, GitHub Secret 등 보안 도구 활용

4. 커밋 메시지 규칙

ex)

feat: 로그인 API 구현
fix: 회원가입 validation 오류 수정
refactor: 사용자 서비스 구조 개선


feat : 신규 기능

fix : 버그 수정

refactor : 리팩토링

docs : 문서 수정

chore : 설정/빌드 관련

6. 백엔드 규약 (Spring Boot)
6.1 패키지 구조
com.example.project
 ├─ controller
 │   └─ user
 │       └─ UserController.java
 │
 ├─ service
 │   └─ user
 │       ├─ UserService.java
 │       └─ UserServiceImpl.java
 │
 ├─ repository
 │   └─ user
 │       ├─ UserRepository.java          // JPA Repository
 │       ├─ UserRepositoryCustom.java    // JPA 커스텀 (QueryDSL 등)
 │       └─ UserRepositoryImpl.java
 │
 ├─ mapper                // ⭐ MyBatis 전용
 │   └─ user
 │       ├─ UserMapper.java              // Mapper 인터페이스
 │       └─ UserMapper.xml               // SQL
 │
 ├─ domain
 │   └─ user
 │       ├─ User.java
 │       ├─ UserPolicy.java
 │       └─ UserValidator.java
 │
 ├─ dto
 │   └─ user
 │       ├─ UserRequestDto.java
 │       ├─ UserResponseDto.java
 │       └─ UserSummaryDto.java
 │
 ├─ exception
 │   ├─ GlobalExceptionHandler.java
 │   ├─ BusinessException.java
 │   └─ ErrorCode.java
 │
 └─ config
     ├─ JpaConfig.java
     ├─ MyBatisConfig.java
     ├─ WebConfig.java
     └─ SecurityConfig.java

6.2 아키텍처 & 코딩 규칙 (패키지 구조 기반)
1. Controller (controller)

요청(Request)과 응답(Response)만 처리.

비즈니스 로직은 Service로 위임.

URI 설계 및 HTTP 상태코드 관리.

2. Service (service)

핵심 비즈니스 로직 집중.

트랜잭션 관리(@Transactional)는 Service 계층에서 처리.

Repository 또는 Mapper를 통해 데이터 접근.

인터페이스(UserService)와 구현체(UserServiceImpl)를 분리하여 테스트 용이성 확보.

3. Repository (repository)

JPA 기반 데이터 접근 계층.

기본 CRUD는 JpaRepository 또는 CrudRepository 사용.

복잡한 쿼리나 동적 쿼리는 커스텀 Repository(UserRepositoryCustom + UserRepositoryImpl) 사용.

역할: Entity 단위 데이터 조작, Service 계층에 데이터 제공.

4. Mapper (mapper) ⭐ MyBatis 전용

MyBatis SQL Mapper 전용 패키지.

Mapper 인터페이스(UserMapper)와 XML(UserMapper.xml) 분리.

SQL 중심의 복잡한 쿼리 처리.

JPA와 혼용 시 역할 명확히 구분.

5. Domain (domain)

Entity 및 도메인 로직 관리.

Validator, Policy, 도메인 규칙 등 포함.

Entity와 DTO를 명확히 분리.

User.java: DB Entity

UserPolicy.java: 비즈니스 정책

UserValidator.java: 도메인 유효성 검증

6. DTO (dto)

요청(Request) 및 응답(Response) 데이터 전송 객체.

Controller ↔ Service 계층 사이에서 데이터 전달.

Entity와 분리하여 API 응답 구조 유연하게 관리.

UserRequestDto.java: 입력 데이터

UserResponseDto.java: 단일 응답

UserSummaryDto.java: 요약 응답

7. Exception (exception)

전역 예외 처리 및 커스텀 예외 정의.

GlobalExceptionHandler.java: @ControllerAdvice 사용

BusinessException.java: 비즈니스 예외

ErrorCode.java: 공통 에러 코드 관리

8. Config (config)

프로젝트 전반 설정 관리.

JpaConfig.java: JPA 설정

MyBatisConfig.java: MyBatis 설정

WebConfig.java: MVC, CORS, Interceptor 등

SecurityConfig.java: Spring Security 설정

7. API 규약
7.1 URL 규칙

/api/v1/... 형태 사용

명사는 복수형 사용

GET    /api/v1/users
POST   /api/v1/users
GET    /api/v1/users/{id}

7.2 API 응답 포맷
{
  "success": true,
  "data": {...},
  "message": null
}


에러 응답:

{
  "success": false,
  "data": null,
  "message": "INVALID_REQUEST"
}

8. 개발 진행 흐름
단계	내용	산출물
1	요구사항 정리	요구사항 명세서, 기능 설명서, 화면 설계 초안, 화면전환도
2	기본설계	API 명세서 ※API명세서 시트 참조, 테이블정의서, 공통예외/에러 처리 규칙 문서
3	상세설계	상세설계서 ※상세설계서 시트 참조
4	백엔드 API 개발	코드, DB 스키마/초기 데이터 스크립트
5	UT(pgm,기능단위 ex서비스,도메인)	UT사양서, 테스트코드, 테스트결과
6	LT(화면에서api호출)	LT사양서, 테스트코드, 테스트결과
7	ST(업무흐름전반)	ST사양서, 테스트코드, 테스트결과
9. 협업 규칙

일주일 1회 이상 진행 상황 공유

PR은 작게, 기능 단위로 생성

공통 규약 변경 시 문서 업데이트 필수

애매한 구현은 혼자 결정하지 않고 공유

10. 기타

공통 상수 / Enum은 중앙 관리

로그 레벨 구분 (INFO / WARN / ERROR)

TODO는 이슈로 등록 후 처리

※ 본 문서는 프로젝트 진행 중 지속적으로 업데이트한다.
