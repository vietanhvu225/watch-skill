# Agents Without Code: Skills, YAML, and Filesystems Replaced Python | Philipp Schmid, Google DeepMind

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=fjF8EKnxKCU)
- **Watch Skill ID:** `c3d62b315a9f40c5`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-16
- **Duration:** 18:27
- **Speaker:** Philipp Schmid (Technical Staff, Google DeepMind)

---

## 1. Định nghĩa & Tuyên ngôn cốt lõi

> *"An LLM agent runs tools in a loop until it achieves a goal."* — Simon Willison

Bài diễn thuyết của Philipp Schmid (Google DeepMind) tại AI Engineer trình diễn một cuộc cách mạng trong kiến trúc AI Agent: **Loại bỏ dần code Python phức tạp, thay thế bằng tệp tin (Filesystem, YAML, Markdown & Skills).**
- Mô hình càng mạnh mẽ, ta càng nên **xóa bớt code điều phối (orchestration code)** thay vì làm nó phình to ra.

---

## 2. Lịch sử 3 giai đoạn kiến trúc Agent: "Xóa code trên từng chặng đường"

Qua ví dụ xây dựng cùng một GitHub PR Review Agent, tác giả minh họa 3 thế hệ phát triển:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       TIẾN HÓA KIẾN TRÚC AGENT: CODE ➔ FILES                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│ 1. Raw Python Loops (1–1.5 năm trước):                                      │
│    - Viết vòng lặp `while` thủ công trong Python.                          │
│    - Tự khai báo JSON Schema khổng lồ cho từng tool.                        │
│    - Tự xử lý routing, kiểm tra function call vs. text response, bắt lỗi.   │
│    - Cồng kềnh, dễ lỗi, gắn chặt vào từng API cụ thể.                       │
│                              │                                              │
│                              ▼                                              │
│ 2. Agent Frameworks (ADK, LangChain, etc.):                                 │
│    - Khung trừu tượng hóa vòng lặp (loop), retries và parse schema tự động. │
│    - Nhưng kỹ sư VẪN PHẢI VIẾT VÀ BẢO TRÌ PYTHON PLUMBING:                  │
│      Mỗi tool mới vẫn đòi hỏi viết hàm Python, cài đặt thư viện và hạ tầng. │
│                              │                                              │
│                              ▼                                              │
│ 3. Remote Agents & File-System Driven (Kỷ nguyên Antigravity & Files):      │
│    - TOÀN BỘ THƯ MỤC SOURCE CODE PYTHON BIẾN MẤT!                           │
│    - Chỉ còn: `agents.md` (chỉ dẫn), `skills/` (kỹ năng) và CLI tools.      │
│    - Agent chạy trong một Cloud Linux Sandbox cô lập, dùng `bash` & `gh`    │
│      CLI tự nhiên thay vì hàng chục Python function calls cứng nhắc.        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Google Gemini Interactions API & Antigravity Remote Agent

Tại Google I/O, Google công bố **Antigravity Remote Agent** trên Gemini API:
- **Harness chung với Antigravity IDE:** Antigravity Remote Agent được vận hành bởi cùng một Agent Harness cốt lõi đang điều khiển IDE Antigravity.
- **Flat Steps Timeline (Thay thế Chat Turn-based):**
  - Trước đây: Lịch sử hội thoại gượng ép qua lượt `user` ➔ `model` (phải giả lập user role để nạp kết quả function call).
  - Chuẩn mới Interactions API: Dòng thời gian phẳng gồm các bước tường minh: `User Input` ➔ `Reasoning` ➔ `Function Call` ➔ `Function Result`.
- **Cloud Linux Sandbox & Môi trường bảo mật:**
  - Hỗ trợ mount nguồn dữ liệu: GitHub Repo, Google Cloud Storage (GCS), hoặc inline files.
  - **Credential Security Proxy:** Cơ chế proxy mạng bao bọc Sandbox, tự động chèn token/credentials khi agent gửi request ra ngoài. Agent **không bao giờ nhìn thấy credentials/API keys trực tiếp**, loại bỏ hoàn toàn nguy cơ rò rỉ token.
  - Tự động quản lý ngữ cảnh (Server-side session state) và nén ngữ cảnh (Context Compaction) khi phiên làm việc kéo dài.

---

## 4. "The Bitter Lessons of Agent Engineering": Minh chứng từ các công ty đầu ngành

| Đội ngũ / Sản phẩm | Thay đổi kiến trúc thực tế |
| :--- | :--- |
| **Cursor** | Thay thế **12,000 dòng code TypeScript** điều phối Git Worktrees bằng một file Markdown agent chỉ **200 dòng**. |
| **Vercel** | Cắt bỏ **80% số lượng Tools** trong Agent, giúp giảm số bước thực thi, phản hồi nhanh hơn và tăng độ chính xác. |
| **Manus** | Tái cấu trúc lại Agent Harness **5 lần trong vòng 6 tháng**. |
| **LangChain** | Tái cấu trúc kiến trúc Open Deep Research **3 lần trong vòng 1 năm**. |

> ⚠️ **Quy tắc cảnh báo:** *"Nếu Harness của bạn ngày càng trở nên phức tạp hơn khi mô hình ngày càng thông minh hơn, bạn chắc chắn đang Over-Engineering cái Harness của mình."*

---

## 5. Ba nguyên tắc thiết kế Agent kỷ nguyên mới

1. **Đừng đấu tranh với mô hình (Don't fight the model):**
   - Dừng việc vi quản lý từng luồng rẽ nhánh bằng code cứng.
   - Cung cấp các công cụ nguyên tử, phổ quát (`bash`, hệ thống tệp tin, CLI tiêu chuẩn) và để mô hình tự do suy luận, khám phá và tìm đường đi tốt nhất.
2. **Làm chủ những gì thuộc về bạn (Own what is yours):**
   - Tập trung vào chỉ dẫn miền nghiệp vụ (`agents.md`), quy trình công việc (`skills/`), và đặc biệt là hệ thống kiểm thử tự động (**Evals**).
3. **Xây dựng để xóa bỏ (Build to delete):**
   - Thiết kế mọi thành phần với tâm thế sẵn sàng xóa code khi model thế hệ kế tiếp ra mắt. Thay thế code điều phối bằng các file tri thức có cấu trúc.

---

## 🔗 Liên kết & Thẻ
- **Chủ đề liên quan:**
  - [How We Solved Agent Building | Andrew Qu, Vercel](file:///f:/source/watch-skill/video-learning-vault/ai-agents/how_we_solved_agent_building.md)
  - [Harness Engineering is Not Enough: Why Software Factories Fail](file:///f:/source/watch-skill/video-learning-vault/ai-agents/why_software_factories_fail.md)
  - [Don't Ship Skills Without Evals](file:///f:/source/watch-skill/video-learning-vault/ai-agents/dont_ship_skills_without_evals.md)
- **Tags:** `#ai-agents` `#google-deepmind` `#antigravity` `#skills` `#yaml` `#filesystem-agents` `#philipp-schmid`
