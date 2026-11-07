# Cách Kỹ Sư Senior Xây Dựng Sản Phẩm Với AI (Full Intercom Clone: MarshalDesk) | Jan Marshal (Re-analyzed)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=H84vfy96m_k)
- **Video ID:** `H84vfy96m_k`
- **Watch Skill ID:** `78562cba15df8752`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 05:55:23 (Gần 6 Giờ Masterclass Thực Chiến)
- **Speaker:** Jan Marshal (`@ski043` — Senior Software Engineer, Founder `SyntaxPath`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong khóa học thực chiến kéo dài gần **6 giờ (`05:55:23`)**, **Jan Marshal** ghi hình toàn bộ quá trình kiến trúc, thiết kế giao diện, lập trình đa tác nhân và triển khai lên môi trường Production ứng dụng **MarshalDesk (`marshaldesk.vercel.app`)** — một nền tảng hỗ trợ khách hàng thời gian thực kiêm AI Agent RAG lấy cảm hứng từ **Intercom**, được xây dựng từ con số 0.

Trọng tâm xuyên suốt của video là sự đối lập căn bản giữa **"Wishful Prompting của Junior"** (gõ 1 câu nhắc bảo AI *"hãy clone Intercom"* rồi nhận về một đống rác UI không chạy được) và **Quy trình Kỹ nghệ Tác nhân (Agentic Engineering Workflow) của Senior Engineer**:

1. **Chốt biên sản phẩm & Chống phình to phạm vi (`Idea Definition` + `/writing-prds`):** Viết rõ trong Notion mục **`What we're NOT building (v1)`** (không làm Email/ticketing, không làm Mobile app, không làm Billing ở v1), vẽ sơ đồ kiến trúc và 3 luồng dữ liệu (`Flow A/B/C`) trên **tldraw**, sau đó đọc giọng nói qua **Willow** (`123 WPM`, tiết kiệm hơn 37 giờ gõ phím) kết hợp ảnh chụp sơ đồ để sinh tài liệu PRD chuẩn chỉnh.
2. **Thiết kế hệ thống Contract-First & Toàn bộ Backend trên 1 Nền tảng (Neon Unified Platform):** Thay vì ghép nối 5 dịch vụ rời rạc, MarshalDesk khai thác trọn bộ **5 dịch vụ trong 1 dự án Neon (`AWS Europe Central 1 Frankfurt`)**: **Neon Postgres + `pgvector`**, **Better Auth**, **Neon Object Storage** (*"Files that branch with your Neon database"*), **Neon Functions** (xử lý nền bất đồng bộ trả về `HTTP 202`), và **Neon AI Gateway** kết hợp **oRPC v2 beta (`orpc.dev`)** cho tầng API định kiểu nghiêm ngặt (`contract.ts`).
3. **Điều phối tính năng 2 Phiên với `Feature Orchestrator` (`github.com/ski043/skills`):**
   - **Session 1 (Product & Architecture Refinement):** Thảo luận chốt mọi góc khuất kỹ thuật và xuất ra một **Copy-Ready Orchestration Prompt** không còn bất kỳ giả định mơ hồ nào.
   - **Session 2 (4-Role Sub-Agent Team):** Tự động điều phối đội hình 4 vai trò (**`Lead`**, **`Researcher`** dùng `claude-sonnet-5-5-high`, **`Implementer`**, và **`Reviewer`** độc lập) làm việc trên các nhánh Git riêng (`feature/realtime`, `feature/knowledge-base`, `feature/ai-agent`), tự kiểm thử trên trình duyệt thực trước khi mở PR.
4. **Kỹ nghệ Thị giác (Visual Taste & Reference-Driven UI):** Kết hợp mẫu thiết kế đoạt giải trên **`motionsites.ai`** (`Geist` + `Geist Pixel`), sinh ảnh mockup **4K (`3840x2160`)** bằng **`GPT Image 2.5` trên Atlas Cloud** cho bố cục Dashboard 70/30, chú thích trực tiếp lên `shadcn/ui Mail` cho Inbox 3 cột, và sẵn sàng **phê bình thẳng thừng (Visual Pushback)** khi Agent đặt sai vị trí thẻ UI.
5. **Triển khai Production "Zero-Touch" qua Vercel MCP + Neon MCP:** Agent tự động chạy migration lên nhánh `production`, đẩy **Cloudflare/PartyKit Worker (`marshaldesk-realtime`)**, kích hoạt **Neon Function (`jobs`)** kèm cron 15 phút, tạo project Vercel tại **Frankfurt** (nằm sát cạnh database Neon để tối ưu độ trễ), nạp 20 biến môi trường nhạy cảm, và gỡ khóa Vercel Deployment Protection để `embed.js` hoạt động xuyên miền ngay từ lần deploy đầu tiên.

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│                        MARSHALDESK: PRODUCTION ARCHITECTURE & DATA FLOWS                         │
├──────────────────────────────────────────────────────────────────────────────────────────────────┤
│  [ EXTERNAL CUSTOMER SITE (e.g. Acme Outdoor Co. :4000 / syntaxpath.com) ]                       │
│  └── <script src="https://marshaldesk.vercel.app/embed.js" data-workspace="01a0..." async>       │
│         │                                                        ▲                               │
│         │ (1) Visitor Message / Action                           │ (7) ~40ms WebSocket Stream    │
│         ▼                                                        │                               │
│  ┌───────────────────────────────────────────────────────────────┴────────────────────────────┐  │
│  │                       NEXT.JS SERVER (Vercel Frankfurt Node 24 + oRPC)                     │  │
│  │  • Domain Allowlist & Visitor Rate-Limit Check (`x-vercel-ip-*` Geo/Timezone Telemetry)    │  │
│  │  • Orchestrates Flow B (RAG Answering) & Flow C (Human Handoff State Machine)              │  │
│  └──────┬──────────────────────────┬─────────────────────────────┬────────────────────────────┘  │
│         │ (3) Classify & Answer    │ (4) Embed Question          │ (Publish Event)               │
│         ▼                          ▼                             ▼                               │
│  ┌──────────────────────┐  ┌──────────────────────┐  ┌────────────────────────────────────────┐  │
│  │   NEON AI GATEWAY    │  │  OPENAI EMBEDDINGS   │  │     PARTYKIT (Cloudflare Workers)      │  │
│  │  • Fast Classifier   │  │  • text-embedding-   │  │  • `marshaldesk-realtime`              │  │
│  │    (GPT-5.4 Nano /   │  │    3-small (1536d)   │  │  • Bi-directional ~40ms sync between   │  │
│  │     System One Jev)  │  │  • `assertEmbedding` │  │    Visitor Widget & 3-Col Operator UI  │  │
│  │  • Grounded Chat Gen │  │    (non-zero check)  │  └────────────────────────────────────────┘  │
│  └──────────────────────┘  └──────────┬───────────┘                                              │
│                                       │ (5) Cosine Search / Save Chunks                          │
│                                       ▼                                                          │
│  ┌────────────────────────────────────────────────────────────────────────────────────────────┐  │
│  │                NEON UNIFIED BACKEND (AWS eu-central-1 Frankfurt - Scale Plan)              │  │
│  │  ┌─────────────────────────┐  ┌──────────────────────────┐  ┌───────────────────────────┐  │  │
│  │  │ 1. Postgres + pgvector  │  │ 2. Neon Object Storage   │  │ 3. Neon Functions (`jobs`)│  │  │
│  │  │ • Workspace Isolation   │  │ • S3-compatible buckets  │  │ • Storage Trigger ONLY on │  │  │
│  │  │ • Chunks + 1536d Vectors│  │   that branch with DB!   │  │   `workspaces/*/sources/` │  │  │
│  │  │ • 4 Conversation States │  │ • Presigned URL uploads  │  │ • `unpdf` -> ~800-tok     │  │  │
│  │  ├─────────────────────────┤  │   for Avatars & Docs     │  │   chunks -> Embed -> Save │  │  │
│  │  │ 4. Better Auth          │  └──────────────────────────┘  │ • 15-min Cron Auto-Close  │  │  │
│  │  │ • Email/Pass + Google   │                                └───────────────────────────┘  │  │
│  │  └─────────────────────────┘                                                               │  │
│  └────────────────────────────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu Sản Phẩm MarshalDesk (Từ Góc Nhìn Thị Giác Trong Video)

Qua 81 khung hình trích xuất trực tiếp từ quá trình xây dựng và kiểm thử (`frame_0001`–`frame_0080`), ứng dụng **MarshalDesk** hoàn chỉnh bao gồm **4 bề mặt sản phẩm** kết nối chặt chẽ:

### 1. Trang Đích Chuẩn Award-Winning (`localhost:3000` / `marshaldesk.vercel.app`)
- Được thiết kế dựa trên cảm hứng từ mẫu **`AI Runtime`** trên **`motionsites.ai`** (`frame_0020`, `frame_0021`) kết hợp bộ phông chữ **`Geist` & `Geist Pixel`** được khóa cứng trong `docs/DESIGN.md` (`frame_0029`).
- **Thông điệp cốt lõi (`frame_0023`, `frame_0042`):**
  - Hero Headline: **"Stop answering the same questions"** — *"MarshalDesk answers visitors from your docs around the clock, and hands the conversation to you when it isn't sure."*
  - 3 chỉ số nổi bật: **`24/7` Answers for your visitors**, **`1` Snippet to paste on your website**, **`~40 ms` Real-time — every message, instantly, on both sides** (`frame_0002`).
  - Minh họa trực quan sự khác biệt giữa **Chatbot ảo giác nguy hiểm** (*"Absolutely! You can get a full refund anytime within 12 months. No questions asked."*) và **MarshalDesk RAG khóa chặt theo tài liệu `refund-policy.pdf`** (*"Refunds are available within 14 days of purchase"*, `frame_0001`).

### 2. Bảng Điều Khiển Cấu Hình Widget & Nạp Tri Thức (`70/30 Split Layout`)
- Giao diện `/dashboard` (`frame_0004`, `frame_0050`, `frame_0056`, `frame_0060`, `frame_0075`) chia làm 2 cột tỉ lệ **70% Cài đặt bên trái — 30% Live Preview bên phải**, tự động lưu mọi thay đổi tức thì (**`✓ Saved`** — hoàn toàn không có nút Save rườm rà):
  - **Checklist 3 bước kích hoạt (`Get your widget live`):**
    1. `Add a source` (*Teach the agent what you know*).
    2. `Add an allowed domain` (*Choose where the widget loads* — ví dụ `example.com`, `syntaxpath.com`).
    3. `Install the snippet` (*Paste one line on your site* trước thẻ đóng `</body>`).
  - **Tùy biến diện mạo (`Appearance`):**
    - `Agent name` (ví dụ: `Marshal Agent`, `Jans agent 123`, `SyntaxPath Agent`).
    - `Agent avatar`: Hỗ trợ tải ảnh `PNG, JPEG, WebP or GIF, up to 2 MB` lên Neon Object Storage; khi chưa tải ảnh, hệ thống tự động sinh avatar gradient deterministically từ tên Agent bằng thư viện **`DiceBear` (`dicebear.com/styles/glass`, `frame_0057`)**.
    - `Accent color`: 7 màu chủ đạo (`Blue`, `Indigo`, `Violet`, `Pink`, `Red`, `Orange`, `Green`) tự động tính toán màu chữ tương phản (*"Green, with dark text so it stays readable"* / *"Blue, with white text so it stays readable"*).
    - `Position`: Chọn góc hiển thị `Bottom left` hoặc `Bottom right`.
    - `Messages / Greeting`: Lời chào mở đầu (giới hạn `300` ký tự) + **Suggested questions** được AI tự động trích xuất và cập nhật mỗi khi bộ tài liệu Knowledge Base thay đổi (`frame_0076`).
  - **Quản lý Kho Tri Thức (`Knowledge base` — `frame_0056`):**
    - Kéo thả nhiều file `PDF, Markdown or text, up to 10 MB each` cùng lúc.
    - Hiển thị trạng thái xử lý thời gian thực cho từng file:
      - `returns-policy.pdf (242 KB)` ➔ **`✓ Ready`**
      - `shipping-and-delivery.md (14 KB)` ➔ **`✓ Ready`**
      - `faq.txt (10 KB)` ➔ **`⟳ Processing`**
      - `price-list-scan.pdf` ➔ **`! Failed`** (*"No text could be extracted from this file"* — báo lỗi rõ ràng khi gặp file PDF scan chỉ có ảnh không có lớp chữ).
  - **Khung Xem Trước Thời Gian Thực (`Preview`):** Nằm bên dưới nút gạt tổng **`● Agent — Answering visitors`**, cho phép chuyển đổi giữa 2 trạng thái **`New visitor`** (hiển thị nhãn `Answers only from our knowledge base`, lời chào, 4 câu hỏi gợi ý, nút `Talk to a human`) và **`After a message`**.

### 3. Hộp Thư Điều Hành 3 Cột Thời Gian Thực (`Operator Inbox` — `/dashboard/inbox`)
- Lấy cảm hứng từ bố cục `shadcn/ui Mail` (`frame_0041`) và hoàn thiện tại `frame_0005`, `frame_0048`, `frame_0054`, `frame_0067`, `frame_0068`:
  - **Cột 1 (Danh sách Hội thoại & Bộ lọc):**
    - 5 tab lọc trạng thái: **`All open` | `Waiting` | `Agent` | `You` | `Closed`** kèm thanh tìm kiếm `Search visitors and last messages`.
    - Hiển thị danh sách khách truy cập thời gian thực (`Visitor from Berlin`, `Visitor from Austin`, `Visitor from Hamburg`, `Visitor 2e02`...) kèm nhãn trạng thái màu sắc rõ ràng:
      - `• Waiting` (*"The agent wasn't sure of the answer"*, *"The visitor asked for a person"*, *"The knowledge base doesn't cover this"*, hoặc *"The agent is off"*).
      - `• Agent` (AI đang tự động trả lời).
      - `• You` (Nhân viên hỗ trợ đã tiếp quản).
  - **Cột 2 (Luồng Chat Trực Tiếp & Trích Dẫn Nguồn RAG):**
    - Hiển thị toàn bộ lịch sử chat giữa khách và AI kèm **nhãn trích dẫn file nguồn** ngay dưới câu trả lời của AI (ví dụ: **`Answered from 📄 shipping-and-delivery.md 📄 account-and-orders.md`**, `frame_0068`).
    - Khi chuyển giao sang người thật, hiển thị thanh ngăn cách rõ ràng: **`Handed to you: the knowledge base doesn't cover this`** (ví dụ khi khách đòi *"give me a 50% coupon"*, `frame_0054`) và **`You took over · just now`**.
    - Thanh công cụ phía trên có nút **`Hand to agent`** (trả lại quyền trả lời cho AI) và **`Close`** (đóng hội thoại).
  - **Cột 3 (`Details` — Telemetry Khách Truy Cập):**
    - Thu thập tự động từ header định vị của Vercel (`x-vercel-ip-*`) và User-Agent: `Location` (`Berlin, Germany`), `Local time` (`17:56 Europe/Berlin`), `Language` (`American English`), `Device` (`Desktop`), `Browser` (`Firefox`), `System` (`Mac OS`), `Current page` (`localhost:4000`), `Came from` (`Direct visit`), `First seen`, `Last seen`, `Visits` (`6`), cùng trạng thái `Conversation State` & `Handoff reason`.

### 4. Trang Thử Nghiệm Widget Nhúng Độc Lập (`Acme Outdoor Co.` — `localhost:4000`)
- Để kiểm thử widget trong môi trường cross-origin thực tế như một khách hàng thật, Jan xây dựng một trang thương mại điện tử giả lập **`Acme Outdoor Co.`** (*"Tents, packs and trail gear since 1998"*, `frame_0052`, `frame_0068`, `frame_0077`) chạy ở cổng `:4000` hoàn toàn tách biệt với ứng dụng chính `:3000`.
- Trang này có khung **`Test your widget snippet`** cho phép dán đoạn mã `<script src=".../embed.js" data-workspace="..." async></script>`, bấm **`Embed and reload`** hoặc **`Remove widget`**, và hỗ trợ mở qua `127.0.0.1:4000` để kiểm thử tính năng chặn miền trái phép (`Allowed domains`).

---

## 3. Ngăn Xếp Công Nghệ & Kiến Trúc 3 Luồng Dữ Liệu (`tldraw` Flows)

Trước khi viết bất kỳ dòng code tính năng nào, Jan phác thảo toàn bộ kiến trúc hệ thống và **3 luồng dữ liệu cốt lõi (`Flow A`, `Flow B`, `Flow C`)** trên **tldraw** (`frame_0019`, `frame_0030`, `frame_0063`):

| Thành phần | Công nghệ / Dịch vụ | Lý do lựa chọn & Giải pháp kỹ thuật |
| :--- | :--- | :--- |
| **Unified Backend** | **Neon (`neon.com` — Scale Plan, `eu-central-1`)** | Gom trọn **5 dịch vụ** vào 1 nền tảng duy nhất (`frame_0038`): **Postgres (`pgvector`)**, **Better Auth**, **Object Storage**, **Functions**, và **AI Gateway**. Hỗ trợ **Database + Storage Branching** (`production` & `Development`). |
| **API Layer** | **oRPC v2 beta (`orpc.dev`)** | Thiết kế **Contract-First (`contract.ts`)** định kiểu đầu-cuối (End-to-End Typesafe APIs) giữa Next.js App Router và client mà không bị rò rỉ kiểu dữ liệu. |
| **Real-Time Engine** | **PartyKit (`partykit.io` / Cloudflare Workers)** | Triển khai worker `marshaldesk-realtime` đồng bộ tin nhắn, trạng thái gõ phím và trạng thái hội thoại giữa Widget và Inbox với độ trễ **`~40 ms`**. |
| **Document Parsing & Embeddings** | **`unpdf` + OpenAI `text-embedding-3-small`** | Tách văn bản PDF trong Neon Function bằng `unpdf`, cắt thành các đoạn **`~800 tokens`**, tạo vector **`1536 chiều`** qua AI Gateway kèm hàm bảo vệ `assertEmbedding` (`frame_0062`, `frame_0069`). |
| **Intent Classifier & Chat LLM** | **Neon AI Gateway (`GPT-5.4 Nano` / `GPT-5.4 Mini` / `Jev`)** | Phân tách mô hình phân loại siêu nhanh/rẻ ở cổng đầu vào với mô hình sinh câu trả lời bám sát ngữ cảnh RAG (`frame_0064`, `frame_0065`). |
| **Agent Tooling** | **T3 Code + `ski043/skills` + Neon MCP + Vercel MCP** | Điều phối đa tác nhân với `Claude Opus 5.5 1M Medium` & `Claude Sonnet 5.5 High`, kết nối trực tiếp vào Neon CLI/MCP (8 skills) và Vercel MCP (244 tools, 33 skills). |

### Chi Tiết 3 Luồng Xử Lý Trong `tldraw` (`frame_0019`, `frame_0062`, `frame_0067`):

1. **Flow A — Chủ sở hữu tải tài liệu lên (`Owner uploads knowledge — Neon Function`):**
   - **Bước 1:** Dashboard tạo bản ghi `source` trong Postgres trước, sau đó xin Presigned URL để tải thẳng file lên **Neon Object Storage** theo đường dẫn `uploads/workspaces/<id>/sources/<sourceId>.pdf`.
   - **Bước 2:** Một **Storage Trigger** được giới hạn nghiêm ngặt trong thư mục `sources/` sẽ gọi hàm Neon Function `jobs` (khai báo trong `neon.ts`). *Lưu ý kiến trúc quan trọng (`frame_0062`):* Nếu không giới hạn trigger ở thư mục `sources/`, mọi bức ảnh avatar tải lên cũng sẽ kích hoạt nhầm quy trình băm tài liệu RAG!
   - **Bước 3:** Route của Neon Function trả về ngay mã **`HTTP 202 Accepted`** và tiếp tục xử lý ngầm: dùng `unpdf` bóc tách chữ, chia thành các đoạn ~800 tokens, gọi `embedPassages` (`text-embedding-3-small`, 1536 chiều) có kiểm tra chống vector toàn số 0 (`if (vector.every(v => v === 0)) throw new Error("Embedding is all zeros")`, `frame_0069`).
   - **Bước 4:** Trong một giao dịch (transaction) duy nhất, hàm thay thế toàn bộ chunks của nguồn đó trong bảng `pgvector`, cập nhật trạng thái thành `ready` (hoặc `failed`), và tự động sinh lại bộ câu hỏi gợi ý (`Suggested questions`) cho workspace.
2. **Flow B — Khách truy cập đặt câu hỏi (`Visitor asks a question — Next.js + PartyKit`):**
   - `1. Widget sends message` ➔ `2. Server checks visitor + domain allowlist + rate limits` ➔ `3. AI Gateway classify` (kiểm tra xem khách có đòi gặp người thật hay hỏi ngoài phạm vi không) ➔ `4. OpenAI embed question` ➔ `5. Postgres pgvector find best chunks` ➔ `6. AI Gateway answer strictly from chunks` ➔ `7. PartyKit stream to widget & inbox`.
3. **Flow C — Máy trạng thái chuyển giao cho người thật (`Handoff State Machine`):**
   - Mọi cuộc hội thoại đi qua 4 trạng thái rõ ràng: **`1. AI answering` (mặc định)** ➔ **`2. Waiting` (khi AI không chắc chắn, tài liệu không đề cập, hoặc khách bấm *"Talk to a human"*)** ➔ **`3. Human / You` (nhân viên hỗ trợ nhảy vào chat, AI lập tức ngừng trả lời)** ➔ **`4. Closed` (đóng thủ công hoặc tự động đóng sau 15 phút không hoạt động nhờ Cron trigger trong Neon Function `jobs`)**.
   - Các thông báo chuyển giao (*Handoff notices*) được cố định bằng văn bản UI tiếng Anh chuẩn thay vì để LLM tự bịa (`frame_0067`).

---

## 4. Quy Trình 6 Bước Của Kỹ Sư Senior Khi Xây Dựng Với AI

Jan Marshal đúc kết quy trình làm việc giúp một kỹ sư Senior biến ý tưởng lớn thành sản phẩm chạy thật mà không rơi vào "bẫy code rác" (AI Slop):

### Bước 1: Định Nghĩa Ý Tưởng & Viết PRD Bằng Giọng Nói (`Willow` + `/writing-prds`)
- Thay vì nhảy vào code ngay, Jan mở Notion viết **`Idea Definition (MarshalDesk)`** (`frame_0014`, `frame_0015`) trả lời 4 câu hỏi: *Nó là gì? Ai dùng? 4 tính năng cốt lõi là gì? Và **chúng ta KHÔNG xây dựng gì ở bản v1?***
- Sử dụng ứng dụng đọc chính tả **`Willow`** (`Hold ^Ctrl + ⌥Opt to dictate`, tốc độ **123 WPM**, đã đọc hơn **237,152 từ**, `frame_0016`) để trút toàn bộ suy nghĩ kiến trúc kèm ảnh chụp sơ đồ `tldraw` vào skill `/writing-prds` (từ bộ `github.com/mattpocock/skills`, `frame_0008`).
- Khi Agent hỏi lại các quyết định thiết kế (ví dụ trong `frame_0017`: chọn cách truy cập dữ liệu nào giữa Neon Data API + RLS hay Drizzle ORM trên server, và lấy vị trí khách truy cập bằng cách nào), Jan thảo luận từng phương án và chốt chọn **Server-side SQL/Drizzle qua oRPC** + **Vercel geolocation headers (`x-vercel-ip-*`)**.

### Bước 2: Neo Thẩm Mỹ Thiết Kế Bằng Ảnh Mẫu & Mockup 4K (`motionsites.ai` + `Atlas Cloud`)
- LLM rất tệ khi phải tự tưởng tượng ra một giao diện đẹp từ chữ thuần túy, nhưng lại cực kỳ xuất sắc khi có **tài liệu thiết kế (`docs/DESIGN.md`)** và **ảnh tham chiếu trực quan**:
  - **Landing Page:** Sao chép cấu trúc mẫu từ **`motionsites.ai`** (`frame_0020`–`frame_0022`) để Agent nắm bắt chính xác cách phối màu tối, lưới pixel và hiệu ứng chuyển động.
  - **Dashboard 70/30:** Dùng **`GPT Image 2.5` trên `atlascloud.ai`** (`frame_0040`) sinh bức ảnh mockup độ phân giải **4K (`3840x2160`)** mô tả trang cấu hình Widget bên trái và khung Live Preview bên phải, sau đó đính kèm bức ảnh này vào prompt.
  - **Inbox 3 Cột:** Chụp màn hình mẫu `v3.shadcn.com/examples/mail` (`frame_0041`) và khoanh vùng trực tiếp các khu vực cần giữ lại hoặc lược bỏ.

### Bước 3: Điều Phối Đa Tác Nhân 2 Phiên Với `Feature Orchestrator` (`github.com/ski043/skills`)
- Đây là "vũ khí bí mật" do chính Jan xây dựng và mã nguồn mở tại **`github.com/ski043/skills`** (`frame_0009`):
  - **Tại sao không làm tất cả trong 1 phiên chat?** Khi vừa bàn bạc thiết kế vừa viết hàng chục ngàn dòng code trong cùng một cửa sổ ngữ cảnh, context của Agent sẽ bị loãng và ô nhiễm bởi các phương án đã bị loại bỏ.
  - **Session 1 (`/feature-orchestrator session 1` — `frame_0062`):** Chỉ tập trung khám phá codebase, đối chiếu tài liệu từ **Neon MCP Plugin** (`frame_0031`), chốt từng chi tiết kỹ thuật và kết thúc bằng việc xuất ra một **bản Prompt Điều Phối hoàn chỉnh (Copy-Ready Orchestration Prompt)**.
  - **Session 2 (`/feature-orchestrator session 2` — `frame_0067`):** Mở một phiên mới tinh, dán bản prompt từ Session 1 vào. Agent chính (`Lead` — `Claude Opus 5.5 1M Medium`) lập tức chia việc cho:
    - **Đội `Researcher` (`claude-sonnet-5-5-high`):** Chạy 4 sub-agents song song đọc sâu tài liệu Neon Functions, AI SDK, PartyKit và oRPC.
    - **Đội `Implementer`:** Viết code theo từng lát cắt dọc (Vertical Slices) trên nhánh riêng và tự mở trình duyệt kiểm tra.
    - **Đội `Reviewer`:** Kiểm toán độc lập mã nguồn, bảo mật multi-workspace và chạy `pnpm typecheck` / `pnpm lint` trước khi báo cáo.

### Bước 4: Phản Biện Thị Giác Thẳng Thừng (Visual Pushback & Taste Enforcement)
- Trong `frame_0058`, khi Agent hoàn thành giao diện Dashboard đầu tiên và chụp ảnh màn hình `.claude/progress/screens/v2-dashboard-light...`, nút bật/tắt `Agent replies` bị đặt lệch lạc trông rất chướng mắt.
- Thay vì chấp nhận tạm bợ, Jan đọc thẳng vào mic:
  > *"Look, you did not do a good job. Let me be absolutely honest with you. Think about it. Look at the browser yourself. Does this agent replies card look good to you? No, not at all. It looks absolutely ugly and misplaced."*
- Ngay lập tức, Agent tự nhìn lại ảnh chụp trình duyệt, nhận ra lỗi bố cục và dời thẻ trạng thái `● Agent — Answering visitors` sang nằm gọn gàng ngay phía trên khung `Preview` (`frame_0060`).

### Bước 5: Kiểm Thử Đa Luồng & Đóng Vai Khách Hàng Thật
- Jan không bao giờ tin lời Agent nói *"Everything works"* nếu chưa tự tay kiểm chứng:
  - Thử tải lên cả file hợp lệ (`returns-policy.pdf`, `shipping-and-delivery.md`) lẫn file PDF rỗng/scan (`price-list-scan.pdf`) để xem UI có hiển thị trạng thái **`! Failed`** tử tế hay làm sập trang (`frame_0056`).
  - Mở song song cửa sổ **Operator Inbox (`localhost:3000/dashboard/inbox`)** bên trái và trang cửa hàng **`Acme Outdoor Co.` (`localhost:4000`)** bên phải (`frame_0077`) để kiểm tra độ trễ `~40ms` khi chat qua lại, kiểm tra nhãn trích dẫn nguồn `Answered from...` (`frame_0068`), và thử đòi mã giảm giá 50% để ép hệ thống kích hoạt luồng chuyển giao sang người thật (`frame_0054`).

### Bước 6: Triển Khai Production Tự Hành Với Vercel MCP & Neon MCP
- Ở chặng cuối (`frame_0070`–`frame_0078`), Jan cài đặt **Vercel Plugin (`v0.48.0`, 244 MCP tools + 33 skills)** và giao cho Agent tự triển khai toàn bộ hệ thống lên môi trường Production (`marshaldesk.vercel.app`) với chỉ thị: *"Cứ tự làm từng bước và báo cáo lại, không cần dừng lại xin phép tôi ở mỗi bước nhỏ"* (`frame_0073`):
  1. Agent tự kiểm tra thấy nhánh `production` trên Neon chưa có bảng ứng dụng ➔ tự chạy migration đẩy schema lên `production`.
  2. Tự khởi tạo các bucket Object Storage trên `production` và sinh bộ khóa bí mật mới lưu vào `apps/web/.env.prod`.
  3. Tự deploy **PartyKit Worker (`marshaldesk-realtime`)** và **Neon Function (`jobs`)** kèm khóa xác thực trigger.
  4. Tự tạo project **`marshaldesk` trên Vercel** chạy **Node 24**, đặc biệt **đặt cụm máy chủ Serverless Functions tại Frankfurt (`fra1`)** nằm sát cạnh cụm database Neon (`AWS eu-central-1 Frankfurt`) để triệt tiêu độ trễ truy vấn DB.
  5. Tự nạp đủ **20 biến môi trường Production**, đồng thời phát hiện và **tắt chế độ Vercel Deployment Protection mặc định trên tên miền `*.vercel.app`** (nếu không tắt, khách truy cập và file `embed.js` nhúng trên web ngoài sẽ bị chặn bởi màn hình bắt đăng nhập Vercel!).
  6. Tự cấu hình tên miền tin cậy (`trusted auth domain`) cho Better Auth và CORS cho Object Storage, sau đó build thẳng từ `main` (`24a293d`) và chạy kiểm thử đầu-cuối thành công.

---

## 5. Tổng Kết 5 Bài Học Đắt Giá Cho Kỹ Sư Phần Mềm

1. **Đừng bao giờ giao việc cho AI khi chính bạn còn chưa rõ kiến trúc:** Thời gian vẽ 3 luồng `Flow A/B/C` trên `tldraw` và chốt danh sách `What we're NOT building` là khoản đầu tư mang lại ROI cao nhất trong toàn bộ 6 giờ làm việc.
2. **Tách biệt Phiên Thiết Kế (Session 1) và Phiên Thực Thi (Session 2):** Dùng `Feature Orchestrator` để kết tinh toàn bộ cuộc thảo luận dài thành một bản đặc tả sạch sẽ trước khi thả đàn Sub-Agents (`Lead`, `Researcher`, `Implementer`, `Reviewer`) vào viết code.
3. **Chọn nền tảng Backend hỗ trợ Branching toàn diện:** Việc **Neon** cho phép phân nhánh cả **Postgres Database lẫn Object Storage** cùng với **Neon MCP** giúp AI Agent thoải mái thử nghiệm migration và upload file trên nhánh `Development` mà không bao giờ làm hỏng dữ liệu `production`.
4. **Cung cấp "Đôi mắt" và "Gu thẩm mỹ" cho Agent:** Luôn đi kèm `DESIGN.md`, ảnh mẫu từ `motionsites.ai` hoặc mockup 4K từ `GPT Image 2.5`, và yêu cầu Agent tự chụp màn hình trình duyệt để đối chiếu.
5. **Tự động hóa DevOps bằng MCP có kiểm soát:** Khi kết hợp **Neon MCP + Vercel MCP**, việc đưa một hệ thống đa dịch vụ (Next.js + Postgres `pgvector` + Object Storage + Background Functions + PartyKit WebSockets + Better Auth) lên Production với đúng cấu hình vùng địa lý (`Frankfurt`) và CORS chỉ còn tính bằng phút.
