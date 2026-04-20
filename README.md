# AI Harness Engineering

AI가 팀의 개발 프로세스를 채팅이 아니라 저장소 아티팩트 기준으로 따르도록 만드는 하네스 템플릿입니다. 핵심은 `docs/`에 명세를 남기고, `.ai/`에 계획·검증·학습 기준을 남겨서 기획부터 구현, 검증, 회고까지 같은 흐름으로 이어지게 하는 것입니다.

---

## 빠른 시작

```bash
bash scripts/bootstrap-template.sh "My Project"
export HARNESS_SMOKE_COMMAND="npm run lint && npm test"
bash scripts/verify.sh
bash scripts/dashboard.sh
```

위 4줄은 그대로 복붙해서 끝나는 명령이 아닙니다.

- `bash scripts/bootstrap-template.sh "My Project"`
  여기의 `"My Project"`는 예시입니다. 당신 프로젝트 이름으로 바꿔야 합니다.
- `export HARNESS_SMOKE_COMMAND="npm run lint && npm test"`
  이것도 예시입니다. 당신 프로젝트의 실제 smoke command로 바꿔야 합니다.
- `bash scripts/verify.sh`
  이건 그대로 실행해도 됩니다. 저장소 구조와 adapter sync 상태를 검증합니다.
- `bash scripts/dashboard.sh`
  이건 그대로 실행해도 됩니다. 현재 상태를 브라우저 대시보드로 생성합니다.

PowerShell에서는 `export` 대신 이렇게 씁니다.

```powershell
$env:HARNESS_SMOKE_COMMAND = "npm run lint && npm test"
```

### smoke command는 무엇을 기준으로 정하나

프로젝트용 smoke command는 “전체 CI”가 아니라, 변경을 빠르게 신뢰할 수 있는 최소 검증 명령이어야 합니다.

기준:

- 너무 오래 걸리지 않을 것
- lint, test, build 중 핵심만 담을 것
- 실제로 자주 깨지는 경로를 포함할 것
- 배포 전 최소 신뢰를 확인할 수 있을 것

예시 5개:

- Vue
  `npm run lint && npm run test:unit && npm run build`
- React
  `npm run lint && npm test -- --runInBand && npm run build`
- Next.js
  `npm run lint && npm test -- --runInBand && npm run build`
- Spring Boot
  `./gradlew test bootJar`
- FastAPI
  `pytest -q tests/smoke && python -m compileall app`

즉 README의 `HARNESS_SMOKE_COMMAND`는 그대로 쓰는 값이 아니라, 당신 프로젝트의 스택과 실행 방식에 맞는 명령으로 바꿔야 합니다.

---

## 최소한 직접 채워야 하는 파일

- `.ai/PROJECT.md`
  이 저장소가 실제로 무엇을 만드는지 적는 파일입니다.
  왜 필요한가:
  기획, 계획, 구현, 검증이 모두 “이 프로젝트의 목표가 무엇인가”를 같은 기준으로 보게 하기 위해서입니다.
  무엇을 적으면 되나:
  - 프로젝트 한 줄 설명
  - 핵심 사용자
  - 해결하려는 문제
  - 이번 저장소의 비목표

- `.ai/ARCHITECTURE.md`
  현재 시스템 구조와 중요한 경계를 적는 파일입니다.
  왜 필요한가:
  구현과 검증이 구조를 추측하지 않고, 어디가 핵심 경계인지 알고 움직이게 하기 위해서입니다.
  무엇을 적으면 되나:
  - 주요 레이어나 모듈
  - 데이터 흐름
  - 외부 연동 지점
  - 인증/권한/상태 같은 핵심 경계

- `.ai/RUNBOOKS/local-setup.md`
  이 저장소를 로컬에서 어떻게 실행하고 준비하는지 적는 파일입니다.
  왜 필요한가:
  구현이나 검증을 할 때 실행 명령, 환경 변수, 초기 세팅을 매번 추측하지 않게 하기 위해서입니다.
  무엇을 적으면 되나:
  - 필수 런타임 버전
  - 패키지 설치 명령
  - 환경 변수 준비 방법
  - 로컬 서버 실행 명령
  - 테스트 실행 명령
  - 처음 세팅할 때 자주 틀리는 부분

`progress.json`은 사람이 처음부터 상세하게 직접 채우는 문서라기보다, 작업이 생기고 진행되면서 하네스나 에이전트가 갱신하는 구조에 더 가깝습니다. 즉 필수 수기 작성 문서로 보기보다는 진행 상태를 구조화해서 기록하는 기계용 파일에 가깝습니다.

### 필수 파일의 최소 예시

`PROJECT.md`는 보통 이렇게 시작하면 됩니다.

```md
# Project

## One-line
- 청각장애인을 위한 길찾기 서비스

## Target User
- 대중교통과 보행 길찾기가 필요한 청각장애인 사용자

## Problem
- 일반 지도 서비스는 청각장애인 관점의 안내와 위험 정보가 부족하다

## Non-goals
- 모든 교통수단 통합 최적화
- 전국 단위 실시간 데이터 완성
```

`ARCHITECTURE.md`는 보통 이렇게 시작하면 됩니다.

```md
# Architecture

## Layers
- Web client
- API server
- Database

## Main Flow
- 사용자가 출발지/도착지를 입력한다
- API가 경로를 계산한다
- DB 또는 외부 데이터에서 보조 정보를 가져온다
- 클라이언트가 결과를 보여준다

## External Boundaries
- 지도 API
- 공공데이터 API
```

`local-setup.md`는 보통 이렇게 시작하면 됩니다.

```md
# Local Setup

## Runtime
- Node 20

## Install
- `npm install`

## Env
- `.env.local`에 `API_URL`, `JWT_SECRET` 필요

## Run
- `npm run dev`

## Test
- `npm test`
```

---

## 권장 사용 순서

### 1. 명세가 없거나 부족하면 먼저 기획 문서를 만든다

```text
/authorSpecs
주문 기능 PRD, ERD, API 문서를 먼저 만들어줘. 기존 문서가 있으면 재사용하고 부족한 것만 생성해줘.
```

이 단계는 `docs/PRD`, `docs/ERD`, `docs/API`를 점검하고, 최신 문서를 재사용하거나 필요한 문서만 새 버전으로 추가합니다.

### 2. 명세가 있으면 구현 계획을 만든다

```text
/planWorkstreams
docs의 최신 PRD, ERD, API와 .ai/DECISIONS를 읽고 구현 계획을 세부 워크스트림으로 나눠줘.
```

이 단계 결과:

- 상위 계획: `.ai/PLANS/current-sprint.md`
- 세부 계획: `.ai/PLANS/current-sprint/*.md`

각 세부 계획은 최소한 아래를 포함해야 합니다.

- `Success Criteria`
- `Implementation Plan`
- `Validation Plan`

프레임워크가 아직 없는 저장소라면 이 단계에서 `framework-setup` 워크스트림이 자동으로 계획에 들어갑니다.

### 3. 기획과 계획을 한 번에 하고 싶으면 autoplan을 쓴다

```text
/autoPlan
결제 플로우를 모바일 웹에 추가하려 한다. 문서가 없으면 PRD, ERD, API부터 만들고, 있으면 그걸 기반으로 구현 계획까지 세워줘.
```

`/autoPlan`은 문서가 부족하면 먼저 보강하고, 충분하면 바로 계획으로 이어집니다.

### 4. 구현과 검증을 한 번에 하고 싶으면 기본 진입점은 deliverChange다

```text
/deliverChange
로그인 에러 토스트 중복 노출 버그를 수정해줘.
```

이 명령은 구현만 하고 멈추지 않습니다. 구현 후 `validate-change` 단계까지 이어지는 기본 경로입니다.

### 5. 검증만 다시 하고 싶으면 validateChange를 쓴다

```text
/validateChange
현재 diff를 기준으로 코드리뷰와 사용자 플로우 검증을 해줘.
```

이 명령은 코드리뷰와 QA를 하나의 게이트로 묶습니다.

### 6. 현재 상태를 보고 싶으면 dashboard를 쓴다

```text
/dashboard
```

또는:

```bash
bash scripts/dashboard.sh
```

---

## 상황별 추천 명령

| 상황 | 추천 명령 | 의미 |
|---|---|---|
| 명세가 비어 있다 | `/authorSpecs` | PRD/ERD/API 문서 생성 또는 보강 |
| 명세는 있고 계획만 필요하다 | `/planWorkstreams` | 상위 계획 + 세부 작업 계획 작성 |
| 문서부터 계획까지 한 번에 하고 싶다 | `/autoPlan` | 기획과 계획 오케스트레이션 |
| 구현과 검증을 한 번에 끝내고 싶다 | `/deliverChange` | 구현 + 검증 기본 경로 |
| 코드리뷰와 QA를 다시 돌리고 싶다 | `/validateChange` | 통합 검증 게이트 |
| 원인 분석이 먼저 필요하다 | `/investigate` | 증거 기반 조사 |
| 반복 실패를 학습으로 승격하고 싶다 | `/learn` | MEMORY/EVAL/SKILL/ADR 승격 |
| 현재 상태를 보고 싶다 | `/dashboard` | 브라우저 대시보드 생성 |

---

## README에서 알아두면 되는 핵심 규칙

- 명세는 `docs/PRD`, `docs/ERD`, `docs/API`에 둡니다.
- 명세 파일은 덮어쓰지 말고 `_v1`, `_v2`처럼 버전을 올립니다.
- 구현 계획은 한 파일에 몰아 쓰지 않고 상위 계획 + 세부 계획으로 나눕니다.
- 세부 계획은 구현과 검증이 바로 사용할 수 있어야 합니다.
- 구현이 끝났다고 done이 아니고, 검증 게이트를 지나야 done입니다.
- 반복 실패는 같은 시도를 계속하지 말고 `learn`으로 승격합니다.

---

## 가드와 자동화

자주 쓰는 기본 명령:

```bash
scripts/check-dangerous-command.sh "<command>"
scripts/record-retry.sh <signature>
scripts/check-circuit-breaker.sh <signature>
scripts/verify.sh
scripts/dashboard.sh
```

역할:

- `check-dangerous-command.sh`
  위험한 명령 실행 전 차단
- `record-retry.sh`
  실패 기록 + circuit breaker 확인
- `check-circuit-breaker.sh`
  같은 실패를 더 시도해도 되는지 확인
- `verify.sh`
  저장소 구조 검증
- `dashboard.sh`
  상태 대시보드 생성

---

## 주요 파일 구조

```text
docs/
├── PRD/                 # 제품 명세
├── ERD/                 # 데이터/도메인 명세
└── API/                 # API/계약 명세

.ai/
├── PROJECT.md
├── ARCHITECTURE.md
├── WORKFLOW.md
├── DECISIONS/
├── PLANS/
│   ├── current-sprint.md
│   ├── current-sprint/
│   └── progress.json
├── EVALS/
├── MEMORY/
├── RUNBOOKS/
└── SKILLS/

.claude/skills/          # Claude adapter
.agents/skills/          # Codex adapter
scripts/                 # helper scripts
```

canonical 변경 순서:

1. `.ai/SKILLS/` 또는 `.ai/` canonical 파일 수정
2. `bash scripts/sync-adapters.sh`
3. `bash scripts/verify.sh`

---

## 운영 규칙 한 줄 요약

- 문서가 없으면 먼저 만든다.
- 계획은 상위 계획과 세부 계획으로 나눈다.
- 구현은 계획 기준으로 한다.
- 완료 판단은 검증 이후에 한다.
- 반복 실패는 학습으로 승격한다.

더 자세한 기준:

- [AGENTS.md](C:/Users/SSAFY/harness_engineering/AGENTS.md)
- [CLAUDE.md](C:/Users/SSAFY/harness_engineering/CLAUDE.md)
- [.ai/WORKFLOW.md](C:/Users/SSAFY/harness_engineering/.ai/WORKFLOW.md)
- [.ai/ARCHITECTURE.md](C:/Users/SSAFY/harness_engineering/.ai/ARCHITECTURE.md)
- [.ai/AUTOMATION.md](C:/Users/SSAFY/harness_engineering/.ai/AUTOMATION.md)

---

## 선택적으로 추가하면 좋은 문서

- `.ai/RUNBOOKS/release.md`
  실제 배포 또는 릴리스 절차를 적는 파일입니다.
  있으면 좋은 이유:
  `ship` 단계에서 배포 전/후 확인 기준이 더 분명해집니다.

- `.ai/RUNBOOKS/rollback.md`
  배포가 잘못됐을 때 어떻게 되돌리는지 적는 파일입니다.
  있으면 좋은 이유:
  변경이 커질수록 rollback 기준이 중요해집니다.

선택 문서 예시:

`release.md`

```md
# Release

## Pre-check
- `npm run lint`
- `npm test -- --runInBand`
- `npm run build`

## Deploy
- main 브랜치 배포

## Post-check
- 로그인
- 메인 페이지 진입
- 핵심 API 응답 확인
```

`rollback.md`

```md
# Rollback

## Trigger
- 로그인 실패율 급증
- 5xx 비율 급증

## Action
- 이전 배포 버전으로 rollback

## Verify
- 메인 페이지 진입 확인
- 로그인 재확인
```
