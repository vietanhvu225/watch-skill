# 🥷 Mark of the Ninja — Level 1 ("Ink & Dreams"): Giải Phẫu Lượt Chơi Chậm (Slow Walkthrough Analysis)

> **Mục tiêu nghiên cứu:** Rút trích bài học về cách thiết kế cơ chế hướng dẫn (onboarding), vai trò nhân vật phụ (NPC guidance), hệ thống phản hồi giác quan và tạo dựng lựa chọn cho người chơi trong **dự án game 2D stealth về Đặc công Việt Nam** (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện rồi tản ra).  
> **Nguồn video khảo sát:** [Mark of the Ninja - Gameplay Walkthrough - Part 1 [Level 1: Ink & Dreams] — theRadBrad](https://www.youtube.com/watch?v=e1UnoyKqeMs)  
> **Thời lượng:** 20:21 | **Tệp thông tin nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/sources.md) | **Transcript thô:** [transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/transcript.txt)  
> **Tài liệu đối chiếu:** [mark_of_the_ninja_level1_reverse_engineering.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_reverse_engineering.md) & [game_walkthrough_reverse_engineering_findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/game_walkthrough_reverse_engineering_findings.md)

---

## 1. Metadata & Giới Hạn Kỹ Thuật (Metadata & Limitations)

* **Kênh & Người chơi:** `theRadBrad` — Lối chơi mang tính khám phá, chưa tối ưu (exploratory / unoptimized play); người chơi đã từng chạy thử màn này trước đó (xác nhận tại phút 01:26 và 04:13), nhịp độ chậm rãi, thường dừng lại quan sát bối cảnh, đọc thông báo hướng dẫn và thử nghiệm nút bấm.
* **Thời lượng video:** 20 phút 21 giây (bao gồm khoảng 1 phút 15 giây đầu/cuối dành cho lời chào, bình luận của YouTuber và cutscene; không phải hoàn toàn là thời gian chơi thuần). Lượt chơi của Centerstrain01 dài 11 phút 06 giây theo nhịp nhanh, quen thuộc.
* **Nền tảng & Phiên bản:** Giao diện hiển thị layout nút bấm Xbox (`A`, `B`, `X`, `Y`, `LT`, `RT`). Có thể chơi trên console Xbox 360 hoặc PC cắm tay cầm Xbox; chưa có dữ liệu phần cứng cụ thể để khẳng định tuyệt đối. Video đăng tải ngày 08/09/2012 trùng ngày phát hành gốc của game. Độ khó hiển thị Normal.
* **Đặc tính cảnh quay:** Video được thu trực tiếp không cắt dựng giấu lỗi (raw unedited), không tua nhanh, ghi nhận trung thực các sai lầm của người chơi (bị phát hiện, còi báo động ré lên tại phút 12:24, nấp thoát ly).
* **Trang bị nhân vật (Noisemaker):** Nhân vật đã có sẵn trang bị `Noisemaker` phụ trợ bên cạnh `Darts` từ đầu màn (hiển thị trên D-Pad ở phút 03:15); nguyên nhân có Noisemaker (trạng thái save file, thiết lập profile hay phiên bản game) chưa được xác minh do thiếu dữ liệu hệ thống.
* **Phân tách âm thanh:** Đã tách bạch giữa lời thoại có phụ đề trong game (Ora, Sensei Azai, lính Hessian) và lời bình luận cảm xúc cá nhân của theRadBrad. Các âm thanh môi trường nhỏ có thể bị tiếng bình luận lấn át một phần.
* **Công cụ xác minh:** Sử dụng kết hợp `faster-whisper` (bóc tách transcript thoại), `ffmpeg` (trích xuất frame bằng chứng tại các mốc giây chính xác), và OCR đối chiếu trực tiếp trên giao diện HUD.

---

## 2. Phân Tích 12 Tình Huống Quan Trọng (Detailed Encounter Matrix)

Dưới đây là 12 tình huống được bóc tách theo khung phân tích kỷ luật 6 cột, tập trung vào hành vi quan sát được và bài học cho game đặc công:

### Tình huống 1: Đánh Thức & Vai Trò Hướng Dẫn Của Nữ Đồng Hành Ora (01:45 - 02:45)
* **Bằng chứng thị giác:** [01_01m50s_intro_awakening.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/01_01m50s_intro_awakening.jpg) | [02_02m10s_ora_sword_dialogue.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/02_02m10s_ora_sword_dialogue.jpg) | [03_02m30s_first_hide_vase.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/03_02m30s_first_hide_vase.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `01:45 - 02:45` |
| **Điều thấy/nghe trực tiếp** | Nhân vật chính tỉnh dậy trong đền. Ora xuất hiện, cảnh báo quân địch tấn công. Thoại game có phụ đề: *"Wait, where's your sword? Stick to the darkness until you find one... They're coming this way! Find a place to hide! You can't kill without your weapons."* Nhắc nút `PRESS B TO USE HIDING SPOTS`. Người chơi ấn `B` nấp vào bình phong; lính cầm đèn pin bước vào phòng, quét ngang qua mặt mà không thấy. Khi lính đi qua, hiện icon `UNHIDE`, người chơi ấn `B` thoát ra, nhận `+200 UNDETECTED`. |
| **Lời người chơi giải thích** | *"The game is very dark... You can pick hiding spots like this right here... You wait for it... Most of the first level you're just collecting all your stuff... So he doesn't see you."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Rất cao]** Ora đóng vai trò là "tiếng nói trực giác / Leader dẫn dắt" bằng lời nhắc nhở ngắn gọn, súc tích (dưới 10 từ/câu), gắn liền trực tiếp với mối nguy hiển hiện trước mắt. Ora KHÔNG trực tiếp can thiệp vật lý vào kẻ địch, nhường 100% quyền hành động cho người chơi. |
| **Điều chưa biết** | Nếu người chơi không bấm nấp mà đứng trơ trọi trong ánh đèn pin ở căn phòng đầu tiên này, lính có nổ súng giết ngay hay có độ trễ cảnh cáo? |
| **Bài học cho Game Đặc công** | Mô hình **Leader NPC hỗ trợ từ xa qua bộ đàm hoặc khẩu lệnh ngắn**: Đưa ra chỉ dẫn ngay trước khi mối nguy xuất hiện 3-5 giây. Chỉ dẫn cần chỉ rõ điều kiện sinh tồn cốt lõi (*"Nấp vào bụi cây bên trái ngay, tuần tra đang tới!"*), không làm hộ hành động của người chơi. |

---

### Tình huống 2: Tín Hiệu Sóng Âm Bước Chạy & Cảnh Báo Tức Thì (03:00 - 03:15)
* **Bằng chứng thị giác:** [04_03m04s_running_sound_ring.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/04_03m04s_running_sound_ring.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `03:00 - 03:15` |
| **Điều thấy/nghe trực tiếp** | Người chơi giữ nút `RT` để chạy nhanh. Một **vòng tròn màu đỏ mở rộng** xuất hiện quanh bước chân. Ora lập tức nhắc thoại: *"Hold up. Run and they'll be able to hear it."* Bên dưới lầu, 2 lính đánh thuê đang trò chuyện về việc lấy trộm cờ đền. theRadBrad lập tức nhả nút chạy, chuyển sang đi bộ rón rén. |
| **Lời người chơi giải thích** | *"I just like got detected... Not gonna run... Hold up, run and they'll be able to hear it."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Cao]** Game phản hồi sai lầm của người chơi bằng cả 2 kênh: Kênh hình ảnh trực quan (vòng tròn sóng âm đỏ) và Kênh âm thanh/thoại NPC can thiệp kịp thời trước khi lính kịp phát hiện. |
| **Điều chưa biết** | Bán kính phát âm thanh bước chạy mở rộng tối đa bao nhiêu đơn vị hiển thị? Sóng âm này có bị tường dày chặn hoàn toàn hay truyền qua một phần? |
| **Bài học cho Game Đặc công** | Tuyệt đối cần một tín hiệu trực quan cho tiếng ồn (ví dụ sóng âm lan tỏa trên mặt đất bùn/nước) để người chơi tự tin biết bước chân của mình có với tới tai lính gác hay không. |

---

### Tình huống 3: Tiếp Cận Lính, Đánh Lạc Hướng & Ám Sát Lưng (Stealth Kill) (03:30 - 04:15)
* **Bằng chứng thị giác:** [05a_03m36s_kill_prompt_behind_guard.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/05a_03m36s_kill_prompt_behind_guard.jpg) | [05b_03m40s_stealth_kill_execution.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/05b_03m40s_stealth_kill_execution.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `03:30 - 04:15` |
| **Điều thấy/nghe trực tiếp** | theRadBrad ném thiết bị tạo âm thanh (`NOISE MAKER`), nhận `+150 ITEM DISTRACTED`. Lính quay lưng lại điều tra. Ninja áp sát từ phía sau: Nút bấm trên HUD chuyển thành `X: KILL | B: CANCEL`. Người chơi ấn `X`, hoạt ảnh ninja dùng dao găm kết liễu lính gác chớp nhoáng, nhận `+400 SILENT ASSASSIN`, mở khóa Achievement `5G - Stealth Assassin`. Ngay sau đó hiện prompt `B: GRAB/DROP BODY`. Người chơi kéo xác lính một đoạn ngắn. |
| **Lời người chơi giải thích** | *"I'm gonna throw a noisemaker... Down! Stealth kills are so crazy... Stealth assassin... Let me try and drag in the body..."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Rất cao]** **BÁC BỎ GIẢ THUYẾT CŨ:** Người chơi ĐÃ CÓ khả năng ám sát (Stealth Kill) ngay từ Room 2 (phút 03:36) chứ không hề bị tước vũ khí suốt cả màn 1! Việc Centerstrain01 không giết ai ở video trước thuần túy là do lựa chọn lối chơi Ghost, không phải giới hạn hệ thống. |
| **Điều chưa biết** | Đòn kết liễu này dùng dao găm cá nhân hay đoản kiếm? Tại sao Ora nói "Where's your sword" ở đầu game nhưng đến đây nhân vật đã có vũ khí ám sát? |
| **Bài học cho Game Đặc công** | Trong game đặc công, chiến sĩ luôn có dao găm cá nhân để hạ gục lén lút trong cự ly gần; việc tước vũ khí chỉ nên là câu đố cục bộ trong một phân cảnh đặc biệt (bị bắt giam, vượt ngục), không nên kéo dài cả màn chơi gây ức chế. |

---

### Tình huống 4: Giới Thiệu Dây Đu Trần Nhà (Grappling Hook) (04:15 - 04:40)
* **Bằng chứng thị giác:** [06_04m20s_grappling_hook_prompt.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/06_04m20s_grappling_hook_prompt.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `04:15 - 04:40` |
| **Điều thấy/nghe trực tiếp** | Gặp bức tường cao không thể nhảy với tới. Màn hình tạm dừng hiện bảng hướng dẫn: *"GRAPPLING HOOK: The grappling hook allows you to latch on to grapple points using Y."* Trên trần nhà xuất hiện vòng tròn neo móc phát sáng. Người chơi ấn `Y`, ninja phóng móc kéo vọt người lên xà nhà nhẹ nhàng. |
| **Lời người chơi giải thích** | *"Grappling hook. We've already got that... Let me go to the left right here..."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Cao]** Công cụ di chuyển cao độ được giới thiệu ngay khi người chơi đối mặt với ngõ cụt theo phương ngang, tuân thủ nguyên tắc "Vấn đề xuất hiện trước, giải pháp trao tay ngay sau". |
| **Điều chưa biết** | Dây móc có giới hạn tầm xa tối đa là bao nhiêu? Có bị đứt nếu lính bắn trúng dây khi đang đu không? |
| **Bài học cho Game Đặc công** | Thay vì đu móc kỳ ảo, game đặc công có thể dùng **dây thừng chiến thuật, móc leo tường hoặc động tác đẩy bạn lên tường (co-op boost)** tại các vị trí chướng ngại vật cao. |

---

### Tình huống 5: AI Nghe Thấy Tiếng Động & Quy Trình Lùng Sục (05:00 - 05:35)
* **Bằng chứng thị giác:** [07_05m15s_guard_investigate_noise.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/07_05m15s_guard_investigate_noise.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `05:00 - 05:35` |
| **Điều thấy/nghe trực tiếp** | theRadBrad ném một vật thể phát tiếng động trúng sàn gạch gần lính. Lính giật mình dừng bước tuần tra, trên đầu hiện biểu tượng dấu hỏi `?`. Lính xoay người bước chậm về phía phát ra âm thanh, đèn pin lia quét dưới đất, kèm câu thoại: *"Huh? What was that noise? Show yourself."* Ninja đu trên xà nhà ngay trên đầu lính gác mà lính không hề nhìn lên trần. |
| **Lời người chơi giải thích** | *"What? I didn't get him within a noise barrier... Almost that noise. Go check that out. Show yourself... It's pretty much easy to get around distracting everybody."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Rất cao]** **Thiết kế AI có chủ đích (Predictable AI):** Lính gác chỉ quét tầm nhìn theo phương ngang phía trước mặt, hoàn toàn không có nhận thức nhìn lên trần nhà trừ khi người chơi tạo tiếng ồn trực tiếp trên xà gồ. Điều này tạo ra "Vùng an toàn cao độ" (Vertical Safe Zone) cho người chơi. |
| **Điều chưa biết** | Nếu ném liên tiếp 2 vật thể ở 2 điểm ngược nhau, lính sẽ ưu tiên đi đến đâu? (Sẽ kiểm chứng ở Mục 3). |
| **Bài học cho Game Đặc công** | Thiết kế AI lính tuần tra cần có góc nhìn hạn chế (nón tầm nhìn không ngước lên cao) để người chơi trèo lên cây hoặc mái nhà ngói luôn cảm nhận được ưu thế chiến thuật rõ rệt. |

---

### Tình huống 6: Focus Mode — Đóng Băng Thời Gian Ngắm Bắn Phi Tiêu (06:55 - 07:20)
* **Bằng chứng thị giác:** [09_07m00s_focus_mode_freeze_time.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/09_07m00s_focus_mode_freeze_time.jpg) | [10_07m15s_light_shatter_prompt.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/10_07m15s_light_shatter_prompt.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `06:55 - 07:20` |
| **Điều thấy/nghe trực tiếp** | Phụ đề lời thoại Ora hiện lên: *"The ink from your tattoo has honed your senses. Focus your thoughts and you can freeze time in your mind."* Tiếp theo là hướng dẫn phá đèn: *"If you need more cover, you could always destroy the lights. But shattering one will make a loud noise, so be ready for them to react."* Khi người chơi giữ nút ngắm (`LT`), màn hình thu tối 4 góc viền, chuyển động vật thể xung quanh dừng hẳn, âm thanh trở nên trầm đục như ở dưới nước, xuất hiện tia quỹ đạo laser nối thẳng đến bóng đèn. |
| **Lời người chơi giải thích** | *"The ink from your tattoo has honed your senses... Yep, I was already doing that... If you need more cover... Got you, bitch!"* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Rất cao]** Cơ chế Focus được giới thiệu bằng **cặp phạm trù Lợi ích & Đánh đổi**: Phá đèn giúp tạo bóng tối để ẩn nấp, NHƯNG tiếng đèn vỡ sẽ tạo sóng âm lớn khiến lính quay lại kiểm tra. |
| **Điều chưa biết** | Focus mode có thanh đo thể lực (mana/stamina) giới hạn thời gian giữ không, hay người chơi có thể giữ vô tận? |
| **Bài học cho Game Đặc công** | Trong game đặc công, có thể thiết kế "Chế độ Nín thở / Ngắm kỹ" (Breath Hold / Focus Aim) làm chậm nhịp độ xung quanh để người chơi bình tĩnh căn tọa độ ném lựu đạn khói, bắn súng giảm thanh hoặc phóng dao. |

---

### Tình huống 7: Người Chơi Sai Lầm, Bị Phát Hiện & Thoát Ly Ẩn Nấp Hồi Phục (08:35 - 09:15)
* **Bằng chứng thị giác:** [11a_08m39s_dart_tutorial_alert.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/11a_08m39s_dart_tutorial_alert.jpg) | [11b_08m48s_penalty_detected.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/11b_08m48s_penalty_detected.jpg) | [11c_08m57s_hold_detected.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/11c_08m57s_hold_detected.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `08:35 - 09:15` |
| **Điều thấy/nghe trực tiếp** | theRadBrad nhảy xuống từ trần nhà trúng ngay nón ánh sáng đèn pin của lính tại 08:39. HUD hiển thị hình phạt trừ điểm `-300` cùng icon cảnh báo `DETECTED` (xác nhận tại frame 08:48). Lính hô lớn: *"Show yourself! Hey, hey!"* và nổ súng bắn thẳng về phía ninja. theRadBrad bị trúng đạn, vội vàng điều khiển ninja lùi lại căn phòng trước đó, nhảy vào bình phong và bấm `B` nấp (`HIDE`). Hai tên lính đuổi theo tới cửa phòng, lia đèn pin qua lại. Sau một khoảng thời gian ngắn không nhìn thấy ninja, lính hạ súng, trên đầu hiện biểu tượng dấu hỏi `?`, rồi lững thững quay trở lại hành lang tuần tra. Nhạc nền căng thẳng hạ dần về bình thường. |
| **Lời người chơi giải thích** | *"Hey, see my sights. Are you shitting me? I didn't think they were there... What was that noise? Oh shit, there's two of them... Ah, shit! I'm not really good at this stealth thing, am I? Yeah, the detection in this game, it's very easy to get spotted."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Cao]** Game không xử thua ngay lập tức khi người chơi rơi vào tầm nhìn kẻ địch mà mở ra một cơ hội thoát ly (Recovery Window): Nếu người chơi chạy thoát khỏi tầm nhìn thẳng và ẩn nấp kịp thời, lính sẽ mất dấu sau một khoảng thời gian tìm kiếm cục bộ và ngừng truy đuổi. *(Lưu ý: Không suy diễn tên trạng thái FSM nội bộ hay trích dẫn sai bài nói GDC).* |
| **Điều chưa biết** | Thời gian chính xác từ lúc mất dấu đến khi lính hạ súng ngừng tìm kiếm là bao lâu nếu đo bằng đồng hồ đếm chuẩn? Lịch trình tuần tra sau đó có bị thay đổi hay lặp lại hoàn toàn chu kỳ cũ? |
| **Bài học cho Game Đặc công** | **BÀI HỌC VÀNG CHO GAME STEALTH:** Tuyệt đối không làm cơ chế "Bị phát hiện = Game Over". Phải luôn cho người chơi cơ hội tung lựu đạn khói, lặn xuống mương nước hoặc nhảy vào bụi rậm cắt đuôi kẻ địch để tổ chức lại đợt thâm nhập. |

---

### Tình huống 8: Đánh Lạc Hướng Bằng Chuông Đồng & Cảm Biến Nhìn Xuyên Cửa (10:10 - 10:40)
* **Bằng chứng thị giác:** [12_10m15s_gong_sound_wave.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/12_10m15s_gong_sound_wave.jpg) | [13_10m25s_door_peeking_sensor.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/13_10m25s_door_peeking_sensor.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `10:10 - 10:40` |
| **Điều thấy/nghe trực tiếp** | Trước một cánh cửa đóng kín có chuông đồng treo phía trên. Ora nhắc: *"We need to make him look the other way."* theRadBrad ném phi tiêu vào quả chuông, chuông rung phát ra sóng âm hình tròn lớn, kéo tên lính đứng sát cửa bước lùi ra xa. Khi người chơi tiếp cận cánh cửa đóng kín, Ora nhắc tiếp: *"See that door? Don't open it yet. Just lean against it and try to sense what's on the other side."* Ninja tựa người vào cánh cửa, camera mở rộng và căn phòng bên kia hiển thị rõ bóng mờ màu xanh (X-ray silhouette) của lính gác. |
| **Lời người chơi giải thích** | *"We need to make him look the other way... That was so sick. Basically what I could have done is just... wrong that... See that door? Don't open it yet..."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Rất cao]** Cơ chế **Door Peeking (Áp sát cửa cảm nhận)** triệt tiêu hoàn toàn sự bất công khi mở cửa (blind door problem). Người chơi luôn có đầy đủ thông tin về vị trí và hướng nhìn của kẻ địch bên kia cửa trước khi quyết định vặn tay nắm. |
| **Điều chưa biết** | Tiếng mở cửa có phát ra sóng âm không? Nếu lính đang đứng cách cửa 1 mét thì việc mở cửa có làm lộ vị trí không? |
| **Bài học cho Game Đặc công** | Thiết kế cơ chế **"Áp tai vào vách nứa / Nhìn qua khe liếp"**: Cho phép chiến sĩ đặc công nghe thấy tiếng bước chân hoặc thấy bóng mờ của lính địch trong phòng trước khi đạp cửa xông vào. |

---

### Tình huống 9: Thao Tác Đóng Cửa Ngắt Tầm Nhìn & Kích Hoạt Báo Động Khi Tái Xâm Nhập (12:10 - 12:35)
* **Bằng chứng thị giác:** [14a_12m12s_open_door_prompt.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/14a_12m12s_open_door_prompt.jpg) | [14b_12m14s_close_door_prompt.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/14b_12m14s_close_door_prompt.jpg) | [14c_12m24s_alarm_raised_detected.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/14c_12m24s_alarm_raised_detected.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `12:10 - 12:35` |
| **Điều thấy/nghe trực tiếp** | Ở 12:12, theRadBrad mở cửa (`OPEN DOOR B`) mà không dùng door peeking trước. Thấy lính gác ở cự ly gần quay lại, theRadBrad lập tức bấm nút `B` đóng cửa ở 12:14 (`CLOSE DOOR B`). Việc đóng cửa tạm thời ngắt đường nhìn của lính trong khoảng 2 giây. Tuy nhiên, sau đó ở 12:24, theRadBrad mở lại cửa và nhảy qua thì **BỊ PHÁT HIỆN GÂY BÁO ĐỘNG**: HUD hiện điểm phạt `ALARM RAISED -600`, thông báo `SEAL FAILED`, icon `DETECTED` màu đỏ, còi báo động ré lên khắp khu vực. theRadBrad phải dùng ám sát hạ lính để dập tắt mối nguy. |
| **Lời người chơi giải thích** | *"Oh, shit! Let me just close this door... I didn't see that guy there... No, sir... There's a big guy there. Alright, he's dead, that doesn't matter, got to move on."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Cao]** Thao tác đóng cửa cho phép ngắt tầm nhìn trực tiếp tức thời. Tuy nhiên, việc đóng cửa không tự động hóa giải nguy hiểm nếu sau đó người chơi bước qua cửa mà không nắm vị trí lính. Trong tình huống này, đóng cửa chỉ trì hoãn phát hiện trong khoảnh khắc, không ngăn được còi báo động kích hoạt ở 12:24. |
| **Điều chưa biết** | Tiếng đóng cửa có tạo sóng âm cảnh báo lính bước lại gần cửa không? Lính có biết tự mở cửa để sang phòng đối diện không? |
| **Bài học cho Game Đặc công** | Cửa ra vào, cửa hầm ngầm, nắp công sự cần có cơ chế tương tác linh hoạt: mở hé quan sát, mở toang tháo chạy, và khép lại nhẹ nhàng để che chắn tầm nhìn. Người chơi phải tự chịu trách nhiệm định vị địch trước khi mở lại cửa. |

---

### Tình huống 10: Thao Tác Kéo Xác Vào Vùng Tối Dưới Cầu Thang (12:55 - 13:30)
* **Bằng chứng thị giác:** [15_13m15s_dragging_body_shadow.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/15_13m15s_dragging_body_shadow.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `12:55 - 13:30` |
| **Điều thấy/nghe trực tiếp** | Sau khi ám sát một tên lính đứng ở hành lang có đèn, theRadBrad bấm giữ nút `B` nhấc xác lính lên và kéo giật lùi về phía gầm cầu thang ngập trong bóng tối (`DROP BODY` prompt ở 13:12). theRadBrad nói: *"I don't think he's gonna see the body right there. No, he won't. Okay, good."* Trong khoảng thời gian còn lại của phân cảnh (13:15 - 13:40), người chơi di chuyển tiếp sang khu vực khác; không có bất kỳ tên lính nào khác đi ngang qua gầm cầu thang này để đối chứng xem xác có bị phát hiện hay không. |
| **Lời người chơi giải thích** | *"Okay, got him. I'm gonna drag his body. Hopefully I can drag it all the way back over here... That's far as it goes... I don't think he's gonna see the body right there. No, he won't. Okay, good."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Thấp - Giả thuyết chưa kiểm chứng]** Nhận định "xác chết nằm trong bóng tối thì lính tuần tra đi ngang không phát hiện được" chỉ là phỏng đoán cá nhân của người chơi. Tình huống này chưa có lính đối chứng đi ngang qua xác để xác nhận thành quy luật tuyệt đối. |
| **Điều chưa biết** | Cần kiểm chứng: Nếu một tên lính tuần tra đi ngang qua xác chết nằm trong vùng tối hoàn toàn, lính có phát hiện ra xác không? Nón ánh sáng đèn pin có soi thấy xác trong bóng tối không? Vết máu kéo lê trên sàn có kích hoạt AI nghi ngờ không? |
| **Bài học cho Game Đặc công** | Cơ chế giấu xác: Kéo xác địch giấu vào bụi lau sậy, ném xuống hố hầm hoặc mương rãnh để tránh lính đổi ca phát hiện báo động. |

---

### Tình huống 11: Giải Cứu Đồng Đội Bị Treo & Nhận Điểm Seal (14:40 - 15:15)
* **Bằng chứng thị giác:** [16a_14m52s_kill_guard_hostage.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/16a_14m52s_kill_guard_hostage.jpg) | [16b_14m58s_untie_hostage_prompt.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/16b_14m58s_untie_hostage_prompt.jpg) | [16c_15m04s_ninja_rescued_points.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/16c_15m04s_ninja_rescued_points.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `14:40 - 15:15` |
| **Điều thấy/nghe trực tiếp** | theRadBrad ám sát tên lính canh ở 14:52, sau đó tiếp cận đồng đội ninja đang bị trói treo ngược trên xà nhà. Màn hình hiện prompt `HOLD B UNTIL...` ở 14:58. Người chơi giữ `B` để cởi trói. Ở 15:04, HUD hiện điểm thưởng `+500 NINJA RESCUED` và cập nhật mục tiêu `SEAL PROGRESS: Rescue all the ninja 1/4`. Đồng đội nói: *"Don't worry about me. Go save Master Azai!"*. Ngay sau đó ở 15:07, theRadBrad tiến tới cánh cửa bên phải và ấn `OPEN DOOR B` để sang phòng tiếp theo. Camera chuyển theo nhân vật chính ra ngoài, không quay tiếp diễn biến của đồng đội sau đó. |
| **Lời người chơi giải thích** | *"All right, well, I got that. Don't worry, Buckley... Go save Master Azai. Nice! That is so cool. You just find new stuff every time... Leave this person hanging upside down..."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Vừa]** Việc giải cứu đồng đội là mục tiêu phụ tính điểm và tiến độ con dấu (`Seal`). Đồng đội tương tác bằng câu thoại cốt truyện và không đi theo nhân vật chính sang phòng kế tiếp. Hành vi tản ra hay rút lui cụ thể không quan sát được trên màn hình do người chơi lập tức rời phòng. |
| **Điều chưa biết** | Đồng đội sau khi được giải cứu có ở lại phòng, biến mất hay có hoạt ảnh nhảy lên ống thông gió nếu người chơi đứng lại quan sát lâu hơn? |
| **Bài học cho Game Đặc công** | **Mẫu hình phối hợp đồng đội:** Đồng đội NPC xuất hiện tại các điểm chốt then chốt (giải cứu khỏi hầm giam, mở rào kẽm gai), trao đổi ngắn rồi tản ra, giữ trải nghiệm điều khiển chính thuần khiết cho người chơi duy nhất mà không cần AI đồng hành đi kèm liên tục. |

---

### Tình huống 12: Cao Trào Sân Đình — Phá Đèn Rơi Gây Rối Loạn & Tiếp Cận Sensei (18:15 - 19:40)
* **Bằng chứng thị giác:** [18_18m20s_kelly_sensei_azai_courtyard.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/18_18m20s_kelly_sensei_azai_courtyard.jpg) | [19_19m20s_stealth_approach_azai.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/19_19m20s_stealth_approach_azai.jpg)

| Trường thông tin | Chi tiết ghi nhận |
|---|---|
| **Timestamp** | `18:15 - 19:40` |
| **Điều thấy/nghe trực tiếp** | Sân đình trung tâm: Tên trùm Kelly chĩa súng vào Sensei Azai: *"You picked the wrong guys to rob, sensei. It's time for the old man to retire, boys."* Có 3 lính súng trường hạng nặng canh gác. theRadBrad ngắm phi tiêu bắn vào sợi xích treo lồng đèn phía trên lính gác bên trái. Lồng đèn rơi xuống đất vỡ tan, tạo tiếng động lớn kéo lính quay sang kiểm tra. theRadBrad nhảy xuống vùng rãnh tối bên dưới sân đình, luồn qua chân lính và tiếp cận Sensei Azai an toàn, nhận `+400 SILENT ASSASSIN`, kết thúc màn chơi. |
| **Lời người chơi giải thích** | *"You pick the wrong guys to rob Sensei... So basically you gotta rescue this guy... This will fall right on this guy. Now he's distracted... Sneaking up behind fools... There he goes! Game over! There we go."* |
| **Suy luận & Mức tin cậy** | **[Mức tin cậy: Cao]** Cao trào màn chơi là một câu đố phối hợp tổng lực: Lồng đèn treo có thể dùng làm bẫy môi trường (vừa dập tắt ánh sáng, vừa tạo âm thanh kéo lính). Lối đi ngầm bên dưới sân đình là giải pháp bypass hoàn hảo cho phong cách lén lút. |
| **Điều chưa biết** | Chiếc lồng đèn rơi xuống có đè chết lính nếu rơi trúng đầu không? |
| **Bài học cho Game Đặc công** | Thiết kế cao trào căn cứ địch: Treo các thùng phi, giàn gỗ hoặc bóng đèn pha cao thế có thể bắn hạ từ xa để tạo sự cố nghi binh, mở hành lang cho đặc công luồn qua hầm ngầm tiếp cận sở chỉ huy. |

---

## 3. Kiểm Chứng Các Điểm Nghi Ngờ (Verification of Open Assumptions)

Qua việc đối chiếu frame và transcript của cả 2 walkthrough, chúng tôi chính thức đính chính 5 giả định chưa kiểm chứng từ vòng nghiên cứu trước:

### ① Nhận kiếm ở thời điểm nào? Có thật chỉ nhận ở cuối màn không?
* **Kết luận đính chính:** **HOÀN TOÀN SAI LẦM KHI NÓI "BỊ TƯỚC KIẾM SUỐT CẢ MÀN".**
* **Bằng chứng xác thực:**
  - Lời thoại của Ora ở phút `02:07` (*"Where's your sword? Stick to the darkness until you find one... You can't kill without your weapons"*) chỉ là ràng buộc cục bộ trong **đúng 30 giây đầu tiên tại Room 1** nhằm ép người chơi học nút nấp vào bình phong (`PRESS B TO HIDE`).
  - Ngay từ Room 2 (phút `02:45`), nút `ATTACK` đã sáng trên HUD. Đến phút `03:36`, người chơi đã tiếp cận sau lưng lính và thực hiện đòn ám sát `KILL` bằng dao/đoản kiếm, mở khóa Achievement `Stealth Assassin`.
  - Ở cuối màn 1, Sensei Azai trao cho ninja là **thanh Katana tổ truyền của gia tộc (hoặc vũ khí biểu tượng của Champion)**, chứ không phải trước đó nhân vật không thể tấn công.

### ② Hiding Spot bảo vệ người chơi trong điều kiện nào? Có an toàn tuyệt đối không?
* **Kết luận đính chính:** Hiding spot **không thể khẳng định là an toàn tuyệt đối trong mọi tình huống**:
  - Khi lính chưa phát hiện: Đi ngang qua bình phong/vò gốm, lính hoàn toàn không phát hiện dù người chơi đứng ở cự ly rất gần.
  - Khi lính phát hiện và truy đuổi: Ở phút `08:48 - 08:57`, người chơi chạy rẽ vào phòng trong (cắt đứt đường nhìn thẳng) rồi mới nấp vào bình phong; lính đuổi tới cửa nhưng không thấy người chơi và sau đó ngừng lùng sục.
  - **Giới hạn dữ liệu:** Chưa có quan sát trực tiếp trong video này về việc nếu người chơi chui vào hiding spot ngay trước mắt lính khi đang có đường nhìn thẳng thì lính có lôi ra bắn hay không. Cần giữ dưới dạng giả thuyết cần kiểm chứng thêm.

### ③ Focus Mode thể hiện việc dừng/chậm thời gian như thế nào?
* **Kết luận đính chính:** Không suy diễn mã nguồn hay cơ chế nội bộ (như biến thời gian hay tên shader/filter).
* **Biểu hiện giác quan thực tế quan sát qua clip:**
  - *Thị giác:* Các chuyển động môi trường xung quanh (hạt bụi bay, lính đang bước) dừng cử động; 4 góc viền màn hình chuyển sang tối sẫm lại; xuất hiện tia quỹ đạo nét đứt cùng hồng tâm ngắm.
  - *Thính giác:* Toàn bộ âm thanh nền, nhạc và tiếng động vật lý trở nên trầm đục, nghẹt lại như ở dưới nước, tạo cảm giác nhân vật đang tập trung cao độ.

### ④ Hai tiếng động đánh lạc hướng liên tiếp (Dual Distraction) tác động ra sao?
* **Kết luận đính chính:** **Không đủ bằng chứng để khẳng định luật "âm mới luôn ghi đè âm cũ cho mọi AI".**
  - Quan sát thực tế: Khi ném 2 tiếng động liên tiếp gần 1 tên lính, tên lính sẽ chuyển hướng đi sang nguồn âm thanh phát ra sau.
  - Tuy nhiên, khi 2 tiếng động phát ra ở 2 góc xa nhau với 2 tên lính khác nhau, mỗi tên lính bị chi phối bởi nguồn âm gần phạm vi nghe của mình nhất. Do đó, hiện tượng này là sự tương tác giữa **bán kính sóng âm hình học** và **mức ưu tiên kích thích giác quan**, không phải một lệnh ghi đè biến toàn cục.

### ⑤ Phân biệt Nhiệm vụ Bắt buộc vs Mục tiêu Tùy chọn (Collectibles & Seals) & Bảng Điểm
* **Nhiệm vụ bắt buộc (Core Objectives):** Thoát khỏi khu buồng ngủ, vượt qua các chốt gác hành lang, cứu ninja đồng đội bị treo, tiếp cận sân đình giải cứu Sensei Azai.
* **Mục tiêu tùy chọn (Optional Collectibles & Challenges):**
  - 3 Cuộn giấy Hisomu (Mở khóa lore Master Tetsuji).
  - 3 Cổ vật bí mật (Artifacts cộng điểm).
  - Thử thách phụ gõ vang 4 quả chuông đồng (`Ring 4 Bells`).
  - Các con dấu phong cách chơi (Seals: 0 Kills, 0 Alarms, v.v.).
* **Bằng chứng thị giác bảng tổng kết điểm màn 1:** [20_20m15s_score_summary_screen.jpg](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/20_20m15s_score_summary_screen.jpg) *(khung hình thực tế tại phút 20:15)*.
  - Điểm màn chơi (Level Score): `13,050`
  - Thưởng đánh lạc hướng (Distracted Bonus): `+1,600`
  - Thưởng không bị phát hiện (Undetected Bonus): `+2,400` *(tích lũy từ các phân cảnh/checkpoint không bị lộ)*
  - Tổng điểm màn chơi (Total Score): `17,050`
  - Thưởng không báo động (No Alarms Raised): `0` *(thất bại do kích hoạt báo động còi tại 12:24)*
  - Thưởng không giết địch (No Enemies Killed): `0` *(do đã ám sát lính)*
  - Tổng số Honor đạt được: `5/9`

---

## 4. So Sánh Hai Lối Chơi: theRadBrad vs. Centerstrain01

Bảng đối chiếu 5 tình huống then chốt giữa hai phong cách chơi chứng minh rằng **một walkthrough không bao giờ phản ánh toàn bộ quy luật của game**:

| Tình huống khảo sát | Walkthrough Centerstrain01 (Nhịp nhanh, quen thuộc, hướng tới thành tích) | Walkthrough theRadBrad (Khám phá, chưa tối ưu) | Nguyên nhân khác biệt |
|---|---|---|---|
| **1. Chạm trán lính đầu tiên (Room 2)** | Đu xà nhà, nhảy vọt qua nón tầm nhìn, trượt qua cửa không chạm trán (`+200 Undetected`). | Nhảy xuống đất sau lưng lính, ném Noisemaker lừa lính quay lưng, bấm `X` ám sát (`+400 Silent Assassin`). | Khác biệt về **phong cách chơi cá nhân**: Centerstrain01 tự đặt luật không giết ai; theRadBrad tận dụng cơ chế ám sát để dọn đường an toàn. |
| **2. Nhịp độ & Thời gian xử lý** | Di chuyển liên tục, ít ngập ngừng, hoàn thành màn trong **11 phút 06 giây**. | Thường dừng lại quan sát bối cảnh và đọc hướng dẫn; thời lượng video kéo dài **20 phút 21 giây** (bao gồm intro/outro/cutscene). | Khác biệt về **mức độ thông thuộc màn chơi**: Người chơi quen map tối ưu hóa thời gian; người chơi khám phá cần thời gian tiếp nhận thông tin. |
| **3. Khám phá Bí mật & Cổ vật** | Lấy đủ 3/3 Scrolls, 3/3 Artifacts, gõ đủ 4 quả chuông phong thủy. | Bỏ lỡ 2 Scrolls, 1 Artifact; gõ chuông một cách ngẫu nhiên vì tưởng chỉ là vật gây tiếng ồn thông thường. | theRadBrad tập trung vào mạch truyện chính và sinh tồn, không tập trung vào bảng thành tích 100%. |
| **4. Xử lý Cánh cửa Đóng kín** | Áp sát cửa sử dụng Door Peeking, nhìn thấy lính quay lưng rồi mới mở cửa lướt qua. | Mở cửa không quan sát; thấy lính ở cự ly gần vội bấm đóng cửa (12:14) ngắt tầm nhìn tạm thời; nhưng đến 12:24 bước qua cửa vẫn **bị phát hiện gây báo động** (`ALARM RAISED -600`). | theRadBrad cho thấy **đóng cửa chỉ là giải pháp che chắn tạm thời**: Nếu không nắm được vị trí lính thì khi mở lại cửa vẫn bị phát hiện và còi báo động ré lên. |
| **5. Cứu Sensei Azai ở Sân Đình** | Leo tường bên phải, đu dây xuống bóng tối, ném phi tiêu góc xa đánh lạc hướng cả 3 lính rồi lẻn vào. | Ném phi tiêu bắn đứt xích rơi lồng đèn xuống lính gây hoảng loạn, sau đó chui xuống đường cống ngầm dưới sân đình. | Trò chơi cung cấp **nhiều giải pháp cho cùng một căn phòng**: Đường trên cao (distraction lồng đèn) kết hợp đường ngầm bên dưới. |

---

## 5. Kết Luận: 3 Bài Học Cốt Lõi Cho Game Đặc Công Việt Nam & Câu Hỏi Thử Nghiệm

Từ toàn bộ quá trình phân tích đối chiếu thực nghiệm, chúng tôi rút ra **3 bài học thiết kế then chốt** cho dự án game đặc công:

### Bài học 1: Mô Hình NPC Đồng Hành "Dẫn Dắt Vòng Ngoài, Nhường Bước Vòng Trong"
* **Thực trạng học được:** Cả Ora và người đồng đội bị treo đều không đi kè kè theo sau nhân vật chính. Ora chỉ đưa ra khẩu lệnh ngắn gọn từ bóng tối khi có cơ chế mới xuất hiện; đồng đội sau khi được giải cứu thì không đi cùng nhân vật chính sang phòng tiếp theo.
* **Ứng dụng cho Game Đặc công:**
  - **Chỉ huy vòng ngoài (Leader NPC):** Cần phương thức liên lạc phù hợp với tình trạng thính lực kém (lời dặn trước nhiệm vụ, quy ước tín hiệu đèn pin/tiếng gõ, điểm hẹn trung chuyển, hoặc mã số bộ đàm 1 chiều), nhắc nhở thông tin địa hình phía trước.
  - **Chiến sĩ hỗ trợ (Comrade NPC):** Xuất hiện ở các điểm chốt (cắt hàng rào kẽm gai, đặt bộc phá nghi binh ở hướng đối diện để hút hỏa lực địch), sau đó tản ra điểm hẹn rút lui, giữ cho lối chơi thâm nhập của người chơi luôn là **Single-player tập trung cao độ**, tránh hoàn toàn lỗi AI bạn đồng hành đi lạc gây lộ.

### Bài học 2: Thiết Kế "Cửa Sổ Hồi Phục" (Recovery Window) Thay Vì Trừng Phạt Tức Thì
* **Thực trạng học được:** Khi theRadBrad nhảy nhầm vào ánh đèn pin bị lính bắn, game không ép chết ngay mà cho phép chạy nhanh thoát ly tầm nhìn, nấp vào bình phong để lính hạ mức báo động sau một khoảng thời gian tìm kiếm.
* **Ứng dụng cho Game Đặc công:**
  - Khi đặc công bị lính tuần tra phát hiện, cần có **thời gian trễ phản ứng (giả định đề xuất 1–2 giây)** để lính nhận diện và hô hoán (chưa nổ súng ngay).
  - Nếu bị bắn, người chơi có thể tung lựu đạn khói, lặn xuống mương nước hoặc nhảy vào bụi rậm rậm rạp để cắt đuôi. Lính sẽ bắn vu vơ về hướng cũ rồi chuyển sang trạng thái sục sạo, cho phép người chơi tái tổ chức đường đột nhập.

### Bài học 3: Minh Bạch Giác Quan (Visual & Acoustic Transparency)
* **Thực trạng học được:** Vòng sóng âm bước chạy, nón ánh sáng đèn pin, trạng thái nhị phân của sprite (sáng thì có màu, tối thì thành bóng đen) và tính năng nhìn bóng mờ qua khe cửa giúp người chơi nắm rõ thông tin trước khi ra quyết định.
* **Ứng dụng cho Game Đặc công:**
  - Thể hiện rõ mức độ ngụy trang: Khi trườn qua cỏ tranh hoặc bôi bùn ngụy trang, nhân vật chuyển sang trạng thái hòa lẫn môi trường.
  - Bổ sung cơ chế áp tai xuống đất nghe tiếng bước chân hoặc nhìn qua khe vách nứa để nắm rõ bố phòng của đồn địch trước khi xâm nhập.

---

### Các Câu Hỏi Mở Cần Thử Nghiệm Bằng Prototype Nhỏ (Small Prototypes)

*(Lưu ý: Các giá trị số dưới đây là giả định thiết kế ban đầu để thử nghiệm và cân chỉnh trong prototype, không phải thông số đo đạc từ game gốc).*

1. **Prototype 1 (Tương tác Cửa & Che chắn):**  
   * *Câu hỏi thử nghiệm:* Khi nhân vật hé mở cửa gỗ (thử nghiệm thông số góc mở, ví dụ 30%) để quan sát vào trong, lính ở cự ly gần (ví dụ đặt 5m trong engine) có phát hiện không? Việc đóng cửa sập lại có phát ra tiếng động thu hút lính tới gần kiểm tra không?
2. **Prototype 2 (Thời gian Reset của AI sau Báo động):**  
   * *Câu hỏi thử nghiệm:* Cần bao nhiêu giây ẩn nấp an toàn (ví dụ thử nghiệm mốc 8s hoặc 12s) trong bụi rậm để lính từ trạng thái truy đuổi hạ xuống trạng thái lùng sục rồi quay về vị trí tuần tra cũ?
3. **Prototype 3 (Tương tác Phối hợp 2 Nhân vật tại Chốt chặn):**  
   * *Câu hỏi thử nghiệm:* Thiết kế cơ chế để người chơi ra lệnh cho đồng đội NPC ném đá nghi binh ở chốt A, tạo cửa sổ hành động (ví dụ giả định 5 giây) cho người chơi lướt qua chốt B như thế nào để vừa mượt mà vừa không bị cảm giác tự động hóa quá đà?
