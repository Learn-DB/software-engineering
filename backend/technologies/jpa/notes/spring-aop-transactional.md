# Spring AOP & @Transactional

## 동작 원리
- Spring은 프록시 기반 AOP로 `@Transactional` 이 붙은 빈을 감싸 트랜잭션 시작/커밋/롤백을 제어한다.
- 프록시는 메서드 진입 시 PlatformTransactionManager를 통해 트랜잭션을 시작하고, 메서드 종료 시 예외 여부에 따라 커밋/롤백한다.

## 주의할 점
- **프록시 내부 호출**: 같은 클래스 내 다른 `@Transactional` 메서드를 직접 호출하면 프록시가 개입하지 않아 트랜잭션이 적용되지 않는다 → 자기 자신 주입(self-injection) 또는 구조 변경 필요.
- **전파 수준**: `Propagation.REQUIRES_NEW`, `MANDATORY` 등 요구사항에 맞는 전파 설정을 명시한다.
- **체크 예외**: 기본적으로 `RuntimeException` 및 `Error` 만 롤백 대상이다. 체크 예외를 롤백하려면 `rollbackFor` 속성을 지정한다.
- **비공개 메서드**: 프록시가 인터페이스/클래스 public 메서드만 감싸므로 private 메서드에는 적용되지 않는다.

## 트랜잭션 경계 설계
- 서비스 계층에서 하나의 use case 를 감싸도록 경계를 설정해 도메인 규칙이 일관되게 적용되도록 한다.
- 긴 트랜잭션에는 타임아웃, 읽기 전용, 격리 수준을 명시해 DB 자원 사용을 제어한다.
- 로깅에 트랜잭션 ID, 전파 수준, 스레드 정보를 남겨 디버깅을 쉽게 한다.
