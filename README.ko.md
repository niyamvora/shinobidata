<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="120" />
</picture>

# ShinobiData

**포트폴리오 분석 + 미국 주식 360° 리서치를 AI 클라이언트에서.**

OAuth로 보호되는 32개 도구. Claude, ChatGPT 및 모든 MCP 호환 클라이언트에서 작동합니다.

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

🌐  [English](README.md)  ·  [日本語](README.ja.md)  ·  **한국어**  ·  [Tiếng Việt](README.vi.md)

</div>

> [!note]
> **번역에 대해.** 이 페이지는 영어 버전 ([README.md](README.md))이 표준입니다. 한국어 번역에 오류나 부자연스러운 표현이 있으면 풀 리퀘스트를 환영합니다. 기술 용어 (OAuth, MCP, PKCE 등)는 영어로 유지됩니다.

---

## ShinobiData란?

**Model Context Protocol (MCP)** 서버로, AI 클라이언트를 포트폴리오 분석가이자 미국 주식 리서치 어시스턴트로 만듭니다. 한 번 연결하면 다음과 같은 질문을 할 수 있습니다:

- *"기술 섹터에 얼마나 집중되어 있나요?"*
- *"지난 1년 동안 S&P 500을 능가했나요?"*
- *"AAPL, MSFT, NVDA, GOOGL, META를 밸류에이션, 성장성, 품질로 비교"*
- *"AAPL이 자체 10년 역사 대비 저평가인지 고평가인지?"*

AI가 도구를 조율하고, 결과를 내레이션하고, 데이터를 인용합니다. 스프레드시트에서 복사 붙여넣기, 별도 대시보드 불필요.

## 빠른 설치

### Claude Desktop

`~/Library/Application Support/Claude/claude_desktop_config.json`에 추가:

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

Claude Desktop 재시작. OAuth 동의 화면이 나타나면 Google로 로그인.

### ChatGPT (Plus / Teams / Enterprise)

설정 → **커넥터** → **사용자 정의 추가** → `https://mcp.shinobidata.com/api/mcp/mcp` 붙여넣기 → 인증.

심층 리서치 모드는 커넥터의 `search` + `fetch` 도구를 자동으로 사용합니다.

### 기타 MCP 호환 클라이언트

커넥터 URL `https://mcp.shinobidata.com/api/mcp/mcp` 직접 사용. Streamable HTTP transport, OAuth 2.1 with PKCE.

자세한 설치 안내는 [`docs/installation.md`](docs/installation.md) 참조 (영어).

---

## 32개 도구, 5개 그룹

| 그룹 | 개수 | 스코프 |
|---|---:|---|
| **포트폴리오 CRUD** | 6 | `portfolio:write` |
| **포트폴리오 분석** | 6 | `portfolio:read` |
| **개별 기업 리서치** | 7 | `market:read` |
| **360° 시장 & 섹터** | 10 | `market:read` |
| **디스커버리** | 3 | `market:read` |

전체 카탈로그는 [`docs/tools.md`](docs/tools.md) 참조 (영어).

---

## 개인정보 & 보안

- **OAuth 토큰**은 SHA-256 해시로 저장. 평문 저장 안 함.
- **리프레시 토큰 로테이션** RFC 6749 §10.4 준수.
- **PKCE S256 필수**, OAuth 2.1에 따라 `plain` 거부.
- **감사 로그** 보안 모니터링을 위해 90일 보관. 인수에서 비밀 정보 제거.

개인정보 처리방침 전문: <https://shinobidata.com/en/legal/privacy-policy>
이용 약관 전문: <https://shinobidata.com/en/legal/terms>

---

## 커뮤니티

- 🐛 **버그?** [이슈 작성](https://github.com/niyamvora/shinobidata/issues/new/choose)
- 💡 **도구 요청?** [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas)
- 📬 **이메일**: [support@shinobidata.com](mailto:support@shinobidata.com)

기여 환영 — [`CONTRIBUTING.md`](CONTRIBUTING.md) 참조 (영어).

---

## 라이선스

이 문서 / 예제 / 자산 저장소의 모든 항목에 대해 MIT — [`LICENSE`](LICENSE) 참조.

MCP 서버 자체는 `https://mcp.shinobidata.com`에서 호스팅 서비스로 실행됩니다. 프로덕션 코드베이스는 초기 액세스 기간 동안 비공개입니다.
