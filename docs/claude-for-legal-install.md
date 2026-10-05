# Claude for Legal 설치 가이드

출처: [anthropics/claude-for-legal](https://github.com/anthropics/claude-for-legal)의 `QUICKSTART.md`와 `README.md`를 정리한 문서입니다.

> 아래 명령은 모두 **로컬 Claude Code 터미널**에서 실행합니다. `/plugin` 명령은 클라우드 세션에서는 지원되지 않습니다.

## 설치 순서

1. 마켓플레이스를 추가합니다. 문서는 저장소를 내려받아 압축을 푼 로컬 폴더 경로를 쓰라고 안내합니다.
   ```
   /plugin marketplace add /Users/you/Desktop/claude-for-legal
   ```
   > 문서에는 GitHub 경로 방식이 없습니다. `/plugin marketplace add anthropics/claude-for-legal`이 되는지는 확인하지 못했습니다.
2. 업무에 맞는 플러그인을 설치합니다. 예시는 `privacy-legal`입니다.
   ```
   /plugin install privacy-legal@claude-for-legal
   ```
3. **Claude Code를 재시작합니다.** 필수 단계입니다. 재시작하지 않으면 "Command not found"가 뜹니다.
4. 설정 인터뷰를 한 번 실행합니다. 빠른 설정은 약 2분, 전체 설정은 10~15분이 걸립니다.
   ```
   /privacy-legal:cold-start-interview
   ```
5. 조사 도구를 연결합니다. 연결하지 않으면 인용이 모두 `[verify]`(미검증)로 표시됩니다.
   - Claude Code: 처음 필요할 때 승인 요청이 뜹니다.
   - Cowork: Settings → Connectors → CourtListener를 추가합니다.

## 설치 범위: user scope

`/plugin install` 때 범위를 물으면 **user scope**를 고릅니다. project scope는 프로젝트 폴더 밖의 파일(다운로드, 문서 폴더 등)을 읽지 못해 대부분의 스킬이 동작하지 않습니다. user scope라도 플러그인이 읽을 수 있는 파일은 직접 지정한 파일과 현재 디렉터리의 파일뿐입니다.

이미 project scope로 설치했다면 다음처럼 다시 설치합니다.
```
/plugin uninstall <plugin>
/plugin install <plugin>@claude-for-legal
```
(홈 디렉터리에서 실행)

## 분야별 플러그인

| 대상 | 플러그인 | 첫 명령 |
|---|---|---|
| 개인정보 / DPO | `privacy-legal` | `/privacy-legal:use-case-triage` |
| 계약 | `commercial-legal` | `/commercial-legal:review` |
| 기업 / M&A | `corporate-legal` | `/corporate-legal:diligence-issue-extraction` |
| 고용 / HR | `employment-legal` | `/employment-legal:wage-hour-qa` |
| 제품 법무 | `product-legal` | `/product-legal:is-this-a-problem` |
| IP | `ip-legal` | `/ip-legal:clearance` |
| 소송 | `litigation-legal` | `/litigation-legal:matter-intake` |
| 규제 / 컴플라이언스 | `regulatory-legal` | `/regulatory-legal:reg-feed-watcher` |
| AI 거버넌스 | `ai-governance-legal` | `/ai-governance-legal:use-case-triage` |
| 로스쿨 클리닉 | `legal-clinic` | `/legal-clinic:cold-start-interview` |
| 로스쿨 학생 | `law-student` | `/law-student:cold-start-interview` |
| 법무 운영 / 스킬 탐색 | `legal-builder-hub` | `/legal-builder-hub:registry-browser` |

## Cowork 설치

1. [Claude Desktop](https://claude.com/download)을 설치합니다.
2. Claude Cowork 접근 권한을 받습니다.
3. 저장소 README의 영상 안내를 따릅니다.

## 참고

- 설정 결과는 `~/.claude/plugins/config/claude-for-legal/<plugin>/CLAUDE.md`에 저장됩니다. 모든 스킬이 이 파일을 읽으며, 직접 수정하거나 설정을 다시 실행할 수 있습니다.
- **모든 출력은 변호사 검토용 초안이며 법률 자문이 아닙니다.** 이 플러그인은 Anthropic의 법적 입장을 나타내지 않습니다.

## 문제 해결

| 증상 | 해결 |
|---|---|
| "Command not found" | Claude Code를 재시작합니다. |
| "Run setup first" | `/<plugin>:cold-start-interview`를 먼저 실행합니다. |
| 인용에 `[verify]` 표시 | 조사 도구를 연결합니다. |
| "I can't read [file]" | project scope로 설치된 경우가 많습니다. user scope로 다시 설치하거나 파일을 프로젝트 폴더로 옮깁니다. |
| 원하는 기능이 없음 | `/legal-builder-hub:related-skills-surfacer`로 더 맞는 스킬을 찾습니다. |
