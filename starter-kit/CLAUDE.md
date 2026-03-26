# CLAUDE.md

이 파일은 팀 복제 시스템의 규칙서입니다. Claude가 프로젝트에 들어올 때 가장 먼저 읽습니다.

---

## Setup Wizard

세션 시작 시 `.setup-state.json`을 읽어서 현재 설정 상태를 확인합니다.

**step이 5 미만이면** 아래 Setup 프로토콜을 따릅니다.
**step이 5이면** 아래 운영 모드 규칙을 따릅니다.

### Step 0: 환영 + MCP 탐지

설정이 완료되지 않은 상태입니다. 다음을 수행합니다:

1. "팀을 복제하고 Claude와 함께 논의하는 시스템을 만들어볼까요? 총 5단계입니다." 안내
2. MCP 가용 여부 자동 탐지:
   - `mcp__atlassian__getAccessibleAtlassianResources` 호출 시도 → Confluence 가용 확인
   - `mcp__claude_ai_Slack__slack_search_channels` (query: "general") 호출 시도 → Slack 가용 확인
   - `mcp__claude_ai_Gmail__gmail_get_profile` 호출 시도 → Gmail 가용 확인
3. 탐지 결과를 `.setup-state.json`의 mcp 필드에 저장
4. 가용한 MCP가 있으면 "Confluence/Slack/Gmail이 연결되어 있어서 자동 수집이 가능합니다" 안내
5. `.setup-state.json`의 step을 1로 업데이트 후 Step 1 진행

### Step 1: 팀 기본 정보 (init.md 완성)

**목표:** init.md의 placeholder 섹션을 채웁니다.

**MCP 있을 때:**
- Confluence: "팀 스페이스 URL 또는 스페이스 키를 알려주세요" → CQL로 최근 페이지 30개 스캔 → 팀원/미션/KPI 자동 추출 → 사용자 확인
- Slack: "팀 채널명을 알려주세요" → 채널 description에서 팀 미션 추출 → 사용자 확인

**MCP 없을 때:**
다음을 순차적으로 질문합니다:
1. "어떤 팀인가요?" (팀명)
2. "팀의 미션을 한 줄로 설명해주세요"
3. "팀원 구성을 알려주세요 (이름 + 역할)"
4. "팀의 핵심 KPI 3개는 무엇인가요?"

init.md의 섹션 1(사용자 프로필), 섹션 3.2(팀 내 프로젝트), 섹션 4(커뮤니케이션 스타일)를 채운 뒤 `.setup-state.json`의 team_name과 step을 업데이트합니다.

### Step 2: 핵심 페르소나 3개 생성

**목표:** `knowledge/personas/` 하위에 최소 3개 페르소나 파일 생성

1. "팀장님의 주요 의사결정 영역은 무엇인가요?" → `_TEMPLATE-LITE.md` 기반 팀장-lite 생성
2. "팀에서 가장 자주 하는 전문 업무 2가지는?" → `_TEMPLATE-FULL.md` 기반 실무 페르소나 2개 생성
3. `_EXAMPLES.md`를 참고하여 패턴에 맞게 작성
4. devils-advocate.md는 이미 제공됨 (추가 페르소나로 안내)

**MCP 있을 때:**
- Confluence에서 R&R/업무분장 페이지 검색 → 역할 추천
- Slack 채널 최근 3개월 대화에서 업무 키워드 빈도 분석 → 역할 추천

파일 저장 후 `.setup-state.json`의 personas_created와 step을 업데이트합니다.

### Step 3: 팀 컨텍스트 생성

**목표:** `knowledge/context/{TEAM}-CONTEXT.md` 1개 생성

`_TEMPLATE-TEAM-CONTEXT.md`를 기반으로 9섹션 구조를 채웁니다.

**MCP 분기 (핵심 차별화 포인트):**

| MCP 가용 | 행동 |
|----------|------|
| Confluence + Slack | 병렬 서브에이전트로 동시 수집 → 병합 → 9섹션 자동 채움 (70~80% 완성) |
| Confluence만 | "팀 스페이스 키를 알려주세요" → CQL로 최근 6개월 페이지 스캔 → 프로젝트/이슈/정책 추출 (50~60%) |
| Slack만 | "팀 채널명을 알려주세요" → 최근 3개월 주요 결정/이슈 추출 (40~50%) |
| 없음 | 다음 질문으로 수동 수집: (1) 현재 진행 중 주요 프로젝트/이슈 3개, (2) 팀 핵심 정책/규칙, (3) 가장 중요한 오픈 이슈 |

**Confluence MCP 상세:**
- `mcp__atlassian__searchConfluenceUsingCql`로 팀 스페이스 내 최근 변경 페이지 검색
- `mcp__atlassian__getConfluencePage`로 주요 페이지 본문 읽기
- 서브에이전트로 페이지 내용 요약하여 TL;DR + 이슈 자동 추출

**Slack MCP 상세:**
- `mcp__claude_ai_Slack__slack_read_channel`로 팀 채널 최근 대화 읽기
- 주요 결정/합의 사항 추출 → P0~P3 분류

파일 저장 후 `.setup-state.json` 업데이트합니다.

### Step 4: 트리거 레지스트리 설정

**목표:** `knowledge/TRIGGER-REGISTRY.md` 생성

`_TEMPLATE-TRIGGER-REGISTRY.md`를 기반으로:
1. 4개 기본 도메인 자동 등록 (과거세션/팀컨텍스트/OKR/정책)
2. Step 3에서 만든 팀 컨텍스트 파일의 키워드를 자동으로 팀컨텍스트 도메인에 등록
3. "자주 언급하는 키워드나 주제가 더 있나요?" → 추가 도메인 2~3개 등록

**Slack MCP 있을 때:** 채널 최빈 키워드 10개 추출하여 트리거 후보로 제안

파일 저장 후 `.setup-state.json` 업데이트합니다.

### Step 5: 첫 스킬 + 최종 확인

**목표:** 실질적으로 유용한 스킬 1개 생성 + 시스템 완성 확인

1. MCP에 따라 추천:
   - Gmail 있음 → "회의록 자동 정리 스킬을 만들까요?" (meeting-digest 패턴)
   - Confluence 있음 → "팀 컨텍스트 자동 업데이트 스킬을 만들까요?"
   - Slack 있음 → "주간 채널 요약 스킬을 만들까요?"
   - 없음 → `_TEMPLATE-SKILL.md` 구조 안내, MCP 연결 후 활성화 가이드
2. `_TEMPLATE-SKILL.md` 기반으로 `.claude/commands/`에 스킬 파일 생성
3. **최종 확인 체크리스트 실행:**
   - init.md 완성 여부 확인
   - personas/ 하위 파일 3개 이상 존재 확인
   - context/ 하위 팀 컨텍스트 파일 존재 확인
   - TRIGGER-REGISTRY.md 존재 확인
   - 스킬 1개 이상 존재 확인
4. 모든 조건 충족 시 `.setup-state.json`의 step을 5로 설정
5. "설정이 완료되었습니다. 이제 주제만 말하면 자동으로 관련 페르소나와 맥락이 로드됩니다."

---

## 운영 모드 (step == 5 이후)

### Quick Start

**주제만 말하면 됩니다.** 관련 페르소나 로드 + 이전 맥락 검색 + 필요 문서 서브에이전트로 수집을 자동 실행합니다.

---

### 트리거 시스템

사용자 메시지에서 키워드 감지 시 `knowledge/TRIGGER-REGISTRY.md`를 읽고 해당 파일을 로드합니다.

---

### 세션 프로토콜

#### Step 1: 컨텍스트 초기화
- 사용자 주제 언급 시, 관련 이전 논의/결정을 SESSION-INDEX 기반으로 확인
- 필요한 참조 문서는 서브에이전트로 읽어서 요약 (메인 컨텍스트 보호)

#### Step 2: 사용자 의도 파악

| 입력 유형 | Claude 행동 |
|----------|------------|
| 명확한 주제 + 목표 | 관련 맥락 서브에이전트로 수집 후 논의 시작 |
| 주제만, 목표 불분명 | "어떤 결정/검토가 필요하신가요?" 질문 |
| 이전 논의 이어서 | SESSION-INDEX에서 관련 세션 찾고 제안 |
| 새로운 주제 | 이전 세션 관련 여부 확인 후 서브에이전트로 검색 |

#### Step 3: 논의 중 자동 지원
- 이전 결정과 충돌 시 → 명시적 언급 + 변경 이유 확인
- 데이터/근거 필요 시 → 서브에이전트로 관련 문서 검색
- 논의 길어지면 → "핵심 사항 중간 정리" 제안

#### Step 4: 세션 마무리
- "세션 저장할까요?" 제안
- 세션 저장 시 → `knowledge/guides/OUTPUT-RULES.md`를 먼저 읽고 규칙에 따라 작성

---

### 서브에이전트 전략

사용자가 요청하지 않아도 아래 상황에서 자동 실행:

| 감지 상황 | 자동 실행 |
|----------|----------|
| 트리거 키워드로 대용량 문서 로드 필요 | 서브에이전트가 문서를 읽고 관련 부분만 요약 반환 |
| 과거 세션 맥락 확인 필요 | SESSION-INDEX + DECISION-DOC 읽고 요약 반환 |
| 복수 문서/세션 비교 필요 | 병렬 서브에이전트로 각각 읽고 비교표 반환 |

---

### 도구 사용 원칙

전용 도구 우선 사용:
- 파일 읽기: `Read` (NOT `cat`, `head`, `tail`)
- 파일 검색: `Glob` (NOT `find`, `ls`)
- 내용 검색: `Grep` (NOT `grep`, `rg`)
- 파일 수정: `Edit` (NOT `sed`, `awk`)
- 파일 생성: `Write` (NOT `echo >`, `cat <<EOF`)
- Bash는 git, npm 등 시스템 명령 전용

---

### 산출물 규칙

세션 저장 시 `knowledge/guides/OUTPUT-RULES.md`를 먼저 읽고 규칙에 따라 작성합니다.

---

### 금지 사항

- 불확실한 정보를 그럴듯하게 단정 금지
- 산출물에 에이전트 역할명(Growth Director, Devil's Advocate 등) 노출 금지
- 세션 문서에 코드 블록 및 이모지 사용 금지
- 어투: `~함/~임/~필요함` (NOT `~한다/~이다/~합니다/~됩니다`)

---

### 페르소나 활용

- 시작은 lite로 (컨텍스트 절약)
- 깊은 토론 시 풀버전 전환
- Devil's Advocate는 마지막에 (주요 방향 정리 후 리스크 체크)
- Data Analyst는 일찍 (데이터 기반 의사결정 시 초반 로드)

---

*Last updated: 2026-03-26*
