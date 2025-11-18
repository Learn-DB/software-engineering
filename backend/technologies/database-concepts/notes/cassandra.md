# Apache Cassandra

## 특징
- 분산형 Wide-Column Store. 멀티 리전/데이터센터에 걸친 무중단 확장이 가능.
- 마스터리스 아키텍처로 어느 노드나 읽기/쓰기를 처리하며, 가십/멤버십 프로토콜로 상태를 공유.
- 튜너블 일관성(Read/Write Consistency Level)을 제공해 CAP 트레이드오프를 조정.

## 데이터 모델
- Keyspace → Table 구조. 테이블은 파티션 키 + 클러스터링 컬럼으로 정렬된 로우를 저장.
- 행은 Sparse 구조라 컬럼이 없어도 저장 공간을 차지하지 않는다.
- 쿼리는 파티션 키 기반으로 설계되어야 하며, 조인/서브쿼리 없이 필요한 데이터를 반환하도록 모델링한다 (query-first 설계).

## 장점
- 쓰기 지연이 낮고, Commit Log + SSTable 구조로 고속 쓰기 처리.
- 선형 확장성: 노드를 추가하면 읽기/쓰기 처리량이 거의 비례 증가.
- 멀티 리전 replication 을 통해 지연을 최소화하며 장애 시에도 가용성을 유지.

## 운영 고려 사항
- TTL과 컴팩션 전략(SizeTiered, Leveled)을 워크로드에 맞춰 설정.
- Tombstone 과행을 모니터링해 읽기 성능 저하를 방지.
- Consistency Level, Replication Factor 조합을 바르게 설정하여 요구 사항을 충족.
