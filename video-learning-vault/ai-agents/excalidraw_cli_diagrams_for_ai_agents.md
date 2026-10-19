# Excalidraw CLI: Kết Xuất Sơ Đồ Kiến Trúc & Phản Hồi Trực Quan Cho AI Agents | Excalidraw Official

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=AzODN_qw53k)
- **Video ID:** `AzODN_qw53k`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 00:40 (Official Product Release Short)
- **Speaker:** Excalidraw Official Team
- **Transcript File:** [excalidraw_cli_diagrams_for_ai_agents_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/excalidraw_cli_diagrams_for_ai_agents_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong bản phát hành sản phẩm chính thức, đội ngũ **Excalidraw** công bố **Excalidraw CLI (`@excalidraw/cli`)** — công cụ giao diện dòng lệnh cho phép lập trình viên và đặc biệt là các **AI Coding Agents (Claude Code, Cursor, Codex, OpenCode)** thao tác lập trình trực tiếp với sơ đồ kiến trúc phần mềm, kết xuất hình ảnh không cần mở trình duyệt và thiết lập vòng lặp phản hồi trực quan (Visual Feedback Loop).

Thay vì phải thao tác kéo thả thủ công trên giao diện web, Excalidraw CLI mở ra 3 khả năng mang tính bản lề cho kỹ nghệ Agent:
1. **Lập danh mục & Quản lý Scenes qua Terminal:** Kết nối không gian làm việc đám mây (Excalidraw+) hoặc tệp tin cục bộ, liệt kê các bộ sưu tập và bản vẽ kiến trúc dưới dạng bảng CLI có cấu trúc.
2. **Kết xuất ảnh tĩnh tức thì (Headless Rendering):** Chuyển đổi sơ đồ Excalidraw thành ảnh PNG/SVG theo chủ đề Sáng (`light`) hoặc Tối (`dark`) bằng một dòng lệnh, trả về siêu dữ liệu kích thước (`width`, `height`, `mimeType`) dạng JSON.
3. **Phân tích cú pháp JSON thuần chủng cho AI Agents (Plain JSON for Agents):** Trích xuất toàn bộ cấu trúc phần tử (`elements`) dưới dạng mảng JSON tất định. Điều này cho phép AI đọc hiểu quan hệ giữa các thành phần kiến trúc, tự động tạo mới các khối chức năng và kiểm tra cấu trúc sơ đồ thông qua các bộ lọc như `jq`.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       EXCALIDRAW CLI IN AGENTIC WORKFLOW LOOPS                              │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                  AI CODING AGENT                                            │
│                       (Claude Code / Cursor / Codex / Antigravity)                          │
│                                           │                                                 │
│                    ┌──────────────────────┴──────────────────────┐                          │
│                    ▼ (CLI Headless Inspection)                   ▼ (Programmatic Edit)      │
│      [ `excalidraw scenes content get` ]          [ `excalidraw scenes render` ]            │
│      - Fetches plain JSON AST structure            - Generates PNG/SVG (Light/Dark)         │
│      - Traverses `.elements` array via jq          - Generates deterministic file on disk   │
│      - Verifies nodes, edges, connectors           - Returns dimension metadata             │
│                    │                                             │                          │
│                    └──────────────────────┬──────────────────────┘                          │
│                                           ▼                                                 │
│                             [ MULTIMODAL VERIFIER GATE ]                                    │
│                 - Agent reads rendered PNG back into its vision context                     │
│                 - Verifies architectural correctness before generating code                 │
│                 - Commits diagram alongside code in Git repository                          │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Chi Tiết Lệnh & Cú Pháp Excalidraw CLI

### 1. Cài Đặt & Đăng Nhập Không Gian Làm Việc
```bash
# Cài đặt toàn cục qua npm
npm i -g @excalidraw/cli

# Đăng nhập vào workspace Excalidraw+
excalidraw login
# Tuỳ chọn xác thực:
# ❯ API key (nhập khóa API từ bảng điều khiển Excalidraw+)
# ◯ OAuth
```
*Thông tin phiên sau khi đăng nhập thành công:*
- `keySource`: `login`
- `workspace.id`: `770VYkU8ePu`
- `workspace.name`: `fireside`
- `workspace.subscriptionStatus`: `active`
- `workspace.userCount`: `10`

---

### 2. Duyệt Danh Sách Bản Vẽ (Browse Scenes)
Tra cứu danh sách các sơ đồ trong một bộ sưu tập cụ thể:
```bash
excalidraw collections scenes list <COLLECTION_ID> -o table
```
*Kết quả mẫu xuất ra terminal:*
```text
ID           NAME                                     COLLECTION     PINNED  UPDATED
9QaZzJ6KtAz  IBIS                                     9gUS8M9SLcb    yes     2026-10-08T14:28:41.533Z
2ijfvoFK9LT  GitHub Actions CI/CD Swimlane - Detailed 9gUS8M9SLcb    no      2026-10-08T14:37:37.874Z
6F8vqBQy3wZ  Habit Tracking App                       9gUS8M9SLcb    no      2026-09-21T19:27:04.344Z
9akmDng4qaX  Devices                                  9gUS8M9SLcb    no      2026-10-08T14:35:44.306Z
9UMBeqpPrb9  Networking                               9gUS8M9SLcb    no      2026-10-08T14:28:18.826Z
```

---

### 3. Kết Xuất Sơ Đồ Thành File Ảnh (Render Engine)
Kết xuất bản vẽ từ xa hoặc cục bộ thành tệp ảnh PNG/SVG để đính kèm tài liệu hoặc phục vụ kiểm thử:
```bash
# Kết xuất mặc định (Giao diện sáng)
excalidraw scenes render 2ijfvoFK9LT --out ci-cd.png

# Kết xuất theo chủ đề Tối (Dark mode)
excalidraw scenes render 2ijfvoFK9LT --out ci-cd-dark.png --theme dark
```
*Phản hồi cấu trúc JSON tự động:*
```json
{
  "sceneId": "2ijfvoFK9LT",
  "path": "/home/dev/diagrams/ci-cd.png",
  "mimeType": "image/png",
  "width": 1216,
  "height": 2047,
  "frameId": null
}
```

---

### 4. Truy Xuất Dữ Liệu JSON Thuần (Plain JSON for Agents)
Điểm cốt lõi dành cho các hệ thống AI Agent là khả năng trích xuất toàn bộ cây cú pháp phần tử (Elements AST) dưới dạng JSON:
```bash
# Đếm số lượng phần tử hình học trong sơ đồ
excalidraw scenes content get 2ijfvoFK9LT | jq '.elements | length'
# Output: 106
```
Agent có thể:
- Đọc vị trí tọa độ các hình hộp (`rectangle`), mũi tên liên kết (`arrow`), nhãn văn bản (`text`).
- Tự động sinh thêm các node kiến trúc mới và đẩy ngược lại vào scene mà không làm hỏng layout hiện có.

---

## 3. Ý Nghĩa Đối Với Kỹ Nghệ Tác Nhân (Agentic Workflows)

1. **Khép Kín Vòng Lặp Trực Quan (Closed Visual Loop):** Trước đây, AI chỉ có thể sinh mã Mermaid dưới dạng văn bản. Với Excalidraw CLI, agent có thể sinh sơ đồ, render ra PNG, dùng công cụ `view_file` (Multimodal Vision) đọc lại ảnh vừa render để tự kiểm tra xem các mũi tên có bị chồng chéo hay sai luồng logic hay không.
2. **Tài Liệu Hóa Tự Động Trong CI/CD:** Tích hợp Excalidraw CLI vào GitHub Actions để tự động kết xuất sơ đồ kiến trúc cập nhật mỗi khi có PR merge, lưu trực tiếp vào thư mục `docs/architecture.png`.
3. **Tiết Kiệm Token:** Thay vì nạp toàn bộ ảnh vào context, agent có thể đọc cấu trúc JSON của sơ đồ để hiểu luồng hệ thống với chi phí token tối thiểu.
