# ShinobiData

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="96" align="right" />
</picture>

Kết nối AI client của bạn với một chuyên gia phân tích danh mục đầu tư thực thụ. 32 công cụ cho việc theo dõi danh mục, so sánh với benchmark, phân tích cơ bản, ngành và thị trường, sàng lọc, tin tức. Cổ phiếu Mỹ, OAuth, miễn phí.

Hoạt động với Claude Desktop, ChatGPT (Plus / Teams / Enterprise), và mọi MCP client khác.

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

[English](README.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md)

> Ghi chú dịch: Bản tiếng Anh ([README.md](README.md)) là chuẩn. Chào đón PR cho lỗi dịch hoặc cách diễn đạt thiếu tự nhiên. Thuật ngữ kỹ thuật (OAuth, MCP, PKCE...) giữ nguyên tiếng Anh.

---

## Bạn có thể hỏi gì

AI điều phối, bạn chỉ cần đặt câu hỏi.

- "Tôi tập trung vào ngành công nghệ bao nhiêu?"
- "Tôi có vượt S&P 500 năm nay không?"
- "So sánh AAPL, MSFT, NVDA, GOOGL, META."
- "AAPL có rẻ so với lịch sử 10 năm không?"
- "Báo cáo lợi nhuận sắp tới trong 14 ngày là gì?"

Mỗi câu hỏi chuyển thành 1-4 lệnh gọi công cụ. AI tường thuật, dữ liệu được trích dẫn.

## Cài đặt

### Claude Desktop

Sửa `~/Library/Application Support/Claude/claude_desktop_config.json`:

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

Tắt hẳn Claude (Cmd-Q, không phải đóng cửa sổ). Mở lại. Màn OAuth chạy một lần, đăng nhập bằng Google. Xong.

### ChatGPT

Cài đặt → Connectors → Add Custom → dán `https://mcp.shinobidata.com/api/mcp/mcp` → ủy quyền.

Chế độ Deep Research tự động dùng `search` và `fetch`.

### Khác

Streamable HTTP, OAuth 2.1 với PKCE. Trỏ tới `https://mcp.shinobidata.com/api/mcp/mcp` và phần còn lại tự suy ra từ well-known endpoints.

Hướng dẫn đầy đủ: [docs/installation.md](docs/installation.md) (tiếng Anh). Khắc phục sự cố: [docs/troubleshooting.md](docs/troubleshooting.md) (tiếng Anh).

## Bên trong

32 công cụ, 5 nhóm.

| Nhóm | Số lượng | Scope |
|---|---:|---|
| CRUD danh mục | 6 | `portfolio:write` |
| Phân tích danh mục | 6 | `portfolio:read` |
| Nghiên cứu công ty | 7 | `market:read` |
| Thị trường & Ngành | 10 | `market:read` |
| Khám phá (`screen`, `search`, `fetch`) | 3 | `market:read` |

Tại màn đồng ý, chỉ cấp scope bạn cần. Chỉ muốn dùng cho nghiên cứu, không cho AI sửa danh mục? Bỏ `portfolio:write`.

Catalog đầy đủ: [docs/tools.md](docs/tools.md) (tiếng Anh).

## Quyền riêng tư & Bảo mật

- OAuth token lưu dạng SHA-256 hash. Refresh token rotate.
- Authorization code dùng một lần, TTL 10 phút.
- PKCE S256 bắt buộc, `plain` bị từ chối theo OAuth 2.1.
- Audit log lưu 90 ngày, đối số đã lọc bí mật.
- Rate limit theo từng token.

Đầy đủ: <https://shinobidata.com/en/legal/privacy-policy> · <https://shinobidata.com/en/legal/terms>

## Trợ giúp / Đóng góp

Lỗi: [Bug template](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml)
Ý tưởng công cụ: [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas)
Câu hỏi: [Discussions → Q&A](https://github.com/niyamvora/shinobidata/discussions/categories/q-a) hoặc <support@shinobidata.com>

Hoan nghênh dịch thuật và sửa tài liệu. Chi tiết: [CONTRIBUTING.md](CONTRIBUTING.md) (tiếng Anh).

## Giấy phép

Tài liệu, ví dụ, cấu hình trong repo này theo MIT. Brand assets (`assets/`) không phải MIT, xem [`assets/README.md`](assets/README.md). Máy chủ MCP chạy tại `mcp.shinobidata.com`, codebase production đóng trong giai đoạn alpha.
