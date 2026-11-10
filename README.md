# Claude Code 세팅 키트 (라이트) v0.2

내 프로젝트에 복사해 넣으면 Claude Code가 이렇게 바뀝니다.

- `.env`·키 파일을 읽거나 고치지 못합니다.
- 강제 push, 전체 삭제 같은 위험한 명령은 실행 직전에 막습니다.
- `git push`, `git reset`처럼 되돌리기 어려운 명령은 매번 물어봅니다.
- 프로젝트 규칙을 적어 두는 `CLAUDE.md` 틀과 `/commit` 명령이 생깁니다.

[규니의 개발블로그](https://hgko-dev.tistory.com)의 팀 세팅 키트([team-ai-kit](https://github.com/hgko1207/team-ai-kit))에서 혼자서도 쓸 수 있는 부분만 덜어 낸 무료판입니다. MIT 라이선스입니다.

## 이 문서에서 쓰는 표시

| 표시 | 어디에 입력하나 |
|---|---|
| **[터미널]** | VS Code 메뉴 **터미널 → 새 터미널** (단축키 `` Ctrl+` ``) |
| **[Claude 입력창]** | Claude Code 대화창. `/`로 시작하는 것은 Claude Code 명령입니다 |

## 1. 준비물 확인 (2분)

| 필요한 것 | 확인 방법 | 없으면 |
|---|---|---|
| Claude Code | VS Code 왼쪽에 Claude 아이콘이 있거나, [터미널]에서 `claude --version` | [설치 안내](https://code.claude.com/docs/en/setup) |
| Node.js 18 이상 | [터미널]에서 `node -v` | [nodejs.org](https://nodejs.org)에서 LTS 설치 후 VS Code를 다시 엽니다 |
| Git for Windows (Windows만) | [터미널]에서 `git --version` | [git-scm.com](https://git-scm.com/downloads/win)에서 설치 |

Node가 없으면 위험 명령 차단이 **아무 말 없이 꺼집니다.** 권한 규칙(읽기 차단, 매번 확인)은 Node 없이도 동작합니다.

## 2. 파일 받기 (1분)

오른쪽 **Releases**에서 최신 `claude-code-starter-kit.zip`을 받아 압축을 풉니다. 안에 있는 것 중 프로젝트에 넣을 것은 두 개입니다.

- `.claude` 폴더
- `CLAUDE.md.template`

## 3. 프로젝트에 넣기 (3분)

넣을 곳은 프로젝트의 **맨 위 폴더**입니다. `.git` 폴더나 `package.json`이 있는 곳입니다.

**처음 세팅하는 프로젝트** (`.claude/settings.json`이 아직 없음)

1. `.claude` 폴더와 `CLAUDE.md.template`을 그대로 복사합니다.
2. 4단계로 갑니다.

**이미 `.claude/settings.json`이 있는 프로젝트**

1. `.claude` 폴더를 복사합니다. 탐색기가 `settings.json`을 덮어쓸지 물으면 **건너뛰기**를 고릅니다. 나머지 파일은 복사합니다.
2. [Claude 입력창]에 아래처럼 입력해 두 설정을 합칩니다. 받은 키트 폴더 경로는 본인 것으로 바꿉니다.
   ```
   C:\Downloads\claude-code-starter-kit\.claude\settings.json 의 permissions와 hooks를
   이 프로젝트의 .claude/settings.json에 합쳐줘. 기존 항목은 지우지 마.
   ```
3. Claude가 보여 주는 변경을 읽고 승인합니다.

합친 뒤에는 Claude가 설정 파일을 더 고칠 수 없게 잠깁니다. 안전장치를 Claude가 스스로 풀지 못하게 하려는 것이라, 이후 설정은 직접 편집기에서 고칩니다.

## 4. CLAUDE.md 만들기 (5분)

`CLAUDE.md`는 Claude가 대화를 시작할 때마다 읽는 프로젝트 안내서입니다.

**CLAUDE.md가 아직 없으면**

1. [Claude 입력창]에 `/init`을 입력합니다. Claude가 프로젝트를 읽고 `CLAUDE.md` 초안을 만듭니다.
2. `CLAUDE.md.template`을 열어 **원칙, 완료 기준, 하지 말 것, 반복된 실수** 부분을 복사해 초안 끝에 붙입니다.
3. `{{ }}`가 남은 줄은 채우거나 지웁니다. 예: `{{예: Conventional Commits}}` → `feat: 로그인 추가처럼 쓴다`
4. `CLAUDE.md.template`은 지웁니다.

**CLAUDE.md가 이미 있으면** 템플릿의 위 네 부분만 기존 파일에 옮기고 템플릿은 지웁니다.

짧게 유지하세요. 길수록 덜 지킵니다. 100줄 안쪽이 기준입니다.

## 5. 동작 확인 (2분)

1. Claude Code를 다시 시작합니다. VS Code라면 **새 대화**를 열면 됩니다. 설정은 대화를 시작할 때 읽습니다.
2. [Claude 입력창]에 `.env.test 파일을 만들고 A=1 한 줄 넣어줘`를 입력합니다.
   - **"보호 경로라서 수정할 수 없습니다"**가 나오면 정상입니다.
   - 파일이 그냥 만들어지면 "막힐 때"를 봅니다. 만들어진 `.env.test`는 지웁니다.
3. [Claude 입력창]에 `/hooks`를 입력하면 등록된 훅 2개(파일 수정, 명령 실행 전)가 보입니다.

## 막힐 때

| 증상 | 확인할 것 |
|---|---|
| 차단이 전혀 안 된다 | ① [터미널]에서 `node -v` ② `.claude/settings.json`에 `"hooks"`가 있는지 ③ 새 대화를 열었는지 |
| 모든 파일 수정과 명령이 막힌다 | `.claude/team-ai.json`의 JSON 문법이 깨졌을 때입니다. 편집기로 직접 고칩니다 |
| 같은 확인 창이 계속 뜬다 | 자주 쓰는 안전한 명령은 `settings.json`의 `"allow"`에 넣습니다 (아래 "고쳐 쓰기") |
| `/commit`이 안 보인다 | `.claude/skills/commit/` 폴더가 복사됐는지, 새 대화를 열었는지 |

## 들어 있는 것

| 파일 | 하는 일 |
|---|---|
| `.claude/settings.json` | 권한 규칙과 훅 연결. 비밀 파일 읽기·수정 차단, 되돌리기 어려운 git 명령과 폴더 삭제는 매번 확인, 권한 우회 모드 끄기 |
| `.claude/hooks/protect-paths.js` | `.env*`, 키 파일(`*.pem`, `*.key`, `id_rsa`, `id_ed25519` 등), `.npmrc`, `secrets/`, `.git/` 수정을 막음. 견본(`.env.example` 등)은 허용 |
| `.claude/hooks/block-dangerous-bash.js` | 루트·홈 전체 삭제, 보호 브랜치 강제 push, `git clean -fd`, `DROP TABLE`, `curl … \| sh` 같은 명령을 막음 (Bash·PowerShell 모두) |
| `.claude/team-ai.json` | 기본 목록에 **더해서** 막을 경로·명령·브랜치를 적는 곳 |
| `.claude/skills/commit/` | `/commit` — 브랜치·비밀값 확인 후 목적별로 나눠 커밋 (push는 안 함) |
| `CLAUDE.md.template` | 프로젝트 안내서 틀 |
| `CHECKLIST.md` | 자가 진단 10문항. "아니요"인 문항만 연결된 글을 읽으면 됩니다 |

<details>
<summary><b>고쳐 쓰기</b> (자주 쓰는 명령 허용, 막을 경로 추가)</summary>

- **자주 쓰는 명령 허용**: `settings.json`의 `"allow"`에 넣으면 묻지 않습니다. Windows는 Bash와 PowerShell을 짝으로 넣습니다.
  예: `"Bash(npm test *)"`, `"PowerShell(npm test *)"`
- **막을 경로·명령 추가**: `.claude/team-ai.json`
  - `protectedPaths`: 수정 금지 경로 (`**` = 하위 전체). 예: `"**/config/prod/**"`
  - `allowPaths`: 직접 추가한 보호 경로 중 예외. 기본 목록(`.env`, 키 파일)은 풀 수 없습니다
  - `protectedBranches`: 강제 push를 막을 브랜치 (기본: main, master, develop, production, release 등)
  - `blockedCommands`: 추가로 막을 명령 (정규식). 예: `"\\bprisma\\s+migrate\\s+reset\\b"`
- 비밀 파일 위치가 다르면 `settings.json`의 `deny`와 `team-ai.json`의 `protectedPaths`를 같이 고칩니다.

</details>

<details>
<summary><b>팀에서 쓰기</b></summary>

- `.claude/`와 `CLAUDE.md`를 git에 커밋하면 팀원 모두에게 같이 적용됩니다.
- 개인 설정은 `.claude/settings.local.json`, 개인 메모는 `CLAUDE.local.md`에 둡니다(커밋하지 않음).
- 권한 확인 창에서 "다시 묻지 않기"를 고르면 저장 위치를 묻습니다. 공유 파일(`.claude/settings.json`)을 고르면 팀 전체 권한이 바뀌니, 혼자 쓸 허용은 Local을 고릅니다.
- 계획→검증 흐름, PR 리뷰까지 쓰려면 플러그인판 [team-ai-kit](https://github.com/hgko1207/team-ai-kit)을 설치합니다(무료, MIT). 터미널에서 `claude plugin marketplace add hgko1207/team-ai-kit` → `claude plugin install team-ai@team-ai-kit` → Claude에 `/team-ai:setup`. 도입 비용은 [AI 코딩 비용 계산기](https://hgko1207.github.io/ai-cost-calc/)로 미리 볼 수 있습니다.

</details>

<details>
<summary><b>알아둘 한계</b></summary>

- 권한 규칙과 훅은 **실수를 막는 장치이지 보안 장치가 아닙니다.** 변형된 명령이나 스크립트를 통한 접근까지 모두 막지는 못합니다. 비밀 정보는 저장소에 두지 마세요.
- 훅은 잘못 막는 일이 없도록 정말 위험한 경우만 막고, 나머지 되돌리기 어려운 명령은 매번 확인(`ask`)으로 둡니다.
- 권한 우회 모드 끄기(`disableBypassPermissionsMode`)는 개인 설정으로 다시 켤 수 있습니다. 회사 전체에 강제하려면 IT가 배포하는 관리 설정(managed settings)을 씁니다.
- 확인 환경: Windows 11 + Git Bash, Claude Code 2.1.83, Node.js 24. Git Bash 없이 PowerShell만 있는 환경은 시험하지 않았습니다.

</details>

## 더 읽을 글

- [CLAUDE.md 작성법](https://hgko-dev.tistory.com/584) · [Claude Code Hooks 실전](https://hgko-dev.tistory.com/597) · [Claude 스킬 만들기](https://hgko-dev.tistory.com/630) · [AI가 짠 코드 리뷰하는 법](https://hgko-dev.tistory.com/606)
- 공식 문서: [권한](https://code.claude.com/docs/en/permissions) · [훅](https://code.claude.com/docs/en/hooks) · [스킬](https://code.claude.com/docs/en/skills) · [CLAUDE.md](https://code.claude.com/docs/en/memory)

---
v0.2 (team-ai-kit v0.2.8 기준) · 바뀐 내용은 [Releases](https://github.com/hgko1207/claude-code-starter-kit/releases)에 적습니다.
