# 04 · 能力矩阵（逐条带证据）

判定规则：每条能力必须能指到**类名、二进制名或包内文件**；只出现在日志/界面文案里的不写。证据列给的是原样字符串或符号，不是转述。

## 1. 阅读与排版

| 能力 | 实现 | 证据 | 复刻判断 |
| :--- | :--- | :--- | :--- |
| EPUB 渲染 | Foliate（JS 引擎）跑在 WKWebView 里 | `foliate-viewer-bundle.js`（包内）、`FoliateReaderWebView`、`foliate` 命中 174 次 | 直接可用，开源（foliate-js，MIT） |
| 排版/分页 | 自研 + 阅读字体 | 10 个 TTF（Merriweather/Domine/Oswald…）、`Typograph*`、`RubyUIView`（注音）、`CustomTextView` | 需要自己做分页器；注音显示是加分项 |
| EPUB3 声字同步 | 标准 Media Overlays + SMIL | `media-overlay` 6、`SMIL` 30、`epub:type`、`nav.xhtml` | 必须做，这是"边听边高亮"的标准解法 |
| PDF | PDFKit + PencilKit 手写批注 | `PDFDocument`、`PDFAnnotation` 4、`PencilKit` 44、`PKCanvasView` 9、`ReaderInkPencilOnlyCanvasView` | PDFKit 够用；批注要自己画 |
| 漫画 | CBZ/CBR/7z 解包 + 图片流 | `CBZ`/`CBR`/`comicbook` 合计 300、`rar` 288、`7z` 214、`ComicInfo` | 建议只做 CBZ（CBR 要 unrar 许可） |
| MOBI/AZW3 | 自带解析（Python 侧 + 原生） | `MOBI`、`kindle`、`PalmDOC`、`HUFF/CDIC` | 优先级低，公版书用 EPUB 就够 |

## 2. 音频与播放

| 能力 | 实现 | 证据 | 复刻判断 |
| :--- | :--- | :--- | :--- |
| 解码 | FFmpeg（libavcodec/format/filter/resample/scale 全套） | 8 个 `libav*` + `ffmpegkit`，共 15 MB | 成本最低的"什么格式都能放"方案 |
| 播放 | AVAudioEngine + AVAudioSession | `AVAudioEngine` 5、`AVAudioSession` 22、`OneBook/SleepTimer` | 音频用系统引擎，别自己写混音 |
| 倍速/后台 | 后台音频模式 | `UIBackgroundModes: [audio]`、`NowPlaying` | 必做 |
| 睡眠定时器 | 独立小组件 | `OneBookSleepTimerWidgetExtension`（`com.linyi.OneBook.SleepTimerWidget`） | 小成本高感知 |

**关键判断**：它**没有**用 mpv/VLC/AVPlayer 做主力播放内核，也没有媒体播放器框架。想复刻"视频跟读"的话，它这套（音频优先 + WebView 视频）不是最优解；`影子跟读Pro` 的 mpv 路线更适合视频。

## 3. 语音：转写、对齐、朗读

| 能力 | 实现 | 证据 | 复刻判断 |
| :--- | :--- | :--- | :--- |
| 端上 ASR（whisper.cpp） | SwiftWhisper 内联源码 | 构建路径 `LocalPackages/SwiftWhisper/Sources/whisper_cpp/whisper.cpp`、`ggml.c`；模型从 `huggingface.co/ggerganov/whisper.cpp` 与 `hf-mirror.com` 下载 | 可复刻，模型几百 MB 需首次下载 |
| 端上 ASR（CoreML） | Whisper CoreML 三段模型 | `MelSpectrogram.mlmodelc`、`AudioEncoder.mlmodelc`、`TextDecoder.mlmodelc` | 走 CoreML 更快，但要自己转模型 |
| 端上 ASR（ONNX） | onnxruntime whisper + beam search | `BeamSearchWhisper`、`CreateWhisperEncoderInputs`、`beam_search_impl_whisper.h` | 第三条路径，说明它在多端兜底 |
| 强制对齐 | 自研 `StoryAlignCore` 内核 | 模块 `StoryAlignCore` 8 个类、`Aligner`、`AlignmentStats` | **这是它的核心难点**，复刻要自己实现 DTW/CTC 对齐 |
| 端上 TTS | sherpa-onnx（VITS / Piper / Kokoro / MeloTTS） | `sherpa_onnx` 命中 111、`kokoro` 47、`vits` 117、`piper` 85；模型仓库前缀 `csukuangfj/…` 共 45 个 | 开源模型直接能用 |
| 云端 TTS | 自有接口 + 多音色 | `/api/v1/tts`、`/api/v1/voices`、`/api/paas/v4/audio/speech`、`assets.onebook2025.top/v1/tts` | 端点已抄到，鉴权要另配 |
| 录音 | AVAudioRecorder | `AVAudioRecorder` 8、`AudioRecorderManager` | 简单 |
| 系统语音识别 | Speech framework 作为兜底 | `SFSpeechRecognizer` 6、`SpeechRecognizer` 10 | 可选 |

**语种覆盖（实测清单）**：端上 TTS 备了 **39 个音色 / 38 个语种**，其中德语走 `vits-mms-deu`、泰语走 `vits-mms-tha`（MMS 系列，与 Piper 系列分开托管）。也就是说**德、泰都在支持范围内**，不是只做英语。

## 4. 词典与生词

| 能力 | 实现 | 证据 | 复刻判断 |
| :--- | :--- | :--- | :--- |
| 本地词典库 | 38 MB SQLite（LZFSE 压缩） | `dictionary.sqlite.lzfse`、表 `dictionary`/`lexicon_term`/`lexicon_meta` | 可直接换库；数据授权要自己解决 |
| MDX/MDD 支持 | Python `mdict_utils` 解析 + 原生桥 | `ExtraPython/site-packages/mdict_utils`、`MDX`/`MDD`/`mdict` 合计 86 | 用 Python 解 MDX 是取巧但有效 |
| 语义检索 | 向量表 | 表 `vector_dictionary`、`AppleNaturalLanguageEmbedding`、`BERTTokenizer` | 词向量查近义词，非必需 |
| 短语库 | 单独 SQLite | `phrase_lexicon.sqlite` 4.4 MB、表 `lexicon_term` | 加分项 |
| 点击取词 | 全文点击 + 弹层 | `PlayableUITextView`、`CustomTextView`、`wordPop`、`vocabularyWordTapEvent`（表） | 核心交互，必做 |
| 间隔重复 | **FSRS**（不是 SM-2） | 模块 `FSRS` 9 个类、`fsrs` 命中 45 | 直接用 FSRS 开源实现，比 SM-2 先进一代 |
| 导出 | 历史表 + 文件导出 | 表 `word_export_history_v*`、`word_export_history` | **没有 Anki 包导出**（`apkg`/`genanki` 零命中），旧报告此处判断有误 |

## 5. AI

| 能力 | 实现 | 证据 | 复刻判断 |
| :--- | :--- | :--- | :--- |
| 端上小模型 | Gemma via LiteRT | `OnDeviceGemmaKit`（1,047 符号）、`CLiteRTLM`（42 MB，123,703 符号）、`gemma` 11、`litert` 6 | 想离线跑 LLM 就走 LiteRT；无 GGUF（`gguf` 零命中） |
| 云模型 | 用户自填 API / 平台代理 | `https://api.siliconflow.cn/v1/chat/completions`、`/api/llm/v1/chat/completions`、`/api/llm/v1/invoke`；`AIGatewayConfigStore` | 用户自带 key 是正确设计 |
| 句子解析 | 端上/云上二选一 | `AIAnalysisView`、`AnalysisChatStore`、`AnalysisChatPanel`、`AIAnalysisResultView` | 复刻成本低，套 prompt 即可 |
| 翻译 | Apple Translation + 服务端 | `Translation` 框架、`AppleTranslationManager`、`/v1/translate` | 本地免费通道可用 |
| OCR | Vision | `VNRecognizeText` 2、`VisionKit` 2 | 取词/取字幕都能用 |

## 6. 内容源（这是它最"重"的部分）

| 来源 | 实现 | 证据 |
| :--- | :--- | :--- |
| YouTube | **Python `yt_dlp` 内嵌** | `ExtraPython/site-packages/yt_dlp`、`/v1/youtube` |
| B 站 | 原生解析器 | `BilibiliParser`、`api.bilibili.com`、`BilibiliWebRecordView` |
| Apple Podcasts | 无头抓取 | `ApplePodcastHeadlessFetcher`、`ApplePodcastImportService` |
| RSS/新闻 | 服务端聚合 + 本地解析 | `/v1/news`、表 `news_feed_item`、`feeds.bbci.co.uk`、`www.theguardian.com` |
| 百度网盘书库 | 自己的书库 API + OAuth | `BaiduShelfAPIClient`、`BaiduNetdiskOAuthService`、`/v1/baidu-shelf`、`/v1/baidu-video-shelf`、URL scheme `onebook://`（Baidu Netdisk OAuth） |
| arXiv | 第三方目录 | `arxiv.maplepop.com/catalog` |

**这一块占包体 16 MB + 1,268 个 `.py`**：它把 yt-dlp 整个塞进 App，等于把"下视频"这件事搬到了端上，不用自建爬虫服务。这是最省服务器成本的做法，也是 Apple 审核容易卡的灰色地带。

## 7. 同步与备份

| 通道 | 实现 | 证据 |
| :--- | :--- | :--- |
| WebDAV | **内嵌 HTTP 服务器（Swifter）** | 模块 `Swifter` 9 个类、`webdav`/`WebDAV` 合计 80、`CloudSyncCoordinator` |
| iCloud | CloudKit | `CloudKit` 28、`CKRecord` |
| 百度网盘备份 | 自有 OAuth + 上传 | `BaiduNetdiskBackupService`、`BaiduNetdiskTokenStore`、`BookBackgroundDownloadCoordinator` |
| 自有服务端 | push 接口 | `/api/sync/v1/push`、session 键 `onebook.baseser.session_id` |

## 8. 商业化

| 项 | 实现 | 证据 |
| :--- | :--- | :--- |
| 内购 | StoreKit 2（`Transaction`/`Product`） | `StoreKit` 68 |
| 收据校验 | **服务端验**，不在端上判 | `/api/subscription/ios/verify` |
| 额度制 | 服务器扣减 | `/api/quota/consume`、`OneBookQuotaStore`、`quota` 21 |
| 邀请码 | 服务端兑换 | `/api/subscription/invite/redeem`、`invite` 8 |

**这一条直接决定 Loon 类插件的可行性**：会员判定在服务端，端上只有"请求额度/校验收据"，本地改布尔值不会生效。任何在端上伪造会员的方案，都会在 `/api/quota/consume` 或 `/api/subscription/ios/verify` 处被服务端否掉。
