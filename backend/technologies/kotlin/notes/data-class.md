# Kotlin Data Class

## 목적
- 값을 표현하는 객체에 대해 `equals()`, `hashCode()`, `toString()`, `copy()`, `componentN()` 을 자동 생성해 보일러플레이트를 줄인다.

## 특징
- 기본 생성자에 정의된 프로퍼티가 `componentN` 과 `copy` 의 대상이 된다.
- `val`/`var` 의 조합으로 불변/가변 모델을 구성할 수 있으나, 대부분의 경우 불변(`val`)을 권장.
- `copy()` 는 일부 필드만 바꿔 새 인스턴스를 만들 때 유용.

## 사용 예시
```kotlin
data class Money(val amount: BigDecimal, val currency: String)

val usd = Money(BigDecimal("10.00"), "USD")
val usdInEur = usd.copy(currency = "EUR")
```

## 주의 사항
- 상속이 불가하며, 추상 클래스/인터페이스 구현만 가능.
- JPA 엔티티와 같이 프레임워크가 기본 생성자와 프록시를 요구하면 data class 사용을 피한다.
- 기본 생성자 외 프로퍼티는 `copy`/`equals` 대상이 아니므로 의도적으로 사용해야 한다.
