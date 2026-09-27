# I Built (And Shipped) a 3D Game With Claude Opus 5.5 | Full Workflow & No-Engine Stack

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=3QwU8TM7Rag)
- **Watch Skill ID:** `47ee6151d3451c4d`
- **Kênh phát hành:** Chong-U — AI Oriented Dev
- **Category:** #ai-agents, #game-dev
- **Date Processed:** 2026-09-28
- **Duration:** 18:11
- **Transcript File:** [built_and_shipped_3d_game_with_claude_opus_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/built_and_shipped_3d_game_with_claude_opus_transcript.txt)

---

## 1. Bối cảnh & Kết quả thực tế: Tự làm và phát hành game 3D trong 6.5 giờ

Trong video này, nhà phát triển Chong-U chia sẻ toàn bộ quy trình xây dựng và phát hành thành công một tựa game 3D hoàn chỉnh mang tên **Pressure Washing** (Rửa xe / Xịt rửa sân vườn áp lực cao) chỉ bằng AI Coding Agents:
- **Thời gian hoàn thiện:** Khoảng **6.5 giờ** chạy nền (tác giả chỉ đưa prompt, để agent tự làm việc trong background và kiểm tra kết quả định kỳ).
- **Kết quả người dùng:** Ngay sau khi phát hành trên cổng web **WaveDash**, game đã thu hút gần **1,000 người chơi thực tế**, thời gian chơi trung bình khoảng 2 phút/phiên trên cả desktop lẫn điện thoại di động mà không cần cài đặt.
- **Quy mô dự án:** Bao gồm đầy đủ logic gameplay, mô hình nhân vật 3D cử động mượt mà, môi trường khu phố, hệ thống nhiệm vụ, nâng cấp vòi xịt, hiệu ứng vật lý dòng nước và màn chơi hướng dẫn tân thủ (FTUE).

---

## 2. Kiến trúc Kỹ thuật Đột phá: "No-Engine" (WASM + WebGPU + Rust)

Khác với đại đa số game dev truyền thống dựa vào Unity, Unreal hay Three.js, tác giả lựa chọn một stack thuần công nghệ web thế hệ mới: **Rust $\to$ WebAssembly (WASM) + WebGPU**:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          NO-ENGINE WEB ARCHITECTURE (WASM + WebGPU)                         │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  Lý thuyết cũ (Three.js / Game Engine):                                                     │
│  Lớp trừu tượng cao cấp (High-level Abstractions) được thiết kế cho con người đọc/viết.    │
│                                                                                             │
│  Tư duy mới thời AI (Eric Provencher - OpenAI):                                            │
│  Khi LLM (Opus 5.5) là người viết code, con người không cần đọc từng dòng API nữa.        │
│  Ta có thể bỏ qua toàn bộ lớp middleware cồng kềnh, viết trực tiếp bằng Rust và WebGPU!     │
│                                                                                             │
│  Lợi ích cốt lõi:                                                                          │
│  ├── Tần số quét 120Hz mượt mà (Xử lý vật lý dòng nước 120 lần/giây).                       │
│  ├── 0-Install (Không cần cài đặt, mở link trình duyệt là chơi ngay).                       │
│  ├── Đa nền tảng native (Hỗ trợ hoàn hảo cả màn hình cảm ứng di động lẫn chuột PC).         │
│  └── Dung lượng siêu nhẹ, không rác runtime từ engine.                                      │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Đội hình Mô hình AI & Chuỗi công cụ (Model Stack Pipeline)

Tác giả không dùng duy nhất một mô hình mà phối hợp một hệ sinh thái AI chuyên biệt theo vai trò:

| Công cụ / Mô hình | Vai trò chuyên biệt |
|---|---|
| **Claude Opus 5.5** | **Kiến trúc sư trưởng & Coder chính:** Viết logic game bằng Rust, WebGPU shaders, hệ thống mô phỏng vật lý lò xo (spring-damper) cho vòi xịt nước, cơ chế nhiệm vụ. |
| **Claude 3.5 Sonnet** | **Sub-agents Hậu cần & Quan sát:** Chạy ngầm trong background để chụp ảnh màn hình game, ghi nhật ký prompts, commit code lên Git. |
| **GPT Images 2.5** | **Concept Art & Mockups:** Tạo ý tưởng trực quan giao diện ban đầu trên mobile/desktop và tạo *Turnaround Sheet* (ảnh nhân vật 3 góc: trước, nghiêng, sau). |
| **Tripo3D API** | **Sinh mô hình 3D:** Chuyển đổi ảnh Turnaround 2D thành file 3D Mesh (GLTF/OBJ). |
| **Blender API (Headless)** | **Rigging & Weight Painting:** Tự động gắn khung xương (armature), cân chỉnh trọng số chuyển động và gắn đầu vòi xịt vào bàn tay nhân vật. |
| **ElevenLabs API** | **Audio & SFX:** Tạo hiệu ứng âm thanh xịt nước áp lực cao, tiếng động cơ máy bơm. |
| **WaveDash** | **Nền tảng phát hành:** Hosting trực tiếp game WebAssembly, chia sẻ link chơi ngay. |

---

## 4. Toàn bộ Quy trình 6 Bước (The 6-Step Workflow)

```
[1. Brainstorm & Upfront Alignment] (Ý tưởng casual rửa sân, mockups qua GPT Images)
                    │
                    ▼
[2. Song song hóa Sub-agents] (Chia 3 luồng chạy ngầm độc lập)
   ├── Luồng A: Tạo Turnaround Sheet 2D ──► Tripo3D ──► Auto Rigging trong Blender
   ├── Luồng B: Tạo Props môi trường (cây cối, ô tô, nhà cửa) thành file Blender riêng
   └── Luồng C: [Gray Box Level Gym] (Opus 5.5 code ngay lõi gameplay bằng khối hộp đơn giản)
                    │
                    ▼
[3. Playtest & Tinh chỉnh Cảm giác Lõi (Core Feel & Juiciness)]
   (Thử nghiệm cảm giác xịt nước, lực cản lò xo mô phỏng quán tính dòng nước ở 120Hz)
                    │
                    ▼
[4. Model Preview Gym & Ghép nối Tài nguyên]
   (Kiểm tra scale mô hình, tự động kết nối đầu vòi xịt vào bàn tay nhân vật khi bắn)
                    │
                    ▼
[5. Đánh bóng Bầu không khí (Atmosphere Polish)]
   (Bọc Skybox 360 độ tạo chiều sâu, sinh thêm khu phố xung quanh sân vườn)
                    │
                    ▼
[6. Thiết kế Trải nghiệm Tân thủ (FTUE) & Ship]
   (Level 1: Vòi xịt xòe lá rụng ──► Level 2: Vòi phản lực tẩy vết dầu ──► Level 3: Áp lực thời gian)
```

---

## 5. Sáu bài học sống còn khi phát triển Game bằng AI

### 1. "Stop One-Shotting Games!" (Ngừng ảo tưởng One-Shot)
- Các video one-shot prompt để máy chạy 24–48 tiếng chỉ là "Tech Demo" phô diễn công nghệ, hoàn toàn không có khả năng kiểm soát sáng tạo.
- Muốn làm một tựa game thực thụ, bắt buộc phải có **bước đồng thuận phía trước (Upfront Alignment)**: Cho AI xem mockups, hỏi *"Do you understand?"* để agent xác nhận định hướng trước khi gõ một dòng code nào.

### 2. Nguyên tắc Tách rời Tài nguyên (Decoupled Blender Assets)
- Không bao giờ nhồi nhét việc vẽ nhân vật, làm cảnh quan trực tiếp vào codebase game.
- Nhân vật có khung xương phải nằm trong file Blender riêng để rig và animate độc lập.
- Các đạo cụ tĩnh (cây cối, xe cộ, nhà cửa) nằm trong file môi trường riêng biệt.

### 3. Gray Box Level Gym (Cơ chế trước, Mỹ thuật sau)
- Trong lúc các sub-agent đang mất hàng chục phút sinh model 3D ở background, đừng ngồi chờ!
- Yêu cầu Opus 5.5 dựng ngay một **phòng gym hình hộp xám (Gray Box Gym)** bằng các khối hình học cơ bản (cubes, cylinders) để kiểm thử gameplay loop: cảm giác xịt nước có "đã tay" (juicy) không, phản lực có chuẩn không.

### 4. Điều phối Sub-agent song song có kỷ luật
- Tác giả dùng tới 21 sub-agents trong suốt dự án nhưng **không bao giờ chạy đồng thời 21 agent**.
- Mô hình chuẩn: **1 Agent chính (Opus 5.5) + 1-2 Sub-agents chạy task song song độc lập (Art/Rigging) + 1 Sub-agent hậu cần nhẹ (Sonnet 3.5) ghi log/chụp ảnh màn hình**.

### 5. Bí quyết nâng tầm thẩm mỹ: Skybox 360 & FTUE
- **Skybox:** Chỉ cần một bức ảnh panorama 360 bọc quanh thế giới, trò chơi lập tức thoát khỏi cảm giác "đồ họa sinh viên" và khoác lên bầu không khí chuyên nghiệp.
- **FTUE (First-Time User Experience):** Game hay không bắt người chơi đọc tài liệu. Hãy thiết kế cơ chế qua màn chơi: bắt đầu bằng việc dọn lá cây đơn giản với vòi xòe, sau đó nâng cấp lên vòi tia áp lực cao để đánh bật vết dầu loang khó nhằn.

### 6. Dùng AI làm Công cụ Tự Giáo dục (AI as an Educator)
- Thay vì để AI làm xong rồi bỏ đó, hãy hỏi AI: *"Làm cách nào bạn tính toán được góc hướng của tia nước?"*.
- Opus 5.5 giải thích thuật toán: vòi xịt thực chất được mô phỏng như một **hệ lò xo đàn hồi (Spring-Damper system)** đuổi theo con trỏ chuột, giúp tái hiện hoàn hảo độ trễ quán tính tự nhiên của dòng nước có áp lực.

---

## 6. Tổng kết chi phí & Giá trị thực tiễn

- **Chi phí ước tính:** ~$233 API tokens (tương đương với một phần gói Claude Max 20X plan).
- **Ý nghĩa với Solo Developer:** Đây là minh chứng rõ rệt nhất cho thấy một lập trình viên độc lập (Solo Dev) hoàn toàn có thể sở hữu năng lực sản xuất của một studio game mini (từ Game Designer, 3D Modeler, Rigger, Gameplay Programmer đến Audio Artist) với tốc độ đóng gói và phát hành tính bằng giờ.
