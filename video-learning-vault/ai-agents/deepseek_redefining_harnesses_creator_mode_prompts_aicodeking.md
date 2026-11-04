# DeepSeek Tái Định Nghĩa Agent Harness: Giải Phẫu 7 Prompts Của Creator Mode & Công Thức Tự Sinh Plugin | AICodeKing

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=XNf8bOaHVTo)
- **Video ID:** `XNf8bOaHVTo`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-05
- **Duration:** 09:11
- **Speaker:** AICodeKing (Phân tích bộ 7 Prompts chính thức của DeepSeek Harness Creator Mode và chiến lược tối ưu chi phí với ClinePass, Cline Desktop & PandaCode)
- **Transcript File:** [deepseek_redefining_harnesses_creator_mode_prompts_aicodeking_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_redefining_harnesses_creator_mode_prompts_aicodeking_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Ứng dụng mã nguồn mở **DeepSeek Harness (DSH)** (giấy phép MIT, hiện đã vượt mốc **269,000 stars trên GitHub**, bản dựng `v0.211-rc5`) đã gây tiếng vang lớn với khẩu hiệu **"Everything is a plugin"** (Mọi thứ đều là plugin) và chế độ **Creator Mode** — nơi ứng dụng tự viết, đóng gói và cài đặt các plugin mở rộng chính nó ngay trong lúc bạn đang trò chuyện.

Trong video demo trước đó của DeepSeek, cộng đồng từng kinh ngạc khi thấy DSH tự biến mình thành phòng trưng bày 3D, trình mô phỏng mạng Transformer, trình dựng phim, hay tự thay đổi giao diện sang phong cách Pixel Art. Mới đây, DeepSeek đã chính thức công bố **trang tài liệu chứa trọn bộ 7 Prompts nguyên bản (bằng cả tiếng Anh và tiếng Trung)** đằng sau các bản demo đó.

Qua việc mổ xẻ chi tiết từng câu chữ trong 7 prompts này, **AICodeKing** đã đúc kết thành **7 bài học thực chiến** và một **Công thức 5 Phần (The 5-Part Creator Mode Prompt Formula)** để điều khiển bất kỳ tác nhân tự thân (self-modifying / plugin-building agent) nào. Đồng thời, anh chỉ ra "cái bẫy chi phí" khi phải tinh chỉnh plugin qua nhiều vòng hội thoại và cách giải quyết triệt để bằng ngăn xếp **ClinePass ($9.99/tháng) + Cline Desktop + PandaCode**.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                   DEEPSEEK HARNESS CREATOR MODE: SELF-MODIFYING ARCHITECTURE                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│   USER PROMPT (5-Part Formula)                                                              │
│   1. Where it lives (Left/Right Sidebar, Chat)   4. What to leave out (Negative rules)      │
│   2. What it does (Core functionality)           5. How it connects back to Conversation    │
│   3. Numbers & Defaults (Exact counts/titles)                                               │
│              │                                                                              │
│              ▼                                                                              │
│   ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│   │ CREATOR MODE INTROSPECTION (2 Read-Only Runtime Tools)                              │   │
│   │ - Tool 1: Liệt kê toàn bộ các plugins/modules đang nạp trong bộ nhớ ứng dụng        │   │
│   │ - Tool 2: Kiểm tra chính xác các hàm (methods) và kiểu dữ liệu (types) khả dụng     │   │
│   └──────────┬──────────────────────────────────────────────────────────────────────────┘   │
│              │                                                                              │
│              ▼                                                                              │
│   ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│   │ DYNAMIC PLUGIN BUNDLING & PERSISTENT INSTALLATION                                   │   │
│   │ - Tự viết code UI/Logic ──► Đóng gói thành Bundle ──► Cài qua Plugin Manager        │   │
│   │ - Lưu trực tiếp vào User Profile (Tồn tại bền vững ngay cả khi khởi động lại App!)  │   │
│   └─────────────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Cơ Chế Kỹ Thuật Của Creator Mode & Lời Thú Nhận Thẳng Thắn Của DeepSeek

Khác với chế độ lập trình thông thường (Standard Mode), khi chuyển sang **Creator Mode**, DeepSeek Harness cấp thêm cho tác nhân AI **2 công cụ chỉ đọc (Read-only Introspection Tools)** đặc biệt:
1. Khả năng nhìn vào bên trong ứng dụng đang chạy (live runtime) để liệt kê những gì đang được nạp.
2. Khả năng tra cứu chính xác chữ ký hàm (methods) và kiểu dữ liệu (types) nội bộ trước khi viết bất kỳ dòng code nào.

Sau khi viết xong, tác nhân đóng gói tính năng mới thành một **Plugin Bundle**, cài đặt qua **Plugin Manager** gắn liền với hồ sơ người dùng (User Profile) nên tính năng này giữ nguyên trạng thái sau khi tắt mở lại ứng dụng.

> [!WARNING]
> **Sự thật đằng sau các video demo hào nhoáng:** Ngay trên trang công bố, DeepSeek thẳng thắn ghi chú rằng nhiều plugin trong video đã được **tinh chỉnh qua nhiều vòng hội thoại liên tiếp (several rounds of conversation)**. Một prompt đơn lẻ chỉ là điểm khởi đầu (starting point), không phải "câu thần chú một phát ăn ngay".

---

## 3. Giải Phẫu 7 Prompts Chính Thức & 7 Bài Học Thiết Kế Tác Nhân

Bảng phân tích chi tiết 7 prompts mẫu của DeepSeek và nguyên tắc kỹ thuật rút ra từ từng trường hợp:

| # | Tên Plugin Demo | Nội Dung & Chi Tiết Đắt Giá Trong Prompt | Bài Học Kỹ Thuật Rút Ra (Takeaway) |
| :-: | :--- | :--- | :--- |
| **1** | **3D Gallery of Famous Landmarks** | Tạo lối vào ở **thanh bên trái (left sidebar entry)** hiển thị mô hình 3D của Tháp Eiffel, Vạn Lý Trường Thành, Tử Cấm Thành và *"thêm 1 công trình nổi tiếng nữa do model tự chọn"*. | **Bài học 1: Luôn chỉ định vị trí cư trú (Where it lives).** 6/7 prompts đều chỉ rõ vị trí (3 ở left sidebar, 2 mở từ chat, 1 ở right sidebar). Đồng thời, chừa một khoảng nhỏ có giới hạn để model sáng tạo. |
| **2** | **Transformer 3D Visualization** | Trò chơi mô phỏng 3D tương tác giải thích phép tính bên trong mạng Transformer. Có câu: *"Giữ tiêu đề bảng và thanh công cụ tránh xa các nút cửa sổ gốc của macOS để không bị đè lên nhau"*. | **Bài học 2: Vá lỗi bằng ràng buộc cụ thể (Surgical Fix).** Khi giao diện bị lỗi hiển thị ở lần chạy đầu, đừng làm lại từ đầu; hãy gọi tên chính xác lỗi đó ở tin nhắn tiếp theo và đưa thẳng vào prompt chuẩn. |
| **3** | **DSH Pixelated Interface** | Plugin thay đổi diện mạo của **toàn bộ ứng dụng** với **5 phong cách khác nhau** để lựa chọn, trong đó có phong cách Pixel Art. | **Bài học 3: Đưa ra con số cụ thể (Give it a count).** Hãy viết *"5 phong cách"* thay vì *"một vài phong cách"*. Con số cụ thể giúp cả người và máy dễ dàng kiểm chứng hơn các tính từ chung chung. |
| **4** | **Poster Design Workbench** | Mở từ cuộc trò chuyện, dùng họa tiết shader và phông chữ tiếng Anh từ trang chủ DSH. Yêu cầu: *"Giữ trang mặc định tối giản, không có tiêu đề phụ hay thông tin thừa, đặt tiêu đề mặc định là 'Everything is a plugin'"*. | **Bài học 4: Nói rõ những gì KHÔNG muốn & Giá trị mặc định.** Các mô hình LLM rất thích tự bịa thêm chữ nghĩa rườm rà (filler). Ràng buộc phủ định và giá trị mặc định sẽ chặn đứng thói quen này. |
| **5** | **Video Editing Workbench** | Giao diện tối, cho phép tải nhiều video, cắt ghép và xuất file. Mô tả: *"Khung xem trước lớn ở phía trên, thanh timeline ở phía dưới, và có thể nhập/chỉnh sửa video bằng ngôn ngữ tự nhiên trong khung chat"*. | **Bài học 5: Mô tả bố cục như Wireframe & Kết nối ngược về Chat.** Một plugin sẽ mạnh gấp bội khi không chỉ người dùng bấm chuột được, mà chính Agent trong khung chat cũng điều khiển được nó. |
| **6** | **Paper Reader** | Nằm ở **thanh bên phải (right sidebar)**, tải bài báo khoa học giữ nguyên 100% bố cục, phông chữ, hình ảnh. Khi bôi đen văn bản, hiện menu nổi gồm 3 nút: *Highlight, Thêm ghi chú, hoặc Hỏi trong chat (ngôn ngữ câu hỏi mặc định phải khớp với ngôn ngữ giao diện)*. | **Bài học 6: Đánh vần từng bước tương tác vi mô (Step-by-step micro-interactions).** Những chi tiết tinh tế như đồng bộ ngôn ngữ prompt mặc định chỉ có được khi bạn hình dung rõ luồng trải nghiệm thực tế. |
| **7** | **Conversation Statistics** | Bảng thống kê dựa trên **dữ liệu hội thoại thực tế (actual conversation data)**: Tổng token, số cuộc hội thoại, chi phí ước tính, ngân sách, biểu đồ thời gian và mức sử dụng CPU. *(Prompt duy nhất quên chỉ định vị trí cư trú)*. | **Bài học 7: Bắt buộc phải ghi chữ "Dữ liệu thật" (Real Data).** Nếu không nhấn mạnh yêu cầu đọc dữ liệu thật từ hệ thống, LLM sẽ vẽ ra một bảng Dashboard tuyệt đẹp nhưng toàn bộ con số bên trong đều là số giả (hardcoded fake numbers)! |

---

## 4. Công Thức 5 Phần Cho Một Creator Mode Prompt Hoàn Hảo

Từ 7 ví dụ trên, mọi prompt yêu cầu AI xây dựng công cụ/plugin mở rộng hệ thống đều nên tuân thủ cấu trúc 5 thành phần:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                     THE 5-PART CREATOR MODE PROMPT FORMULA                                  │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. WHERE IT LIVES (Vị trí cư trú):                                                         │
│     ➔ Thanh bên trái (Left Sidebar), Thanh bên phải (Right Sidebar), hay mở từ Khung Chat?  │
│                                                                                             │
│  2. WHAT IT DOES (Chức năng & Luồng tương tác):                                             │
│     ➔ Bố cục không gian (Trên/Dưới) và các hành động cụ thể từng bước khi người dùng click. │
│                                                                                             │
│  3. NUMBERS & DEFAULTS (Số lượng & Giá trị mặc định):                                       │
│     ➔ Số lượng tùy chọn chính xác (VD: 5 styles), tiêu đề mặc định, và yêu cầu "Real Data". │
│                                                                                             │
│  4. WHAT TO LEAVE OUT (Ràng buộc loại trừ / Negative Constraints):                          │
│     ➔ Cấm thêm subheadings thừa, cấm chèn dữ liệu mẫu giả tạo, tránh đè lên nút hệ điều hành│
│                                                                                             │
│  5. HOW IT CONNECTS BACK TO CONVERSATION (Liên kết hai chiều với khung Chat):               │
│     ➔ Cho phép gửi vùng chọn vào chat để hỏi, hoặc dùng câu lệnh chat để điều khiển plugin. │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Giải Bài Toán Chi Phí Token: Kết Hợp DSH + ClinePass + Cline Desktop + PandaCode

### 1. Cái Bẫy Chi Phí Của Creator Mode & Giải Pháp ClinePass ($9.99/tháng)
- Trong Creator Mode, mỗi lần tinh chỉnh plugin là một lần agent đọc lại runtime, viết lại code, đóng gói bundle và trao đổi qua lại. Nếu trả tiền theo từng token (Pay-per-token API), chi phí sẽ phình to rất nhanh.
- **Cách cấu hình ClinePass vào DeepSeek Harness:**
  - **ClinePass** là gói thuê bao **$9.99/tháng** của đội ngũ Cline, cung cấp hạn mức sử dụng cao gấp **2 đến 5 lần** so với trả tiền API trực tiếp cho các mô hình mở hàng đầu (như **Kimi K3.1 Pro**, **DeepSeek**, **GLM**), đồng thời cấp một API Key chuẩn OpenAI không bị khóa cứng vào ứng dụng Cline.
  - Trong DeepSeek Harness: Vào `Settings` ➔ `Models` ➔ `Add Model Provider` ➔ Chọn `Custom Model API` ➔ Dán Base URL của ClinePass, chọn giao thức `OpenAI Chat Completions` và nhập API Key.
  - **Chiến lược phân bổ:** Dùng **Kimi K3.1 Pro** để khởi tạo các plugin lớn phức tạp, sau đó chuyển sang các mô hình **Flash** cho các vòng lặp sửa lỗi giao diện nhỏ nhặt.

### 2. Phân Vai Rõ Ràng: DeepSeek Harness vs. Cline Desktop vs. PandaCode
- **DeepSeek Harness (DSH):** Dùng khi bạn muốn **ứng dụng tự xây dựng và biến đổi chính nó** (Self-modifying plugins).
- **Cline Desktop (Standalone App):** Dùng cho công việc kỹ thuật thường ngày (xây dựng web app độc lập, quản lý tác vụ định kỳ). Điểm mạnh là tính năng **Schedule** (đặt lịch chạy prompt tự động mỗi sáng để review repo) và **Import** (nhập nguyên vẹn lịch sử phiên làm việc từ Claude Code hoặc Codex sang chạy tiếp bằng model giá rẻ trên ClinePass).
- **PandaCode (Orchestrator cấp cao nhất):**
  - PandaCode có thể nạp cả **DeepSeek Harness** (thông qua gói npm `dsh` chạy trên giao thức ACP) lẫn **Cline** làm các Harnesses con.
  - Bạn có thể dùng một Architect trong PandaCode để chia việc: giao cả 7 ý tưởng plugin trên cho 7 Workers chạy song song trên DSH và Cline, sau đó nghiệm thu toàn bộ kết quả tại một giao diện duy nhất!

---

## 6. Các Hạn Chế Cần Lưu Ý (Limitations)

1. **DSH đang ở giai đoạn Developer Preview:** Các bản Release Candidate được tung ra liên tục nên cấu trúc API nội bộ có thể thay đổi giữa các bản cập nhật.
2. **Nguy cơ hỏng giao diện (UI Breakage):** Các plugin can thiệp toàn cục (như đổi skin sang Pixel Art) có thể làm vỡ bố cục ứng dụng. Bạn cần biết vị trí của **Plugin Manager** để tắt (disable) plugin đó khi cần khôi phục.
3. **Giới hạn của ClinePass:** Dù hào phóng gấp 2–5 lần giá gốc, nếu bạn treo Kimi K3.1 Pro chạy vòng lặp Creator Mode liên tục cả ngày thì vẫn sẽ chạm trần giới hạn (rate limits).

---

## 7. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [Giải Phẫu Kiến Trúc DeepSeek Harness & Cordis Framework (Sam Witteveen)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/new_deepseek_harness_super_agent_cordis_sam_witteveen.md) — Cơ chế Spatio-Temporal Composability của framework Cordis giúp DSH nạp/gỡ plugin sạch sẽ 100%.
- [9 NEW Claude Mods That Can Truly Change How You Work (Robonuggets)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/9_new_claude_mods_robonuggets.md) — Đối chiếu kiến trúc Malleable Software (Phần mềm tự biến đổi) giữa Claude Code Mods và DeepSeek Creator Mode.
- [Tự Chế Mô Hình Cục Bộ 31MB Đánh Bại Cả Opus: Panda Router (AICodeKing)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/tiny_local_model_outperforms_opus_orchestration_aicodeking.md) — Cách PandaCode điều phối đa harness và leo bậc thang tự động.
