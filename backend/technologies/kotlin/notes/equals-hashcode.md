# equals / hashCode / equals의 역할

## equals()
- 두 객체가 논리적으로 동일한지 비교한다. 참조가 달라도 의미상 동일하면 true.
- 컬렉션(Set, Map)이나 테스트 비교에서 값 동등성을 판단한다.
- 데이터 클래스는 `primary constructor` 프로퍼티를 기반으로 equals를 자동 생성한다.

## hashCode()
- 객체를 해시 기반 컬렉션에 저장할 때 버킷을 결정한다.
- equals가 true인 객체는 반드시 같은 hashCode를 반환해야 한다. 그렇지 않으면 Set/Map에서 중복 삽입, 조회 실패가 발생.

## 구현 가이드
- 수동 구현 시 모든 비교 대상 필드를 고려하고, null-safe 연산을 사용한다.
- 오버라이드하면 `data class` 혹은 IDE 코드 생성기를 사용해 equals/hashCode 쌍을 동시에 생성한다.
- 상속 구조에서는 `canEqual` 패턴 등을 검토하여 대칭성과 추이성을 유지한다.

## equals의 역할 요약
- 값 객체(Value Object) 간 비교, 캐시 키, ID 매핑과 같은 로직에서 객체 동일성 판단 기준을 제공.
- 비지니스 키를 기반으로 equals를 정의하면 동일 ID를 가진 엔티티가 자연스럽게 묶인다.

## 테스트 팁
- `assertThat(a).isEqualTo(b)` 와 해시 컬렉션 삽입/조회 테스트를 같이 작성해 계약을 검증한다.
