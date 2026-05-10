# ShinobiData

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="96" align="right" />
</picture>

AI 클라이언트를 진짜 포트폴리오 분석가에 연결합니다. 포트폴리오 추적, 벤치마크 비교, 펀더멘털, 섹터·시장 뷰, 스크리닝, 뉴스를 다루는 32개 도구. 미국 주식, OAuth, 무료.

Claude Desktop, ChatGPT (Plus / Teams / Enterprise), 기타 MCP 클라이언트에서 작동.

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

[English](README.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md)

> 번역 노트: 영어 버전 ([README.md](README.md))이 표준입니다. 오역이나 부자연스러운 표현이 있으면 PR 환영. 기술 용어 (OAuth, MCP, PKCE 등)는 영어로 유지됩니다.

---

## 이런 걸 물어볼 수 있어요

AI가 도구를 조율하고, 당신은 질문만 하면 됩니다.

- "기술 섹터 집중도는 얼마나?"
- "올해 S&P 500을 능가했나?"
- "AAPL, MSFT, NVDA, GOOGL, META 비교해줘."
- "AAPL이 자체 10년 역사 대비 저평가야?"
- "앞으로 14일 동안 발표 예정인 실적은?"

각 질문이 1-4번의 도구 호출로 변환됩니다. AI가 내레이션하고, 데이터는 출처와 함께 인용됩니다.

## 설치

### Claude Desktop

`~/Library/Application Support/Claude/claude_desktop_config.json` 편집:

```json
{
  "mcpServers": {
    "shinobidata": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.shinobidata.com/api/mcp/mcp"]
    }
  }
}
```

Claude 완전 종료 (Cmd-Q, 창 닫기만으로는 안 됨). 재시작. OAuth 화면이 한 번 뜨면 Google로 로그인. 끝.

### ChatGPT

설정 → 커넥터 → 사용자 정의 추가 → `https://mcp.shinobidata.com/api/mcp/mcp` 붙여넣기 → 인증.

Deep Research 모드는 `search`와 `fetch`를 자동으로 사용합니다.

### 기타

Streamable HTTP, OAuth 2.1 with PKCE. `https://mcp.shinobidata.com/api/mcp/mcp`를 지정하면 well-known 엔드포인트에서 자동 설정.

전체 안내: [docs/installation.md](docs/installation.md) (영어). 문제 해결: [docs/troubleshooting.md](docs/troubleshooting.md) (영어).

## 안에는

32개 도구, 5개 그룹.

| 그룹 | 개수 | 스코프 |
|---|---:|---|
| 포트폴리오 CRUD | 6 | `portfolio:write` |
| 포트폴리오 분석 | 6 | `portfolio:read` |
| 개별 기업 리서치 | 7 | `market:read` |
| 시장 & 섹터 | 10 | `market:read` |
| 디스커버리 (`screen`, `search`, `fetch`) | 3 | `market:read` |

동의 화면에서 필요한 스코프만 부여 가능. 리서치만 하고 포트폴리오 조작은 안 시키려면 `portfolio:write` 건너뛰기.

전체 카탈로그: [docs/tools.md](docs/tools.md) (영어).

## 개인정보와 보안

- OAuth 토큰은 SHA-256 해시로 저장. 리프레시 토큰 로테이션.
- 인증 코드는 단일 사용, TTL 10분.
- PKCE S256 필수, OAuth 2.1에 따라 `plain` 거부.
- 감사 로그 90일 보관, 인수에서 비밀 정보 제거.
- 토큰별 속도 제한.

전문: <https://shinobidata.com/en/legal/privacy-policy> · <https://shinobidata.com/en/legal/terms>

## 도움 / 기여

버그: [버그 템플릿](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml)
도구 아이디어: [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas)
질문: [Discussions → Q&A](https://github.com/niyamvora/shinobidata/discussions/categories/q-a) 또는 <support@shinobidata.com>

번역과 문서 수정 환영. 자세한 내용은 [CONTRIBUTING.md](CONTRIBUTING.md) (영어).

## 라이선스

저장소 내 문서, 예제, 설정은 MIT. 브랜드 자산 (`assets/`)은 MIT가 아님, [`assets/README.md`](assets/README.md) 참조. MCP 서버 자체는 `mcp.shinobidata.com`에서 호스팅되며, 프로덕션 코드베이스는 알파 기간 동안 비공개입니다.
