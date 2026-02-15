📌 API 명세 – 사용자 조회
1. 기본 정보

Method: GET

설명: 특정 ID를 가진 사용자 정보를 조회한다

인증: 필요 (JWT)

2. 요청 파라미터
이름	위치	타입	필수	설명
id	Path	Long	O	조회할 사용자 ID
3. 요청 예시
GET /api/v1/users/123
Authorization: Bearer eyJhbGci...

4. 응답 필드
이름	타입	설명
success	Boolean	요청 성공 여부
data	Object	사용자 정보
data.id	Long	사용자 ID
data.name	String	이름
data.email	String	이메일
message	String	에러 발생 시 메시지
5. 응답 예시
✅ 성공
{
  "success": true,
  "data": {
    "id": 123,
    "name": "홍길동",
    "email": "hong@example.com"
  },
  "message": null
}

❌ 실패
{
  "success": false,
  "data": null,
  "message": "NOT_FOUND"
}

6. 에러 코드
코드	HTTP 상태	메시지	설명
NOT_FOUND	404	리소스 없음	요청한 사용자 없음
INVALID_REQUEST	400	잘못된 요청	파라미터 형식 오류
INTERNAL_SERVER_ERROR	500	서버 오류	서버 처리 중 예외 발생
📌 JPA 규약 (참고용)
1. Entity 외부 노출 금지 (필수)
규칙

Controller / API 응답에 Entity 사용 금지

DTO만 사용

이유

Lazy 로딩 폭발

필드 추가/변경 시 API 깨짐

보안 사고 위험

// ❌
public User getUser() {}

// ⭕
public UserResponseDto getUser() {}

2. 연관관계는 기본 LAZY (강제)
규칙

@ManyToOne(fetch = FetchType.LAZY)

EAGER 금지

필요한 경우만 fetch join 사용

이유

N+1 방지

예측 불가능한 쿼리 차단

3. 트랜잭션은 Service 계층에만
규칙

@Transactional → Service 계층만

Controller / Repository 사용 금지

@Service
@Transactional
public class UserServiceImpl {}

4. Entity에 Setter 금지 (부분 허용)
규칙

무분별한 setter 금지

의미 있는 메서드만 허용

// ❌
user.setEmail(email);

// ⭕
user.changeEmail(email);

5. 조회 전용 API도 DTO 변환

조회라도 Entity 그대로 반환 금지

반드시 DTO 변환

6. 복잡한 조회는 JPA로 억지로 하지 않는다
기준

3개 이상 JOIN

통계 / 리포트 / 복잡한 페이징

👉 MyBatis 또는 QueryDSL 사용

7. N+1 체크 규칙

리스트 조회 시 반드시

fetch join

또는 DTO projection

리뷰 포인트

forEach(getX()) 있으면 의심

8. equals / hashCode 규칙

식별자(id) 기준

엔티티 필드 전체 사용 금지 ❌

9. 영속성 컨텍스트 믿고 save 남발 금지
규칙

변경 감지는 트랜잭션 안에서만

명시적 save는 생성 시만 사용

10. 패키지 의존 방향 고정
Controller → Service → Domain → Repository


역참조 금지

Domain이 Service 의존 ❌

📌 TODO
1. 구조 및 설계

벨리데이션 체크 정의를 한 곳에 마스터북처럼 관리

설계서 / 데이터 보존 장소를 디렉터리 단위로 구체화

2. 정리해야 할 규약 목록
항목	세부 내용
코딩 스타일 / 룰	들여쓰기, 네이밍 규칙, 주석 규칙
테스트 규약	UT, LT, ST 기준 / 커버리지 목표
에러/예외 처리	공통 에러 코드, HTTP 상태 매핑
로그 & 모니터링	로그 포맷, 레벨 기준, 알람 정책
보안 규약	JWT/OAuth 정책, 암호화 방식, CORS/CSRF
공통 라이브러리	사용 라이브러리 및 버전 관리
문서 관리 규약	문서 위치, 갱신 주기, 버전 관리
환경별 설정 관리	개발/테스트/배포 환경 변수 규칙
버전 관리	상세 버전 전략 정의
