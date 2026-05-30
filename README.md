# kio-scaffold

A Claude Code **plugin marketplace** for Harness Engineering tooling.

> **Harness Engineering** = AI가 혼자서도 잘 일할 수 있는 *작업 환경 자체*를 설계하는 것.
> 이 마켓플레이스는 그 환경을 자동으로 잡아주는 플러그인을 제공합니다.

## 설치

```bash
# 1) 마켓플레이스 추가
claude plugin marketplace add jsk4581/kio-scaffold

# 2) 플러그인 설치
claude plugin install harness@kio-scaffold
```

설치하면 `/harness:scaffold` 스킬을 바로 쓸 수 있습니다.

## 들어있는 플러그인

### `harness` — Harness Engineering toolkit

| 스킬 | 호출 | 설명 |
|------|------|------|
| scaffold | `/harness:scaffold` | 프로젝트에 `CLAUDE.md` + `.claude/rules/` + 역할별 폴더 구조를 한 번에 생성. 인터뷰로 스택·범위를 확정한 뒤 채워준다. 기존 프로젝트면 점검·단순화 모드로도 동작. |

#### `/harness:scaffold`가 만드는 것
```
your-project/
├── CLAUDE.md                       # 프로젝트 지도 (짧게 유지, ≤120줄)
├── .claude/rules/
│   ├── project-structure.md        # 폴더 셋업 규칙 (항상 로드)
│   ├── code-style.md               # 코드 스타일 (항상 로드)
│   ├── testing.md                  # glob 조건부 로드
│   └── security.md                 # glob 조건부 로드
├── docs/        # 사람이 관리하는 문서 (비즈니스 진실)
├── .dev/        # AI가 남기는 작업 기록
├── src/  tests/  out/
```

핵심 원칙: User→Project→Folder 스코프 상속, Progressive Disclosure, 경계의 이중 안전장치, "좋은 Harness는 점점 단순해진다".

## 레포 구조

```
kio-scaffold/
├── .claude-plugin/marketplace.json     # 마켓플레이스 매니페스트
└── plugins/
    └── harness/
        ├── .claude-plugin/plugin.json  # 플러그인 매니페스트
        └── skills/
            └── scaffold/
                ├── SKILL.md
                ├── references/conventions.md
                └── assets/template/     # 생성에 쓰는 템플릿
```

## 로컬 개발/테스트

```bash
git clone https://github.com/jsk4581/kio-scaffold
claude plugin marketplace add ./kio-scaffold
claude plugin install harness@kio-scaffold
```

## 출처

Harness Engineering 강의(Team Attention · 이호연)의 구조·맥락·경계 원칙을 기반으로 합니다.

## License

[MIT](LICENSE)
