# Vercel Agent Desktop: Computer Use Mã Nguồn Mở Tốc Độ Gấp 40 Lần Bằng Accessibility Tree (Không Dùng Screenshot) | Prism Labs

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=iEhCLn9utDU)
- **Video ID:** `iEhCLn9utDU`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-03
- **Duration:** 07:16
- **Speaker:** Prism Labs (Phân tích kiến trúc mã nguồn mở Vercel Agent Desktop — Chris Tate / Vercel Labs)
- **Transcript File:** [agent_desktop_computer_use_accessibility_tree_prism_labs_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/agent_desktop_computer_use_accessibility_tree_prism_labs_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Vào ngày 31 tháng 10 năm 2026, **Vercel Labs** đã bất ngờ công bố mã nguồn mở dự án **Agent Desktop** (giấy phép Apache 2.0) với tuyên bố gây chấn động: *"agent-browser, cho toàn bộ máy tính của bạn"*. Dự án đạt gần **9,000 stars và hơn 600 forks** chỉ sau vài ngày ra mắt bản 1.0.

Điểm đột phá mang tính cách mạng của Agent Desktop là: **Loại bỏ hoàn toàn ảnh chụp màn hình (Screenshots) ra khỏi vòng lặp điều khiển máy tính (Computer Use Loop)**. 
- Các hệ thống Computer Use hiện nay từ Anthropic (Claude Computer Use), OpenAI, và Google đều dựa vào quy trình: Chụp màn hình ➔ Gửi ảnh vào mô hình thị giác (Vision LLM) ➔ Dự đoán tọa độ pixel `(x, y)` ➔ Click chuột. Quy trình này tốn **3 đến 8 giây** cho mỗi bước quan sát, tiêu tốn hàng nghìn vision tokens đắt đỏ, và cực kỳ dễ gãy (click lệch 2 pixel, sai lệch khi đổi độ phân giải màn hình hoặc DPI scaling).
- Ngược lại, **Agent Desktop đọc trực tiếp Cây Trợ Năng (OS Accessibility Tree)** của hệ điều hành — tầng dữ liệu ngữ nghĩa mà các ứng dụng đọc màn hình (Screen Readers) sử dụng. 
- **Tốc độ nhảy vọt 40x đến 150x:** Một lần chụp cấu trúc UI chỉ mất **50 đến 200 mili-giây** (thực tế ~120ms, lời gọi nhị phân Rust native ~21ms).
- Thay vì truyền hình ảnh, Agent Desktop trả về **JSON có cấu trúc** với các định danh phần tử ngữ nghĩa (**Qualified Refs**). Tác nhân AI tương tác trực tiếp lên ref, triệt tiêu hoàn toàn bài toán ước lượng tọa độ pixel.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    VISION SCREENSHOT COMPUTER USE VS. VERCEL AGENT DESKTOP                  │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL VISION APPROACH (Anthropic / OpenAI / Gemini):                                 │
│                                                                                             │
│   Desktop Screen ──► [Screenshot PNG] ──► [Vision LLM (Sonnet/GPT-4o)] ──► Bounding Boxes   │
│                             │                           │                       │           │
│                       Tốn 3-8 giây                Tốn hàng nghìn           Tọa độ (x, y)    │
│                       mỗi quan sát                Vision Tokens            dễ lệch 2 pixel  │
│                                                                                             │
│  ➔ VÒNG LẶP 50 BƯỚC: Mất 4 đến 7 phút, chi phí token khổng lồ, dễ click nhầm khi UI đổi!   │
│                                                                                             │
│  VERCEL AGENT DESKTOP (OS Accessibility Tree + Rust CLI):                                   │
│                                                                                             │
│   Desktop Screen ──► [OS Accessibility API] ──► [JSON Semantic Tree] ──► [LLM thuần Text]   │
│                      - macOS: AXUIElement       - snapshot_id: "snap_94a"      │            │
│                      - Windows: UI Automation   - ref: "snap_94a:btn_save"     ▼            │
│                      - Linux: AT-SPI            - role: button, label: "Save"  Lệnh gọi Ref │
│                             │                           │                      (Tất định)   │
│                       Tốn 50-200 ms               Tốn <500 Text Tokens                      │
│                       (Nhanh gấp 40x)             (Cực kỳ rẻ)                               │
│                                                                                             │
│  ➔ VÒNG LẶP 50 BƯỚC: Hoàn thành trong vài giây, an toàn tuyệt đối nhờ cơ chế từ chối Stale Ref!│
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. So Sánh Chi Tiết: Vision Computer Use vs. Accessibility Tree

Bảng phân tích đối sánh kiến trúc giữa hai trường phái điều khiển máy tính:

| Tiêu Chí Kỹ Thuật | Phương Pháp Chụp Màn Hình (Anthropic / OpenAI) | Vercel Agent Desktop (OS Accessibility Tree) |
| :--- | :--- | :--- |
| **Bản chất quan sát** | Xử lý ảnh raster (Pixels) qua Vision Encoder | Đọc cây phân cấp đối tượng ngữ nghĩa từ OS kernel |
| **Thời gian quan sát** | **3,000 – 8,000 ms** (3 đến 8 giây / bước) | **50 – 200 ms** (Trung bình 120ms, native 21ms) |
| **Định dạng dữ liệu** | Ảnh PNG/JPEG kích thước lớn | JSON thuần túy (Role, Label, State, Value) |
| **Tiêu thụ Token** | Hàng nghìn Vision Tokens đắt đỏ | Vài trăm Text Tokens thông thường |
| **Cơ chế xác định mục tiêu**| Ước lượng tọa độ pixel `(x, y)` tương đối | Tham chiếu định danh chuẩn hóa (`Qualified Ref`) |
| **Khả năng gãy (Brittle)** | Rất cao (kẹt DPI scale, đổi theme, lệch 2px) | Rất thấp (tương tác trực tiếp lên OS element handle) |
| **Xử lý UI thay đổi ngầm** | Dễ click nhầm do agent không nhận ra UI trượt | **Tự động từ chối (Reject)** nếu snapshot ID bị cũ |
| **Chiếm quyền người dùng** | Thường chiếm trỏ chuột và cửa sổ (Headed) | **Headless mặc định** (không cướp chuột, không đổi focus)|
| **Hạn chế lớn nhất** | Chậm và tốn kém | Mù đối với Canvas / WebGL (Figma, Games) |

---

## 3. Kiến Trúc Lõi Của Vercel Agent Desktop

Dự án được xây dựng bằng ngôn ngữ **Rust** thuần túy với các tầng adapter chuyên biệt cho từng hệ điều hành:
- **macOS Adapter:** Giao tiếp qua `AXUIElement` (Accessibility API của Apple).
- **Windows Adapter:** Tích hợp sâu với Microsoft `UI Automation` (UIA COM interfaces).
- **Linux Adapter:** Kết nối với daemon `AT-SPI` (Assistive Technology Service Provider Interface) qua D-Bus.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          AGENT DESKTOP INTERNAL ARCHITECTURE                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│   AI Agent (Claude Code / Codex / Cursor / Python Script / Shell)                           │
│        │                                                                                    │
│        ├─── (1) `agent-desktop snapshot --app "VSCode"`                                      │
│        │                                                                                    │
│        ▼                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│   │ RUST CORE CLI & C-BINDINGS (60+ Commands)                                           │   │
│   ├─────────────────────────────────────────────────────────────────────────────────────┤   │
│   │ [macOS: AXUIElement]   │   [Windows: UI Automation]   │   [Linux: AT-SPI D-Bus]     │   │
│   └────────────────────────┴──────────────────────────────┴─────────────────────────────┘   │
│        │                                                                                    │
│        ▼ (2) Trả về JSON Snapshot                                                           │
│   {                                                                                         │
│     "snapshot_id": "snap_x810",                                                             │
│     "elements": [                                                                           │
│       { "ref": "snap_x810:e12", "role": "button", "label": "Run Tests", "enabled": true },  │
│       { "ref": "snap_x810:e13", "role": "editor", "label": "main.rs", "value": "..." }     │
│     ]                                                                                       │
│   }                                                                                         │
│        │                                                                                    │
│        ├─── (3) `agent-desktop click --ref "snap_x810:e12"`                                 │
│        │    `agent-desktop type --ref "snap_x810:e13" --text "cargo test"`                  │
│        ▼                                                                                    │
│   ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│   │ VERIFICATION & RE-SNAPSHOT LOOP: Observe ➔ Act ➔ Observe                            │   │
│   │ - Kiểm tra stale ref (Nếu snapshot_id cũ ➔ Từ chối ngay)                            │   │
│   │ - Xác nhận event dispatch thành công trước khi trả control về agent                 │   │
│   └─────────────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 1. Vòng lặp: Observe ➔ Act ➔ Verify (Không Khớp Pixel)
Mô hình hoạt động của Agent Desktop tuân theo 3 nhịp:
1. **Observe (Quan sát):** Gọi lệnh snapshot cửa sổ ứng dụng. Hệ điều hành trả về danh sách các node có thể tương tác kèm nhãn, vai trò và trạng thái.
2. **Act (Hành động):** Agent thực hiện lệnh click, gõ chữ, chọn menu dựa trên mã `ref`. Công cụ xác nhận OS đã nhận lệnh thành công rồi mới phản hồi.
3. **Verify (Xác minh):** Snapshot lại lần nữa để kiểm tra xem giá trị hoặc trạng thái của UI đã thay đổi đúng như kỳ vọng hay chưa.

### 2. Định Danh Phần Tử Chuẩn Hóa (Qualified Ref System)
Trong Agent Desktop, một `ref` không bao giờ đứng độc lập. Nó luôn mang theo `snapshot_id`:
$$\text{Ref Format} = \text{snapshot\_id} : \text{element\_id}$$

*Ví dụ:* `snap_94a2:btn_submit`.
- Nếu màn hình đã thay đổi (người dùng click chuyển trang hoặc ứng dụng tự cập nhật), `snapshot_id` hiện tại sẽ trở thành `snap_94a3`.
- Khi agent cố tình bắn lệnh click vào ref cũ `snap_94a2:btn_submit`, hệ thống **ngay lập tức từ chối lệnh (Hard Reject)** thay vì bấm mù vào một phần tử mới chiếm chỗ cũ.
- Thiết kế này tiêu diệt tận gốc lỗi kinh điển của Computer Use: **Giao diện đã trôi đi nhưng Agent vẫn tưởng đang ở trang cũ và click bừa vào nút nguy hiểm**.

### 3. Cầu Nối DevTools Bridge Cho Ứng Dụng Electron & Chromium
Một vấn đề nan giải của các ứng dụng viết bằng Electron (như Slack, VS Code, Discord) là các lập trình viên web thường lười viết thẻ Accessibility đầy đủ, khiến cây trợ năng bị thiếu thông tin.

Vercel Labs giải quyết vấn đề này cực kỳ thông minh:
- Agent Desktop có thể tự động mở cổng **Chrome DevTools Protocol (CDP)** trên tiến trình Electron đó.
- Sau đó, nó chuyển quyền điều khiển nội dung web bên trong ứng dụng cho **agent-browser** (công cụ điều khiển DOM web nổi tiếng của Vercel).
- Hai công cụ kết hợp hoàn hảo: Agent Desktop quản lý viền cửa sổ, menu native OS; `agent-browser` quản lý trực tiếp DOM bên trong!

---

## 4. Mô Hình An Ninh Cực Đoan: "Safety by Refusal" (An Toàn Bằng Cách Từ Chối)

Khác với các công cụ tự động hóa cẩu thả thường đòi hỏi full quyền Admin hoặc tự do chiếm chuột, Agent Desktop áp dụng triết lý an toàn nghiêm ngặt:

1. **Headless Mặc Định:** Không di chuyển con trỏ chuột của người dùng, không giật focus cửa sổ trừ khi lập trình viên chủ động truyền cờ `--headed`. Người dùng vẫn có thể tiếp tục gõ phím làm việc trên máy trong khi agent chạy ngầm.
2. **Khóa Các Primitive Nguy Hiểm:** Các lệnh thô như `mouse_down` hoặc `key_down` cấp thấp bị vô hiệu hóa hoàn toàn cho đến khi cơ chế release được kiểm định chặt chẽ.
3. **Bảo Vệ Tiến Trình Hệ Thống:** Không cho phép agent ra lệnh đóng hoặc can thiệp vào các tiến trình bảo mật cốt lõi của hệ điều hành.
4. **Chặn Leo Thang Đặc Quyền (Privilege Escalation):** Trên Windows và Linux, các ứng dụng chạy quyền Administrator/Root sẽ tự động từ chối lời gọi từ Agent Desktop với thông báo lỗi phân quyền, ngăn chặn AI vô tình hoặc bị prompt injection chiếm quyền root máy tính.

---

## 5. Cuộc Tranh Luận Thẳng Thắn: Hạn Chế Của Cây Trợ Năng

Dù vượt trội về tốc độ và chi phí, cộng đồng kỹ sư vẫn chỉ ra những ranh giới rõ ràng của phương pháp này:

- **Ứng dụng vẽ trên Canvas/WebGL:** Toàn bộ các ứng dụng vẽ đồ họa như Figma Canvas, Game 3D, Blender viewport chỉ xuất hiện trên OS Accessibility Tree dưới dạng **một khối Canvas rỗng duy nhất**. Cây trợ năng hoàn toàn "mù" trước các đối tượng bên trong canvas.
- **Phụ thuộc vào ý thức Accessibility của Developer:** Nếu ứng dụng dùng custom UI toolkit mà không gán nhãn trợ năng (`aria-label`, accessibility identifier), cây JSON trả về sẽ thiếu thuộc tính.
- **Giải pháp lai tối ưu (Hybrid Approach):** 
  - 95% tác vụ văn phòng, quản trị hệ thống, lập trình, nhập liệu form: **Dùng Agent Desktop** để đạt tốc độ 50ms và tiết kiệm tối đa token.
  - 5% tác vụ đồ họa, chỉnh sửa canvas hoặc game: **Chuyển sang Screenshot Vision fallback** khi Accessibility Tree trả về node Canvas.

---

## 6. Bài Học Hành Động Cho Kỹ Sư AI & Agentic Developers

1. **Đưa Văn Bản Vào LLM Thay Vì Đưa Hình Ảnh:**
   - LLM bản chất là công cụ xử lý ngôn ngữ. Việc cố ép LLM làm bài toán thị giác máy tính (Computer Vision) để nhận diện nút bấm là hướng đi lãng phí và độ trễ cao. Biến UI thành JSON ngữ nghĩa là cách làm đúng đắn nhất.
2. **Tách Rời Tooling Khỏi Model:**
   - Agent Desktop không chứa bất kỳ mô hình AI nào bên trong. Nó là một **công cụ thuần túy tất định (Deterministic CLI)** viết bằng Rust, có thể tích hợp vào bất kỳ agent nào (Claude Code, Antigravity, Codex, Cursor, Python CLI).
3. **Xây Dựng Phần Mềm Trước, Làm Truyền Thông Sau:**
   - Dự án được phát triển âm thầm từ tháng 2/2026, hoàn thiện bản 1.0.0, tung bản patch 1.0.2 ổn định rồi mới công bố ra công chúng vào ngày Halloween. Đây là tiêu chuẩn vàng về tính chuyên nghiệp của Vercel Labs.

---

## 7. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [Vercel CEO: Agentic Engineering Workflow (Guillermo Rauch)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/vercel_ceo_agentic_engineering_workflow_guillermo_rauch.md) — Triết lý của CEO Vercel về việc xây dựng Software Factory và hạ tầng máy ảo Sandbox cho AI Agents.
- [AWS Infinity: AI Agent Runtime Viết Bằng Rust](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Kiến trúc Rust hiệu năng cao giải phóng bộ nhớ khi chờ đợi.
- [Panda Router: Mô Hình Local 31MB Đánh Bại Cả Opus](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/tiny_local_model_outperforms_opus_orchestration_aicodeking.md) — Tách bạch giữa Phán đoán định tính trong Model và Chính sách tất định trong Code.
- [WebMCP Changes Everything (ByteGrad)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/webmcp_changes_everything_bytegrad.md) — Giao thức Model Context Protocol mở rộng cho tương tác web và desktop UI.

