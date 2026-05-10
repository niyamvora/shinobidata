<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img src="assets/logo-light.svg" alt="ShinobiData" width="120" />
</picture>

# ShinobiData

**Phân tích danh mục đầu tư + Nghiên cứu thị trường cổ phiếu Mỹ 360° trong AI client của bạn.**

32 công cụ được bảo vệ bằng OAuth. Hoạt động trên Claude, ChatGPT và mọi client tương thích MCP.

[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.shinobidata%2Fresearch-blue)](https://registry.modelcontextprotocol.io/?search=shinobidata)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

🌐  [English](README.md)  ·  [日本語](README.ja.md)  ·  [한국어](README.ko.md)  ·  **Tiếng Việt**

</div>

> [!note]
> **Về bản dịch.** Trang này lấy bản tiếng Anh ([README.md](README.md)) làm chuẩn. Hoan nghênh pull request nếu bạn phát hiện lỗi hoặc cách diễn đạt thiếu tự nhiên trong bản tiếng Việt. Các thuật ngữ kỹ thuật (OAuth, MCP, PKCE...) được giữ nguyên bằng tiếng Anh.

---

## ShinobiData là gì?

Một **máy chủ Model Context Protocol (MCP)** biến AI client của bạn thành chuyên gia phân tích danh mục đầu tư và trợ lý nghiên cứu cổ phiếu Mỹ. Kết nối một lần và hỏi những câu như:

- *"Danh mục của tôi tập trung vào ngành công nghệ bao nhiêu?"*
- *"Tôi có vượt S&P 500 trong năm vừa qua không?"*
- *"So sánh AAPL, MSFT, NVDA, GOOGL, META về định giá, tăng trưởng, chất lượng."*
- *"AAPL đang rẻ hay đắt so với lịch sử 10 năm của chính nó?"*

AI điều phối các công cụ, tường thuật kết quả và trích dẫn dữ liệu. Không cần copy-paste từ bảng tính, không cần dashboard riêng.

## Cài đặt nhanh

### Claude Desktop

Thêm vào `~/Library/Application Support/Claude/claude_desktop_config.json`:

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

Khởi động lại Claude Desktop. Màn hình đồng ý OAuth xuất hiện; đăng nhập bằng Google.

### ChatGPT (Plus / Teams / Enterprise)

Cài đặt → **Connectors** → **Add Custom** → dán `https://mcp.shinobidata.com/api/mcp/mcp` → ủy quyền.

Chế độ Deep Research tự động sử dụng các công cụ `search` + `fetch` của connector.

### Bất kỳ client tương thích MCP nào khác

Sử dụng URL connector `https://mcp.shinobidata.com/api/mcp/mcp` trực tiếp. Streamable HTTP transport, OAuth 2.1 với PKCE.

Hướng dẫn cài đặt chi tiết: [`docs/installation.md`](docs/installation.md) (tiếng Anh).

---

## 32 công cụ, 5 nhóm

| Nhóm | Số lượng | Scope |
|---|---:|---|
| **CRUD danh mục** | 6 | `portfolio:write` |
| **Phân tích danh mục** | 6 | `portfolio:read` |
| **Nghiên cứu công ty** | 7 | `market:read` |
| **Thị trường & Ngành 360°** | 10 | `market:read` |
| **Khám phá** | 3 | `market:read` |

Catalog đầy đủ: [`docs/tools.md`](docs/tools.md) (tiếng Anh).

---

## Quyền riêng tư & Bảo mật

- **OAuth token** lưu dưới dạng SHA-256 hash, không bao giờ ở dạng plaintext.
- **Refresh-token rotation** theo RFC 6749 §10.4.
- **PKCE S256 bắt buộc**; `plain` bị từ chối theo OAuth 2.1.
- **Audit logs** lưu 90 ngày để giám sát bảo mật. Đối số đã được lọc bí mật.

Chính sách quyền riêng tư đầy đủ: <https://shinobidata.com/en/legal/privacy-policy>
Điều khoản dịch vụ đầy đủ: <https://shinobidata.com/en/legal/terms>

---

## Cộng đồng

- 🐛 **Lỗi?** [Tạo issue](https://github.com/niyamvora/shinobidata/issues/new/choose)
- 💡 **Yêu cầu công cụ?** [Discussions → Ideas](https://github.com/niyamvora/shinobidata/discussions/categories/ideas)
- 📬 **Email**: [support@shinobidata.com](mailto:support@shinobidata.com)

Hoan nghênh đóng góp — xem [`CONTRIBUTING.md`](CONTRIBUTING.md) (tiếng Anh).

---

## Giấy phép

MIT cho mọi thứ trong repo docs / examples / assets này — xem [`LICENSE`](LICENSE).

Bản thân máy chủ MCP chạy như một dịch vụ được host tại `https://mcp.shinobidata.com`; codebase production đóng trong giai đoạn truy cập sớm.
