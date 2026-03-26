팀 복제 시스템 Setup Wizard를 실행합니다.

".setup-state.json"을 읽어서 현재 진행 상태를 확인하고, 아직 완료되지 않은 단계부터 안내를 시작합니다.

설정을 처음부터 다시 시작하려면 ".setup-state.json"의 step을 0으로 변경한 뒤 이 커맨드를 실행하세요.

---

# 실행 절차

1. `.setup-state.json`을 Read로 읽기
2. CLAUDE.md의 "Setup Wizard" 섹션을 참조하여 현재 step부터 진행
3. 각 step 완료 시 `.setup-state.json` 업데이트
4. step이 5에 도달하면 "설정 완료" 메시지 출력
