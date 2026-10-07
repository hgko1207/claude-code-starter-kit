# Claude Code 세팅 키트 (라이트) v0.1

Claude Code를 처음 제대로 세팅할 때 프로젝트에 바로 넣어 쓰는 파일 묶음입니다.
**규니의 개발블로그**(hgko-dev.tistory.com)의 팀 세팅 키트(team-ai-kit v0.2.2)에서 혼자서도 바로 쓸 수 있는 부분만 덜어 낸 무료판입니다. 공식 문서(2026-10 기준)와 대조하고, 실제로 동작하는지 시험했습니다.

- 확인 환경: Windows 11 + Git Bash, Claude Code 2.1.83, Node.js 24
- 받기: 오른쪽 **Releases**에서 zip을 받거나, 이 저장소의 파일을 그대로 복사하세요. 업데이트 소식은 저장소 **Watch → Custom → Releases**로 받을 수 있습니다.

## 들어 있는 것

| 파일 | 하는 일 |
|---|---|
| `CLAUDE.md.template` | Claude가 매번 읽는 프로젝트 안내서 틀. 명령·원칙 4가지·완료 기준·하지 말 것·반복된 실수 |
| `.claude/settings.json` | 권한 규칙 + 훅 연결. 비밀 파일 읽기 차단, `git push`·`reset`·`clean` 같은 되돌리기 어려운 명령은 매번 확인 |
| `.claude/hooks/protect-paths.js` | `.env`·키 파일·`.npmrc`·`secrets/`·`.git/` 수정을 막음 (견본 `.env.example`·`.sample`·`.template`은 허용) |
| `.claude/hooks/block-dangerous-bash.js` | 루트·홈 전체 삭제, main 브랜치 강제 push, `git clean -fd`, `DROP TABLE`, `curl … \| sh` 같은 명령을 실행 직전에 막음 (Bash·PowerShell 모두) |
| `.claude/team-ai.json` | 기본 보호 목록은 훅에 들어 있고, 여기에는 **추가로** 막을 경로·명령·브랜치만 적음 |
| `.claude/skills/commit/` | `/commit` — 브랜치·비밀값 확인 후 목적별로 나눠 커밋 (push는 안 함) |
| `CHECKLIST.md` | 자가 진단 10문항. "아니요"인 문항만 연결된 글을 읽으면 됨 |

## 필요한 것

- Claude Code
- **Node.js 18 이상** — 훅이 Node로 동작합니다 (`jq` 같은 추가 설치 불필요)
  - Node가 없다면 `.claude/settings.json`에서 `"hooks"` 부분을 지우고 쓰세요. 권한 규칙과 나머지는 그대로 동작합니다.

## 넣는 순서 (3단계)

**1. `.claude` 폴더를 프로젝트 맨 위 폴더에 복사**

- 이미 `.claude/settings.json`이 있다면 **덮어쓰지 말고** `permissions`와 `hooks` 내용을 기존 파일에 합치세요.

**2. `CLAUDE.md.template`을 `CLAUDE.md`로 이름 바꾸고 `{{ }}` 칸 채우기**

- Claude Code에서 `/init`을 실행하면 프로젝트를 읽고 초안을 만들어 줍니다. 그 초안에 템플릿의 "원칙·완료 기준·하지 말 것·반복된 실수"만 옮겨도 됩니다.
- 짧게 유지하세요. 공식 문서는 파일당 200줄 이하를 권장합니다(길수록 덜 지킴). 이 템플릿은 100줄 안쪽이 기준입니다.

**3. Claude Code를 다시 시작하고 확인**

- Claude에게 `.env 파일에 한 줄 추가해줘`라고 해 보세요. "보호 경로라서 수정할 수 없습니다"가 나오면 정상입니다.
- `/hooks`로 등록된 훅 목록도 볼 수 있습니다.

## 프로젝트에 맞게 고칠 곳

- **자주 쓰는 명령 허용**: `settings.json`의 `"allow"`에 추가하면 매번 묻지 않습니다.
  - 예: `"Bash(npm test *)"`, `"Bash(./gradlew test *)"` — Windows에서 PowerShell을 쓴다면 `"PowerShell(npm test *)"`도 짝으로
- **막을 경로·명령 추가**: `.claude/team-ai.json` — 적은 것은 기본 목록에 **더해집니다**
  - `protectedPaths`: 수정 금지 경로 (`**` = 하위 전체). 예: `"**/config/prod/**"`
  - `allowPaths`: 위에서 **직접 추가한** 보호 경로 중 예외로 풀 파일. 예: `"**/config/prod/README.md"` (기본 보호 목록 — `.env`·키 파일 등 — 은 풀 수 없음)
  - `protectedBranches`: 강제 push를 막을 브랜치 (기본: main·master·develop·production·release 등)
  - `blockedCommands`: 추가로 막을 명령 (정규식). 예: `"\\bprisma\\s+migrate\\s+reset\\b"`
- **비밀 파일 위치가 다르면** `settings.json`의 `deny`와 `team-ai.json`의 `protectedPaths`를 같이 고치세요.
- 이 설정 파일들(`settings.json`, `settings.local.json`, `team-ai.json`)은 **Claude가 고치지 못하게 막혀 있습니다.** 안전장치를 Claude가 스스로 풀 수 없게 하려는 것이라, 직접 편집기에서 고치세요.
- `team-ai.json` 문법이 틀리면 훅이 **모든 수정·명령을 막습니다**(설정이 깨진 채로 보호가 꺼지는 것보다 안전). 갑자기 전부 막히면 JSON 문법부터 확인하세요.

## 팀에서 쓰려면

- `.claude/`(settings.json, hooks, skills, team-ai.json)와 `CLAUDE.md`를 **git에 커밋**하면 팀원 모두에게 같이 적용됩니다.
- 개인만 쓸 설정은 `.claude/settings.local.json`, 개인 메모는 `CLAUDE.local.md`에 두세요(보통 git에서 제외).
- 팀 전체 표준(기능 계획→검증 흐름, PR 리뷰, 대화 압축 후 규칙 유지, 2주 점검)까지 맞추는 팀용 세팅은 [블로그](https://hgko-dev.tistory.com)에서 따로 안내합니다. 팀 도입 비용은 [AI 코딩 비용 계산기](https://hgko1207.github.io/ai-cost-calc/)로 미리 볼 수 있습니다.

## 알아둘 한계

- 권한 규칙과 훅은 **실수를 막는 안전장치이지 보안 장치가 아닙니다.** 공식 문서도 명령 패턴 차단은 변형된 명령으로 우회될 수 있다고 안내합니다. 비밀 정보는 애초에 저장소에 두지 마세요.
- 훅은 잘못 막는 일이 적도록 **정말 위험한 경우만** 좁게 막습니다. 그 외 되돌리기 어려운 명령은 `ask`(매번 확인)로 둡니다.

## 더 읽을 글

- [CLAUDE.md 작성법](https://hgko-dev.tistory.com/584)
- [Claude Code Hooks 실전](https://hgko-dev.tistory.com/597)
- [Claude 스킬 만들기](https://hgko-dev.tistory.com/630)
- [AI가 짠 코드 리뷰하는 법](https://hgko-dev.tistory.com/606)

## 공식 문서

- 권한: https://code.claude.com/docs/en/permissions
- 훅: https://code.claude.com/docs/en/hooks
- 스킬: https://code.claude.com/docs/en/skills
- CLAUDE.md(메모리): https://code.claude.com/docs/en/memory

---
v0.1 · team-ai-kit v0.2.2 기반 · MIT 라이선스 — 개인·팀 프로젝트에 자유롭게 쓰고 고쳐도 됩니다.
