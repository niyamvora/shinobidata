# ShinobiData

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="96" align="right" />
</picture>

AIクライアントを本格的なポートフォリオアナリストに繋ぎます。ポートフォリオ管理、ベンチマーク比較、ファンダメンタルズ、セクター・市場分析、スクリーニング、ニュースをカバーする32ツール。米国株、OAuth、無料。

Claude Desktop、ChatGPT (Plus / Teams / Enterprise)、その他のMCPクライアントで動作。

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

[English](README.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md)

> 翻訳ノート: 英語版 ([README.md](README.md)) が正本です。誤訳や不自然な表現があれば PR を歓迎します。技術用語 (OAuth、MCP、PKCE等) は英語のままです。

---

## こんなことが聞けます

AIがツールを連携させ、あなたは質問するだけ。

- 「テクノロジーの集中度はどれくらい?」
- 「今年S&P 500を上回った?」
- 「AAPL、MSFT、NVDA、GOOGL、METAを比較して」
- 「AAPLは過去10年と比較して割安?」
- 「次の14日間に発表予定の決算は?」

各質問が1〜4回のツール呼び出しに変換されます。AIが結果をナレートし、データには出典がつきます。

## インストール

### Claude Desktop

`~/Library/Application Support/Claude/claude_desktop_config.json` を編集:

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

Claudeを完全終了 (Cmd-Q、ウィンドウ閉じるだけではNG)。再起動。OAuth画面が一度だけ表示されるのでGoogleでサインイン。完了。

### ChatGPT

設定 → コネクタ → カスタム追加 → `https://mcp.shinobidata.com/api/mcp/mcp` を貼り付け → 認証。

Deep Researchモードでは `search` と `fetch` が自動で使われます。

### その他

Streamable HTTP、OAuth 2.1 with PKCE。`https://mcp.shinobidata.com/api/mcp/mcp` を指定すれば、well-knownエンドポイントから自動設定されます。

詳細は [docs/installation.md](docs/installation.md) (英語)、トラブルシュートは [docs/troubleshooting.md](docs/troubleshooting.md) (英語)。

## 中身

32ツール、5つのグループ。

| グループ | ツール数 | スコープ |
|---|---:|---|
| ポートフォリオCRUD | 6 | `portfolio:write` |
| ポートフォリオ分析 | 6 | `portfolio:read` |
| 個別企業リサーチ | 7 | `market:read` |
| マーケット & セクター | 10 | `market:read` |
| 探索 (`screen`、`search`、`fetch`) | 3 | `market:read` |

同意画面で必要なスコープだけ許可できます。リサーチだけに使ってポートフォリオ操作は許可しないなら `portfolio:write` をスキップ。

カタログ全体: [docs/tools.md](docs/tools.md) (英語)。

## プライバシーとセキュリティ

- OAuthトークンはSHA-256でハッシュ化して保存。リフレッシュトークンはローテーション。
- 認可コードは単回使用、TTL 10分。
- PKCE S256必須、`plain` はOAuth 2.1に従い拒否。
- 監査ログは90日保持、引数からシークレットは除去。
- トークン単位のレート制限あり。

詳細: <https://shinobidata.com/en/legal/privacy-policy> · <https://shinobidata.com/en/legal/terms>

## ヘルプ・コントリビュート

バグ: [バグレポート](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml)
ツールアイデア: [Discussions → Ideas](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/ideas)
質問: [Discussions → Q&A](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/q-a) または <support@shinobidata.com>

翻訳・ドキュメント修正歓迎。詳細は [CONTRIBUTING.md](CONTRIBUTING.md) (英語)。

## ライセンス

リポジトリ内のドキュメント、サンプル、設定はMIT。ブランドアセット (`assets/`) はMITではなく、[`assets/README.md`](assets/README.md) を参照。MCPサーバー本体は `mcp.shinobidata.com` でホストされており、プロダクションコードベースはアルファ期間中はクローズドです。
