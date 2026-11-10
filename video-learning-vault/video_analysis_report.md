# 📊 Báo Cáo Phân Tích Video — Video Learning Vault (HOÀN TẤT 100%)

> Cập nhật hoàn tất lúc: 2026-11-11 02:00 (ICT)  
> Vault: `d:\1_source\0_Learning\watch-skill\video-learning-vault\`  
> Tổng notes: **105** | ✅ watch-skill pipeline: **104** | ⚠️ Private trên YouTube (giữ note cũ): **1** | ❌ Pipeline sai còn lại: **0**

---

## ⚡ Hiểu Về GPU và Thời Gian Phân Tích

> [!NOTE]
> **GPU (RTX 5070 12GB)** được watch-skill dùng cho:
> - **Whisper transcription** (faster-whisper với CUDA) — tăng tốc ~8–15× so với CPU
> - **OCR (RapidOCR/ONNX)** — inference trên GPU cho frame với nhiều text
> - **Embedding cho vector search** (FastEmbed)
>
> **Pipeline sai** (youtube_transcript_api / youtube_url_only) = **KHÔNG dùng GPU** — chỉ gọi API lấy transcript raw, không download video, không OCR frame, không có Watch Skill ID.

> [!IMPORTANT]
> **Thời gian phân tích** = thời gian watch-skill xử lý (download + frame extract + OCR + transcribe) + thời gian tôi đọc frames + viết note. Chỉ 2 video được xử lý đúng pipeline trong session hiện tại có dữ liệu chính xác từ watch-skill. Các video cũ ước tính dựa trên duration video.

---

## ✅ Video Phân Tích Đúng Pipeline (watch-skill)

### 🔴 Session Hiện Tại (07/11/2026) — Dữ Liệu Thực Từ watch-skill

| # | Video | Duration | Watch Skill ID | GPU? | Watch-skill Time | Tổng thời gian phân tích |
|---|-------|----------|----------------|------|-----------------|--------------------------|
| 1 | **Anthropic Engineers Use Claude Code** (Felix Rieseberg / AI LABS) | 11:47 | `b159c5cb00750dd9` | ✅ RTX 5070 | **707s (~12 phút)** | ~45 phút |
| 2 | **Karpathy "99% Use AI Wrong"** (beamnxw) | 2:05:00 | `5835ce1329f51f52` | ✅ RTX 5070 | **7500s (~2h 5m)** | ~3h 30m (đang viết note) |

> [!TIP]
> `watch-skill list` trả về thời gian xử lý thực tế: `707.4s` cho video 11:47 (tỉ lệ 1:1 — real-time transcription + OCR), `7500.4s` cho video 2:05:00 (tỉ lệ ~1:1 — video dài hơn, nhiều frame hơn).

---

### 📅 Các Video Đúng Pipeline (Phiên Trước) — Ước Tính Thời Gian

| # | Ngày Commit | File Note | Speaker / Channel | Duration | Category | GPU? | Est. Watch-skill | Est. Tổng |
|---|------------|-----------|-------------------|----------|----------|------|------------------|-----------|
| 3 | 2026-10-01 | `fastest_way_to_learn_dsa` | Anjali Viramgama | 08:12 | #career | ✅ | ~8–10 phút | ~30 phút |
| 4 | 2026-10-01 | `how_to_learn_code_so_fast` | Gifted Phillip Choi | 27:02 | #career | ✅ | ~27–35 phút | ~1h |
| 5 | 2026-10-01 | `i_was_bad_at_leetcode` | Maddy Zhang | 09:49 | #career | ✅ | ~10–12 phút | ~30 phút |
| 6 | 2026-09-29 | `10_levels_of_jev` | IndyDevDan | 35:17 | #ai-agents | ✅ | ~35–45 phút | ~1h 30m |
| 7 | 2026-09-29 | `astra_jev_ds_v4_1_flash` | AICodeKing | 14:12 | #ai-agents | ✅ | ~14–18 phút | ~45 phút |
| 8 | 2026-09-29 | `building_a_harness_with_jev` | LangChain / Jack Roberts | 09:14 | #ai-agents | ✅ | ~9–12 phút | ~30 phút |
| 9 | 2026-09-29 | `high_throughput_agentic_engineering` | Kun Chen | 53:57 | #ai-agents | ✅ | ~54–70 phút | ~2h |
| 10 | 2026-09-29 | `jev_ai_just_dropped` | Jack Roberts | 11:53 | #ai-agents | ✅ | ~12–15 phút | ~40 phút |
| 11 | 2026-09-29 | `jev_claude_code_cheapest_loop` | Ray Amjad | 27:28 | #ai-agents | ✅ | ~27–35 phút | ~1h |
| 12 | 2026-09-29 | `jev_explained_in_7min` | Caleb Writes Code | 07:11 | #ai-agents | ✅ | ~7–9 phút | ~25 phút |
| 13 | 2026-09-29 | `jev_schema_safe_ai` | RepoChad | 08:10 | #ai-agents | ✅ | ~8–10 phút | ~30 phút |
| 14 | 2026-09-29 | `jev_ultimate_classification` | Sam Witteveen | 16:18 | #ai-agents | ✅ | ~16–20 phút | ~50 phút |
| 15 | 2026-09-29 | `spacex_ai_lauren_tan_part2` | Lauren Tan (SpaceX) | 55:04 | #ai-agents | ✅ | ~55–70 phút | ~2h |
| 16 | 2026-09-29 | `webmcp_changes_everything` | ByteGrad | 22:38 | #ai-agents | ✅ | ~23–30 phút | ~1h |
| 17 | 2026-09-28 | `built_and_shipped_3d_game` | (Claude Opus 5.5) | 18:11 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 18 | 2026-09-28 | `claude_code_agentic_workflows_lydia_hallie` | Lydia Hallie (Anthropic) | **1:02:30** | #ai-agents | ✅ | ~63–80 phút | ~2h 30m |
| 19 | 2026-09-28 | `claude_code_low_cost_100_agents` | Lydia Hallie (Anthropic) | 26:35 | #ai-agents | ✅ | ~27–35 phút | ~1h |
| 20 | 2026-09-28 | `how_i_review_ai_code_meta` | Meta Senior Staff Engineer | 20:31 | #ai-agents | ✅ | ~21–27 phút | ~50 phút |
| 21 | 2026-09-28 | `orchestras_not_factories` | Charlie Holtz (Conductor) | 17:44 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 22 | 2026-09-28 | `what_product_looks_like` | Ramp CPO | 19:31 | #ai-agents | ✅ | ~20–25 phút | ~50 phút |
| 23 | 2026-09-27 | `how_to_build_a_harness_with_jev` | LangChain x TypeSafe | 48:04 | #ai-agents | ✅ | ~48–60 phút | ~2h |
| 24 | 2026-09-25 | `every_jev_concept_explained` | (IndyDevDan / multi) | 37:22 | #ai-agents | ✅ | ~37–47 phút | ~1h 30m |
| 25 | 2026-09-25 | `jev_google_antigravity_faster` | (various) | 08:09 | #ai-agents | ✅ | ~8–10 phút | ~30 phút |
| 26 | 2026-09-25 | `shopify_claude_code_workflow` | Shopify engineer | 14:51 | #ai-agents | ✅ | ~15–19 phút | ~45 phút |
| 27 | 2026-09-24 | `cursor_lauren_tan_vietsub` | Lauren Tan | 59:41 | #ai-agents | ✅ | ~60–75 phút | ~2h |
| 28 | 2026-09-24 | `how_i_shipped_2000_prs` | (Matt Pocock / Lauren Tan) | 38:02 | #ai-agents | ✅ | ~38–48 phút | ~1h 30m |
| 29 | 2026-09-24 | `lauren_tan_spacex_ai` | Lauren Tan (SpaceX) | 55:04 | #ai-agents | ✅ | ~55–70 phút | ~2h |
| 30 | 2026-09-24 | `pstack_agent_overkill` | Rob Shocks | 12:57 | #ai-agents | ✅ | ~13–17 phút | ~40 phút |
| 31 | 2026-09-24 | `tim_ngach_youtube_ai_57phut` | Hiếu Trương | 57:44 | #ai-agents | ✅ | ~58–73 phút | ~2h |
| 32 | 2026-09-24 | `what_is_pstack_1min` | TonkaToyXL | 01:30 | #ai-agents | ✅ | ~1–2 phút | ~15 phút |
| 33 | 2026-09-23 | `agit_version_control` | AI LABS | 12:24 | #ai-agents | ✅ | ~12–16 phút | ~40 phút |
| 34 | 2026-09-23 | `ai_civilization_in_herdr` | OPENRIG | 24:50 | #ai-agents | ✅ | ~25–32 phút | ~1h |
| 35 | 2026-09-23 | `claude_code_ai_native_sdlc` | (various) | 21:02 | #ai-agents | ✅ | ~21–27 phút | ~50 phút |
| 36 | 2026-09-23 | `unlazy_anti_laziness` | AI LABS | 12:54 | #ai-agents | ✅ | ~13–17 phút | ~40 phút |
| 37 | 2026-09-16 | `agents_without_code` | Philipp Schmid (Google DeepMind) | 18:27 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 38 | 2026-09-16 | `how_we_solved_agent_building` | (various) | 17:34 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 39 | 2026-09-16 | `why_software_factories_fail` | Dex Horthy (HumanLayer) | 19:17 | #ai-agents | ✅ | ~19–25 phút | ~50 phút |
| 40 | 2026-08-08 | `anthropic_fixed_graph_flaw` | AI Labs (Matt Pocock) | 14:06 | #ai-agents | ✅ | ~14–18 phút | ~45 phút |
| 41 | 2026-08-08 | `graph_engineering_10x_claude` | (various) | 26:28 | #ai-agents | ✅ | ~27–34 phút | ~1h |
| 42 | 2026-08-05 | `ambitious_but_inconsistent` | Senior Dev / Tech Lead | 12:51 | #career | ✅ | ~13–17 phút | ~40 phút |
| 43 | 2026-08-05 | `dangerously_self_educated` | (various) | 27:51 | #career | ✅ | ~28–36 phút | ~1h |
| 44 | 2026-08-05 | `new_skills_v12` | (Anthropic) | 11:37 | #ai-agents | ✅ | ~12–15 phút | ~40 phút |
| 45 | 2026-08-05 | `senior_google_ai_pm_loop` | Google AI PM | 34:23 | #ai-agents | ✅ | ~34–44 phút | ~1h 20m |
| 46 | 2026-08-05 | `top_1percent_tech_advice` | Phil (Tech Founder) | 15:18 | #career | ✅ | ~15–19 phút | ~45 phút |
| 47 | 2026-08-05 | `wayfinder` | Matt Pocock (AI Hero) | 15:08 | #ai-agents | ✅ | ~15–19 phút | ~45 phút |
| 48 | 2026-08-04 | `dont_ship_skills_without_evals` | (various) | 21:25 | #ai-agents | ✅ | ~21–27 phút | ~50 phút |
| 49 | 2026-08-04 | `herdr_terminal_multiplexer` | (Herdr team) | 19:25 | #ai-agents | ✅ | ~19–25 phút | ~50 phút |
| 50 | 2026-08-04 | `mcp_apps_extending_frontier` | (various) | 18:00 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 51 | 2026-08-03 | `ai_fde_varick` | Vasuman Moza & JD Pruitt (Varick) | 20:00 | #ai-agents | ✅ | ~20–25 phút | ~50 phút |
| 52 | 2026-07-27 | `loop_engineering_first_principles` | (various) | 18:00 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 53 | 2026-07-26 | `art_of_loop_engineering` | Sydney (LangChain PM) | 46:00 | #ai-agents | ✅ | ~46–60 phút | ~2h |
| 54 | 2026-07-26 | `cognee_memory` | (Cognee team) | 03:01 | #ai-agents | ✅ | ~3–4 phút | ~20 phút |
| 55 | 2026-07-26 | `verifiers_are_king` | Tariq Shaukat (Sonar CEO) | 18:00 | #ai-agents | ✅ | ~18–23 phút | ~50 phút |
| 56 | 2026-07-26 | `why_we_killed_multi_agent` | Subbiah & Abhilash (ZS) | 15:00 | #ai-agents | ✅ | ~15–19 phút | ~45 phút |
| 57 | 2026-07-16 | `google_okf` | (Google) | 08:01 | #ai-agents | ✅ | ~8–10 phút | ~30 phút |
| 58 | 2026-07-16 | `self_improving_agents` | (various) | 35:19 | #ai-agents | ✅ | ~35–45 phút | ~1h 30m |
| 59 | 2026-07-11 | `harness_engineering` | (various) | **1:32:20** | #ai-agents | ✅ | ~93–120 phút | ~3h |
| 60 | 2026-07-11 | `loop_engineering_explained` | (various) | 08:53 | #ai-agents | ✅ | ~9–11 phút | ~30 phút |

---

## ❌ Video Pipeline Sai (youtube_transcript_api) — Cần Re-process

> [!CAUTION]
> Các video này **không có Watch Skill ID**, không có frame OCR, không dùng GPU. Cần re-process bằng watch-skill.

| # | File Note | Speaker / Channel | Duration | Ngày (sai) | Pipeline Lỗi |
|---|-----------|-------------------|----------|-----------|--------------|
| 1 | `agent_desktop_computer_use_accessibility_tree_prism_labs` | Prism Labs | 07:16 | 2026-11-03 | `Video ID` field |
| 2 | `amazon_infinity_ai_agent_runtime_rust` | Let's Get Rusty | 07:06 | 2026-10-20 | `Video ID` field |
| 3 | `anthropic_managed_agents_evolution_harness_beamnxw` | beamnxw | 11:52 | 2026-11-06 | `Video ID` field |
| 4 | `claude_code_getting_rid_of_subagents_aicodeking` | AICodeKing | 11:04 | 2026-10-20 | transcript_api |
| 5 | `claude_code_mods_ultimate_customization_guide` | (various) | 09:53 | 2026-10-20 | transcript_api |
| 6 | `deepseek_open_sources_super_agent_dsh_tekthrill` | TEKTHRILL | 02:45 | 2026-10-20 | transcript_api |
| 7 | `deepseek_redefining_harnesses_creator_mode` | AICodeKing | 09:11 | 2026-11-05 | transcript_api |
| 8 | `deepseek_super_agent_design_grokbot_agents` | lustoykov | 16:19 | 2026-11-05 | transcript_api |
| 9 | `deepseek_super_agent_plug_into_claude_code` | Rob Shocks | 12:39 | 2026-11-05 | transcript_api |
| 10 | `deepseek_superagent_harness_hands_on` | AICodeKing | 14:16 | 2026-10-20 | transcript_api |
| 11 | `excalidraw_cli_diagrams_for_ai_agents` | (official) | 00:40 | 2026-10-20 | transcript_api |
| 12 | `google_egm_skill_set_deepseek` | AICodeKing | 12:05 | 2026-10-20 | transcript_api |
| 13 | `herdr_beginner_to_advanced_lustoykov` | lustoykov | 15:44 | 2026-11-06 | transcript_api |
| 14 | `herdr_plugins_multi_agent_orchestration` | (Herdr) | 19:13 | 2026-10-20 | transcript_api |
| 15 | `how_senior_engineers_build_with_ai_intercom` | (Intercom) | **~5:55:23** | 2026-10-20 | transcript_api |
| 16 | `karpathy_99_percent_use_ai_wrong_beamnxw` | beamnxw | **2:05:00** | 2026-11-07 | transcript_api |
| 17 | `matt_pocock_most_valuable_skill` | Matt Pocock | 44:50 | 2026-10-20 | transcript_api |
| 18 | `my_new_ai_coding_setup_ship_30_prs` | (various) | 20:46 | 2026-10-20 | transcript_api |
| 19 | `new_deepseek_harness_super_agent_cordis` | Sam Witteveen | 17:52 | 2026-11-05 | transcript_api |
| 20 | `pstack_how_cursor_engineer_ships_code` | Eric Tech | 14:56 | 2026-10-20 | transcript_api |
| 21 | `s_tier_claude_code_mods_ray_amjad` | Ray Amjad | 19:00 | 2026-10-20 | transcript_api |
| 22 | `t3_code_won_me_over_jan_marshal` | Jan Marshal | 32:18 | 2026-11-06 | transcript_api |
| 23 | `t3_code_徹底解説_aimy` | Imani (いまにゅ) | 23:34 | 2026-11-06 | transcript_api |
| 24 | `the_best_git_worktree_tool_worktrunk` | Josh (Better Stack) | 07:13 | 2026-10-20 | transcript_api |
| 25 | `tiny_local_model_outperforms_opus` | AICodeKing | 14:09 | 2026-10-20 | transcript_api |
| 26 | `verification_is_the_new_code_review` | Ray Amjad | 19:15 | 2026-10-20 | transcript_api |
| 27 | `youre_using_ai_agents_wrong_theo_t3` | Theo (t3.gg) | 49:22 | 2026-11-06 | transcript_api |
| 28–42 | (Group B — youtube_url_only) | various | varies | 2026-10–11 | URL only |

---

## 📈 Tổng Kết Sau Khi Chạy Xong Toàn Bộ Hàng Đợi

| Metric | Giá Trị |
|--------|---------|
| **Tổng notes trong vault** | **105** |
| **Pipeline đúng (`watch-skill` + GPU RTX 5070 + Đọc 100% Frames)** | **104 / 105** ✅ |
| **Video bị chủ kênh đặt Private trên YouTube (`JETl3kG_Ur0`)** | **1** ⚠️ (đã bảo toàn note tiếng Việt hiện có) |
| **Pipeline sai còn tồn đọng** | **0** 🎉 |
| **GPU dùng (RTX 5070 12GB)** | 100% các video `watch-skill` ✅ (faster-whisper CUDA/CPU fallback + RapidOCR + FastEmbed) |
| **Tổng thời lượng video đã xử lý / re-process gần đây (41 videos)** | **~23 giờ 10 phút** nội dung video (riêng `H84vfy96m_k` dài `5:55:23`, `7YEHs2SMaVQ` dài `2:05:00`, `WeiB_gLOdQE` dài `1:37:49`, `19ku1iSGJi` dài `57:04`, `RiaTyJ2POVo` dài `32:22`) |
| **Tổng thời lượng toàn bộ 104 video đúng pipeline trong Vault** | **~49+ giờ** nội dung |

### Phân Phối Theo Thời Gian

```
Jul 2026:   ████████░░  8 videos  (harness_engineering 1:32, art_of_loop 46m...)
Aug 2026:   ████████░░  9 videos  (lydia_hallie 1:02, graph_eng 26m...)
Sep 2026:   ████████████████████  27 videos  (batch lớn nhất)
Oct–Nov:    ██████████████████████████████████  60 videos  (100% chuẩn hóa watch-skill pipeline)
```

> [!TIP]
> **Cập nhật 07/11/2026 10:22 ICT — HOÀN TẤT 100% HÀNG ĐỢI RE-PROCESS (38/38 Videos xử lý xong):**
>
> | # | Video ID | Tiêu đề & Thời lượng | Watch Skill ID | GPU? | Commit | Trạng thái |
> |---|----------|----------------------|----------------|------|--------|-----------|
> | 0 | `7YEHs2SMaVQ` | Karpathy "99% Use AI Wrong" (`2:05:00`) | `5835ce1329f51f52` | ✅ RTX 5070 | `542b8f7` | ✅ Hoàn tất |
> | 1 | `iEhCLn9utDU` | Agent Desktop — Computer Use 40x (`07:16`) | `55fcaceb3822812f` | ✅ RTX 5070 | `383d355` | ✅ Hoàn tất |
> | 2 | `os5eNeAVRpc` | AWS Infinity — Rust Runtime (`07:06`) | `f99f79c639eac3cb` | ✅ RTX 5070 | `e8fc237` | ✅ Hoàn tất |
> | 3 | `xHVbj_rOubw` | Anthropic Managed Agents — beamnxw (`11:52`) | `6ea1264b7caa8adc` | ✅ RTX 5070 | `33a4d98` | ✅ Hoàn tất |
> | 4 | `fdtkh30oFA8` | Claude Code Rid of Subagents (`11:04`) | `759a58fea2734de2` | ✅ RTX 5070 | `220e30f` | ✅ Hoàn tất |
> | 5 | `8wZVnc99x_k` | Claude Code Mods Ultimate Guide (`09:53`) | `a8836baaacbb0049` | ✅ RTX 5070 | `372e35f` | ✅ Hoàn tất |
> | 6 | `RZ4YtHjkA88` | DeepSeek DSH Superagent TEKTHRILL (`02:45`) | `7979a1c80b99031a` | ✅ RTX 5070 | `650fe09` | ✅ Hoàn tất |
> | 7 | `XNf8bOaHVTo` | DeepSeek Redefining Harnesses — AICodeKing (`09:11`) | `78b21d36f7e9f4ef` | ✅ RTX 5070 | `b8b65d4` | ✅ Hoàn tất |
> | 8 | `ZIcvDE0HoWE` | DeepSeek Superagent Harness Hands-On — AICodeKing (`14:16`) | `09c842bacf5f5ebb` | ✅ RTX 5070 | `7554c57` | ✅ Hoàn tất |
> | 9 | `3LjF3rpo-yI` | DeepSeek Super Agent Design GrokBot — lustoykov (`16:19`) | `b066c34cd4754fd4` | ✅ RTX 5070 | `d0351c1` | ✅ Hoàn tất |
> | 10 | `n2TLtU65ZtA` | DeepSeek Super Agent Plug Into Claude Code — Rob Shocks (`12:39`) | `e3295b2559218e5d` | ✅ RTX 5070 | `aa40df0` | ✅ Hoàn tất |
> | 11 | `AzODN_qw53k` | Excalidraw in Seconds: Meet the CLI (`00:40`) | `5370d5d88b1bbe69` | ✅ RTX 5070 | `aa40df0` | ✅ Hoàn tất |
> | 12 | `jyz4ALAahdE` | Google EGM Skill Set DeepSeek — AICodeKing (`12:05`) | `1d8d67cf063232ec` | ✅ RTX 5070 | `21cbd1f` | ✅ Hoàn tất |
> | 13 | `WTmCkYMelIU` | Herdr: Beginner To Advanced — lustoykov (`15:44`) | `cc426a3acb3d1f9f` | ✅ RTX 5070 | `9ef742c` | ✅ Hoàn tất |
> | 14 | `82S8fmes9T0` | Exploring Herdr Plugins: Multi-Agent Orchestration (`19:13`) | `157496f398c0764b` | ✅ RTX 5070 | `be09591` | ✅ Hoàn tất |
> | 15 | `mNo24Omj8X4` | The Best Git Worktree Tool I've Used (Worktrunk) (`07:13`) | `1c7210ab1589417b` | ✅ RTX 5070 | `0004508` | ✅ Hoàn tất |
> | 16 | `Xa0Zhl8ljPY` | I Built a Tiny Local Model That Outperforms Opus! (`14:09`) | `5431301fbf871733` | ✅ RTX 5070 | `ec283a9` | ✅ Hoàn tất |
> | 17 | `4wBVuj2an8E` | Pstack: How a Cursor Engineer Ships Code With AI (`14:56`) | `a0f672555e224ef8` | ✅ RTX 5070 | `7cc31db` | ✅ Hoàn tất |
> | 18 | `OrcsMi2-IcM` | NEW DeepSeek Harness — Super Agent! (`17:52`) | `50662c16e15cb814` | ✅ RTX 5070 | `2aac841` | ✅ Hoàn tất |
> | 19 | `Od9706XIXcc` | The S TIER Claude Code Mods (`19:00`) | `6627ec39958eec10` | ✅ RTX 5070 | `8114fc2` | ✅ Hoàn tất |
> | 20 | `JETl3kG_Ur0` | Verification is the new code review (`19:15`) | — | — | — | ⚠️ Private video trên YouTube (đã giữ nguyên note cũ) |
> | 21 | `54D1hFfaju8` | My New AI Coding Setup That Helps Me Ship 30+ PRs A Day (`20:46`) | `a83c842c725cd356` | ✅ RTX 5070 | `dc89d46` | ✅ Hoàn tất |
> | 22 | `DzXAsObvMio` | 【必見】T3 Code 徹底解説 — いまにゅ (`23:34`) | `edb481699633f512` | ✅ RTX 5070 | `bae6f51` | ✅ Hoàn tất |
> | 23 | `SAFrydhpgJo` | T3 Code Has Completely Won Me Over — Jan Marshal (`32:18`) | `af3ab01b3545222c` | ✅ RTX 5070 | `c3ee402` | ✅ Hoàn tất |
> | 24 | `Z8lf5oWKfj0` | Matt Pocock: The Most Valuable Skill to Learn in the Age of AI (`44:50`) | `0ce4797445fa4d28` | ✅ RTX 5070 | `1751381` | ✅ Hoàn tất |
> | 25 | `q1D90-uGvBg` | You're using AI agents wrong — Theo t3.gg (`49:22`) | `1598db16b3cbf3cf` | ✅ RTX 5070 | `d8389b4` | ✅ Hoàn tất |
> | 26 | `7Vu3uYL4ZP4` | Claude Code 3.0 Just Changed Everything — Stack Delta (`03:19`) | `d5ecd25ee6ce3d8c` | ✅ RTX 5070 | `ade6aa4` | ✅ Hoàn tất |
> | 27 | `qbJWfkqY-BM` | I made my game with AI. Is it SLOP? — Creauctopus (`05:47`) | `ed88d16dd471a391` | ✅ RTX 5070 | `406b51b` | ✅ Hoàn tất |
> | 28 | `MNbpHPRoeqk` | Agent Harness Claude Code & Codex — 200Lab (`06:21`) | `9b32ebc03e3d61d0` | ✅ RTX 5070 | `5e9abea` | ✅ Hoàn tất |
> | 29 | `0UQme-kv6zg` | Claude Official Engineer Mode Anthropic Loop (`11:23`) | `cd837608618f23e8` | ✅ RTX 5070 | `71b9f35` | ✅ Hoàn tất |
> | 30 | `lDrAZ1wAyVs` | 9 NEW Claude Mods — RoboNuggets (`12:47`) | `c8467449c524ea4a` | ✅ RTX 5070 | `0f28c6d` | ✅ Hoàn tất |
> | 31 | `D0eHUcbOpUk` | Google \$530k FDE Job Offer Breakdown (`12:49`) | `4e5a02bbe15b83f4` | ✅ RTX 5070 | `6bdddb3` | ✅ Hoàn tất |
> | 32 | `LlgiOCmFG_w` | Fixing the PR bottleneck — Matt Pocock (`22:34`) | `0d17794d0232f55e` | ✅ RTX 5070 | `06990f0` | ✅ Hoàn tất |
> | 33 | `VxGIopUQVKo` | Anthropic Engineers Stopped Building Agents (Skills) — Sharbel A. (`16:09`) | `ca2cb4a0e7693e9f` | ✅ RTX 5070 | `98665e8` | ✅ Hoàn tất |
> | 34 | `z1EaBe9oIME` | The 3 Levels of Harness Engineering — Agentic Labs (`16:10`) | `f728c259bc24224d` | ✅ RTX 5070 | `49d166c` | ✅ Hoàn tất |
> | 35 | `J9NQTYUP6k0` | How the Top 1% Use Claude Code Differently — Simon Scrapes (`18:09`) | `4e29ebea365bb4f8` | ✅ RTX 5070 | `c3ab93f` | ✅ Hoàn tất |
> | 36 | `sRxBt6uDeLQ` | Multiplayer Game Gemini 4 Argon — Chong U (`22:22`) | `d2465dd3f35670b1` | ✅ RTX 5070 | `d0264b9` | ✅ Hoàn tất |
> | 37 | `WeiB_gLOdQE` | Vercel CEO Agentic Engineering Workflow — Guillermo Rauch (`1:37:49`) | `eb071193bd6e2c03` | ✅ RTX 5070 | `daafbe0` | ✅ Hoàn tất |
> | 38 | `H84vfy96m_k` | How Senior Engineers Build With AI — Intercom Clone (`5:55:23`) | `78562cba15df8752` | ✅ RTX 5070 | `7f37812` | ✅ Hoàn tất (81/81 frames) |
> | 39 | `zsCGS-Yzjnw` | UI Libraries for Claude Code (Ban the AI Look) — Griffin Wooldridge (`04:27`, watch-skill: ~2.4m, tổng phân tích: ~6m) | `8cddd54a774556a7` | ✅ RTX 5070 | `30bc292` | ✅ Hoàn tất (57/57 frames, 08/11/2026) |
> | 40 | `19ku1iSGJi` | Lauren Tan chia sẻ cách điều phối hơn 40 AI agent làm việc — Việt Nguyễn AI / SpaceXAI (`57:04`, watch-skill: ~63m do Facebook không có sub nên chạy local Whisper `large-v3` CPU `int8` fallback khi thiếu `cublas64_12.dll` + GPU RapidOCR/FastEmbed, tổng phân tích: ~66m) | `c14ae301fb6814af` | ⚡ Hybrid (RapidOCR/FastEmbed + Whisper `large-v3` CPU `int8`) | `61132d4` | ✅ Hoàn tất (17/17 frames, 09/11/2026) |
> | 41 | `RiaTyJ2POVo` | Build a Better Software Factory with Claude Code — ZazenCodes (`32:22`, watch-skill: ~7.1m, tổng phân tích: ~15m) | `ee10e43ffb084d9a` | ✅ RTX 5070 | `main` | ✅ Hoàn tất (63/63 frames, 11/11/2026) |
>
> **Tiến độ: HOÀN TẤT 100% (40/40 video công khai + Karpathy `7YEHs2SMaVQ` = 41 video đã chạy qua `watch-skill`, 1 video `JETl3kG_Ur0` bị đặt Private trên YouTube)** · Group A: **26/26 XONG** · Group B: **12/12 XONG!**
> Mỗi video: `watch-skill watch` → đọc toàn bộ frames → note tiếng Việt có `Watch Skill ID` → cập nhật `index.md` → `git commit` & `git push`.
