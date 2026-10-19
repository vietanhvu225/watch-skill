# Thiết Lập AI Coding Mới: Quy Trình Vận Hành Hơn 30 PRs Mỗi Ngày | Your Average Tech Bro

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=54D1hFfaju8)
- **Video ID:** `54D1hFfaju8`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 20:46
- **Speaker:** Your Average Tech Bro (Serial App Builder, Đồng sáng lập Yorby.ai — ~$10k MRR)
- **Transcript File:** [my_new_ai_coding_setup_ship_30_prs_a_day_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong video thực chiến này, **Your Average Tech Bro** (nhà sáng lập đã xây dựng 14 ứng dụng trong 5 năm, hiện vận hành SaaS Yorby.ai đạt doanh thu gần $10,000 MRR với đội ngũ siêu tinh gọn chỉ 2 người) giải mã toàn bộ ngăn xếp AI coding hiện đại giúp anh phát hành hơn **30 Pull Requests (PRs) chất lượng cao mỗi ngày**.

Tác giả đã từ bỏ thiết lập cũ (Neovim + Claude Code) để chuyển sang mô hình kết hợp thực dụng **Dual-Driver ($400+/tháng)**:
1. **OpenAI Codex Desktop App ($200/tháng):** Chuyên trách tinh chỉnh Frontend cục bộ (Local Fine-tuning), hỗ trợ chuyển đổi viewports đa kích thước thông qua device toolbar mà không làm sập cửa sổ chat, tận dụng công cụ visual annotation mượt mà không độ trễ.
2. **Cursor Cloud Agents + Pstack ($200/tháng):** Chuyên trách thực thi tác vụ phức tạp trên máy ảo đám mây (Cloud VM Desktop). Kết hợp kỹ nghệ Pstack (của Lauren Tan) để chạy **Multi-model planning**, phân việc cho mô hình giá rẻ, và đặc biệt là hệ thống kỹ năng xác minh tự động (**Verification Skills**) cung cấp ảnh chụp màn hình (screenshots) và video phát lại (playbacks) làm bằng chứng nghiệm thu (receipts).
3. **Mô hình "Bot Đồng Nghiệp" (GrokBot Teammates / Feature Owners):** Không coi AI là chatbot vô danh, tác giả tạo các bot GrokBot đóng vai trò **Tech Lead / Staff Engineer** sở hữu độc quyền từng bề mặt sản phẩm (UGC Studio, Account Spy, Analytics...). Mỗi khi PR được merge, bot tự cập nhật knowledge base và duy trì bộ kỹ năng kiểm thử của module đó.
4. **Vòng lặp tự động hóa vận hành 24/7:** Bot PostHog On-Call tự động phân loại lỗi từ môi trường production, chuyển tiếp cho bot sở hữu module sửa đổi, và được cấp quyền **Auto-merge PR trong đêm** đối với sự cố diện rộng cấp bách (P0) nhờ niềm tin tuyệt đối vào hệ thống Verification Skills.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    THE 30+ PRS/DAY DUAL-DRIVER & TEAMMATE ARCHITECTURE                     │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                               HUMAN FOUNDER / ARCHITECT                                     │
│                  (Invests 10-15m upfront writing detailed ticket & spec)                    │
│                                           │                                                 │
│                    ┌──────────────────────┴──────────────────────┐                          │
│                    ▼                                             ▼                          │
│      [ LOCAL FINE-TUNING LANE ]                    [ CLOUD AUTONOMOUS LANE ]                │
│       Codex Desktop App ($200/mo)                   Cursor Cloud Agents ($200/mo)           │
│   - Pure Frontend UI/UX adjustments             - Cloud VM Full Desktop Environment        │
│   - Multi-viewport device toolbar               - Pstack Framework Integration              │
│   - Visual annotation & pixel tweaks            - Multi-Model Planning + Judge Orchestrator │
│   - Instant hot-reload responsiveness           - Cost-effective implementation workers     │
│                    │                                             │                          │
│                    └──────────────────────┬──────────────────────┘                          │
│                                           ▼                                                 │
│                            [ VERIFICATION SKILLS LAYER ]                                    │
│                 - Reusable test specs: `verify-remotion`, `verify-resend`                   │
│                 - Browser automation inside Cloud VM                                        │
│                 - Visual receipts: Screenshots & Video Playback                             │
│                                           │                                                 │
│                    ┌──────────────────────┴──────────────────────┐                          │
│                    ▼                                             ▼                          │
│     [ GROKBOT TEAMMATES / OWNERS ]                 [ 24/7 PRODUCTION ON-CALL ]              │
│   Dedicated Tech Leads per Surface:             - PostHog Error Stream Ingestion            │
│   - UGC Studio Lead Bot                         - Automated Surface Triaging                │
│   - Account Spy Lead Bot                        - Route to Owner Bot or Misc Fixer          │
│   - Analytics & Calendar Lead Bots              - Autonomous P0 Auto-Merge (High Trust)     │
│   * On Merge: Auto-update Knowledge Base & Verifiers                                        │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu Ngăn Xếp Dual-Driver: Codex Desktop vs. Cursor Cloud Agents

Tác giả đầu tư hơn $400 mỗi tháng cho công cụ lập trình AI và phân định ranh giới nghiêm ngặt giữa hai nền tảng:

| Tiêu chí | OpenAI Codex Desktop App ($200/tháng) | Cursor Cloud Agents + Pstack ($200/tháng) |
| :--- | :--- | :--- |
| **Môi trường thực thi** | Local Machine (Laptop), Local Dev Server | Đám mây (Dedicated Cloud Virtual Machine) |
| **Thế mạnh chủ đạo** | Frontend, tinh chỉnh UI/UX, vi chỉnh pixel | Backend, Fullstack feature, Logic phức tạp, E2E flow |
| **Trải nghiệm giao diện** | Tách biệt Device Toolbar & Chat Window, không lag khi mở nhiều Git worktrees | Máy ảo chạy Linux/Desktop đầy đủ, hỗ trợ browser headless/headed |
| **Tốc độ phản hồi** | Tức thì (Instant), annotation hình ảnh mượt | Chạy ngầm không đồng bộ (Asynchronous execution) |
| **Độ tin cậy** | Con người trực tiếp quan sát hot-reload | Agent tự chạy, tự click browser, tự gửi ảnh chụp màn hình nghiệm thu |

### Trải Nghiệm "Device Toolbar" Giải Quyết Nỗi Đau Frontend
- Trên các môi trường chat thông thường, khi chuyển kích thước trình duyệt sang full-screen hoặc co kéo để test giao diện điện thoại (responsive), khung hội thoại lịch sử thường bị thu nhỏ thành nút pill hoặc biến mất, gây gián đoạn mạch tư duy.
- Codex Desktop cho phép ghim device toolbar để thay đổi độ phân giải màn hình kiểm thử liên tục trong khi lịch sử chat và bảng chú thích hình ảnh (Visual Annotation Tool) vẫn mở song song toàn thời gian.

---

## 3. Thực Chiến Pstack: Multi-Model Planning & Kỹ Năng Xác Minh (Verification Skills)

Tác giả áp dụng bộ khung **Pstack** (do Lauren Tan từ nhóm SpaceX AI / xAI GrokBot phát triển) bên trong Cursor Cloud Agents, tận dụng 2 trụ cột cốt lõi:

### Trụ Cột 1: Multi-Model Planning + Judge Orchestrator
1. **Khảo sát đa hướng:** Khi nhận yêu cầu tính năng mới, hệ thống kích hoạt nhiều mô hình độc lập (ví dụ Claude, Grok, GPT) để mỗi mô hình đề xuất một phương án kiến trúc khác nhau.
2. **Tổng hợp thẩm định (Judge Orchestrator):** Một mô hình cấp cao (High-Intelligence Orchestrator) đóng vai trò thẩm phán, đối chiếu ưu/nhược điểm của từng phương án, trích xuất các thành phần tối ưu nhất để kết tinh thành một Kế hoạch Thực thi Duy nhất (Master Plan).
3. **Thực thi giá rẻ:** Quá trình viết code thô (raw code implementation) được bàn giao cho các mô hình kinh tế hơn (cheaper models), giúp giảm thiểu chi phí token mà vẫn bảo toàn độ chặt chẽ của kiến trúc.

### Trụ Cột 2: Verification Skills — Chìa Khóa Cho Phép Kỹ Sư "Ngừng Đọc Từng Dòng Code"
Theo tác giả, bước ngoặt lớn nhất để nâng tầm từ lập trình viên AI thông thường lên kỹ sư AI năng suất cao (10x AI Engineer) là **sự dũng cảm từ bỏ việc đọc từng dòng mã** (`let go of reading all the code`). Tuy nhiên, việc không đọc code chỉ an toàn khi có **Verification Skills**:

- Agent không được phép dừng lại ở câu lệnh suông *"Tôi đã viết xong code rồi"*.
- Agent phải tự mở trình duyệt bên trong Cloud VM, thao tác click qua các luồng thực tế, chụp ảnh màn hình hoặc quay lại video phát lại (playback) để đính kèm vào PR như một biên lai xác nhận (receipts).
- **Kỹ năng tái sử dụng (Reusable Verification Skills):**
  - `create verification skill`: Khi xây dựng tính năng mới, agent có nhiệm vụ ghi lại tài liệu hướng dẫn cách kiểm thử bề mặt đó.
  - `maintain verification skill`: Khi tính năng thay đổi, kỹ năng kiểm thử tương ứng được cập nhật.
  - Dự án Yorby duy trì danh mục kỹ năng kiểm thử chuyên biệt: `verify-remotion-lambda`, `verify-resend`, `verify-tiktok-connect`, `verify-yorbi-web`...

---

## 4. Mô Hình "GrokBot Teammates": Biến AI Thành Tech Lead Từng Module

Thay vì dùng một agent tổng quát cho toàn bộ dự án khổng lồ, tác giả phân chia quyền sở hữu module sản phẩm tương tự một cấu trúc phòng ban kỹ thuật:

```
                      [ GITHUB REPOSITORY / PRODUCT ]
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
[ UGC Studio Bot ]           [ Account Spy Bot ]         [ Analytics Bot ]
- Quản lý tạo video AI       - Quản lý theo dõi đối thủ  - Quản lý số liệu & tracking
- Tech Lead module           - Tech Lead module          - Tech Lead module
- Nắm toàn bộ context        - Nắm toàn bộ context       - Nắm toàn bộ context
         │                           │                           │
         └───────────────────────────┼───────────────────────────┘
                                     ▼
                      [ KNOWLEDGE & VERIFICATION RETENTION ]
                      1. Cập nhật context khi merge PR
                      2. Duy trì verification skills cho module
                      3. Phối hợp chéo qua Group Chat giữa các Bot
```

### Quy Trình Sau Khi Merge PR (Post-Merge Protocol)
Mỗi khi một Pull Request được chấp thuận và tích hợp vào nhánh chính, Tech Lead Bot của module đó phải thực thi 2 nhiệm vụ bắt buộc:
1. **Cập nhật Knowledge Base:** Đọc lại toàn bộ diff code mới merge để làm mới bộ nhớ dài hạn về kiến trúc hiện tại của module.
2. **Cập nhật Verification Skills:** Đảm bảo kịch bản kiểm thử tự động của module bao quát cả các tính năng mới vừa bổ sung, sẵn sàng hướng dẫn cho các agent con sau này.

---

## 5. Vận Hành Tự Động Hóa Toàn Diện: Customer Support & 24/7 On-Call Bot

Ngoài việc code sản phẩm, tác giả thiết lập các agent chạy ngầm để bảo đảm vận hành 24/7/365 cho startup:

### 1. Bot Hỗ Trợ Khách Hàng (Customer Support Agent)
- Thay thế hoàn toàn nền tảng Intercom đắt đỏ bằng luồng xử lý email trực tiếp qua bot.
- Bot giám sát hòm thư hỗ trợ, xử lý tự động các trường hợp nằm trong tập hướng dẫn (instructions).
- Nếu phát sinh tình huống lạ, bot kích hoạt Human-in-the-loop (gửi thông báo cho 2 nhà sáng lập can thiệp). Sau khi nhà sáng lập trả lời, giải pháp mới được tự động nạp ngược lại vào playbook của bot.

### 2. Bot Trực Lỗi 24/7 Kết Nối PostHog (On-Call Error Fixing Bot)
- Tích hợp trực tiếp luồng bắt lỗi theo thời gian thực từ **PostHog**.
- Khi có lỗi phát sinh trong môi trường production:
  - Bot phân tích stack trace và phân loại (triage) xem lỗi thuộc bề mặt module nào (UGC Studio, Account Spy...).
  - Chuyển giao gói thông tin lỗi cho Bot Tech Lead sở hữu module đó để tiến hành vá lỗi trên Cloud Agent.
  - **Chính sách Auto-Merge lúc nửa đêm:** Đối với các lỗi nghiêm trọng diện rộng (P0 incident) ảnh hưởng đến toàn bộ người dùng, bot được cấp quyền tự động merge PR vá lỗi mà không cần chờ con người thức dậy phê duyệt, bởi vì mã nguồn đã phải vượt qua các bài kiểm tra visual verification nghiêm ngặt trước khi merge.

---

## 6. Kỹ Nghệ "Upfront Engineering": Chuyển Trọng Tâm Vào Bản Đặc Tả Ban Đầu

Tác giả chỉ ra rằng khi chuyển sang quy trình Agentic Engineering, thời gian gõ code biến mất, nhưng thời gian tư duy không hề giảm mà dịch chuyển lên giai đoạn đầu vào:

```
[ QUY TRÌNH TRUYỀN THỐNG ]
Nghĩ ý tưởng sơ sài (2m) ────► Bắt đầu gõ code, vừa gõ vừa sửa, debug thủ công (3 - 5 giờ)

[ QUY TRÌNH AGENTIC HIỆN ĐẠI ]
Kỹ nghệ Upfront Spec (10 - 15m) ────► Cloud Agent One-shot, Chạy Test, Chụp ảnh Proof (5m) ────► Review nhanh & Merge (2m)
```

- **Một bản prompt / ticket chuẩn chỉnh bao gồm:**
  1. Mô tả chính xác hành vi tính năng cần xây dựng.
  2. Ranh giới bề mặt tệp tin được phép tác động (Boundaries & Blast Radius).
  3. Danh sách các kịch bản kiểm thử bắt buộc phải pass (`Verification Scenarios`).
  4. Các kịch bản hồi quy (Regressions) cần kiểm chứng để không làm hỏng tính năng cũ.

---

## 7. Bài Học Đúc Kết & Hướng Dẫn Hành Động

1. **Đầu tư vào Verifiers quan trọng hơn đầu tư vào Prompts:** Một agent dù thông minh đến đâu cũng sẽ tạo ra ảo giác nếu không có công cụ tự kiểm chứng trực quan trên giao diện thực tế.
2. **Cấu trúc phân quyền Module:** Tránh nạp toàn bộ codebase vào một cửa sổ ngữ cảnh khổng lồ; hãy chia nhỏ thành các Bot chuyên trách bề mặt tính năng.
3. **Thực dụng về chi phí:** Đầu tư $400/tháng cho bộ đôi công cụ frontier (Codex + Cursor) mang lại giá trị tương đương một đội ngũ kỹ sư 3-4 người cho một startup tinh gọn.
4. **Biên lai thực chứng (Receipts):** Luôn yêu cầu agent xuất trình bằng chứng thực thi (ảnh chụp giao diện, logs kiểm thử, video playback) trước khi xem xét merge code.
