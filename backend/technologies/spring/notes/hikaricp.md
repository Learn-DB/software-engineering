# HikariCP

## 소개
- Spring Boot 기본 커넥션 풀 구현. 경량, 낮은 지연, 빠른 커넥션 확보/반납 속도를 목표로 한다.

## 핵심 설정
- **maximumPoolSize**: 동시에 유지할 최대 커넥션 수. DB 제한과 애플리케이션 스레드 수를 고려해 설정.
- **minimumIdle**: 대기 상태로 유지할 커넥션 수. 스파이크 대비 여유를 둔다.
- **connectionTimeout**: 풀에서 커넥션을 얻기까지 기다리는 최대 시간 (ms). 초과 시 예외 발생.
- **idleTimeout**: 사용되지 않는 커넥션을 정리하기 전 대기 시간.
- **maxLifetime**: 커넥션의 전체 수명. DB/로드밸런서의 keepalive 정책보다 짧게 설정해 깨끗한 재생성을 보장.
- **validationTimeout**, **connectionTestQuery**: 헬스체크에 사용.

## 모니터링
- Micrometer/Actuator 를 통해 풀 상태 지표(active, idle, pending) 수집.
- 풀 개수, 대기 시간, timeout 발생 횟수에 알람 설정.

## 모범 사례
- `maxLifetime` < DB 세션 타임아웃으로 설정하여 서버측 강제 종료를 방지.
- 풀 크기는 "애플리케이션 동시성"과 "DB가 처리 가능한 세션 수" 사이 균형점에 맞춰 측정 기반으로 조정.
- 블로킹 I/O 작업이나 긴 쿼리는 다른 풀 또는 비동기 구조로 분리해 커넥션 고갈을 방지.
- Failover/Read replica 사용 시 풀 설정을 커넥션 문자열별로 나눠 관리한다.
