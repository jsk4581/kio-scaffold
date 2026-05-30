---
name: scaffold
description: Bootstrap a project's AI work environment (harness) — generates CLAUDE.md, .claude/rules/, and a role-based folder structure following Harness Engineering principles. Use when starting a new project, setting up CLAUDE.md, adding .claude/rules, scaffolding folder structure, or when the user asks to "scaffold", "set up the harness", or "init claude config".
---

# Scaffold — 프로젝트 Harness 셋업

대상 프로젝트에 **CLAUDE.md + `.claude/rules/` + 역할별 폴더 구조**를 한 번에 만든다.
원칙: 짧은 CLAUDE.md(지도), glob 조건부 규칙, User→Project→Folder 스코프, 점진적 노출.
배경/이유 상세는 [references/conventions.md](references/conventions.md).

## 워크플로우

### 1. 인터뷰 먼저 ("해줘" 말고 "물어봐")
바로 생성하지 말고 `AskUserQuestion`으로 빈 맥락을 채운다. 모호하면 한 번 더 묻는다.
- **프로젝트 이름 / 한 줄 설명** (무엇을 푸는가)
- **기술 스택** — 코드 스타일·테스트·보안 규칙의 glob과 기본값을 결정 (예: TS/React, Python/FastAPI, Go)
- **포함할 규칙 모듈** — code-style(항상) / testing / security 중 무엇
- **이미 있는 폴더 구조** — 기존 레이아웃이 있으면 덮어쓰지 말고 맞춘다
- **이중 안전장치 Hook** 추가 여부 (force-push 차단 등)

### 2. 템플릿 로드 + 채우기
`assets/template/`을 기준으로 삼아 답변으로 `{{...}}` 자리표시자를 **모두 치환**한다.
- 빈 placeholder를 남기지 않는다. 스택에 맞게 glob/스타일/보안 값을 구체화한다.
- 기존 파일이 있으면 **덮어쓰기 전에 보여주고 확인**받는다(특히 CLAUDE.md).

### 3. 생성 (대상 프로젝트 루트에)
- 폴더: `src/ docs/ tests/ .dev/ .claude/rules/ out/`
- `CLAUDE.md` ← `assets/template/CLAUDE.md` (≤120줄로 유지, 길면 docs/·rules/로 위임)
- `.claude/rules/`: `project-structure.md`(항상) + 선택한 `code-style/testing/security.md`
- `docs/README.md`, `.dev/README.md` (사람 문서 vs AI 기록 분리)

### 4. 선택 항목
- **폴더 단위 CLAUDE.md** — 전역과 다른 특수 제약이 있는 폴더에만. 템플릿: `assets/template/src/auth/CLAUDE.md.example`. (모든 폴더에 만들지 않는다.)
- **Hook** — `.claude/settings.json`의 PreToolUse로 `git push --force` 차단(CLAUDE.md 금지 + Hook = 이중 안전장치).

### 5. 마무리 보고
생성한 파일 목록 + 사용자가 채워야 할 빈칸 + 다음 단계(`docs/architecture.md` 등 작성)를 알려준다.

## 체크리스트
- [ ] 생성 전에 인터뷰로 스택·범위를 확정했다
- [ ] `{{...}}` placeholder를 모두 치환했다 (빈칸 없음)
- [ ] CLAUDE.md는 지도 수준으로 짧다 (≤120줄)
- [ ] 규칙은 항상-로드 / glob 조건부-로드로 올바르게 분리했다
- [ ] 기존 파일을 덮어쓸 땐 확인받았다
- [ ] 폴더 단위 CLAUDE.md·Hook은 필요할 때만 추가했다

## 점검 (이미 셋업된 프로젝트)
신규 셋업이 아니라 점검 요청이면: 위 구조 대비 빠진 것(분리 안 된 docs/.dev, 비대한 CLAUDE.md,
중복 규칙, 안 쓰는 설정)을 진단하고 개선안을 제시한다 — 추가가 아니라 **단순화**가 기본 방향.
