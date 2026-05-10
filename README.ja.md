<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="120" />
</picture>

# ShinobiData

**ポートフォリオ分析 + 米国株 360°リサーチを、あなたのAIクライアントで。**

OAuthで保護された32ツール。Claude、ChatGPT、その他のMCP対応クライアントで動作します。

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

🌐  [English](README.md)  ·  **日本語**  ·  [한국어](README.ko.md)  ·  [Tiếng Việt](README.vi.md)

</div>

> [!note]
> **翻訳について。** このページは英語版（[README.md](README.md)）が正本です。日本語訳に誤りや不自然な表現があればプルリクエストを歓迎します。専門用語（OAuth、MCP、PKCEなど）は英語のまま保持しています。

---

## ShinobiDataとは

**Model Context Protocol (MCP)** サーバーで、AIクライアントをポートフォリオ分析者・米国株リサーチアシスタントに変えます。一度接続すれば、こんな質問ができます：

- *「テクノロジーセクターの集中度は？」*
- *「過去1年でS&P 500を上回ったか？」*
- *「AAPL、MSFT、NVDA、GOOGL、METAをバリュエーション・成長性・品質で比較」*
- *「AAPLは過去10年の自分自身と比べて割安か割高か？」*

AIがツールを連携させ、結果をナレートし、データを引用します。スプレッドシートからのコピペは不要、別のダッシュボードも不要。

## クイックインストール

### Claude Desktop

`~/Library/Application Support/Claude/claude_desktop_config.json` に追加：

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

Claude Desktopを再起動すると、OAuth同意画面が表示されます。Googleでサインインすれば完了。

### ChatGPT (Plus / Teams / Enterprise)

設定 → **コネクタ** → **カスタム追加** → `https://mcp.shinobidata.com/api/mcp/mcp` を貼り付け → 認証。

ディープリサーチモードでは、コネクタの `search` + `fetch` ツールが自動的に使用されます。

### その他のMCP対応クライアント

コネクタURL `https://mcp.shinobidata.com/api/mcp/mcp` を直接指定。Streamable HTTP transport、OAuth 2.1 with PKCE。

詳しいインストール手順は [`docs/installation.md`](docs/installation.md) を参照（英語）。

---

## 32ツール、5つのグループ

| グループ | 数 | スコープ |
|---|---:|---|
| **ポートフォリオCRUD** | 6 | `portfolio:write` |
| **ポートフォリオ分析** | 6 | `portfolio:read` |
| **個別企業リサーチ** | 7 | `market:read` |
| **360°マーケット & セクター** | 10 | `market:read` |
| **ディスカバリー** | 3 | `market:read` |

詳細は [`docs/tools.md`](docs/tools.md)（英語）。

---

## プライバシー & セキュリティ

- **OAuthトークン** はSHA-256でハッシュ化して保存。プレーンテキストでは保存しません。
- **リフレッシュトークンのローテーション** RFC 6749 §10.4準拠。
- **PKCE S256必須**、`plain` はOAuth 2.1に従い拒否。
- **監査ログ** はセキュリティ監視のため90日間保持。引数からシークレットは除去済み。

プライバシーポリシー全文：<https://shinobidata.com/ja/legal/privacy-policy>
利用規約全文：<https://shinobidata.com/ja/legal/terms>

---

## コミュニティ

- 🐛 **バグ？** [Issue を作成](https://github.com/niyamvora/shinobidata/issues/new/choose)
- 💡 **ツールリクエスト？** [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas)
- 📬 **メール**: [support@shinobidata.com](mailto:support@shinobidata.com)

コントリビューション歓迎 — [`CONTRIBUTING.md`](CONTRIBUTING.md)（英語）参照。

---

## ライセンス

このドキュメント / 例 / アセットリポジトリのすべてに対してMIT — [`LICENSE`](LICENSE) 参照。

MCPサーバー本体は `https://mcp.shinobidata.com` でホストされたサービスとして動作します。本番コードベースは早期アクセス期間中はクローズドです。
