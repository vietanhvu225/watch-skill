# Giải Phẫu Kiến Trúc DeepSeek Harness & Cordis Framework: Tại Sao "Mọi Thứ Đều Là Plugin" Là Tương Lai Của Super Agent | Sam Witteveen

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=OrcsMi2-IcM)
- **Video ID:** `OrcsMi2-IcM`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-05
- **Duration:** 17:52
- **Speaker:** Sam Witteveen (Chuyên gia AI/LLM & Đồng dẫn chương trình podcast *Beyond the Pilot* — Giải phẫu kiến trúc Cordis IoC, bảng xếp hạng Frontier Harness Eval và nguyên mẫu DeepSeek Super Agent)
- **Transcript File:** [new_deepseek_harness_super_agent_cordis_sam_witteveen_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/new_deepseek_harness_super_agent_cordis_sam_witteveen_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Khi DeepSeek công bố nguyên mẫu **Super Agent** (`dsh-superagent-prototype`) — một tác nhân tự động canh gác kho mã nguồn GitHub và ứng dụng nhắn tin Feishu, tự mở phiên làm việc riêng và trả về Pull Request hoàn chỉnh — phần lớn cộng đồng chỉ chú ý đến tính năng bề nổi. Tuy nhiên, chuyên gia **Sam Witteveen** chỉ ra rằng điều thực sự chấn động nằm ở **cách thức họ xây dựng nó**:

> **Toàn bộ hệ thống Super Agent phức tạp này được lắp ghép hoàn toàn từ một vài file cấu hình patch (`cordis.patch.yaml`) và các plugins thả vào DeepSeek Harness mà KHÔNG HỀ CHẠM VÀO LÕI (Zero Core Forking) của Harness!**

Để làm được điều đó, DeepSeek đã xây dựng Harness của mình (ra mắt ngày 13/08/2026 cùng DeepSeek V4 Pro) dựa trên **Cordis** — một framework lập trình mang tính học thuật cao của các nhà nghiên cứu thuộc **Đại học Bắc Kinh (Peking University)** về **Khả năng hợp thành Không-Thời gian (Spatio-Temporal Composability)**.

Ngoài ra, Sam Witteveen công bố số liệu từ bộ quy chuẩn **Frontier Harness Eval**: Khi giữ cố định cùng một mô hình (**Kimi K3**) và chạy qua 9 Harnesses khác nhau trên cùng 30 tác vụ, **DeepSeek Harness đạt tỷ lệ thành công ngang ngửa Claude Code (63.3%) nhưng chi phí chỉ bằng gần 1/6 ($3.28 so với $18.34 mỗi tác vụ thành công)**. Khoảng cách chi phí giữa cấu hình đắt nhất và rẻ nhất lên tới **17.5 lần** — chứng minh đanh thép rằng ở thời điểm hiện tại, **Harness quan trọng hơn cả Model**.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                TRADITIONAL MONOLITHIC HARNESS VS. DEEPSEEK CORDIS PLUGIN TREE               │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL AGENT HARNESS (Claude Code / Codex / Monolithic Core):                         │
│                                                                                             │
│     ┌────────────────────────────────────────────────────────┐                              │
│     │ MONOLITHIC CORE (Agent Loop + Model Call + Session DB) │                              │
│     └──────┬─────────────────────────────┬───────────────────┘                              │
│            ├──► [Plugin A (Side-effect)] └──► [Plugin B (Leaves residual junk on unload)]   │
│     ➔ Muốn đổi sang kiến trúc Super-Agent? Phải FORK và đập đi xây lại toàn bộ Core!        │
│                                                                                             │
│  DEEPSEEK HARNESS + CORDIS FRAMEWORK ("EVERYTHING IS A PLUGIN" LITERALLY):                  │
│                                                                                             │
│     [KHÔNG CÓ CORE CỐ ĐỊNH - MỌI THÀNH PHẦN ĐỀU LÀ NODE TRONG CÂY CORDIS]:                  │
│     ├── `plugin:agent-loop`    ├── `plugin:router`       ├── `plugin:tool-registry`         │
│     └── `plugin:session-log`   └── `plugin:github-watch` └── `plugin:feishu-watch`          │
│                                                                                             │
│     4-LAYER BOOT COMPOSITION (`dsh dump-config`):                                           │
│     [1. Base Bundles] ──► [2. Profile `cordis.patch.yaml`] ──► [3. Home Patch] ──► [4. CLI] │
│                                                                                             │
│     - Temporal Composability: Gỡ plugin là hoàn tác 100% hooks/tools/state (Zero Junk).     │
│     - Spatial Composability: Tự động bật/tắt component theo đồ thị phụ thuộc (Dependency).  │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Cuộc Tranh Luận Lớn: Model Quan Trọng Hơn Hay Harness Quan Trọng Hơn?

Trong các cuộc phỏng vấn gần đây trên podcast *Beyond the Pilot* với **CTO của Instacart** và **CTO của Box**, khi được hỏi *"Hiện nay cái gì quan trọng hơn: Mô hình (Model) hay Khung vận hành (Harness)?"*, cả hai vị CTO đều khẳng định: **Harness quan trọng hơn**.

### Bằng Chứng Thực Nghiệm: Bảng Xếp Hạng Frontier Harness Eval
Bộ benchmark **Frontier Harness Eval** đã thực hiện một thí nghiệm kiểm soát chặt chẽ:
- **Biến cố định:** Dùng duy nhất 1 mô hình (**Kimi K3**), cùng 1 môi trường runtime, giải quyết cùng **30 bài toán lập trình**.
- **Biến thay đổi:** Chạy qua **9 coding harnesses** khác nhau (tổng cộng 12 cấu hình: Codex, Claude Code, DeepSeek Harness, OpenCode, Pi,...).

| Tên Harness & Cấu Hình | Mô Hình Sử Dụng | Tỷ Lệ Giải Đúng (Pass Rate) | Chi Phí / Tác Vụ Thành Công | Nhận Xét Kinh Tế & Kỹ Thuật |
| :--- | :---: | :---: | :---: | :--- |
| **Codex CLI** | Kimi K3 | **66.7%** (20/30) | Trung bình | Đứng đầu bảng (chỉ hơn hạng 2 đúng 1 bài toán = 3.3%) |
| **DeepSeek Harness (Creator Config)**| Kimi K3 | **63.3%** (19/30) | **$3.28** | **Hiệu suất ngang ngửa Claude Code nhưng rẻ hơn 5.6 lần!** |
| **Claude Code** | Kimi K3 | **63.3%** (19/30) | **$18.34** | Ngốn token gấp 5.6 lần DSH trên cùng một model và kết quả |
| **Chênh lệch toàn bảng (Min vs Max)**| Kimi K3 | Biến thiên mạnh | **Chênh lệch 17.5x** | Harness quyết định trực tiếp tốc độ, caching, giá thành và tỉ lệ đỗ |

> [!TIP]
> **Lời khuyên cho Kỹ sư Đánh giá Mô hình (Model Evaluation):** Đừng bao giờ đánh giá một mô hình mới chỉ bằng cách chạy nó trên một Harness duy nhất. Hãy thử nó trên 2 đến 3 Harnesses khác nhau; nếu không, bạn rất dễ đổ lỗi cho mô hình kém trong khi thủ phạm thực sự là cơ chế quản lý context/cache của Harness!

---

## 3. Giải Phẫu Framework Cordis: "Khả Năng Hợp Thành Không-Thời Gian"

Trái tim bên dưới DeepSeek Harness là **Cordis** — một framework dựa trên bài báo khoa học của 3 nhà nghiên cứu tại Đại học Bắc Kinh mang tên *"A Programming Paradigm for Spatio-Temporal Composability"*.

Trong hầu hết các framework agent hiện nay, luôn có một phần lõi cứng (Core) phụ trách vòng lặp suy luận, gọi model và lưu nhật ký; các plugin chỉ là phần phụ gắn thêm ở rìa. Trong DeepSeek Harness, câu nói **"Everything is a plugin"** mang nghĩa đen tuyệt đối:
- **Bộ định tuyến (Router)** là một plugin.
- **Sổ đăng ký công cụ (Tool Registry)** là một plugin.
- **Nhật ký phiên (Session Log)** là một plugin.
- **Ngay cả Vòng lặp Tác nhân (Agent Loop)** cũng chỉ là một plugin!

### Hai Nguyên Lý Toán Học Của Cordis:
1. **Temporal Composability (Hợp thành theo Thời gian — Gỡ Bỏ Sạch Sẽ 100%):**
   - Khi bạn gỡ (unload) một thành phần lúc hệ thống đang chạy, **mọi tác động mà nó từng tạo ra đều được hoàn tác sạch sẽ (completely undone)**.
   - Nếu plugin đó từng đăng ký một tool, gắn một hook vào sự kiện hoặc sửa đổi giao diện, việc gỡ plugin sẽ đảo ngược toàn bộ các thao tác đó mà không để lại bất kỳ rác bộ nhớ (residual junk) nào.
2. **Spatial Composability (Hợp thành theo Không gian — Đồ Thị Phụ Thuộc Động):**
   - Mỗi plugin khai báo rõ những dịch vụ mà nó phụ thuộc vào. Runtime của Cordis sẽ tự động kích hoạt (activate) hoặc tạm ngưng (deactivate) các plugin một cách an toàn ngay khi các thành phần phụ thuộc xuất hiện hoặc biến mất.

### Cơ Chế Khởi Động Phân Lớp 4 Tầng (`cordis.patch.yaml`):
Khi bạn chạy DeepSeek Harness, hệ thống lắp ghép một cây cấu hình từ 4 tầng ưu tiên tăng dần:
1. **Tầng 1 (Bundles):** Các gói plugin cơ sở đi kèm cấu hình gốc.
2. **Tầng 2 (Profile Patch):** File `cordis.patch.yaml` định nghĩa chế độ chạy (4 chế độ trên UI gồm *Standard, PGC, Minimal, Creator* thực chất chỉ là 4 tập hợp plugin khác nhau!).
3. **Tầng 3 (Home-level Patch):** Cấu hình tùy chỉnh cá nhân của người dùng trên máy.
4. **Tầng 4 (CLI Arguments):** Các tham số ghi đè truyền trực tiếp từ dòng lệnh (có thể kiểm tra toàn bộ cây đang chạy bằng lệnh `dsh dump-config`).

> [!NOTE]
> **Continual Learning ở tầng Harness:** Trong khi cả thế giới đang vật lộn tìm cách giúp mô hình học liên tục (Continual Learning in weights), DeepSeek đưa khả năng học liên tục vào tầng Harness thông qua **Creator Mode**: Agent tự viết plugin mới, cài vào cây Cordis và kiểm tra nó chạy sống ngay lập tức. Đó cũng là lý do repo chính của DSH **đóng cửa không nhận Pull Request từ bên ngoài vào Core** — vì không có Core nào cần vá cả, mọi đóng góp đều được thực hiện dưới dạng Plugin!

---

## 4. Cách DeepSeek Biến Harness Thành "Super Agent" Chỉ Bằng File Patch

Khi mở kho mã nguồn `dsh-superagent-prototype`, file quan trọng nhất chính là `cordis.patch.yaml`. File này biến đổi một coding agent thông thường thành hệ thống Super Agent tự chủ bằng cách:

1. **Cắm thêm 3 Plugins vào cây khởi động:**
   - `super-agent-core` (Bộ điều phối trung tâm).
   - `github-watcher` (Trình lắng nghe sự kiện GitHub Issues/PRs).
   - `feishu-watcher` (Trình lắng nghe tin nhắn nhóm trên Feishu/Lark).
2. **Định nghĩa lại Preset bị cắt gọt (Stripped-down Preset) cho Workers:**
   - Lấy preset `Standard` nhưng **gỡ bỏ các plugins không phù hợp với chạy ngầm tự động**: Cấm hỏi lại người dùng (`no ask_user`), tắt chế độ lập kế hoạch tương tác (`no plan_mode`), tắt `web_fetch`, và cấm đẻ thêm sub-agents con (`no subagents`).
   - **Tự động đẩy lệnh chạy lâu xuống nền (>5 giây):** Bất kỳ lệnh shell nào chạy quá **5 giây** đều bị ép chuyển thành một tiến trình chạy nền (background job). Nhờ đó, một lệnh build kéo dài 10 phút sẽ **không bao giờ khóa cứng (block) Worker**, giúp Worker nhận biết ngay lập tức nếu có ai đó vừa để lại bình luận mới trên PR để đổi hướng kịp thời!

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│               AGENT TEAMS VS. SUPER AGENT & THE "TICKET + MANDATE" WORKFLOW                 │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  PHÂN BIỆT BẢN CHẤT:                                                                        │
│  - AGENT TEAMS (Claude Code / DSH Teams): Phối hợp nhiều agents BÊN TRONG MỘT công việc.    │
│  - SUPER AGENT (DSH Super-Agent): Xử lý HÀNG LOẠT công việc độc lập từ nhiều người khác     │
│    nhau gửi tới bất đồng bộ qua thời gian dài.                                              │
│                                                                                             │
│  LUỒNG XỬ LÝ SỰ KIỆN & ĐĂNG KÝ TRỰC TIẾP (DIRECT SUBSCRIPTION):                             │
│                                                                                             │
│  [Trigger: GitHub / Feishu]                                                                 │
│              │                                                                              │
│              ▼                                                                              │
│  ┌───────────────────────────────────────┐                                                  │
│  │ 1. DETERMINISTIC CODE GATE            │ ──(Không thuộc Allowlist?)──► [DROP (0 Token)]   │
│  └───────────────────┬───────────────────┘                                                  │
│                      │ (Hợp lệ)                                                             │
│                      ▼                                                                      │
│  ┌───────────────────────────────────────┐                                                  │
│  │ 2. ROUTER (Fast/Cheap Model, No Bash) │ ──(Gom lô Batching: 40 triggers/phút             │
│  │    Tạo TICKET + MANDATE               │    giảm thời gian quyết định 6.4s ➔ 1.6s!)       │
│  │    (Ai yêu cầu? Quy tắc nào? Giới hạn)│                                                  │
│  └───────────────────┬───────────────────┘                                                  │
│                      │ Giao việc lần đầu                                                    │
│                      ▼                                                                      │
│  ┌───────────────────────────────────────┐     Bình luận PR tiếp theo đi thẳng vào Worker!  │
│  │ 3. ISOLATED WORKER (Strong Model)     │◄═════════════════════════════════════════════╗   │
│  │    - Subscribe thẳng vào PR Thread    │     (BỎ QUA ROUTER: Giảm 30% quyết định của  ║   │
│  │    - Shell > 5s tự đẩy xuống nền      │      Router & tiết kiệm lượng lớn Tokens!)   ║   │
│  └───────────────────────────────────────┘                                              ║   │
│                                                                                         ║   │
│  [Người dùng bình luận thêm trên Pull Request] ═════════════════════════════════════════╝   │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Khái Niệm "Ticket + Mandate" (Ủy Nhiệm Thư) & Chế Độ Ghi Nhận Thử Nghiệm:
- Khi Router xác nhận một sự kiện là công việc thật, nó khởi tạo một **Ticket (Phiếu việc)** đi kèm một **Mandate (Ủy nhiệm thư)** ghi rõ: Ai là người yêu cầu, áp dụng bộ quy tắc nào, và giới hạn phạm vi được phép làm đến đâu.
- **Thực nghiệm tắt tính năng PR Subscription:** Khi đội ngũ DeepSeek thử tắt tính năng cho Worker đăng ký theo dõi trực tiếp PR thread (bắt mọi bình luận phải đi vòng qua Router), **số lượng quyết định mà Router phải xử lý đã phình to thêm 30%** kèm theo chi phí token tăng vọt tương ứng.
- **Chế độ tự khởi xướng an toàn (Dry-run Proactive Mode):** Khi Super Agent tự phát hiện việc cần làm mà chưa ai yêu cầu, nó khởi động ở chế độ **chỉ ghi chép lại những gì nó dự định làm (record-only)**, tự kiểm chứng xem giải pháp có hoạt động không, và chỉ lên tiếng khi đã có kết quả chắc chắn.

---

## 5. Bốn Lưu Ý & Hạn Chế Thực Tế (Caveats)

1. **Khoảng cách giữa Bài báo và Mã nguồn mở:** Đây là một nguyên mẫu nghiên cứu; mã nguồn công bố chưa hoàn toàn giống 100% bản nội bộ của DeepSeek (ví dụ: phiên Watchdog độc lập được mô tả trong bài viết vẫn chưa hiện diện đầy đủ trong repo hiện tại).
2. **Phạm vi tích hợp mặc định còn hẹp:** Hiện mới cài sẵn plugin cho 1 GitHub repo và ứng dụng Feishu (chưa có Slack hay Email mặc định), nhưng nhờ kiến trúc Cordis và tài liệu Cookbook đi kèm, cộng đồng có thể viết thêm plugin nguồn tin rất dễ dàng.
3. **Số liệu Benchmark dựa trên tải giả lập (Synthetic Workloads):** Các con số thử nghiệm hiệu năng của Super Agent được chạy trên môi trường mô phỏng đơn giản hóa, nên cần được xem là tài liệu tham khảo định hướng.
4. **Bài toán Bảo mật chưa giải quyết triệt để:** Chính DeepSeek liệt kê rõ các nguy cơ về Prompt Injection, lộ thông tin xác thực (credentials leaking) và phân quyền đa thiết bị là những bài toán mở. Người dùng nên chạy hệ thống với mô hình cục bộ hoặc trong môi trường khóa chặt hoàn toàn.

---

## 6. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [DeepSeek Tái Định Nghĩa Agent Harness: 7 Prompts Của Creator Mode (AICodeKing)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_redefining_harnesses_creator_mode_prompts_aicodeking.md) — Hướng dẫn chi tiết cách sử dụng Creator Mode để tự sinh plugin trên nền tảng Cordis.
- [Giải Mã DeepSeek Super-Agent & Tích Hợp Vào Claude Code (Rob Shocks)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_plug_into_claude_code_rob_shocks.md) — Giải pháp khắc phục lỗ hổng bảo mật của nguyên mẫu DeepSeek bằng Docker Sandbox và cổng CLI `sa`.
- [Bản Thiết Kế Hệ Thống Phân Tán Của DeepSeek Super Agent (lustoykov)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_design_grokbot_agents_lustoykov.md) — Phân tích sâu 5 khối kiến trúc khoa học máy tính (Chain of Recall, Fast/Slow Path, Full-Duplex, Stateless Daemon).

