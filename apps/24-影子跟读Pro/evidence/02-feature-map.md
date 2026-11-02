# 功能面定位（P3 第一批，2026-11-02）

对象：`evidence/Runner.i64`（IDA 9.5，32.9 万函数 / 129,489 字符串）
方法：`mcp__ida__execute_python` 直查符号表与字符串表。**只记能指到具体字面量的结果**；靠名字形态推测的一律标"未证"。

## 已证（字符串表里有字面量）

| 能力 | 证据（原样字面量） | 含义 |
| :--- | :--- | :--- |
| mpv 播放内核 | `@rpath/Mpv.framework/Mpv`、`MPVHelpers: error:`、`disposeMPV` | 端上播放走 mpv（media_kit 封装，符号里有 `media_kit_video` 的 `MPVHelpers`/`MPVVideoOutParams`） |
| Whisper 端上推理 | `WhisperBeamSearch_Microsoft_ver1`（onnxruntime 内部）、`ONNX Runtime environment not initialized`、`flutter_onnxruntime`、`@rpath/sherpa_onnx.framework/sherpa_onnx` | ASR 是 **ONNX Runtime + Whisper 模型 + sherpa-onnx**，不是调云端 |
| Vision OCR | `/System/Library/Frameworks/Vision.framework/Vision`、`ios_vision_ocr` | 硬字幕提取走 Apple Vision，非第三方 OCR |
| AVPlayer 备用 | `-[BUPlayer getAVPlayer]`、`aNavplayeritems`、`aAvplayeritemdi` | 火山（BU/CSJ）广告 SDK 自带的播放器，与主播放链路分开 |

## 未证（字符串表里找不到，不等于不存在）

| 猜测 | 实测 | 说明 |
| :--- | :--- | :--- |
| 词典走 Dehelper / Duden | `dehelper` 0 命中、`duden` 0 命中 | 本报告不成立 |
| 字幕格式 .ass/.srt/.vtt | `subtitle` 0、`.srt` 0、`.vtt` 1、`libass` 2 命中 | 不能据此断言"不支持字幕" |
| 远程 LLM（OpenAI/DeepSeek） | `openai` 0、`deepseek` 0、`chat/completions` 0、`gemini` 2（均为广告/统计命名） | 未找到远程 LLM 调用字面量 |

## 方法论要点（这条比结论重要）

**Flutter 应用的业务字符串不在 Mach-O 的 C 字符串表里。**
Dart 的字符串常量编进 AOT kernel snapshot（`__snapshot` 一类区段，二进制大块），`idautils.Strings()` 扫不到。这解释了为什么 33 万函数的样本里，`subtitle`/`.srt`/`chat/completions` 全是 0 —— 不是功能不存在，是**查错了层**。

正确做法：用 `inspect_dart_aot` 解析 snapshot（sections / headers / string pool），或从 Flutter 资产里取 `kernel_blob.bin`（Debug/JIT 包才有）。**任何"该 App 没有 X"的结论，在没查 Dart 层之前都不成立。**

## 顺带纠正

我第一次用子串匹配统计时得到 `ass_`=8329、`dictionary`=4182、`tts`=146 —— 全是假命中（`class_addMethod`、`jsonDictionary`、`TTSDK`）。**符号匹配必须用完整前缀或正则边界，子串计数没有意义。**

## 工具可行性（补记）

| 工具 | 对 iOS Flutter 是否可用 | 实测 |
| :--- | :--- | :--- |
| `rea inspect_dart_aot` | **不可用** | 报 `invalid_format: Selected APK is not a regular file`，只接受 APK（且只支持 Dart 2.14 老式 VM/isolate 分包布局） |
| 容器内 `blutter` | 可用（待跑） | `/opt/blutter` 已在容器里，配合 Dart SDK 可反出 Dart 类/函数/AOT 结构 |

所以 iOS 侧 Dart 层解析走 blutter，不走 REA。
