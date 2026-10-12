# 협업 규칙 — WikiPulse (S15P21A609)

- 작성일: 2026-08-28
- 기반: 이전 프로젝트(지게차 · S15P11A304) 협업 규칙을 본 프로젝트에 맞게 이식
- 브랜치 모델: git-flow

## 프로젝트 좌표

| 항목 | 값 |
| --- | --- |
| Jira 프로젝트 키 | `S15P21A609` |
| Jira 보드 | https://ssafy.atlassian.net/jira/software/c/projects/S15P21A609/boards/15275 (ID `15275`, 스크럼) |
| GitLab | https://lab.ssafy.com/s15-bigdata-dist-sub1/S15P21A609 (프로젝트 ID `1443035`) |
| 스프린트 ID | 특화 2주차 `53960` / 3주차 `53961` / 4주차 `55646` / 5주차 `57344` |

**스프린트 상태·기간·현재 배정은 이 문서에서 관리하지 않는다.** 2026-09-17 실측에서
특화 5주차가 active였지만, 상태는 바뀌므로 Jira 보드가 정본이다. 이 표는 API·이슈 조회에
필요한 고정 ID만 남긴다.

⚠️ **`state=active`가 빈 배열이라고 "스프린트가 없다"로 결론짓지 말 것.** 시작 전이거나
스프린트 사이 구간이면 `state=future`를 봐야 한다. 문서의 과거 상태 문구로 현재 배정을
판정하지 않는다.

---

## 1. Jira 운영 원칙

**계층은 Epic → Story/Task 2단계만 사용한다.** (Sub-task 안 씀)

| 단위 | 의미 | 예시 |
| --- | --- | --- |
| **Epic** | 큰 기능 묶음 / 도메인 | `실시간 수집 파이프라인`, `기업-이슈 엔티티 매핑` |
| **Story** | 실제 작업 단위 (= 브랜치 1개) | `Kafka 컨슈머 구현`, `이슈 피드 API` |
| **Task** | 기능이 아닌 기술·지원 작업 | `Hadoop 클러스터 구축`, `yfinance 적재 스크립트` |

- **모든 Story/Task는 반드시 Epic에 소속**시킨다. (백로그에서 Epic 하위로 정리)

### 신규 이슈의 활성 스프린트 배정 (2026-09-17 사용자 지시)

- 이 프로젝트에서 Jira 이슈를 생성할 때마다 보드 `15275`의 **현재 활성 스프린트**를 조회하고, 생성 직후 해당 스프린트로 배정한다. 별도 스프린트 배치 확인을 반복하지 않는다.
- 스프린트 ID를 고정하거나 과거 문서의 값을 재사용하지 않는다. `sprint in openSprints()` 결과의 Sprint 필드에서 `state=active`와 `boardId=15275`를 확인하거나 보드의 활성 스프린트를 직접 조회한다.
- 배정 후 이슈의 Sprint 필드(`customfield_10020`)와 `key = {이슈키} AND sprint in openSprints()` 결과를 다시 조회해 실제 반영을 확인한다.
- 활성 스프린트가 없거나 복수여서 대상이 모호하면 임의로 future/closed 스프린트를 선택하거나 시작하지 않는다. 생성된 이슈는 보존하고 배정 미완료 사유와 대상 확인 필요를 알린다. Epic처럼 Sprint 필드가 적용되지 않는 유형도 예외 사유를 명시한다.
- 사용자가 특정 작업에 다른 스프린트나 백로그를 명시하면 그 지시를 우선한다.

현재 등록된 Epic (2026-08-27 생성, 12건):

| 키 | 제목 |
| --- | --- |
| S15P21A609-1 | 요구사항 정의 및 아키텍처 확정 |
| S15P21A609-2 | 데이터 소스 검증 |
| S15P21A609-3 | 인프라 구축 |
| S15P21A609-4 | 배치 수집 파이프라인 |
| S15P21A609-5 | 실시간 수집 파이프라인 |
| S15P21A609-6 | MapReduce 알고리즘 구현 |
| S15P21A609-7 | Spark 마이그레이션/최적화 |
| S15P21A609-8 | 실시간 이슈 탐지 로직 |
| S15P21A609-9 | 기업-이슈 엔티티 매핑 |
| S15P21A609-10 | 주가 데이터 연동 및 상관분석 |
| S15P21A609-11 | 백엔드 API 서버 |
| S15P21A609-12 | 프론트엔드 대시보드 |

Epic 외: `S15P21A609-13` 서비스 타겟층 확정 (스토리, 이경호) · `S15P21A609-15` 요구사항 명세·협업 규칙 저장소 문서화 (작업, 방지섭).

---

## 2. Story 작성 규칙

**제목**: `[동사] + [대상]` 형태로 간결하게

```
O  Kafka 컨슈머 구현
O  이슈 피드 목록 API
X  Kafka            (너무 모호)
```

**설명(Description) 템플릿**

```
## 목적
사용자로서 ~할 수 있다. (한 줄 사용자 스토리)

## 인수 조건 (Acceptance Criteria)
- [ ] 조건 1
- [ ] 조건 2

## 참고
- 관련 화면/API 명세 링크 등
```

---

## 3. Story 상태 흐름

```
해야 할 일(To Do)  ->  진행 중(In Progress)  ->  완료(Done)
```

전환 ID는 `11`(해야 할 일) / `21`(진행 중) / `31`(완료)이다. (2026-08-28 확인)

- 작업 **시작할 때** 본인이 담당자 지정 + `진행 중`으로 이동
- 코드가 **develop(기본 브랜치)에 머지되면** GitLab 연동으로 **자동 `완료`** 처리 (8번 참고)
  - master까지 갈 필요 없다. feature MR을 develop에 머지하는 순간 전환된다.

---

## 4. GitLab 브랜치 & 커밋 규칙 (git-flow)

**브랜치 모델**

```
master   <- 발표/배포용. 스프린트 종료 시에만 갱신 (release 머지)
develop  <- 통합 브랜치 = GitLab 기본 브랜치. 모든 feature가 여기로 머지
feature/{이슈키}-{설명}   <- 작업 단위. develop에서 분기 -> develop으로 MR
release/sprint-{N}        <- 스프린트 종료 시 develop에서 분기 -> master로 MR + 태그
hotfix/{이슈키}-{설명}    <- master 긴급 수정. master에서 분기 -> master·develop 양쪽 머지
```

- **feature는 반드시 develop에서 분기**하고, MR 대상도 develop이다. (master 대상 MR은 release/hotfix만)
- 스프린트 종료 절차: `release/sprint-{N}` 생성 -> master로 MR·머지 -> 태그 `sprint-{N}` -> develop에 역머지(필요 시)
- docs 브랜치도 develop 기준으로 작업한다.

**브랜치 네이밍** — 반드시 Story 키를 포함

```
{타입}/{이슈키}-{짧은설명}

예) feature/S15P21A609-42-kafka-consumer
    fix/S15P21A609-22-eventstream-reconnect
```

타입: `feature` / `fix` / `refactor` / `docs` (+ git-flow의 `release` / `hotfix`)

- Jira의 **"브랜치 만들기"** 버튼 사용 시 이름이 `S15P21A609-N-...`으로 제안된다. 생성 폼에서 앞에 **`feature/`를 직접 붙일 것.** (분기 소스·MR 대상은 develop으로 자동 설정되므로 이름만 고치면 된다)

**커밋 메시지** — 앞에 이슈 키를 붙인다

```
S15P21A609-42 EventStreams SSE 컨슈머 골격 작성
S15P21A609-42 Kafka 프로듀서 연결 및 재시도 처리
```

이렇게만 써도 해당 Jira Story에 **커밋 링크가 자동 연결**된다.

---

## 5. 이슈 자동 완료 (Closes 키워드)

Story를 끝내는 MR에는 **닫기 키워드 + 이슈 키**를 사용한다.

```
Closes S15P21A609-42
```

- 키워드는 `Closes` / `Fixes` / `Resolves` 모두 가능 (대소문자 무관)
- 브랜치 이름에 이슈 키가 있으면 MR 설명에 `Closes ...`가 **자동으로 채워진다**

이슈 키가 포함된 **커밋/MR이 GitLab 기본 브랜치(develop)에 머지되는 순간** Jira Story가 자동으로 `완료`로 전환된다.

✅ **2026-08-28 실동작 검증 완료.** MR !1(`feature/S15P21A609-16-readme-and-jira-check` → develop)을 머지한 지 3초 만에 S15P21A609-16이 `진행 중` → `완료`로 전환됐다. 머지 16:15:28, 이슈 갱신 16:15:31.

**주의**

- 닫기 키워드는 **GitLab 기본 브랜치 머지**에서만 동작한다. 기본 브랜치가 master인 채로 두면 이슈가 스프린트 말 release 머지 때 한꺼번에 완료되어 **번다운이 절벽**이 된다.
  - 설정 위치: GitLab → `Settings → Repository → Default branch` = `develop`
- **1 Story = 1 브랜치 = 1 MR** 원칙을 지킬 것. 한 Story를 여러 MR로 쪼개 머지하면 첫 머지에서 이미 완료되어 버릴 수 있다.
- 아직 진행 중인 작업이면 그 브랜치/MR을 develop으로 머지하지 말 것.
- release → master 머지 MR에는 닫기 키워드를 **쓰지 않는다** (이미 완료된 이슈 재전환 방지).

---

## 6. Merge Request 규칙

- **MR 제목**에도 이슈 키 포함: `S15P21A609-42 Kafka 컨슈머 구현`
- feature MR 대상은 **develop**, release/hotfix MR만 master 대상
- MR 설명에 완료 조건 체크 및 `Closes S15P21A609-42` 명시

**MR 강제 여부는 팀에서 정한다.** 이전 프로젝트에서는 "권장이되 강제는 아님"으로 운영했고, develop이 protected 상태여도 Developer 직접 푸시를 허용했다.

직접 푸시를 허용하는 경우 반드시 알아둘 것:

- 직접 푸시한 작업은 **커밋 메시지에 이슈 키가 없으면 Jira에 안 잡힌다.** 자동 연동이 동작하지 않아 번다운에서 통째로 빠진다.
- 이전 프로젝트에서 백엔드 리팩터(338파일 / -30,728줄)가 MR·이슈 없이 들어와, 정합 점검에서 발견해 사후에 이슈를 신설한 사례가 있다.
- 이런 경우 **규칙 위반으로 취급하지 않는다.** 정합 점검에서 발견하면 지적이 아니라 사후 이슈 신설로 처리한다.
- 직접 푸시하더라도 **커밋 메시지에 이슈 키를 넣으면** 연동은 그대로 동작한다. 가장 싼 예방책이다.

---

## 7. 담당 & 데일리

- Story는 **한 명이 담당**한다 (담당자 지정 필수)
- 매일 스크럼 때 각자 **진행 중 Story** 기준으로 공유

---

## 8. GitLab ↔ Jira 연동 설정 (관리자 · 트러블슈팅)

자동 완료가 동작하려면 GitLab 쪽 연동이 올바르게 설정돼 있어야 한다.

**위치**: GitLab 프로젝트 → `Settings → Integrations → Jira issues`

🚨 **폼 전체가 회색으로 잠겨 보이면** 상단 드롭다운이 `Use default settings`(인스턴스 기본값 상속)로 잡힌 것이다. **`Use custom settings`로 바꿔야** 편집이 열린다. 바꾸는 순간 입력값이 초기화되므로 다시 채워야 한다. 2026-08-28에 여기서 한참 막혔다 — 권한 문제나 GitLab 버그로 오해하기 쉽다.

| 항목 | 값 |
| --- | --- |
| Web URL | `https://ssafy.atlassian.net` |
| Authentication | Basic |
| Email | 연동용 Jira 계정 이메일 |
| API token | 그 계정의 Jira API 토큰 (id.atlassian.com → Security → API tokens 에서 발급) |
| Trigger | Commit 체크, Merge request 체크 |
| Enable comments | 체크 |
| Transition Jira issues | Use custom transitions = `11,21,31` |

**Web URL 함정**: 반드시 **사이트 루트**(`https://ssafy.atlassian.net`)여야 한다. `.../projects/S15P21A609` 처럼 프로젝트 경로를 넣으면 API 호출이 전부 실패해 아무 동작도 하지 않는다. 이전 프로젝트에서 실제로 이 설정 오류로 한동안 자동 전환이 되지 않았다.

연동에 쓰는 Jira 계정은 이 프로젝트에서 **이슈 전환·댓글 권한**이 있어야 한다. 전환/댓글은 그 계정 이름으로 기록된다.

**동작 확인**: 설정 화면 하단 **Test settings** → `Connection successful`이 떠야 정상.

### 증상별 대처 — 머지했는데 완료가 안 됨

1. **MR이 실제로 머지됐는지 먼저 확인** (`state: merged`). MR을 *생성*만 해도 Jira에 개발 정보(브랜치·MR 링크)가 붙으면서 이슈의 `updated` 시각이 갱신되므로, 반영된 것처럼 보이지만 상태는 그대로다.
2. **기본 브랜치(develop)로 머지됐는지** 확인. 닫기 키워드는 기본 브랜치 머지에서만 동작한다.
3. MR/커밋에 이슈 키와 닫기 키워드가 실제로 들어갔는지 확인
4. GitLab Jira 통합에서 **Test settings** 실행 → 실패 메시지 확인
5. Web URL이 사이트 루트인지, API 토큰이 유효한지 확인

로컬 `git branch -a`에 `origin/HEAD -> origin/master`로 보이더라도 그건 **로컬 캐시**다. 원격 기본 브랜치와 다를 수 있으니 `git remote set-head origin -a`로 갱신한다.

---

## 9. 이전 프로젝트에서 실제로 터진 함정

### FR 번호는 이슈 키가 아니다

브랜치·커밋·MR에 들어가는 키는 **Jira 이슈 번호**(`S15P21A609-45`)이고, `FR-503` 같은 **요구사항 번호와는 전혀 다른 체계**다. 둘을 섞으면 자동 완료가 조용히 실패한다.

```
X  feature/FR-503-emergency-stop           -> Closes FR-503 (아무 이슈도 안 닫힘)
X  feature/S15P21A609-201-storage-space    -> 201이 FR 번호면 실제 이슈와 불일치
O  feature/S15P21A609-45-fr-503-emergency-stop
```

FR 번호를 남기고 싶으면 **이슈 키 뒤에 설명으로 붙인다.** 이전 프로젝트에서 두 번 터졌다 — MR이 `Closes FR-503`으로 나가 이슈가 안 닫혀 같은 작업을 다시 머지했고, 또 다른 MR은 브랜치에 FR 번호를 써서 MR을 두 번 만들었다.

**확인법**: MR 머지 후 Jira 이슈가 실제로 `완료`로 넘어갔는지 눈으로 볼 것. 안 넘어갔으면 키가 틀린 것이다.

### 이슈 키와 작업 내용 불일치

브랜치를 재사용하다 보면 엉뚱한 이슈에 `Closes`가 붙어 **하지도 않은 작업이 완료 처리**된다. 번다운은 내려가는데 실제 산출물은 없어 스프린트 말에야 드러난다. MR 제목·설명의 작업 내용과 이슈 키가 맞는지 확인할 것.

---

## 한 장 요약 (치트시트)

```
1. Jira에서 Epic 아래 Story 생성 -> 현재 활성 스프린트 배정 및 재조회 확인
2. Story 잡으면 -> 담당자 지정 + '진행 중'
3. 브랜치:  develop에서 분기 -> feature/S15P21A609-42-kafka-consumer
4. 커밋:    S15P21A609-42 작업 내용
5. 완료 MR: Closes S15P21A609-42 -> develop 머지 -> 자동 Done
6. 스프린트 종료: release/sprint-N -> master 머지 + 태그
```
