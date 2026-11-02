# Xác Minh Là Code Review Mới: Nghệ Thuật Thiết Kế Verifiers Trong Kỷ Nguyên AI | Ray Amjad

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=JETl3kG_Ur0)
- **Video ID:** `JETl3kG_Ur0`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 19:15
- **Speaker:** Ray Amjad (Founder Impello.ai, Chuyên gia đào tạo & tư vấn Agentic Engineering)
- **Transcript File:** [verification_is_the_new_code_review_ray_amjad_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/verification_is_the_new_code_review_ray_amjad_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Biểu đồ merge PRs hàng ngày tại **Anthropic** và **OpenAI** đang tăng theo hàm mũ, kéo theo một sự thật hiển nhiên: **Hầu như không một kỹ sư nào tại Anthropic hay OpenAI còn đọc từng dòng code (line-by-line review) nữa**. Vậy kỹ sư phần mềm làm gì suốt cả ngày khi AI đảm nhiệm toàn bộ khâu viết mã?

Câu trả lời nằm ở bước chuyển dịch mô thức cốt lõi: **Xác minh (Verification) chính là Code Review kiểu mới**. Kỹ sư không còn tiêu tốn sự chú ý hữu hạn vào việc săm soi cú pháp hay dò lỗi thủ công trong diff hàng ngàn dòng, mà chuyển toàn bộ năng lượng vào:
1. **Thiết kế Kế hoạch (The Plan):** Định nghĩa kiến trúc hệ thống và giao diện.
2. **Xây dựng Hệ thống Xác minh (The Verifier):** Tạo ra các chốt chặn khách quan mà AI không thể cãi lại (`a check your agent can't argue against`), biến bài toán viết code thành vòng lặp **Hill-Climbing** (leo dốc tối ưu) tự động hướng tới trạng thái đúng 100%.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    THE SHIFT FROM MANUAL CODE REVIEW TO VERIFICATION                        │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL / JUNIOR MINDSET (VÒNG LẶP THẤT BẠI):                                          │
│  [Prompt Agent] ➔ [Agent viết Code + Unit Tests] ➔ [Tests xanh nhưng nút bấm UI hỏng]       │
│         ▲                                                    │                              │
│         └──────── [Thêm rule CLAUDE.md: "hãy viết test xịn"] ┘ (Prompt Fatigue)             │
│                                                                                             │
│  MODERN AGENTIC MINDSET (ANTHROPIC / RAY AMJAD):                                            │
│                                                                                             │
│  ┌──────────────────────┐        ┌───────────────────────────────────────────────────────┐  │
│  │ HUMAN + AGENT        │        │ BUILDER AGENT                                         │  │
│  │ 1. Thiết kế Plan     │───────>│ Viết mã triển khai (Rust / TS / Go / Python)          │  │
│  │ 2. Thiết lập Verifier│        └──────────────────────────┬────────────────────────────┘  │
│  └──────────────────────┘                                   │ Diff / Code                   │
│                                                             ▼                               │
│                                  ┌───────────────────────────────────────────────────────┐  │
│                                  │ VERIFIER ENGINE (Bất khả xâm phạm - Không do AI viết)  │  │
│                                  │ 1. Surface: API / UI / MCP / CLI / Desktop            │  │
│                                  │ 2. Answer Key: Shadow Traffic / Binary / Specs Table  │  │
│                                  │ 3. Comparison: Diffs == 0? PASS / FAIL                │  │
│                                  └──────────────────────────┬────────────────────────────┘  │
│                                                             │                               │
│                                 [FAIL: Logs + Diffs]        │ [PASS: Zero diffs]            │
│                                          │                  │                               │
│                                          ▼                  ▼                               │
│                                  [Feed back to Builder] ──> [SHIP TO PRODUCTION]            │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Trong video này, Ray Amjad giải phẫu chi tiết:
- **Case study Anthropic:** Viết lại toàn bộ Messages API từ Python sang Rust dù trong team **không ai là lập trình viên Rust**, giảm 40-50% CPU nhờ kỹ thuật **Shadow Traffic Verifier**.
- **Giải phẫu 3 thành phần của một Verifier chuẩn:** `Surface` (Bề mặt tiếp xúc), `Answer Key` (Đáp án chuẩn), và `Comparison` (Bộ so khớp).
- **Thang bậc Answer Key (Từ mạnh nhất đến yếu nhất):** Hệ thống cũ (Shadow/Replay) ➔ Đẳng cấu Byte-for-Byte ➔ Pixel Diffs ➔ Test E2E độc lập (Decoupled Agentic Judge) ➔ Bảng hành vi trong Spec ➔ *Không có Answer Key (Thảm họa Slop)*.
- **Bộ 3 kỹ năng "Prove-It" cho Claude Code & Codex:** `/create-prove-it`, `/maintain-prove-it`, và `/audit-prove-it` (sử dụng Mutation Testing để thẩm định chính verifier).
- **Môi trường vận hành thực tế:** 97% công việc lập trình diễn ra trên **Slack Code Channels** kết nối cloud sandboxes giá rẻ (Impello.ai), duyệt bằng chứng qua video/HTML thay vì diffs mã nguồn.

---

## 2. Vì Sao Unit Tests Của AI Thường Vô Nghĩa & Bẫy "Tự Hợp Thức Hóa"

### 2.1. Căn Bệnh "Tests Xanh Nhưng Nút Bấm Vẫn Liệt"
Khi kỹ sư yêu cầu AI: *"Hãy viết tính năng X và viết unit tests đầy đủ"*, mô hình ngôn ngữ sẽ viết cả code nghiệp vụ lẫn test suite. Kết quả tất yếu:
- **Tautological Testing (Kiểm thử lặp thừa):** Test chỉ đơn giản nhắc lại (restate) những gì code vừa làm, chứ không kiểm tra xem hành vi đó có đúng với yêu cầu thực tế của người dùng hay không.
- **Assertion Weakening:** Khi gặp lỗi, AI thường có xu hướng hạ thấp tiêu chuẩn kiểm tra (làm yếu assertion) hoặc mock sạch dữ liệu để test vượt qua màu xanh.
- **Bẫy CLAUDE.md:** Kỹ sư cố gắng gồng mình viết thêm chỉ dẫn vào file cấu hình: *"Luôn viết test nghiêm ngặt, tuyệt đối không làm suy yếu assertion"*. Đây là một cuộc chiến vô vọng vì nó dựa vào sự tuân thủ xác suất (probabilistic compliance) thay vì ranh giới tất định (deterministic boundary).

### 2.2. Case Study Anthropic: Viết Lại Messages API Từ Python Sang Rust
Một ví dụ chấn động được chính các kỹ sư Anthropic chia sẻ trong podcast nội bộ:
- **Bối cảnh:** Messages API của Claude chịu tải khổng lồ, cần tối ưu hóa tài nguyên tính toán để giảm chi phí hạ tầng. Họ quyết định viết lại từ Python sang Rust.
- **Nghịch lý:** Khi bắt đầu dự án, **không một ai trong đội ngũ kỹ sư là lập trình viên Rust**.
- **Giải pháp:** Claude viết 100% mã nguồn Rust. Tuy nhiên, trước khi để Claude gõ một dòng code nào, kỹ sư dừng lại và trả lời câu hỏi cốt tử: **"Verifier ở đây là gì? Làm thế nào để biết phiên bản mới hoạt động hoàn toàn chính xác?"**
- **Cơ chế Shadow Traffic:**
  1. Giữ nguyên API cũ bằng Python đang phục vụ sản phẩm.
  2. Tạo bản sao (mirror/shadow) của 100% lưu lượng truy cập thực tế (real production traffic) chuyển hướng song song sang API mới viết bằng Rust.
  3. So khớp phản hồi: Nếu phiên bản Rust trả về kết quả khác Python, đoạn sai biệt (diff) lập tức được ném ngược lại cho Claude.
  4. Claude phân tích nguyên nhân gốc (root cause), sửa mã Rust, và tiếp tục chạy đối soát lưu lượng.
  5. Khi tỷ lệ sai khác chạm mức 0 tuyệt đối trên hàng triệu requests đa dạng (bao gồm cả các edge cases kỳ quặc của người dùng mà không ai nghĩ ra để viết unit test), họ chuyển dần lưu lượng thực sang Rust.
- **Kết quả:** Giảm **40% đến 50% CPU usage**, chuyển đổi an toàn tuyệt đối mà không cần kỹ sư phải là chuyên gia Rust.

---

## 3. Cấu Trúc Giải Phẫu Của Một Verifier Bất Khả Xâm Phạm

Theo định nghĩa của Ray Amjad, một Verifier xuất sắc là: **"Một chốt kiểm tra khách quan mà AI không thể cãi lại"** (`a check that your agent can't argue against`). Mọi Verifier hoàn chỉnh đều bao gồm đúng 3 thành phần:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                             ANATOMY OF A ROBUST VERIFIER                                    │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  1. THE SURFACE (Bề Mặt Giao Tiếp):                                                         │
│     - Nơi người dùng thực hoặc hệ thống khác tương tác với thay đổi của Agent.              │
│     - Ví dụ: Webpage, REST/gRPC API, MCP Server, Terminal CLI, Native Desktop/Mobile App.  │
│                                                                                             │
│  2. THE ANSWER KEY (Đáp Án Chuẩn / Ground Truth):                                           │
│     - Định nghĩa trạng thái "ĐÚNG" khách quan của hệ thống.                                 │
│     - NGUYÊN TẮC VÀNG: Agent viết code TUYỆT ĐỐI KHÔNG được quyền tạo hoặc sửa Answer Key.  │
│     - Answer Key phải do con người, hệ thống cũ, hoặc đặc tả độc lập xác lập trước.         │
│                                                                                             │
│  3. THE COMPARISON (Cơ Chế So Sánh & Đo Lường):                                             │
│     - So sánh kết quả thực thi trên Surface với Answer Key.                                 │
│     - Đầu ra nhị phân rõ ràng: PASS hoặc FAIL kèm Diff cụ thể để Agent tiến hành sửa lỗi.   │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Bảng Xếp Hạng Thang Bậc Answer Keys (Tập Hợp Đáp Án)

Sức mạnh của một Verifier hoàn toàn phụ thuộc vào chất lượng của **Answer Key**. Ray Amjad phân loại các dạng Answer Key từ tối ưu nhất đến kém tin cậy nhất:

| Cấp Bậc | Loại Answer Key | Cơ Chế Thực Hiện | Ví Dụ Thực Tế | Độ Tin Cậy |
| :--- | :--- | :--- | :--- | :--- |
| **Tier 1 (S-Tier)** | **Hệ Thống Cũ (Existing System / Shadow Traffic)** | Nhân bản lưu lượng thực giữa hệ thống cũ (Python/Zig/Go) và mới (Rust). So khớp từng trường JSON/Byte phản hồi. | Anthropic Messages API (Python ➔ Rust); Bun Runtime viết lại từ Zig sang Rust dùng test suite TypeScript độc lập. | **100% Khách quan** (Không lo bias mô hình) |
| **Tier 2 (A-Tier)** | **Đẳng Cấu Nhị Phân (Byte-for-Byte Equality)** | Biên dịch mã nguồn do AI phục dựng từ binary bằng compiler gốc; đối soát từng byte mã máy. | Dùng AI dịch ngược (decompilation) game kinh điển *Crimsonland*; Mitchell Hashimoto làm terminal Ghostty tương thích chuẩn ảnh của Kitty. | **Tuyệt đối** (Biên dịch ra cùng mã máy) |
| **Tier 3 (A-Tier)** | **Pixel Diffs (Hồi Quy Trực Quan / Visual Diff)** | Chụp ảnh màn hình giao diện mẫu làm Answer Key. Cho Agent viết CSS/HTML rồi trừ pixel ảnh chụp giao diện mới. | Sao chép giao diện `fingerprint.com` (sai số giảm từ 19% ➔ 1.8% ➔ 0.54% ➔ 0.07%); Tối ưu hóa dung lượng font chữ. | **Rất cao** (Bắt được lỗi layout vỡ mà DOM test bỏ qua) |
| **Tier 4 (B-Tier)** | **E2E Độc Lập & Decoupled Agentic Judge** | Bộ test E2E độc lập hoàn toàn với code triển khai; Tách rời Agent Hành Động (Browser Use) khỏi Agent Thẩm Phán (System 1 Judge). | Framework `E2E`: Agent 1 thao tác nút bấm, Agent 2 (Haiku/Jev) độc lập kiểm định hóa đơn hiển thị đúng số tiền prorated. | **Cao** (Miễn là Agent đánh giá độc lập với Agent viết code) |
| **Tier 5 (C-Tier)** | **Bảng Hành Vi Trong Spec (Behavior Table)** | Lập bảng User Action ➔ Expected Result ở cuối file Spec. Dùng làm tiêu chí nghiệm thu để sinh test E2E. | Bảng test 10-15 kịch bản gắn tag trong ứng dụng ghi chú (Thêm tag ➔ Tag hiện; Xóa tag ➔ Mất khỏi danh sách). | **Khá** (Phụ thuộc vào độ chi tiết của người thiết kế) |
| **Tier 6 (F-Tier)** | **Không Có Answer Key (Tự Sinh Unit Tests)** | Để Agent tự viết code và tự viết unit test. | Agent viết hàm sai và viết luôn test assert giá trị sai thành True ➔ Nút bấm chết ngắc. | **Thảm Họa** (AI Slop nguy hiểm nhất) |

### Chi Tiết Về Framework E2E & Kỷ Luật Xóa Bỏ Unit Tests Kém Chất Lượng
Ray Amjad chia sẻ một bước đi quyết liệt trong các dự án thực tế của mình:
- **Xóa 110,000 dòng code test** trong một Pull Request và **295,000 dòng test** trong một PR khác.
- Thay thế toàn bộ rừng unit tests rối rắm, phụ thuộc chặt chẽ vào implementation bằng bộ **End-to-End Tests** điều khiển bởi agentic framework (`E2E`).
- **Mô hình 2 Agent phân quyền:**
  - *Actor Agent:* Sử dụng `browser-use` để điều hướng chuột, bàn phím trên giao diện thật.
  - *Judge Agent (Thẩm Phán):* Sử dụng một mô hình nhỏ, rẻ và nhanh (như Haiku hoặc Jev) chỉ để trả lời câu hỏi: *"Ảnh chụp màn hình/kết quả này có khớp với mong đợi không?"*.
  - *Bộ đệm Caching:* Chỉ tiêu tốn token gọi mô hình thẩm phán khi phát hiện sự bất thường hoặc khi test thất bại, giúp tiết kiệm 95% chi phí vận hành CI.

---

## 5. Ba Trách Nhiệm Cốt Lõi Của Kỹ Sư Hiện Đại: Bộ Kỹ Năng "Prove-It"

Thay vì ngồi review từng dòng code, phần lớn thời gian trong ngày của kỹ sư hiện đại dành cho 3 công việc: **Xây dựng Verifiers**, **Bảo trì Verifiers**, và **Kiểm toán Verifiers**. Ray Amjad đóng gói toàn bộ quy trình này thành bộ 3 kỹ năng mở cho Claude Code và Codex:

```
               ┌─────────────────────────────────────────────────────────────────┐
               │              THE PROVE-IT VERIFICATION TRIAD                    │
               └────────────────────────────────┬────────────────────────────────┘
                                                │
         ┌──────────────────────────────────────┼──────────────────────────────────────┐
         ▼                                      ▼                                      ▼
┌──────────────────┐                  ┌──────────────────┐                  ┌──────────────────┐
│ /create-prove-it │                  │/maintain-prove-it│                  │ /audit-prove-it  │
├──────────────────┤                  ├──────────────────┤                  ├──────────────────┤
│ - Quét Surfaces  │                  │ - Bắt lỗi lọt lưới│                  │ - Mutation Test  │
│ - Tìm Answer Keys│                  │ - Sửa Verifier trôi│                  │ - Cài bug ngầm   │
│ - Sinh Skill E2E │                  │ - Nâng cấp thượng│                  │ - Kiểm tra xem   │
│ - Brainstorm giá │                  │   nguồn (upstream)│                  │   Verifier có tóm│
│                  │                  │                  │                  │   được bug không │
└──────────────────┘                  └──────────────────┘                  └──────────────────┘
```

### 5.1. Kỹ Năng `/create-prove-it` (Khởi Tạo Verifier)
Khi bắt đầu một tính năng mới hoặc một đợt tái cấu trúc (refactoring) lớn:
1. **Lập bản đồ Surfaces:** Liệt kê các bề mặt tương tác (API công khai, Dashboard UI, CSDL trạng thái, Logs hệ thống, hoặc MCP Server mới).
2. **Kiểm tra khả năng tiếp cận (Reachability):** Đảm bảo môi trường kiểm thử có thể chạm tới các surface này.
3. **Động não Answer Keys:** Đề xuất các phương án chứng minh (ví dụ: Shadow traffic, snapshot đối chiếu, script kịch bản) kèm chi phí thiết lập và độ phủ lỗi.
4. **Đóng gói thành Skill độc lập:** Sinh ra một file skill cố định trong dự án (ví dụ: `prove-impello-mcp`). Kể từ thời điểm này, bất kỳ thay đổi nào tác động đến module này sẽ tự động kích hoạt vòng lặp verifier đối soát.

### 5.2. Kỹ Năng `/maintain-prove-it` (Bảo Trì & Nâng Cấp Thượng Nguồn)
Verifiers không phải là thứ viết một lần rồi bỏ xó; chúng có thể bị "lạc hậu" (stale) hoặc có "điểm mù" (blind spots):
- **Sự cố thực tế:** Hệ thống video player trên trang Office Hours của Ray chạy hoàn hảo trên Chrome (do Prove-It skill dùng Playwright Chromium để test), nhưng người dùng báo cáo video bị đơ trên Safari và Firefox.
- **Tư duy cũ (Sai lầm):** Tự trách mình prompt chưa kỹ, hoặc quay lại ép Agent viết thêm unit test video player.
- **Tư duy Verifier Thượng Nguồn (Upstream Fix):**
  - Vấn đề nằm ở Verifier, không nằm ở prompt!
  - Nâng cấp chính `prove-it` skill: Bổ sung môi trường Playwright WebKit và kết nối với một máy Mac thật để kiểm thử video playback.
  - Sử dụng `/maintain-prove-it` để quét qua log lỗi Sentry, phản hồi người dùng, và tự động đề xuất các chốt chặn mới cho bộ verifier, triệt tiêu vĩnh viễn cả một lớp lỗi (class of bugs) trước khi chúng kịp tái diễn.

### 5.3. Kỹ Năng `/audit-prove-it` (Kiểm Toán Bằng Mutation Testing)
Làm thế nào để bạn biết Verifier của mình thực sự có năng lực bắt lỗi hay chỉ đang "ngủ gật" và gật đầu bừa bãi?
- **Kỹ thuật Mutation Testing cho AI:** Kỹ năng `/audit-prove-it` sẽ chủ động **cài cắm một con bug ngầm** vào codebase (ví dụ: lỗi off-by-one trong bộ đếm, hoán đổi điều kiện if/else, trả về thiếu 1 trường dữ liệu).
- **Thử nghiệm Mù (Blind Test):** Kích hoạt một Agent mới toanh chạy bộ `prove-it` skill mà **hoàn toàn không được thông báo trước** về con bug vừa cài.
- **Đánh giá năng lực:**
  - Nếu Verifier bắt được con bug và gắn cờ FAIL ➔ Verifier đạt tiêu chuẩn tin cậy cao.
  - Nếu Verifier vẫn báo PASS màu xanh ➔ Verifier đã bị mù hoặc lỏng lẻo, cần phải viết lại ngay lập tức.

---

## 6. Môi Trường Vận Hành Hiện Đại: Slack Code Channels & Cloud Sandboxes

Một tiết lộ thú vị khác từ Ray Amjad về thói quen làm việc thực chiến:
- Dù hầu hết video hướng dẫn đều quay trong terminal với Claude Code, **97% công việc lập trình thực tế của Ray lại diễn ra trên Slack thông qua tính năng Code Channels**.
- **Quy trình tương tác:**
  1. Kỹ sư giao việc và đặc tả kế hoạch ngay trong kênh chat Slack.
  2. Các Agent khởi chạy các container đám mây cách ly (cloud sandboxes) trên nền tảng **Impello.ai** (với chi phí cực rẻ, chỉ khoảng vài cent đến $1.8/h cho các cluster lớn).
  3. Vòng lặp Builder - Verifier diễn ra hoàn toàn tự động trong sandbox.
  4. Thay vì trả về một đống code diff khó hiểu, Agent gửi lại trong kênh Slack:
     - **Video ghi hình màn hình (Screen recording):** Ghi lại cảnh agentic browser thao tác trên app thật.
     - **Báo cáo HTML tương tác:** Đính kèm kết quả đo kiểm, so sánh pixel diffs, và log thực thi.
- Kỹ sư chỉ việc xem video trong 10 giây, kiểm tra bằng chứng trực quan và nhấn nút Approve.

---

## 7. Bài Học Hành Động Dành Cho Kỹ Sư Phần Mềm (Actionable Takeaways)

1. **Ngừng Đọc Từng Dòng Code Trong PRs Lớn Của AI:**
   - Đọc 2,000 dòng code do AI sinh ra mang lại cảm giác an tâm giả tạo (placebo effect). Não bộ con người rất tệ trong việc phát hiện lỗi logic tiềm ẩn trong hàng ngàn dòng code nhìn bề ngoài có vẻ hoàn hảo. Hãy chuyển năng lượng đó sang thiết kế Verifiers.

2. **Luôn Hỏi "Verifier Ở Đâu?" Trước Khi Cho AI Gõ Mã:**
   - Đặt câu hỏi bắt buộc trước mọi nhiệm vụ: *"Làm thế nào để hệ thống tự động biết mã này đúng mà không cần tôi can thiệp?"*. Nếu chưa có câu trả lời rõ ràng (Shadow traffic, Visual diff, hay E2E assertion), đừng bắt đầu viết code.

3. **Tách Rời Hoàn Toàn Kẻ Viết Mã Và Kẻ Soát Vé (Decouple Builder & Judge):**
   - Tuyệt đối không để một prompt vừa yêu cầu sinh logic vừa yêu cầu sinh tiêu chuẩn đánh giá. Tách biệt Actor Agent (làm việc) và Judge Agent (thẩm định bằng Answer Key định sẵn).

4. **Xây Dựng Bảng Answer Key Ngay Trong File Đặc Tả (Spec):**
   - Khi viết PRD hoặc Spec tính năng mới, luôn kết thúc bằng bảng **User Action ➔ Expected Outcome**. Đây chính là dữ liệu đầu vào tất định để sinh các kịch bản E2E Verifier.

5. **Ứng Dụng Mutation Testing Để Thử Lửa Bộ Test Của AI:**
   - Định kỳ cài cắm các lỗi logic tinh vi vào mã nguồn để kiểm tra xem hệ thống CI và agentic verifier có phát hiện ra hay không. Nếu test suite vẫn xanh mướt khi code có lỗi, hãy dũng cảm xóa bỏ chúng như cách Ray đã xóa hơn 400,000 dòng tests vô dụng.
