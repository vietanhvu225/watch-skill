# ⏱️ Excalidraw in Seconds: Meet the CLI 🚀 — Sơ Đồ Kiến Trúc & Phản Hồi Trực Quan Cho AI Agents (Re-analyzed)

- **Source URL:** https://www.youtube.com/watch?v=AzODN_qw53k
- **Watch Skill ID:** `5370d5d88b1bbe69`
- **Uploader:** Excalidraw (Official)
- **Duration:** 00:40 (40.1s)
- **Resolution:** 1280x720 (av1)
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Frames Analyzed:** 29/29 frames (`C:\Users\Admin\.watch-skill\frames\5370d5d88b1bbe69\frame_0000.jpg` → `frame_0028.jpg`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong video giới thiệu sản phẩm chính thức dài 40 giây (**"⏱️ Excalidraw in Seconds: Meet the CLI 🚀"**), đội ngũ **Excalidraw** ra mắt công cụ dòng lệnh **`@excalidraw/cli`** — giao diện terminal chính chủ cho phép lập trình viên và các **AI Coding Agents (Claude Code, Cursor, Codex, OpenCode, Antigravity)** truy vấn, kết xuất hình ảnh hàng loạt và đọc/ghi cấu trúc JSON thuần của sơ đồ kiến trúc Excalidraw mà không cần mở trình duyệt.

Toàn bộ video được trình diễn trực tiếp trên cửa sổ terminal macOS (`dev — ~/diagrams — zsh`) qua **5 bước thao tác cốt lõi**:
1. **Meet the Excalidraw CLI (`00:01–00:04`):** Cài đặt toàn cục siêu nhẹ qua `npm i -g @excalidraw/cli` (`added 2 packages in 1s`).
2. **Log in to your workspace (`00:05–00:12`):** Xác thực với `https://api.excalidraw.com` qua `excalidraw login` bằng **API key** và kiểm tra thông tin workspace (`excalidraw whoami -o table`).
3. **Browse your scenes (`00:14–00:16`):** Liệt kê toàn bộ sơ đồ trong một bộ sưu tập đám mây (`excalidraw collections scenes list 9gUS8M9SLcb -o table`).
4. **Render any scene to PNG — In light & dark (`00:17–00:24`):** Kết xuất sơ đồ luồng CI/CD phức tạp (`2ijfvoFK9LT` — *GitHub Actions CI/CD Swimlane - Detailed*) ra file ảnh `ci-cd.png` (`1216 × 2047`) ở cả giao diện sáng mặc định và giao diện tối (`--theme dark`), trả về metadata JSON chuẩn xác.
5. **Script whole collections & Plain JSON ready for agents (`00:25–00:32`):** Kết hợp `jq` + `xargs` để kết xuất tự động toàn bộ bộ sưu tập trong một dòng lệnh (`Script whole collections`), đồng thời trích xuất trực tiếp cấu trúc phần tử JSON thuần (`excalidraw scenes content get 2ijfvoFK9LT | jq '.elements | length'` → `106` phần tử) sẵn sàng cho AI Agents đọc hiểu và chỉnh sửa.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    EXCALIDRAW CLI (`@excalidraw/cli`) — 5-STEP WORKFLOW                     │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. INSTALL        npm i -g @excalidraw/cli                 ──► added 2 packages in 1s      │
│  2. AUTHENTICATE   excalidraw login + excalidraw whoami     ──► workspace: fireside (10 u)  │
│  3. BROWSE SCENES  excalidraw collections scenes list <id>  ──► 5 scenes in 9gUS8M9SLcb     │
│  4. RENDER PNG     excalidraw scenes render <id> --out ...  ──► 1216x2047 Light & Dark PNG  │
│  5. AGENT JSON     excalidraw scenes content get <id> | jq  ──► 106 elements Plain JSON     │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Phân Tích Chi Tiết Từng Phân Đoạn & Khung Hình (Frame-by-Frame Breakdown)

### 2.1. Cài Đặt `@excalidraw/cli` (`00:01` – `00:04` | `frame_0000` – `frame_0003`)

- **Tiêu đề trên màn hình:** `Meet the Excalidraw CLI`
- **Môi trường Terminal:** `dev — ~/diagrams — zsh`
- **Lệnh thực thi (`frame_0002`, `frame_0003`):**
  ```bash
  ~/diagrams > npm i -g @excalidraw/cli

  added 2 packages in 1s
  ~/diagrams >
  ```
- **Điểm đáng chú ý:** Package CLI cực kỳ gọn nhẹ — chỉ cài đặt **2 packages trong đúng 1 giây**, không kéo theo dependency nặng nề.

---

### 2.2. Đăng Nhập & Kiểm Tra Phiên Làm Việc (`00:05` – `00:12` | `frame_0004` – `frame_0008`)

- **Tiêu đề trên màn hình:** `Log in to your workspace`
- **Luồng xác thực tương tác (`frame_0005` – `frame_0007`):**
  ```text
  ~/diagrams > excalidraw login
  ┌  Log in to https://api.excalidraw.com
  │
  ◇  Pick which method to sign in with:
  │  API key
  │
  │  Need an API key? See https://plus.excalidraw.com/docs/api/authentication
  │
  ◇  Paste your API key:
  │  ****************************************************************
  │
  ◇  Key accepted
  │
  └  Logged in to fireside
  ```
- **Kiểm tra trạng thái tài khoản & Workspace (`frame_0008`):**
  Ngay sau khi đăng nhập vào workspace `fireside`, người dùng chạy lệnh kiểm tra định danh với cờ định dạng bảng (`-o table`):
  ```bash
  ~/diagrams > excalidraw whoami -o table
  ```
  **Bảng kết quả trả về (`frame_0008`):**

  | `FIELD` | `VALUE` |
  | :--- | :--- |
  | `apiUrl` | `https://api.excalidraw.com` |
  | `keyType` | `personal` |
  | `keySource` | `login` |
  | `workspace.id` | `770VYkU8ePu` |
  | `workspace.name` | `fireside` |
  | `workspace.subscriptionStatus` | `active` |
  | `workspace.userCount` | `10` |

---

### 2.3. Duyệt Danh Sách Sơ Đồ Trong Collection (`00:14` – `00:16` | `frame_0009`)

- **Tiêu đề trên màn hình:** `Browse your scenes`
- **Lệnh thực thi (`frame_0009`):**
  ```bash
  ~/diagrams > excalidraw collections scenes list 9gUS8M9SLcb -o table
  ```
- **Bảng kết quả danh sách 5 scenes trong collection `9gUS8M9SLcb` (`frame_0009`):**

  | `ID` | `NAME` | `COLLECTION` | `PINNED` | `UPDATED` |
  | :--- | :--- | :--- | :--- | :--- |
  | `9QaZzJ6KtAz` | `IBIS` | `9gUS8M9SLcb` | `yes` | `2026-10-08T14:28:41.533Z` |
  | `2ijfvoFK9LT` | `GitHub Actions CI/CD Swimlane - Detailed` | `9gUS8M9SLcb` | `no` | `2026-10-08T14:37:37.874Z` |
  | `6F8vqBQy3wz` | `Habit Tracking App` | `9gUS8M9SLcb` | `no` | `2026-09-21T19:27:04.344Z` |
  | `9akmDng4qaX` | `Devices` | `9gUS8M9SLcb` | `no` | `2026-10-08T14:35:44.306Z` |
  | `9UMBeqpPrb9` | `Networking` | `9gUS8M9SLcb` | `no` | `2026-10-08T14:28:18.826Z` |

  *(Trong `frame_0009`, dòng thứ hai `2ijfvoFK9LT GitHub Actions CI/CD Swimlane - Detailed` được bôi sáng màu tím để chuẩn bị copy ID cho bước render).*

---

### 2.4. Kết Xuất Sơ Đồ Ra PNG — Light Mode & Dark Mode (`00:17` – `00:24` | `frame_0010` – `frame_0016`)

- **Tiêu đề trên màn hình:** `Render any scene to PNG` (`frame_0010`) → chuyển sang `In light` (`frame_0015`) và Dark mode (`frame_0016`).
- **Lệnh kết xuất chế độ sáng mặc định (`frame_0010`, `frame_0015`):**
  ```bash
  ~/diagrams > excalidraw scenes render 2ijfvoFK9LT --out ci-cd.png
  ```
  **JSON Output trả về trên terminal (`frame_0015`):**
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
- **Chi tiết sơ đồ `GitHub Actions CI/CD Workflow` được kết xuất (`frame_0011` – `frame_0014`):**
  - Ảnh `ci-cd.png` có kích thước **`1216 × 2047`** (`frame_0014`, `frame_0015`).
  - Phụ đề sơ đồ: *"Detailed swimlane with matrix jobs, branch protection, GitHub environments, scoped secrets, OIDC auth, approvals, deployment, and rollback."*
  - Gồm **5 cột Swimlane** được tô màu rõ nét:
    1. **Developer (Xám):** `Commit and push feature branch` ──► `Address failed checks and re-push` ──► `Apply review changes` ──► `Patch hotfix and re-run CI`.
    2. **GitHub (Xanh dương):** `Open PR and enter merge queue` ──► `Branch protection CODEOWNERS + checks` ──► `Workflow dispatch concurrency + cancel stale` ──► `Protected merge or release tag`.
    3. **CI Pipeline (Xanh lá):** `Build matrix ubuntu, macOS, node 18/20` ──► `Run unit and integration tests` ──► `Security scans SAST, deps, secrets` ──► `Upload coverage, artifacts, and SARIF` ──► cổng điều kiện thoi vàng `All green?` (`fail` quay về Developer, `pass` sang Release & Approval).
    4. **Release & Approval (Vàng):** `Publish signed artifact SBOM + attestation` ──► `Deploy preview or staging environment` ──► `Run E2E suite and QA validation` ──► `Prod environment reviewers + wait timer` ──► `Scoped prod secrets released to deploy job` ──► cổng điều kiện `Approved?` (`changes requested` quay về Developer, `approved` sang Protected merge & Production).
    5. **Production & Ops (Xanh ngọc):** `OIDC assumes cloud deploy role` ──► `Progressive prod deploy canary batches + flags` ──► `Smoke tests, SLOs, and error budgets` ──► cổng điều kiện `Healthy?` (`healthy` ──► `Serve production traffic`; `rollback` ──► `Auto rollback last good release` ──► quay về Developer `Patch hotfix and re-run CI`).

- **Lệnh kết xuất chế độ tối (`frame_0016`):**
  ```bash
  ~/diagrams > excalidraw scenes render 2ijfvoFK9LT --out ci-cd.png --theme dark
  ```
  - Trả về cùng cấu trúc JSON (`width: 1216`, `height: 2047`, `mimeType: "image/png"`) và ngay lập tức hiển thị bản vẽ nền đen (`dark theme`) sắc nét bên phải màn hình (`frame_0016`).

---

### 2.5. Lập Trình Hóa Toàn Bộ Bộ Sưu Tập — `Script whole collections` (`00:25` – `00:29` | `frame_0017` – `frame_0020`)

- **Tiêu đề trên màn hình:** `Script whole collections`
- Vì mặc định khi pipe (`|`), đầu ra của `@excalidraw/cli` là JSON chuẩn (thay vì bảng khi có `-o table`), lập trình viên có thể kết hợp với `jq` và `xargs` để tự động hóa việc kết xuất toàn bộ scenes trong một collection chỉ bằng một dòng lệnh duy nhất (`frame_0018`):
  ```bash
  ~/diagrams > excalidraw collections scenes list 9gUS8M9SLcb | jq -r '.data[].metadata.id' | xargs -I{} excalidraw scenes render {} --out {}.png
  ```
- **Chuỗi JSON kết quả liên tiếp xuất hiện trên terminal kèm hiệu ứng xếp chồng 5 bức ảnh PNG bên phải (`frame_0018` – `frame_0020`):**
  1. Scene 1 (`9QaZzJ6KtAz` — *IBIS*, sơ đồ xác thực Client/Server/Database):
     ```json
     {
       "sceneId": "9QaZzJ6KtAz",
       "path": "/home/dev/diagrams/9QaZzJ6KtAz.png",
       "mimeType": "image/png",
       "width": 788,
       "height": 465,
       "frameId": null
     }
     ```
  2. Scene 2 (`2ijfvoFK9LT` — *GitHub Actions CI/CD Swimlane*, `1216 × 2047`) & Scene 3 (`6F8vqBQy3wz` — *Habit Tracking App*, `width: 2048, height: 1376`).
  3. Scene 4 (`9akmDng4qaX` — *Devices*, bộ icon 3D isometric phần cứng máy tính, `frame_0019`):
     ```json
     {
       "sceneId": "9akmDng4qaX",
       "path": "/home/dev/diagrams/9akmDng4qaX.png",
       "mimeType": "image/png",
       "width": 1070,
       "height": 812,
       "frameId": null
     }
     ```
  4. Scene 5 (`9UMBeqpPrb9` — *Networking*, bộ linh kiện mạng & server rack, `frame_0020`):
     ```json
     {
       "sceneId": "9UMBeqpPrb9",
       "path": "/home/dev/diagrams/9UMBeqpPrb9.png",
       "mimeType": "image/png",
       "width": 1440,
       "height": 931,
       "frameId": null
     }
     ```

---

### 2.6. JSON Thuần Chủng Cho AI Agents — `Plain JSON, ready for agents` (`00:30` – `00:39` | `frame_0021` – `frame_0028`)

- **Tiêu đề trên màn hình:** `Plain JSON, ready for agents` (`frame_0021`, `frame_0022`)
- **Lệnh thực thi (`frame_0022`):**
  ```bash
  ~/diagrams > excalidraw scenes content get 2ijfvoFK9LT | jq '.elements | length'
  106
  ```
  *(Con số `106` được khoanh tròn bằng nét vẽ tay màu tím đặc trưng của Excalidraw trên `frame_0022`).*
- **Outro (`frame_0023` – `frame_0028`):** Quay lại gợi ý lệnh cài đặt `npm i -g @excalidraw/cli` (`frame_0024`) và hiệu ứng logo **EXCALIDRAW** (`frame_0025` – `frame_0027`).

---

## 3. Bảng Tổng Hợp Tham Chiếu Lệnh `@excalidraw/cli`

| Nhóm Chức Năng | Câu Lệnh CLI | Mô Tả & Đầu Ra |
| :--- | :--- | :--- |
| **Cài đặt** | `npm i -g @excalidraw/cli` | Cài đặt toàn cục package CLI chính thức từ Excalidraw (2 packages, ~1s). |
| **Đăng nhập** | `excalidraw login` | Đăng nhập tương tác vào `https://api.excalidraw.com` qua API Key (`plus.excalidraw.com/docs/api/authentication`). |
| **Kiểm tra phiên** | `excalidraw whoami -o table` | Hiển thị `apiUrl`, `keyType`, `keySource`, `workspace.id`, `workspace.name`, `subscriptionStatus`, `userCount`. |
| **Duyệt Collection** | `excalidraw collections scenes list <COLLECTION_ID> [-o table]` | Liệt kê các scenes trong collection. Có `-o table` xuất bảng ASCII; không có `-o table` xuất JSON chuẩn có mảng `.data[].metadata.id`. |
| **Render 1 Scene** | `excalidraw scenes render <SCENE_ID> --out <FILE.png> [--theme dark]` | Kết xuất scene thành ảnh PNG ở chế độ sáng (mặc định) hoặc tối (`--theme dark`), trả về JSON (`sceneId`, `path`, `mimeType`, `width`, `height`, `frameId`). |
| **Render Hàng Loạt** | `excalidraw collections scenes list <ID> \| jq -r '.data[].metadata.id' \| xargs -I{} excalidraw scenes render {} --out {}.png` | Pipeline kết xuất toàn bộ các bản vẽ trong một collection thành các file PNG tương ứng. |
| **Đọc JSON Nội Dung** | `excalidraw scenes content get <SCENE_ID>` | Trả về toàn bộ nội dung bản vẽ dạng Plain JSON (bao gồm mảng `.elements`), sẵn sàng cho AI Agent hoặc `jq` phân tích. |

---

## 4. Ứng Dụng Thực Chiến Cho Hệ Sinh Thái AI Coding Agents

1. **Đầu Ra Mặc Định Thân Thiện Với Máy (Machine-First JSON Output):**
   - Thiết kế của `@excalidraw/cli` tuân thủ triết lý CLI hiện đại cho AI Agents: các lệnh như `scenes render`, `collections scenes list` (khi pipe) và `scenes content get` đều trả về **Plain JSON** có cấu trúc chặt chẽ, trong khi con người có thể thêm cờ `-o table` khi cần xem dạng bảng.
2. **Đọc Hiểu Sơ Đồ Kiến Trúc Với Chi Phí Token Thấp (`scenes content get`):**
   - Với `excalidraw scenes content get <SCENE_ID>`, AI Agent có thể đọc trực tiếp 106 phần tử (`elements`) của sơ đồ CI/CD dưới dạng JSON (chứa nhãn văn bản, tọa độ, liên kết mũi tên) để nắm bắt luồng kiến trúc hiện tại của dự án trước khi viết code.
3. **Vòng Lặp Tự Kiểm Chứng Trực Quan (Visual Self-Verification Loop):**
   - Kết hợp `excalidraw scenes render <ID> --out diagram.png` với khả năng đọc ảnh đa phương thức (Multimodal Vision) của Agent: Agent có thể tự kết xuất sơ đồ ra PNG, kiểm tra trực quan bố cục (cả Light mode lẫn Dark mode `--theme dark`) và đính kèm tự động vào tài liệu PR hoặc kho lưu trữ kiến trúc.
