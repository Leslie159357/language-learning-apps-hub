# 模块地图（`package:video/*` 全量 136 路径归类，2026-11-02）

来源：`Frameworks/App.framework/App` 字符串池里的 `package:video/...` 路径。共 136 条，其中 views 66 / utils 17 / api 20 / components 10 / dao 8 / shared 8 / 其他 7。
下文路径均按"目录/文件名"原样列出，`.dart` 省略。带尾部 `r` 的是字符串池原始形态。

## 页面模块（views，约 30 个业务模块，GetX 的 logic+view 成对）

| 业务 | 模块 |
| :--- | :--- |
| 入口与框架 | `main`、`home_page`、`base`、`about` |
| 认证 | `auth/login` |
| 视频 | `video_list`、`video_detail`、`video_add`、`video_player_page`、`my_downloads_page`、`history` |
| **精读（新发现）** | `video_player_page/reading/{reading_audio_cache, reading_paragraph, video_reading_tab}` |
| 字幕 | `subtitle`、`subtitle_edit`、`video_collection_subtitle_statement` |
| 生词与词书 | `word_book_page/{word_book_logic, word_book_page}`、`wishlist_page` |
| 学习计划 | `plan`、`plan/plan_days`、`plan/plan_days/day_detail` |
| **公开学习计划（社交向）** | `public_study_plan/{list_page, list_tab, day_view, detail_view, local_sync, logicr}` |
| 网盘 | `pan`、`baidu`、`baidu_web_view` |
| 付费 | `purchase`、`credit_recharge` |
| 统计与排行 | `statics`、`ranking_page` |
| 其他 | `profile_page`、`cache_manage`、`web_view` |

## 能力层（utils 17 个，最能说明产品技术取向）

| 文件 | 说明 |
| :--- | :--- |
| `ocr/pp_ocr_v5_util` | **PaddleOCR v5** —— 端上有自己的 OCR，不是只用 Apple Vision |
| `ocr/onnx_runtime_manager` | ONNX Runtime 统一管理（Whisper 与 PP-OCR 共用） |
| `ocr/ocr_service` | OCR 服务封装 |
| `asr/realtime_asr_service` | 实时 ASR（配合 sherpa-onnx / Whisper） |
| `subtitle_parser` | 字幕解析（SRT 等） |
| `translation_util` | 翻译（对应 `google_mlkit_translation`） |
| `video_info_fetch` | 视频元信息抓取 |
| `inAppPurchaseManager` | 内购 |
| `login_guard` | 登录态守卫 |
| `downlaodLocal` | 下载到本地（文件名拼写如此） |
| 其他 | `device`、`dialogUtil`、`helper`、`profile_editor`、`rating_manager`、`share_util`、`ume_stub` |

## 公共服务（shared 8）

`app_bootstrap`、`app_startup_controllerr`、`app_theme`、`app_theme_controller`、`crash_log_service`、`pangle_splash_ad_service`（开屏广告）、`privacy_consent_service`、`soe_quota_service`（**打分配额本地记账**）。

## 三条修正与新发现

1. **OCR 是双引擎**：上一份 `02-feature-map.md` 只写了 Vision（依据 `ios_vision_ocr` 字面量）。实际还有 `pp_ocr_v5_util` —— PaddleOCR v5 通过 ONNX 在端上跑。做复刻时这条路更值得抄（不依赖系统 OCR 精度、可离线）。
2. **有"精读"这一整个模块**：`reading/reading_audio_cache`、`reading_paragraph`、`video_reading_tab`。之前把产品理解成"跟读 + 字幕"，实际还有按段落精读 + 音频缓存的独立链路。
3. **公开学习计划是社交功能**：`public_study_plan` 有 join/quit/list/day/clip-complete/reset 一整套接口 + `local_sync`，说明有"组队打卡/共享计划"，不只是单人学习。

## 复刻优先级（按依赖与不可替代性）

| 优先级 | 模块 | 理由 |
| :--- | :--- | :--- |
| P0 | 播放（media_kit+mpv）、字幕解析、精读切分 | 产品主干，且全是端上能力，不依赖后端 |
| P0 | OCR 字幕提取（PP-OCR v5 + Vision 兜底） | 生肉视频转字幕的唯一入口 |
| P1 | ASR（sherpa-onnx / Whisper）+ 实时 ASR | 端上已有，可完全离线 |
| P1 | 生词本 + 词书（word/wordList/category 已有接口形态） | 数据模型简单，可先本地 |
| P2 | SOE 打分、句子解析（服务端） | 需自建或替代，端上只能做音素级对比 |
| P2 | 学习计划/公开计划、排行、额度计费 | 社交与商业化，可后置 |
