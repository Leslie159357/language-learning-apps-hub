# [最新 v2.3.6] DejaVocab (得记单词 · 个人语料库) 全量逆向分析报告

目标：`com.dejavocab.app.DejavocabApp` (脱壳包 522 MB)

---

## 一、 核心架构亮点 (对我们最值钱的个人语料库引擎)

| 模块 | DejaVocab 真实底层设计 | 对自研播放器的价值 |
|---|---|---|
| **语料库核心** | 基于 Readium 体系 (`swift-toolkit`) 的电子书与有声书引擎 | **目前最成熟的 EPUB/PDF/音频对齐框架** |
| **生词上下文** | `CorpusContextExtractor`：精准截取生词所在的**前后句语境**与**原声音频切片** | 解决“背单词离开语境就忘”的痛点 |
| **划词取词** | 原生注入 WebKit 的 `DiffableDecoration+HTML.swift` 标注系统 | 支持在长篇剧本、小说中做平滑的荧光笔高亮与点词查词 |
| **离线服务** | 内置 `GCDHTTPServer` (GCDWebServer) 本地服务器 | 在 App 内部搭建虚拟 Web 容器，加载富文本交互组件 |

---

## 二、 内部原生内置 Anki 引擎全量架构 (全证据链确认)

DejaVocab 内部不是简单的「导出 Anki 卡片」，而是**在 App 内部完整手搓并内置了一套原生的 Anki 学习与复习引擎**！

提取到的 7 个核心原生 Swift 文件与架构职责：

1. **`AnkiService.swift` / `AnkiViewModel.swift`**：
   - 调度核心：负责 Anki 卡片状态流转（`AnkiCardState`: `new` / `learning` / `review` / `relearning`）；
   - 难度评级算法：完整实现 Anki 原生 4 档评级（`Again` / `Hard` / `Good` / `Easy`）；
   - 掌握度热力图：`learning_heatmap_anki_mastery_score` 与每日复习限额 (`daily_review_limit`)。

2. **`AnkiCardAudioPlayer.swift`**：
   - 原声伴读播放器：直接在 Anki 翻卡界面上无缝播放该句的**原声 3 秒音频切片** (`anki_original_audio`)！

3. **`AnkiCardView.swift` & `AnkiCardDetailView.swift`**：
   - 翻卡卡片 UI：支持「点击翻转答案 (`anki_tap_to_flip_answer`)」；
   - 拼写模式：内置 `anki_phrase_typing_mode`（短语打字核对模式，支持逐字校验正确与标红错误）。

4. **`AnkiMasteryDashboardView.swift` & `AnkiCardManagementView.swift`**：
   - 卡片管理与掌握度看板：统计卡片总数、记忆率、平均间隔天数 (`anki_avg_interval`) 与学习热力图。

> **基准版本更新**：已通过官方 ipatool 重新脱壳至 **最新正式版 v2.3.6**，并由 REA 6.8.0 重新投影 Application Graph 全量架构证据链。

