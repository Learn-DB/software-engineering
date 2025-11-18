# ClickHouse

## 특징
- Yandex가 개발한 오픈소스 열 지향 DBMS. 실시간 분석/OLAP에 최적화.
- 컬럼 단위 압축, 벡터화 실행, 파이프라인 병렬 처리로 매우 빠른 집계를 제공.
- MergeTree 엔진을 기반으로 다양한 변형(ReplicatedMergeTree, AggregatingMergeTree 등)을 제공.

## 장점
- 고속 INSERT (배치 단위)와 초저지연 SELECT.
- Materialized View, TTL, Data Skipping Index 등을 통해 파티션/샤딩 없이도 성능 유지.
- S3/HDFS와 통합해 데이터 레이크 아키텍처로 확장 가능.

## 제약/주의
- 단건 트랜잭션 OLTP에는 적합하지 않으며, 업데이트/삭제는 Merge 프로세스로 지연 적용된다.
- 조인은 지원하지만 대규모 분산 조인 시 메모리 사용량이 높다.
- Replication, Distributed 테이블 구성 시 샤드/리플리카 설정을 명시적으로 관리해야 한다.

## 운영 팁
- 적절한 파티션 키(일자 등)를 사용해 Merge 작업과 쿼리 프루닝을 최적화한다.
- Background Merge, Mutation 큐를 모니터링하고 지연 시 경보를 설정.
- Zookeeper 의존성을 줄이기 위해 Keeper 또는 완전관리형 서비스 사용을 검토한다.
