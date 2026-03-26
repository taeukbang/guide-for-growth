# 팀 복제 스타터 킷 — Claude와 함께 논의하는 시스템

Claude Code에서 이 폴더를 열면 자동으로 Setup Wizard가 시작됩니다.

## 시작하기

1. 이 `starter-kit/` 폴더를 복사하여 원하는 위치에 배치
2. Claude Code에서 해당 폴더를 열기
3. 아무 메시지나 입력하면 CLAUDE.md가 자동 로드되며 설정 안내가 시작됨

## Setup 과정 (5단계)

| Step | 내용 | 소요 시간 |
|------|------|----------|
| 1 | 팀 기본 정보 입력 (init.md 완성) | 10분 |
| 2 | 핵심 페르소나 3개 생성 | 20분 |
| 3 | 팀 컨텍스트 문서 생성 | 20분 |
| 4 | 트리거 레지스트리 설정 | 10분 |
| 5 | 첫 스킬 생성 + 최종 확인 | 10분 |

중간에 멈추고 나중에 이어서 할 수 있습니다. 진행 상태는 `.setup-state.json`에 저장됩니다.

## MCP 연결 (선택, 권장)

MCP가 연결되어 있으면 Confluence/Slack/Gmail에서 데이터를 자동 수집하여 초안을 만들어줍니다.

| MCP | 효과 |
|-----|------|
| Confluence | 팀 스페이스에서 프로젝트/이슈/정책 자동 추출 |
| Slack | 팀 채널에서 최근 결정/논의 자동 추출 |
| Gmail | 회의 요약 메일에서 회의록 자동 정리 |

MCP 없이도 수동 입력으로 모든 설정을 완료할 수 있습니다.

## 폴더 구조

```
starter-kit/
  CLAUDE.md                 ← 규칙서 + Setup Wizard
  init.md                   ← LLM 컨텍스트 (마리트 프리셋)
  .setup-state.json         ← 설정 진행 상태

  knowledge/
    personas/               ← 전문가 관점 파일
      _TEMPLATE-FULL.md     풀 페르소나 작성 템플릿
      _TEMPLATE-LITE.md     라이트 페르소나 작성 템플릿
      _EXAMPLES.md          실제 예시 (참고용)
      devils-advocate.md    범용 비판적 검토 (즉시 사용)

    context/                ← 팀 맥락 정보
      _TEMPLATE-TEAM-CONTEXT.md

    guides/                 ← 산출물/세션 규칙
      OUTPUT-RULES.md       DECISION-DOC/MEETING-NOTES 구조
      SESSION-GUIDE.md      세션 사용 가이드

    policies/               ← 정책 문서
      _TEMPLATE-POLICY.md

    _TEMPLATE-TRIGGER-REGISTRY.md

  .claude/commands/         ← 자동화 스킬
    setup.md                /setup 커맨드
    _TEMPLATE-SKILL.md      스킬 작성 템플릿

  sessions/                 ← 세션 기록
    SESSION-INDEX.md        마스터 인덱스
```

## 수동 설정

자동 Wizard 대신 직접 설정하고 싶다면:

1. `init.md`의 `<!-- SETUP -->` 주석 부분을 채우기
2. `_TEMPLATE-FULL.md`를 복사하여 페르소나 파일 생성 (최소 3개)
3. `_TEMPLATE-TEAM-CONTEXT.md`를 복사하여 팀 컨텍스트 작성
4. `_TEMPLATE-TRIGGER-REGISTRY.md`를 `knowledge/TRIGGER-REGISTRY.md`로 복사 후 편집
5. `.setup-state.json`의 step을 5로 변경

## 참고

이 스타터 킷은 마이리얼트립 그로스실의 팀 복제 시스템(MRT-GROWTH)을 기반으로 만들었습니다.
교안: https://guide-for-growth.vercel.app
