# Isolation Levels

## 관련 이상 현상
- **Dirty Read**: 아직 커밋되지 않은 데이터를 읽음.
- **Non-repeatable Read**: 같은 트랜잭션 내 같은 조건 조회가 서로 다른 결과를 반환.
- **Phantom Read**: 조건에 맞는 레코드 집합이 추가/삭제되어 결과 건수가 달라짐.

## ANSI/ISO 표준 격리 수준
| Level | 방지되는 현상 | 설명 |
| --- | --- | --- |
| Read Uncommitted | Dirty Read X | 잠금 없이 읽기, 쓰기 경쟁이 심하지만 최대 성능 |
| Read Committed (기본: Oracle, SQL Server) | Dirty Read O | 커밋된 데이터만 읽지만 Non-repeatable/Phantom 가능 |
| Repeatable Read (기본: MySQL InnoDB) | Dirty+Non-repeatable O | 동일 조건 읽기 일관성 보장, Phantom 가능 (MVCC 스냅샷 사용 시 완화) |
| Serializable | Dirty+Non-repeatable+Phantom O | 모든 트랜잭션을 직렬로 실행한 것과 동일, 성능 비용 큼 |

## Snapshot / MVCC 변형
- PostgreSQL `Repeatable Read` 는 MVCC 스냅샷으로 Dirty/Non-repeatable/Phantom 모두 방지한다.
- SQL Server `READ_COMMITTED_SNAPSHOT` 옵션은 버전 스토어를 활용해 읽기 잠금을 줄인다.

## 선택 기준
- 온라인 트랜잭션 처리(OLTP): 대부분 `Read Committed` 혹은 MVCC 기반 `Repeatable Read` + 검사 로직.
- 재무/결제: 명시적으로 `Serializable` 또는 추가 비즈니스 검증을 사용.
- 리포팅/분석: 스냅샷 격리로 장기 실행 쿼리의 읽기 잠금을 제거.

## 운영 팁
- 각 서비스별로 필요한 일관성 수준을 정의하고 ORM/쿼리 레벨에서 설정을 강제한다.
- 격리 수준 변경 시 모니터링 지표(대기 시간, Deadlock, 버전 스토어 사용량)를 함께 추적한다.
- 트랜잭션 내부에서 외부 시스템 호출을 자제해 격리 유지 시간을 줄인다.
