📌 프로젝트 규약 (JPA 규약)
1️⃣ Entity 외부 노출 금지 (필수)
✅ 규칙

Controller / API 응답에 Entity 사용 금지

반드시 DTO만 사용

🎯 이유

Lazy 로딩 폭발 방지

필드 추가/변경 시 API 깨짐 방지

보안 사고 예방

// ❌ 잘못된 예
public User getUser() {}

// ⭕ 올바른 예
public UserResponseDto getUser() {}

2️⃣ 연관관계는 기본 LAZY (강제)
✅ 규칙

@ManyToOne(fetch = FetchType.LAZY)

EAGER 사용 금지

필요한 경우만 fetch join 사용

🎯 이유

N+1 문제 방지

예측 불가능한 쿼리 차단

3️⃣ 트랜잭션은 Service 계층에만
✅ 규칙

@Transactional → Service 계층에만 사용

Controller / Repository 사용 금지

@Service
@Transactional
public class UserServiceImpl {}

4️⃣ Entity에 Setter 금지 (부분 허용)
✅ 규칙

무분별한 setter 사용 금지

의미 있는 메서드만 허용

// ❌
user.setEmail(email);

// ⭕
user.changeEmail(email);

5️⃣ 조회 전용 API도 DTO 변환
✅ 규칙

조회 API라도 Entity 그대로 반환 금지

반드시 DTO로 변환

6️⃣ 복잡한 조회는 JPA로 억지로 하지 않는다
✅ 기준

3개 이상 JOIN

통계 / 리포트 / 복잡한 페이징

👉 MyBatis 또는 QueryDSL 사용

7️⃣ N+1 체크 규칙
✅ 규칙

리스트 조회 시 반드시

fetch join 사용

또는 DTO Projection 사용

🔎 리뷰 포인트

forEach(getX()) 코드가 보이면 N+1 의심

8️⃣ equals / hashCode 규칙
✅ 규칙

식별자(id) 기준으로 구현

엔티티 전체 필드 사용 금지 ❌

9️⃣ 영속성 컨텍스트 신뢰, save 남발 금지
✅ 규칙

변경 감지는 트랜잭션 내부에서만

명시적 save는 생성 시에만 사용

🔟 패키지 의존 방향 고정
Controller → Service → Domain → Repository

✅ 규칙

역참조 금지

Domain이 Service 의존 ❌
