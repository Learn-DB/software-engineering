# 열 기반 DB (Column-oriented Store)

## 특징
- 동일한 컬럼 데이터를 연속 저장해 압축률과 스캔 효율을 높인다.
- 분석/OLAP 워크로드, 대량 집계에 최적화.

## 장점
- 필요한 컬럼만 읽으므로 I/O 감소.
- 컬럼별로 같은 타입 데이터가 이어져 저장되어 압축 효율이 좋고 CPU 캐시 친화적.
- 벡터화 실행 엔진과 결합하면 초당 수억 행 처리가 가능.

## 단점
- 단일 행을 완전히 읽거나 자주 업데이트하는 OLTP에는 비효율적.
- INSERT/UPDATE 시 Write Amplification이 발생해 지연 시간이 길어질 수 있다.

## 활용
- 데이터 웨어하우스 (Snowflake, BigQuery), ClickHouse, Apache Parquet/Hudi 같은 데이터 레이크 포맷.
- 실시간 분석에서도 Materialized View와 결합하여 고속 집계를 제공.
- 하이브리드 엔진(MemSQL, PostgreSQL cstore_fdw)은 행 저장 + 열 저장을 병행한다.
