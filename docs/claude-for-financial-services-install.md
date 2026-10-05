# Claude for Financial Services 설치 가이드

출처: [anthropics/financial-services](https://github.com/anthropics/financial-services)의 `README.md`를 정리한 문서입니다. 이 저장소에는 `QUICKSTART.md`가 없고, 설치법은 README의 Getting Started에 있습니다.

> `claude plugin ...` 명령은 **로컬 Claude Code 터미널**에서 실행합니다. `/plugin` 슬래시 명령은 클라우드 세션에서는 지원되지 않습니다.

## Claude Code 설치

```bash
# 마켓플레이스 추가
claude plugin marketplace add anthropics/financial-services

# 핵심 스킬 + 데이터 커넥터 (먼저 설치)
claude plugin install financial-analysis@claude-for-financial-services

# 에이전트 (필요한 것만)
claude plugin install pitch-agent@claude-for-financial-services
claude plugin install gl-reconciler@claude-for-financial-services
claude plugin install market-researcher@claude-for-financial-services

# 분야별 번들
claude plugin install investment-banking@claude-for-financial-services
claude plugin install equity-research@claude-for-financial-services
```

설치하면 에이전트가 Cowork dispatch에 나타나고, 스킬이 관련 상황에서 자동으로 동작하며, `/comps`, `/dcf`, `/earnings`, `/ic-memo` 같은 슬래시 명령을 쓸 수 있습니다.

## Cowork 설치

Cowork에서 **Settings → Plugins → Add plugin**을 열고 다음 중 하나를 합니다.

- 저장소 URL `https://github.com/anthropics/financial-services`를 붙여넣고, 마켓플레이스 목록에서 원하는 에이전트와 분야를 고릅니다.
- `plugins/` 아래 디렉터리(예: `plugins/agent-plugins/pitch-agent/`)를 zip으로 만들어 올립니다.

## Managed Agents API 배포

```bash
export ANTHROPIC_API_KEY=sk-ant-...
scripts/deploy-managed-agent.sh gl-reconciler
```

각 템플릿은 `managed-agent-cookbooks/` 아래에 있고, 같은 이름의 플러그인과 같은 시스템 프롬프트와 스킬을 씁니다. 하위 에이전트 위임(`callable_agents`)은 리서치 프리뷰 기능입니다. API 키는 저장소에 커밋하지 않습니다.

## 에이전트

각 에이전트 플러그인은 필요한 스킬을 내장하고 있어서 에이전트만 설치하면 됩니다.

| 영역 | 에이전트 | 하는 일 |
|---|---|---|
| 커버리지·자문 | Pitch Agent | comps, precedents, LBO를 거쳐 피치덱까지 작성 |
| | Meeting Prep Agent | 고객 미팅 전 브리핑 자료 작성 |
| 리서치·모델링 | Market Researcher | 섹터·테마 개요, 경쟁 구도, 피어 comps, 아이디어 목록 |
| | Earnings Reviewer | 실적 콜·공시를 읽고 모델 갱신과 노트 초안 작성 |
| | Model Builder | DCF, LBO, 3-statement, comps를 Excel에서 작성 |
| 펀드 관리·재무 | Valuation Reviewer | GP 자료를 받아 밸류에이션 템플릿 실행, LP 보고 준비 |
| | GL Reconciler | 불일치를 찾고 원인을 추적해 승인 경로로 전달 |
| | Month-End Closer | 발생액, 롤포워드, 차이 설명 |
| | Statement Auditor | 배분 전 LP 명세서 감사 |
| 운영·온보딩 | KYC Screener | 온보딩 문서 파싱, 규칙 엔진 실행, 누락 표시 |

## 분야별 플러그인

`financial-analysis`를 먼저 설치합니다. 공통 모델링 스킬과 모든 데이터 커넥터가 여기에 들어 있습니다.

| 플러그인 | 추가되는 기능 |
|---|---|
| `financial-analysis` (핵심) | comps, DCF, LBO, 3-statement, 덱 QC, Excel 감사, 커넥터 11개 |
| `investment-banking` | CIM, 티저, 프로세스 레터, 바이어 리스트, 합병 모델, 딜 트래킹 |
| `equity-research` | 실적 노트, 개시 보고서, 모델 갱신, 투자 논리·촉매 추적 |
| `private-equity` | 소싱, 스크리닝, 실사 체크리스트, IC 메모, 포트폴리오 모니터링 |
| `fund-admin` | GL 대사, 불일치 추적, 발생액, 롤포워드, NAV 대조 |
| `operations` | KYC 문서 파싱과 규칙 평가 |
| `claude-for-financial-advisors` | 어드바이저 워크플로(미팅 준비, 컴플라이언스 사전 점검, 리밸런스 검토 등) |
| `lseg` (파트너) | LSEG 데이터 기반 채권·스왑·FX·옵션·매크로 분석 |
| `sp-global` (파트너) | S&P Capital IQ 기반 티어시트, 실적 프리뷰 |

## MCP 데이터 커넥터

모든 커넥터는 `financial-analysis`에 모여 있고 다른 플러그인이 함께 씁니다. 제공사 구독이나 API 키가 필요할 수 있습니다.

| 제공사 | URL |
|---|---|
| Daloopa | `https://mcp.daloopa.com/server/mcp` |
| Morningstar | `https://mcp.morningstar.com/mcp` |
| S&P Global | `https://kfinance.kensho.com/integrations/mcp` |
| FactSet | `https://mcp.factset.com/mcp` |
| Moody's | `https://api.moodys.com/genai-ready-data/m1/mcp` |
| MT Newswires | `https://vast-mcp.blueskyapi.com/mtnewswires` |
| Aiera | `https://mcp-pub.aiera.com` |
| LSEG | `https://api.analytics.lseg.com/lfa/mcp` |
| PitchBook | `https://premium.mcp.pitchbook.com/mcp` |
| Chronograph | `https://ai.chronograph.pe/mcp` |
| Egnyte | `https://mcp-server.egnyte.com/mcp` |
| Box | `https://mcp.box.com` |

## Microsoft 365 add-in 배포 (관리자용)

`claude-for-msft-365-install`은 Excel, PowerPoint, Word, Outlook용 Claude add-in을 자체 클라우드(Vertex AI, Bedrock, 내부 LLM 게이트웨이)에 맞춰 배포하는 IT 관리자용 Claude Code 플러그인입니다. 위의 에이전트·분야별 플러그인과는 별개입니다.

```bash
claude plugin install claude-for-msft-365-install@claude-for-financial-services
/claude-for-msft-365-install:setup
```

## 참고

- 이 저장소의 어떤 내용도 투자·법률·세무·회계 자문이 아닙니다.
- 에이전트는 모델, 메모, 리서치 노트, 대사 같은 초안을 만들 뿐이며, 투자 권고나 거래 실행, 승인은 하지 않습니다. 모든 출력은 전문가의 검토와 서명이 필요합니다.
