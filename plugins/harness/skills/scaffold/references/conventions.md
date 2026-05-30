# Scaffold — 배경 원칙 (왜 이렇게 만드는가)

생성/커스터마이즈할 때 판단 기준으로 참고. 출처: Harness Engineering 강의(구조·맥락·경계 축).

## 1. 구조가 곧 품질
- Monorepo + 역할별 폴더. AI가 전체 맥락을 한눈에 보고, "어디에 둘지"를 구조로 안다.
- 구조가 잘 잡히면 시간이 갈수록 AI 산출물 품질이 올라간다(구조 없으면 반대). 3개월 후 30% vs 85%.
- 새 코드는 **이웃 패턴을 복제**한다. 새 구조를 발명하지 않는다.

## 2. 사람 문서 vs AI 기록 분리
- `docs/` = 사람의 책임 · 비즈니스의 진실(룰·스펙·ADR·용어). AI는 작업 **전** 참고.
- `.dev/` = AI의 작업 흔적(learnings·디버깅·스크래치). AI는 작업 **중/후** 기록.
- 분리하지 않으면 사람이 관리를 멈춘 순간 AI도 엉뚱한 맥락으로 일한다.

## 3. CLAUDE.md = 지도, 짧게
- 전체 그림 + "어디를 보면 되는지"만. 상세는 `docs/`·`rules/`로 위임.
- ~120줄(최대 200) 유지. 너무 길면 AI 성능이 급격히 저하된다.

## 4. 스코프: User → Project → Folder
- 범위가 좁을수록 우선. 하위는 상위를 자동 상속 + 오버라이드.
- USER `~/.claude/CLAUDE.md`(개인 습관, git 제외) / PROJECT `./CLAUDE.md`(팀 공유, git 커밋) / FOLDER `./{폴더}/CLAUDE.md`.
- **폴더 단위 CLAUDE.md는 의무가 아니다.** 전역 규칙으로 표현 안 되는 특수 제약이 있는 폴더에만 둔다(예: `src/auth/` → JWT만). 그 외엔 만들지 않는 게 정상.

## 5. Progressive Disclosure (점진적 노출)
- "이 상황엔 이 문서를 봐"로 상황별 참조시킨다. CLAUDE.md/규칙에 다 때려넣지 않는다.
- `.claude/rules/`: 항상 적용은 `alwaysApply: true`, 상황별은 `globs:`로 조건부 로드.
- CLAUDE.md가 길어지면 주제별로 `rules/`로 쪼갠다 → 컨텍스트 절약.

## 6. 경계 = 알려주기 + 권한 + 차단
- 뭘 알려줄까: CLAUDE.md / rules (규칙·금기).
- 어디까지 허용: Permission Mode (plan / auto / bypass) — `settings.json`.
- 뭘 막을까: Hook으로 위험 명령 실행 전 차단.
- **이중 안전장치**: CLAUDE.md에 "force-push 금지" 명시 + Hook으로 `git push --force` 차단.

## 7. 개선 = 단순화
- 같은 작업 3번 → Skill. 같은 실수 3번 → Rule.
- 안 쓰는 규칙·MCP·설정은 주기적으로 삭제(쌓이면 AI slop).
- **좋은 Harness는 점점 단순해진다.** 복잡해지면 잘못 가고 있는 신호.

## 스택별 채우기 가이드 (placeholder 치환 예)
| placeholder | TS/React | Python/FastAPI | Go |
|---|---|---|---|
| 코드 스타일 네이밍 | camelCase / PascalCase(컴포넌트) | snake_case / PascalCase(클래스) | MixedCaps |
| testing globs | `**/*.test.*`, `**/__tests__/**` | `**/test_*.py`, `tests/**` | `**/*_test.go` |
| security globs | `**/auth/**`, `**/*.sql`, `**/.env*` | `**/auth/**`, `**/*.sql`, `**/.env*` | `**/auth/**`, `**/*.sql`, `**/.env*` |
| 빌드 산출물 | `dist/`, `build/` | `__pycache__/`, `dist/` | `bin/` |
