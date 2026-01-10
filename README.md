#Backend 개발 규약

## 5. 백엔드 규약 (Spring Boot)

### 5.1 패키지 구조

com.example.project
├─ controller
│  └─ user
│     └─ UserController.java
│
├─ service
│  └─ user
│     ├─ UserService.java
│     └─ UserServiceImpl.java
│
├─ repository
│  └─ user
│     ├─ UserRepository.java
│     ├─ UserRepositoryCustom.java
│     └─ UserRepositoryImpl.java
│
├─ mapper                  # MyBatis 전용
│  └─ user
│     ├─ UserMapper.java
│     └─ UserMapper.xml
│
├─ domain
│  └─ user
│     ├─ User.java
│     ├─ UserPolicy.java
│     └─ UserValidator.java
│
├─ dto
│  └─ user
│     ├─ UserRequestDto.java
│     ├─ UserResponseDto.java
│     └─ UserSummaryDto.java
│
├─ exception
│  ├─ GlobalExceptionHandler.java
│  ├─ BusinessException.java
│  └─ ErrorCode.java
│
└─ config
   ├─ JpaConfig.java
   ├─ MyBatisConfig.java
   ├─ WebConfig.java
   └─ SecurityConfig.java
