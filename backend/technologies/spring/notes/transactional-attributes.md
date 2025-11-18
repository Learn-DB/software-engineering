# Spring @Transactional 속성

## 주요 속성
- **propagation**: 트랜잭션 전파 방식.
  - `REQUIRED` (기본): 존재하면 참여, 없으면 새로 시작.
  - `REQUIRES_NEW`: 기존 트랜잭션을 일시 중단하고 새 트랜잭션 시작.
  - `MANDATORY`, `SUPPORTS`, `NOT_SUPPORTED`, `NEVER`, `NESTED` 등 요구사항에 맞게 선택.
- **isolation**: DB 격리 수준 (`DEFAULT`, `READ_COMMITTED`, `REPEATABLE_READ`, `SERIALIZABLE` 등) 지정.
- **timeout**: 초 단위 제한. 초과 시 `TransactionTimedOutException` 으로 롤백.
- **readOnly**: 읽기 전용 힌트. JPA/Hibernate는 flush 최소화, 일부 DB는 DML 차단.
- **rollbackFor / noRollbackFor**: 어떤 예외에서 롤백/커밋할지 세밀하게 제어.
- **value / transactionManager**: 특정 트랜잭션 매니저 Bean 이름 지정.

## 사용 가이드
- 서비스 use case 단위로 `@Transactional` 을 선언해 도메인 규칙과 트랜잭션 경계를 일치시킨다.
- 멀티 데이터소스 환경에서는 각 매니저 별로 전파/격리 수준을 구체적으로 설정.
- 배치/비동기 처리에서 `REQUIRES_NEW` 를 사용해 부분 실패를 격리하거나, `NESTED` 로 savepoint를 활용한다.
- `timeout` 과 `readOnly` 를 조합하여 긴 조회/보고서 쿼리의 리소스 사용을 통제한다.
- AOP 프록시 특성상 public 메서드에 선언하고, 내부 호출 문제는 self-injection 또는 설계 리팩터링으로 해결한다.
