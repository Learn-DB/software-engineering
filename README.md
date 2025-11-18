# Software Engineering Learning DB

소프트웨어 학습 사이트에 필요한 프론트엔드, 백엔드, 데브옵스 학습 DB를 분리·운영하기 위한 저장소다. 각 영역은 단계별 로드맵과 기술별 Q&A를 포함하며, 콘텐츠 생산·검수·배포를 `operations` 파이프라인으로 관리한다.

## 폴더 구조

```
.
├── frontend | backend | devops
│   ├── roadmap/0n-*         # 단계별 학습 경로 (공통 템플릿 사용)
│   └── technologies/
│       └── _template/       # 기술별 노트·Q&A 디렉터리 구조 예시
├── shared/templates         # 로드맵, Q&A 작성 템플릿
└── operations
    ├── intake               # 신규 제안, 사용자 질문 수집
    ├── review               # 콘텐츠 검수, 중복/품질 체크
    └── publication          # 승인된 자료와 Q&A 인덱스
```

### 도메인 디렉터리 (frontend/backend/devops)
- `roadmap/` 는 `01-foundations → 02-core-skills → 03-projects` 처럼 단계가 보이도록 숫자 prefix를 사용한다.
- 각 단계 폴더에는 모듈, 실습, 체크리스트 등을 Markdown 혹은 JSON으로 추가한다.
- `technologies/<tech-name>/notes` 에는 해당 기술의 요약·예제 코드, `technologies/<tech-name>/qa` 에는 질문/답변만 담긴 문서를 둔다. `_template` 폴더를 복사해 새 기술 디렉터리를 만든다.

### 템플릿
- `shared/templates/roadmap-stage.md`: 단계 문서를 만들 때 채워야 할 항목 (학습 목표, 핵심 주제, 리소스, 완료 조건 등).
- `shared/templates/qa-entry.md`: 기술별 Q&A 문서 작성 형식 (질문, 답변, 참고 자료, 상태 값 등).

## 운영 방식 제안
1. **콘텐츠 수집 (operations/intake)**  
   - 신규 학습 단계 제안, 기술별 질문, 사용자 피드백을 `intake`에 모은다.  
   - 간단한 `YYYY-MM-DD-title.md` 파일에 제안 내용을 기록한다.
2. **검수 (operations/review)**  
   - 담당자가 제안과 기존 로드맵/기술 문서를 비교해 중복, 난이도, 선행 지식 등을 조정한다.  
   - 리뷰 시 템플릿의 `Status`를 `reviewed`로 바꾸고 필요한 경우 보강 TODO를 남긴다.
3. **배포 (operations/publication)**  
   - 승인된 로드맵/기술/Q&A는 각 도메인 폴더에 반영하고, 사용자 노출용 목차나 API 스키마를 `publication`에 생성한다.  
   - 특정 기술의 질문·답변만 모은 Markdown/JSON을 이 디렉터리에 넣어 서비스가 바로 읽을 수 있게 한다.

## 작성 가이드
- **단계 추가**: `shared/templates/roadmap-stage.md`를 복사해 해당 도메인/단계 폴더에 넣고 구체적인 학습 목표, 리소스, 평가 기준을 채운다.
- **Q&A 추가**: 기술 폴더의 `qa/` 안에 `topic-slug.md`를 만든 뒤 템플릿 항목을 채운다. 질문은 단일 주제만 다루고, 답변엔 코드/링크를 포함해 재사용성을 높인다.
- **버전 관리**: 변경 로그나 메타 정보가 필요하면 각 문서의 헤더에 YAML front matter (`status`, `tags`, `updated_at`)를 추가한다.
- **자동화 여지**: 후속 단계에서 `operations/publication`을 기준으로 API 또는 정적 JSON을 생성하면 서비스로의 동기화가 단순해진다.

이 구조를 기반으로 각 파트의 학습 흐름과 기술별 Q&A를 독립적으로 확장하면서도, 공통 템플릿과 파이프라인으로 일관된 품질을 유지할 수 있다.
