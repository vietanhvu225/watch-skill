# Claude Code: The Complete AI-Native SDLC Guide

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=6CaQ9ZFuuKI)
- **Watch Skill ID:** `d1629c49a7a466a2`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-23
- **Duration:** 21:02 (1262.1s)
- **Speaker:** Eric Tech (YouTube channel: Eric Tech)
- **Source Material:** Anthropic Claude Academy — *"The AI-Native SDLC Playbook"* (Boris Tane, Head of Claude Code)

---

## 📌 Tổng quan: Tái định nghĩa toàn diện SDLC trong kỷ nguyên Agentic

Video này phân tích chuyên sâu khóa học chính thức từ Anthropic: **"The AI-Native SDLC Playbook"** của Boris Tane (Head of Claude Code). Luận điểm nền tảng của toàn bộ tài liệu:

> *"Code không còn là bottleneck của phát triển phần mềm. AI có khả năng viết hàng ngàn dòng code trong vài chục giây. Điểm nghẽn thực sự đã dịch chuyển sang: (1) Độ rõ ràng của ý định ban đầu (Intent), (2) Khả năng kiểm định tự động (Verification & Evals), và (3) Các vòng lặp phản hồi khép kín (Closed feedback loops)."*

### So sánh Traditional SDLC vs. AI-Native SDLC

```
[TRUYỀN THỐNG: Tuyến tính, Tốc độ con người ở mọi khâu]
Plan (Họp, PRD dài) ➔ Design (Spec tĩnh) ➔ Build (LÂU NHẤT, Con người gõ code) ➔ Test (QA thủ công) ➔ Deploy (Chậm) ➔ Maintain (Bị động)

[AI-NATIVE SDLC: Vòng lặp đóng (Closed Loop), Tốc độ Agent ở mọi khâu có guardrails]
   ┌────────────────────────────────────────────────────────┐
   ▼                                                        │
[1. Plan] ──────► [2. Design] ─────► [3. Build] ──┐         │ (Loop back
intent.md          spec.md           plan.md      │         │  from metrics)
(Human Owned)     + Skills          CLAUDE.md     │         │
                                    (Agent Speed) │         │
                                                  ▼         │
[6. Maintain] ◄── [5. Deploy] ◄───── [4. Test] ◄──┘         │
bands.yaml         REVIEW.md         Continuous             │
SPC / Metrics      Headless CI       Evals & TDD            │
   │                                                        │
   └────────────────────────────────────────────────────────┘
```

| Giai đoạn | Traditional SDLC | AI-Native SDLC (Claude Code) | Artifact chuẩn |
| :--- | :--- | :--- | :--- |
| **Plan** | Họp hội đồng, viết PRD hàng chục trang mất vài tuần | Phỏng vấn chuyên sâu trực tiếp (`/grill-me`), nén vào 1 file súc tích | `intent.md` |
| **Design** | Analyst viết spec rời rạc, designer parse, dev đoán mò | Nén vào 1 working session có hướng dẫn từ **Skills** đã version trong Git | `spec.md` |
| **Build** | Lập trình viên viết code và test thủ công, docs viết sau | Code & tests do AI tạo ra; tri thức tổ chức duy trì qua file markdown máy đọc được | `plan.md`, `CLAUDE.md` |
| **Test** | QA gate thủ công cuối sprint, chạy test hồi quy chậm | TDD tự động + Continuous Evals trong CI (20–50 task thực tế) | Evals, Vitest/Jest, RTL |
| **Deploy** | Review PR dàn trải, deploy thủ công, debug log bằng tay | Multi-layer Review (Phân tầng rõ ràng) + Headless Claude tự fix build | `REVIEW.md`, CI Actions |
| **Maintain** | Chờ user báo lỗi, phản ứng thụ động với ticket | Theo dõi thống kê (SPC), tự động chẩn đoán và mở PR hoặc trigger `intent.md` mới | `bands.yaml` |

---

## 🧭 Ma trận Phân quyền & Sở hữu (Who Owns Each Step)

Sơ đồ trách nhiệm được hiển thị trực tiếp trong khóa học của Anthropic:

```
+--------------------------------------------------------------+
| Giai đoạn    | Chủ sở hữu (Owner) | Vai trò của Claude       |
+--------------------------------------------------------------+
| Plan         | 👤 Con người       | Trợ lý / Phỏng vấn viên  |
| Design       | 👤 Con người + AI  | Đồng thiết kế / So khớp  |
| Build        | 🤖 Claude (Agent)  | Thực thi độc lập (Agent) |
| Test         | 🤖 Claude (Agent)  | Tự chạy test & Verify    |
| Deploy       | 👤 Con người Gate  | Headless CI / Auto PR    |
| Maintain     | 🤖 Claude (Agent)  | Quan sát metrics & đề xuất|
+--------------------------------------------------------------+
```

---

## 1. Planning Phase — `intent.md` & Phỏng vấn không khoan nhượng

### Cấu trúc bắt buộc của `intent.md`
Anthropic quy định `intent.md` phải tuyệt đối chuẩn hóa, bao gồm 2 dòng header và đúng 5 đề mục cấp 2 (Level-2 headings), không được thay đổi thứ tự:

```markdown
# Intent: <Tên tính năng hoặc thay đổi>
Author: <Tên người viết> (<Team>). Status: draft.

## Problem
Mô tả chính xác nỗi đau của người dùng/hệ thống. Định lượng nếu có thể 
(ví dụ: "30% thời lượng cuộc gọi hỗ trợ chỉ để hỏi trạng thái hồ sơ").

## Proposed outcome
Kết quả mong đợi khi hoàn thành, đứng từ góc độ người dùng hoặc hệ thống
(ví dụ: "Khách hàng có thể tự xem trạng thái, bước tiếp theo và ngày dự kiến trên Portal").

## Affected users and systems
Danh sách cụ thể các bên liên quan: Claims handlers, Portal team, Claims-core API...

## Constraints
Các ràng buộc kỹ thuật và chính sách không được vi phạm 
(ví dụ: "Không được lưu thêm PII vào session, dùng hệ thống Authentication hiện có").

## Open questions
Những điểm chưa rõ cần làm rõ với stakeholder 
(ví dụ: "Bên thẩm định thứ ba (third-party adjusters) có cần quyền truy cập không?").
```

### Nguyên tắc vàng khi tạo `intent.md`
- **Quy tắc "Không bịa đặt" (Do not invent):** Prompt hướng dẫn Claude: *"Chỉ ghi những gì tôi đã nói. Bất cứ điều gì tôi chưa nói rõ, hãy đưa vào mục `Open questions`. Tuyệt đối không tự suy diễn."*
- **Kỹ năng phỏng vấn (`/grill-me`):** Thay vì để con người ngồi nghĩ PRD, Claude đóng vai trò phỏng vấn viên không khoan nhượng (relentless interviewer) — chất vấn từng nhánh quyết định, đào sâu logic nghiệp vụ cho đến khi đạt được sự thống nhất hoàn toàn.
- **Tổ chức thư mục:** Lưu tại `intent/YYYY-MM-DD-<ten-thay-doi>/intent.md` (mỗi thay đổi một thư mục riêng để lưu trữ audit trail qua Git).
- **Nguyên lý một nguồn chân lý (Single Source of Truth):** Chọn **Repository Git** HOẶC **Issue Tracker** (Jira/Linear). Không dùng cả hai cùng lúc để tránh phân mảnh ngữ cảnh (*"Pick one. Not both"*).

---

## 2. Design Phase — `spec.md` + Kỹ năng kiến trúc (Skills)

Giai đoạn Design chuyển hóa mục tiêu kinh doanh (`why` trong `intent.md`) thành đặc tả kỹ thuật chi tiết (`what` và `how` trong `spec.md`).

### Prompt chuẩn của Anthropic biến `intent.md` thành `spec.md`
```text
Read the attached intent.md and produce a requirements and design spec for integrating it into 
our existing codebase. Apply the skills available to you so the plan conforms to our brand 
guidelines, security policies and UX standards. Document the spec fully as spec.md, ready to 
hand to the engineering team. Describe clearly any areas of concern, especially where you 
cannot satisfy contradicting policies.
```

### Ứng dụng Skills & Thiết kế Lát cắt Dọc (Vertical Slices)
Trong video, Eric Tech sử dụng bộ kỹ năng từ `mattpocock/skills` và `superpowers`:
- `to-tickets`: Chuyển đổi spec thành các đầu việc dạng **Tracer Bullet / Vertical Slices**.
  - **Mỗi lát cắt đi qua toàn bộ các tầng:** Schema ➔ API Endpoint ➔ UI Components ➔ Automated Tests.
  - **Tránh lát cắt ngang (Horizontal slices):** Không viết toàn bộ database schema trước rồi mới làm backend/frontend. Mỗi slice phải có khả năng demo hoặc verify độc lập.
- `writing-plans`: Tạo kế hoạch thực thi chi tiết, đánh dấu rõ ràng file nào tạo mới (`NEW`), sửa (`MODIFY`), xóa (`DELETE`).

---

## 3. Build Phase — `plan.md`, `CLAUDE.md` & TDD Loop

### Dạy Claude hiểu thế nào là "Green" (Dấu ngoặc kép quyết định)
Để Claude tự chủ hoàn toàn trong việc build và fix code, file `CLAUDE.md` trong root repo phải định nghĩa chính xác tiêu chuẩn nghiệm thu:

```markdown
## Verifying your work
make test
(finishes with "0 failed")

make lint
(finishes with no output)

make build
(finishes with "Build succeeded")

Do not report done until all three are green.
```

> **Insight then chốt từ video:** Phần trong ngoặc đơn chính là phần quan trọng nhất! Nếu chỉ đưa ra lệnh `make test` mà không kèm chuỗi ký tự kết quả kỳ vọng, agent sẽ tự suy diễn xem như thế nào là đạt (ví dụ: thấy in ra log là tưởng thành công). Chuỗi kết quả rõ ràng biến terminal thành máy chấm điểm nhị phân.

### Cấu trúc `plan.md` (Plan Mode của Claude Code)
Trước khi gõ code, Claude Code tự động hoặc được yêu cầu kích hoạt Plan Mode để tạo `plan.md`:
```markdown
# Plan: claims status self-service (from intent.md 2026-06-02)

## Files that change
- portal/src/claims/StatusPanel.tsx (new)
- claims-api/routes/status.py (modify)
- claims-api/tests/test_status.py (new)

## Order of work
1. Add the status endpoint behind existing auth.
2. Panel against the endpoint.
3. Wire into the portal nav.
```

### Vòng lặp phản hồi TDD (Give Claude a Feedback Loop)
```
       ┌────────────────────────┐
       ▼                        │
[Viết Test trước] ──► [Chạy Kiểm tra]
                             │
                      Pass toàn bộ?
                     /            \
                 [KHÔNG]          [CÓ]
                    │               │
             [Claude Tự Fix]  [Commit & Push]
                    │
                    └───────────────┘
```

---

## 4. Test Phase — Continuous Evals trong CI

Khóa học phân biệt rõ ràng giữa **Unit Test thông thường** và **Continuous Evals**:

| Đặc điểm | Unit / Integration Tests | Continuous Evals |
| :--- | :--- | :--- |
| **Bảo vệ cái gì?** | Tính đúng đắn của code logic hiện tại | Năng lực thực thi nhiệm vụ của AI Agent |
| **Khi nào chạy?** | Khi code trong codebase thay đổi | Khi đổi Model, sửa System Prompt, cập nhật Rules/Skills |
| **Công cụ** | Vitest, Jest, React Testing Library | Test harness với 20–50 task thực tế đã từng giải quyết |

### Nguyên lý thiết kế Evals từ Boris Tane
- *"An eval is a task you already solved"* (Một bài test eval phải là một bài toán bạn đã từng giải quyết thành công trong thực tế).
- *"Collect them. Do not invent them. A task you never solved cannot tell you anything."* (Thu thập từ bug thực tế, ticket đã đóng. Không tự tưởng tượng ra đề bài).
- Bộ đề chuẩn: 20 đến 50 tasks (ví dụ: `fix tenant cache key`, `add status endpoint`, `rename config flag`).
- Khi nâng cấp model từ Claude 3.5 Sonnet lên Sonnet 4 hoặc sửa prompt: Chạy lại toàn bộ bộ 50 tasks. Nếu tỷ lệ pass giảm từ 95% xuống 85%, agent đã bị thoái lui (regression).

---

## 5. Deploy Phase — `REVIEW.md`, Hooks & Headless CI

### 1. File hướng dẫn Review chuẩn (`REVIEW.md`)
Anthropic khuyên dùng file `REVIEW.md` để hướng dẫn AI khi chạy PR code review:

```markdown
# Review instructions

## Passes
Run three passes and tag each finding with its pass:
- Compliance: the change matches spec.md, plan.md and our design principles
- Security: injection risks, authentication gaps, PII in logs
- Bugs: logic errors, broken edge cases, subtle regressions

## What Important means here
Reserve Important for findings that would break behavior, leak data or breach a policy. 
Style and naming are nits.

## Cap the nits
Report at most five nits per review; summarize the rest as a count.

## Do not report
Generated files under src/gen/ and anything CI already enforces (linter/formatter).
```

### 2. Pre-commit Hooks & Guardrails (Exit code 2)
Sử dụng hooks để chặn đứng các hành vi nguy hiểm của Agent trước khi lọt vào git history:
- `blocked: exit 2`: Trả về exit code 2 khi agent vi phạm chính sách.
- `$block file edit`: Ngăn chặn agent chỉnh sửa file production config, credential hoặc file nhạy cảm.
- `$block credentials`: Tự động grep phát hiện secrets/tokens trong diff và abort lệnh.
- `$update plan`: Yêu cầu con người phê duyệt trước khi agent thay đổi kiến trúc trong `plan.md`.

### 3. Tự chữa lành trong CI/CD (Headless Claude Integration)
- **Pre-deployment (Trước khi release):** CI pipeline gọi Claude ở chế độ headless (`claude -p`). Nếu build hoặc test fail, Claude tự đọc logs, xác định nguyên nhân, tạo git branch mới, sửa lỗi và mở Pull Request sửa build tự động.
- **Post-deployment (Sau khi release):** Kết nối Claude với monitoring tools (Datadog, Sentry). Nếu có spike lỗi hoặc bất thường sau khi deploy, Claude kích hoạt runbook rollback, phân tích root cause và tạo hotfix PR.

---

## 6. Maintain Phase — Vòng lặp khép kín với SPC (`bands.yaml`)

Đây là mảnh ghép đột phá nhất trong playbook của Anthropic: **Áp dụng Kiểm soát quy trình thống kê (Statistical Process Control - Western Electric Rules)** để đóng hoàn toàn vòng lặp SDLC.

### File cấu hình `bands.yaml`
```yaml
metric: ci_test_failure_rate
baseline: rolling_30d
rules: western_electric
tiers:
  1sigma: 
    action: log
  2sigma: 
    action: diagnose
    tools: "Read,Grep,Bash(gh run view *)"
  3sigma: 
    action: propose
    routes: [pull_request, runbook:rollback-deploy]
```

### Cơ chế hoạt động của 3 tầng phản ứng:
1. **1-Sigma ($\sigma$):** Biến động nhẹ trong ngưỡng thống kê bình thường ➔ Chỉ ghi log theo dõi.
2. **2-Sigma ($2\sigma$):** Có xu hướng bất thường kéo dài ➔ Claude tự động kích hoạt chế độ chẩn đoán, dùng các công cụ đọc log GitHub Actions, grep code để tìm ra flakiness hoặc điểm nghẽn hiệu năng.
3. **3-Sigma ($3\sigma$):** Lỗi nghiêm trọng vượt ngưỡng 3 độ lệch chuẩn ➔ Claude lập tức kích hoạt runbook rollback phiên bản deploy và mở Pull Request đề xuất bản vá.
4. **Kích hoạt vòng lặp mới:** Từ báo cáo bảo trì và phân tích log 30 ngày, Claude tự động soạn thảo một file `intent.md` mới (ví dụ: tái cấu trúc query chậm, sửa bug lặp lại) ➔ **Đưa hệ thống quay trở lại Bước 1 (Planning).**

---

## 🔗 Chuỗi Artifact committed trong Git (Audit Trail)

Toàn bộ quá trình SDLC tạo ra một chuỗi tài liệu liên kết chặt chẽ được commit trực tiếp vào Git, đảm bảo tính minh bạch tuyệt đối:

```
intent.md (Why - Nhu cầu kinh doanh)
    │
    ▼
spec.md (What - Đặc tả kiến trúc & UX)
    │
    ▼
plan.md (How - File thay đổi & Thứ tự làm việc)
    │
    ▼
code + tests (Executable artifacts - TDD verified)
    │
    ▼
REVIEW.md (Audit - 3 passes: Compliance, Security, Bugs)
    │
    ▼
bands.yaml (Monitor - SPC Closed loop ➔ new intent.md)
```

---

## 🛠️ Checklist Hành động cho Engineering Teams

- [ ] **Khởi tạo bộ khung Artifacts:** Tạo thư mục `intent/`, bổ sung `CLAUDE.md` ở root repo với định nghĩa rõ ràng về "Green output".
- [ ] **Cài đặt Rules cho Code Review:** Tạo file `REVIEW.md` với quy tắc giới hạn tối đa 5 nits và 3 passes (Compliance, Security, Bugs).
- [ ] **Thiết lập Git Guardrails:** Cài đặt hook pre-commit với `exit 2` để chặn rò rỉ credential và chặn sửa đổi trái phép các file core/prod.
- [ ] **Xây dựng bộ Continuous Evals:** Gom 20–50 bài toán thực tế đã giải trong quá khứ thành eval suite để chạy tự động mỗi khi cập nhật model hoặc rules.
- [ ] **Tự động hóa CI/CD với Headless Agent:** Cấu hình GitHub Actions cho phép Claude đọc build log khi thất bại và tự động mở PR sửa lỗi.
- [ ] **Đóng vòng lặp giám sát:** Thiết lập `bands.yaml` kết nối Sentry/Datadog để biến lỗi runtime thành `intent.md` cho sprint tiếp theo.

---

## 🔍 Từ khóa tìm kiếm liên quan
`Claude Code`, `AI-Native SDLC`, `Anthropic Playbook`, `Boris Tane`, `intent.md`, `spec.md`, `plan.md`, `CLAUDE.md`, `REVIEW.md`, `Continuous Evals`, `Statistical Process Control`, `bands.yaml`, `TDD AI`, `Headless CI/CD`, `Superpowers`, `Matt Pocock Skills`.
