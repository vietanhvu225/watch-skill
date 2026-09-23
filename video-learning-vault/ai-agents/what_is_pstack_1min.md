# What Is Pstack? Better AI Coding Explained | TonkaToyXL

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=TI2rFCY9cGQ)
- **Watch Skill ID:** `fc62b7a5188244a4`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-24
- **Duration:** 01:30
- **Speaker:** TonkaToyXL (AI Test Cabin)
- **Transcript File:** [what_is_pstack_1min_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/what_is_pstack_1min_transcript.txt)

---

## 1. Tóm tắt Điều hành (Executive Summary)

Video giải thích nhanh trong 90 giây về bản chất và cơ chế hoạt động của **Pstack** — bộ plugin Cursor mã nguồn mở (giấy phép MIT) do Lauren Tan xây dựng:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           PSTACK WORKFLOW IN 90 SECONDS                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  Mục tiêu (User Goal) + Tiêu chí Nghiệm thu (Verification Check)            │
│                              │                                              │
│                              ▼                                              │
│                      [POTATO MODE ROUTER]                                   │
│            Chọn Playbook -> Thực thi -> Kéo tool cần thiết                  │
│                              │                                              │
│                              ▼                                              │
│                   [VERIFICATION CLOSED LOOP]                                │
│    - Kiểm soát app thực tế, đọc telemetry, chứng minh task PASS.           │
│    - Nếu FAIL: Agent tự dùng bằng chứng lỗi để sửa lại.                     │
│    - KHÔNG bắt người dùng phải bấm duyệt từng bước nhỏ.                     │
│                              │                                              │
│                              ▼                                              │
│                        [FEATURE MAP]                                        │
│    - Tìm kiếm nhanh khu vực code liên quan mà không phải cào lại toàn bộ.   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Điểm Khác Biệt & Rào Cản Sử Dụng (The Catch)

- **Triết lý Cốt lõi:** Pstack không tập trung vào việc *"viết nhiều code hơn"*, mà tập trung vào việc **"bắt buộc Coding Agents phải chứng minh code của mình chạy đúng (Prove their work)"**.
- **Cảnh báo Quan trọng (The Catch):** Pstack **không thể cứu vãn** một bộ test yếu kém hoặc một môi trường phát triển (dev environment) thiếu ổn định. Pstack phát huy tối đa sức mạnh khi:
  1. Môi trường chạy local/cloud của bạn hoạt động đáng tin cậy (reliable build & run).
  2. Các tiêu chí nghiệm thu (acceptance checks) được định nghĩa rõ ràng, cụ thể.
