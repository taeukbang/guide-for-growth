<!-- GUIDE: 이 파일은 .claude/commands/ 디렉토리에 저장.
     파일명이 곧 커맨드명. 예: team-weekly.md -> /team-weekly

     참고(growth-context.md): 팀 컨텍스트 자동 동기화 스킬.
     Confluence + Slack에서 데이터를 수집하여 CONTEXT.md를 업데이트. -->

[이 스킬이 수행하는 작업을 한 줄로 설명].

"[트리거 문장 1]", "[트리거 문장 2]" 등의 요청 시 사용.

---

# 배경

<!-- GUIDE: 왜 이 자동화가 필요한지. 수동으로 하면 어떤 문제가 있는지. -->

---

# 워크플로우

## Step 1: 데이터 수집

<!-- GUIDE: MCP 도구를 활용한 데이터 수집.
     사용 가능한 MCP 도구:
     - Gmail: mcp__claude_ai_Gmail__gmail_search_messages
     - Slack: mcp__claude_ai_Slack__slack_read_channel
     - Confluence: mcp__atlassian__searchConfluenceUsingCql
     - Google Sheets: mcp__google-workspace__sheets_values_get -->

## Step 2: 데이터 가공

<!-- GUIDE: 수집 데이터를 가공/분석.
     구조화 패턴: 3파트(현상/진단/진행방향), 테이블, 우선순위 분류 등 -->

## Step 3: 저장/업데이트

<!-- GUIDE: 결과를 어디에 저장하는지.
     세션 저장: sessions/YYYY-MM-DD_topic/
     컨텍스트 업데이트: knowledge/context/
     인덱스 업데이트: sessions/SESSION-INDEX.md -->

## Step 4: 결과 보고

<!-- GUIDE: 사용자에게 보고할 형식.
     요약 테이블, 변경 내역, 다음 액션 등 -->

---

# 가드레일

<!-- GUIDE: 실행 시 주의사항.
     - MCP 인증 실패 시: 해당 MCP 건너뛰고 나머지로 진행
     - 데이터 과다 시: 최신 20건만 처리
     - 어투: 보고서체(~함/~임)
     - 에이전트 역할명 노출 금지 -->

- MCP 인증 실패 시 해당 소스 건너뛰고 나머지로 진행
- 데이터 20건 초과 시 최신 20건만 처리
- 어투: 보고서체 (~함/~임/~필요함)
- 에이전트 역할명 산출물에 노출 금지

---

# 참조 파일

| 파일 | 용도 |
|------|------|
| [경로] | [설명] |
