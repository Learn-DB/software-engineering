# 커버링 인덱스 (Covering Index)

## 정의
- 쿼리에서 필요한 모든 컬럼이 인덱스에 포함되어 테이블 데이터 페이지를 읽지 않고도 결과를 반환할 수 있는 인덱스.
- `INCLUDE` 컬럼(PostgreSQL), `INVISIBLE` 컬럼(MySQL 8) 등 DB 별 표현이 다르다.

## 장점
- 랜덤 I/O를 줄이고, 버퍼 캐시 사용량 감소.
- RDB 옵티마이저가 Index Only Scan을 선택해 CPU/디스크 사용을 크게 줄인다.

## 설계 팁
- 조건절 컬럼 + SELECT 절 컬럼을 모두 인덱스에 포함하되, 쓰기 비용 증가에 주의한다.
- 변경 빈도가 낮고 조회 비중이 높은 보고서/검색 쿼리에서 효과적.
- 너무 많은 컬럼을 포함하면 인덱스 크기가 커져 메모리 효율이 떨어진다.

## 모니터링
- PostgreSQL: `idx_scan`, `idx_tup_fetch` 지표, `pg_stat_statements` 로 Index Only Scan 사용률 확인.
- MySQL: `EXPLAIN` 결과에 `Using index` 가 표시되면 커버링 인덱스를 사용 중임을 의미.
