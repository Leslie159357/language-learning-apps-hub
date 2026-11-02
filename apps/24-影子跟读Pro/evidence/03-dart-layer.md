# Dart 层（Flutter 业务逻辑）实测 · 2026-11-02

对象：`baseline/Payload/Runner.app/Frameworks/App.framework/App`（12.24 MB，`__text` 6.67 MB，`__const` 5.18 MB）
方法：直接扫 printable strings（长度 ≥5），共 30,412 条。**不需要 blutter**——官方 blutter 只支持 Android `libapp.so`；iOS 侧 Dart 的 `package:` URI、URL、源文件路径都是 AOT snapshot 里的明文，扫得出来。

## 依赖栈（90 个 pub 包，取关键）

| 类别 | 包 |
| :--- | :--- |
| 框架/状态 | `get`(GetX)、`dio`、`get_storage`、`sqflite`、`rxdart`、`protobuf` |
| 播放 | `media_kit`、`media_kit_video`、`audioplayers`、`ffmpeg_kit_flutter_new`、`wakelock_plus` |
| AI / CV | `flutter_onnxruntime`、`google_mlkit_translation`、`dartcv4`、`google_mlkit_commons` |
| 录制 | `record` |
| 商业化 | `in_app_purchase`（+storekit）、`pangle_ad_config`（穿山甲）、`umeng_common_sdk`、`rate_my_app` |
| 其他 | `webview_flutter`、`image_picker`、`file_picker`、`cached_network_image`、`showcaseview`、`barcode_scan2` |

注意两个之前没料到的：`google_mlkit_translation` = **端上翻译**（不联网也能译）；`dartcv4` = OpenCV 走 Dart 绑定，与 OCR/字幕区域处理对接。

## 应用自身代码结构（136 个 `package:video/*` 路径）

| 目录 | 文件数 | 说明 |
| :--- | ---: | :--- |
| `views/` | 66 | 页面层（最大头） |
| `api/` | 20 | 请求层：`auth` `bilibili` `baidu` `common` `crash_log` `home` `media` `pan` `pay` `plan` `public_study_plan` `soe` `statics` `subtitle` `voice_recognition` `wishlist` `words` |
| `utils/` | 17 | 含 `utils/ocr/ocr_service.dart` |
| `components/` | 10 | `ocr_region_selector`、`ocr_subtitle_panel`、`clickableWordText`、`wordPop`、`calendar_heatmap`、`draggableProgressBar`、`customerDialog`、`app_launch_gate`、`privacy_consent_gate` |
| `dao/` | 8 | `models/{video,subtitle,series,words,word_tags,tags,download}` |
| `shared/` | 8 | 共享层 |
| `middlewares/` | 2 | GetX 中间件 |
| 其他 | 4 | `main.dart`、`routes`、`model`、`logic/user.dart`、`defined/pangle_ad_config.dart` |

`api/request/soe.dart` + `voice_recognition.dart` 说明**跟读评分（SOE = Speech Opinion Engine）是独立请求链路**，不是本地打分。

## 端点（原文，28 条去重后的可控域名）

| 域名 | 用途 |
| :--- | :--- |
| `video-app-api.chengmu.cc` | 主 API（`/baidu/code`、`/baidu/url`） |
| `yd-h5.chengmu.cc` | 业务 H5：`/sentence-analysis?sentence=`（**逐句解析走服务端**）、`/wishlist`、`/agent-center`（分销代理）、`/h5/admin`、`/app/private.html` |
| `static-video.chengmu.cc` | 静态资源 + `test.srt`（证明支持 SRT） |
| `static-listener.chengmu.cc` | 头像等静态资源 |
| `book-static.chengmu.cc` | 书/绘本静态资源 |
| `dict.youdao.com` | **词典发音**：`/dictvoice?type=0&audio=`、`type=1`（英/美音） |

另有一批 `flowus.cn/bzyu/share/...`（3 条分享页，疑似用户引导/文档）与 `txc.qq.com` 客服组件图片。

## 三条结论

1. **AI 能力不在端上，在服务端**：`sentence-analysis` 是 H5 URL，句子解析由后端返回；端上只有 Whisper 转写（ONNX）与 SOE 打分请求。
2. **词典发音用有道**，不是 Dehelper/Duden——之前那版报告写"集成德语助手/Duden"在这份包里找不到支撑（`dehelper`/`duden` 字面量均为 0）。这直接影响复刻：TTS 走有道公开接口即可，不需要逆向第三方词典 SDK。
3. **字幕格式支持 SRT**（`test.srt` + `dao/models/subtitle.dart` + `components/ocr_subtitle_panel.dart`），OCR 面板是"选取区域 → 识别 → 生成字幕"的完整闭环。

## 修正上一份文档的说法

`02-feature-map.md` 里我写"Flutter 业务字符串不在 Mach-O 字符串表里，查错了层"——只对了一半：它确实不在 `Runner` 里，但在 `App.framework/App` 里是明文可扫的。**结论是"要扫对文件"，不是"字符串不可得"。**
