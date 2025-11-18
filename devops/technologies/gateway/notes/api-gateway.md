# API 게이트웨이

## 역할
- 외부 클라이언트와 마이크로서비스 사이에서 단일 진입점을 제공한다.
- 인증/인가, 라우팅, 속도 제한, 요청 변환, 관측성(로그/메트릭)을 담당한다.

## 아키텍처 패턴
- **Edge Gateway**: CDN/프록시 근처에서 동작하며 REST/GraphQL/gRPC 를 단일 엔드포인트로 노출.
- **Service Mesh Ingress**: Istio, Linkerd 등과 연계되어 L7 정책과 mTLS를 적용.
- **Function Gateway**: 서버리스(FaaS) 트리거를 HTTP 요청과 연결.

## 필수 기능
- **인증**: OAuth2, JWT 검증, API Key 관리.
- **보안**: WAF 룰, Bot 차단, TLS termination, Zero Trust 정책.
- **트래픽 제어**: Rate limiting, Circuit Breaker, Canary/Blue-Green 배포 지원.
- **관측성**: 분산 트레이싱 헤더 전파, 요청/응답 로깅, SLA 모니터링.

## 운영 모범 사례
- 정책을 코드로 관리(GitOps)하여 환경 간 차이를 최소화.
- Service Discovery(k8s, Consul)와 연동해 백엔드 서비스 변화를 자동 반영.
- SLA 기반 라우팅(프리미엄 고객, 내부 API 등)을 정의하고 테스트 자동화.
- 게이트웨이 장애 시 우회 경로/페일오버 계획을 문서화해 복원력을 확보.
