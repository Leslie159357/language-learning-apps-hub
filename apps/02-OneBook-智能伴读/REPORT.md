# OneBook v3.7.24 · B 档结构全拆报告

> 样本：`com.linyi.OneBook` 3.7.24（2026-10-30 上架版）｜解密后 13 个二进制全部 `cryptid=0`｜静态分析，未运行
> 数据基座见 `SAMPLE.md`，逐项证据见 `evidence/01…06`

## 一句话结论

**它不是"AI 阅读器"，是一个"内容管道 + 排版引擎 + 本地语音栈"的三合一工具**：内容从 YouTube/B站/抖音/小红书/RSS/OPDS/百度网盘进来，排版交给开源的 Foliate（JS），发音和对齐全在端上跑（whisper.cpp + sherpa-onnx + 自研 `StoryAlignCore`），AI 只是挂在这三层之上的可选增强。它的护城河在**对齐内核 + 内容管道**，不在模型。

## 与旧报告的三处纠正

旧版 `全功能零遗漏规格说明书.md` 声称"294 个源文件 / 957 个核心类全量审查"。本轮用编译器元数据重测，结论如下：

| 旧说法 | 实测 | 判定 |
| :--- | :--- | :--- |
| 957 个核心类 | 应用自有 **257** 个 Swift 类（`_TtC7OneBook*` 反解）+ 2,256 个类型描述符（含 SDK） | 旧口径把 SDK 和自己代码混在一起数 |
| 有 Anki 导出 | `apkg`/`genanki` **零命中**，只有 `word_export_history_v2` 导出历史表 | **旧结论错误**，它不产 Anki 包 |
| 端上只有 Whisper | 端上还有 sherpa-onnx TTS（39 音色/38 语种）、FSRS 复习算法、StoryAlignCore 对齐、LiteRT 上的 Gemma | 旧报告漏了四个子系统 |

另外旧报告完全没提两件大事：**它把整个 yt-dlp 塞进了 App（1,268 个 `.py`，16 MB）**，以及**它内嵌了一个 WebDAV 服务器（Swifter）**。

## 技术栈判定

| 层 | 选型 | 依据 |
| :--- | :--- | :--- |
| UI | SwiftUI（290 个 View 类） | `__swift5_types` 类型表 |
| 数据 | GRDB（SQLite）+ FTS5（porter/unicode61） | `grdb_migrations`、`FTS5` 29 处 |
| 阅读器 | **Foliate（JS）+ WKWebView** | `foliate-viewer-bundle.js`、`FoliateReaderWebView`、`foliate` 174 处 |
| 音频解码 | FFmpeg 全家桶（8 个 libav*） | 15 MB 二进制 |
| 播放 | AVAudioEngine + 后台音频 | `AVAudioEngine`、`UIBackgroundModes: audio` |
| ASR | 三条路：whisper.cpp（SwiftWhisper）/ CoreML Whisper / onnxruntime Whisper | 三套符号并存 |
| TTS | sherpa-onnx（VITS/Piper/Kokoro/MeloTTS）+ 20 家云 TTS | 39 个 `.onnx` 音色名 + 各厂商端点 |
| LLM | Gemma on LiteRT（端上）+ 任意 OpenAI 兼容 API（云上） | `OnDeviceGemmaKit` + `CLiteRTLM` 42 MB |
| 复习 | FSRS（不是 SM-2） | `FSRS` 模块 9 个类 |
| 同步 | WebDAV（本地服务器）/ CloudKit / 百度网盘 / 自有 push | 四条通道并存 |
| 脚本 | Python 3.10（kivy-ios 构建）| `Python-iOS_PythonSupport.bundle`、`yt_dlp`、`mdict_utils` |

## 架构七层

```
① 内容入口   YouTube(yt-dlp) B站 抖音 小红书 Reddit RSS×14 OPDS×3 百度网盘 本地文件
② 解析层     EPUB(Foliate) PDF(PDFKit) MOBI/CBZ/CBR(原生) MDX/MDD(Python) M4B 播客RSS
③ 排版层     Foliate JS：分页/注音/主题/字体；EPUB3 Media Overlays(SMIL) 声字同步
④ 语音层     whisper.cpp / CoreML / ONNX 三路 ASR → StoryAlignCore 强制对齐 → sherpa TTS
⑤ 学习层     点词取词 → 词典(LZFSE 38MB + MDX + 在线) → 生词(FSRS) → CEFR 难度画像
⑥ AI 层      Gemma(LiteRT) 本地 / 用户自带 key / 平台额度代理；翻译走 Apple Translation
⑦ 数据层     GRDB+FTS5 本地库 → WebDAV / CloudKit / 百度网盘 / 自有 push 四路同步
```

## 五个真正的难点（按复刻成本排序）

1. **正文与音频的强制对齐（`StoryAlignCore`）**。它把"音频某秒对应书里哪个词"算出来，才可能做逐词高亮、点词跳音频。这是整个产品体验的根，也是唯一没有现成开源替代的一块。
2. **内容管道**。YouTube/B站/抖音/小红书/播客/OPDS 各自有反爬与格式差异，它选择把 yt-dlp 塞进端上，把其余交给自有代理（`podcast.onebook2025.top`）。这块是纯工程量，不是智力活，但很脏。
3. **EPUB3 Media Overlays 的排版整合**。标准存在，但要把 SMIL 时间轴映射到 Foliate 的 DOM 上，还要处理分页、注音、横竖排，工程量巨大。
4. **三路 ASR 的兜底策略**。它同时实现了 whisper.cpp、CoreML、ONNX 三条路，说明作者在真机上被兼容性和性能坑过。自己复刻时选一条即可，别学它三路全做。
5. **TTS 供应商抽象层**。20 家 TTS 的鉴权、音频格式、流式协议各不相同，它做成枚举 + 统一播放器（`StreamingTtsPlayerKit`）。这块抄起来性价比最高。

## 复刻优先级（面向"德语精读 + 跟读"这一个具体目标）

| 优先级 | 模块 | 理由 | 可否直接用开源 |
| :--- | :--- | :--- | :--- |
| **P0** | Foliate 阅读 + EPUB 声字同步 | 没有排版就没有产品 | `foliate-js`（MIT） |
| **P0** | 音频解码播放（FFmpeg + AVAudioEngine） | 德语素材多是 m4a/mp3/mkv | FFmpeg（LGPL 需注意） |
| **P0** | 点词取词 + 本地词典 | 精读的核心动作 | 词典数据需自备 |
| **P1** | 强制对齐（词级时间轴） | 决定"逐词高亮"能不能做 | 无现成；可用 CTC 对齐自研 |
| **P1** | 端上 TTS（德语 `vits-mms-deu`） | 生词发音、跟读示范 | sherpa-onnx（Apache-2.0） |
| **P1** | FSRS 复习 | 生词沉淀 | `fsrs` 有多语言实现 |
| **P2** | 云 ASR / 云 TTS | 端上跑不动时的兜底 | 各家 API |
| **P2** | 内容管道（YT/B站等） | 只有要"一键导入视频"才需要 | yt-dlp |
| **P3** | 社交/额度/订阅 | 商业化，自用不需要 | — |

**给你的具体建议**：德语场景下 P0+P1 就够了，重点是 **Foliate + FFmpeg + 词典 + 词级对齐 + 德语 TTS**。内容管道可以先只做"本地文件导入"，把 yt-dlp 留到需要时再加。

## 抄不到的部分

| 项 | 原因 |
| :--- | :--- |
| 会员/额度 | 服务端持有（`/api/quota/consume`、`/api/subscription/ios/verify`），端上不判定 |
| 自有内容代理 | `podcast.onebook2025.top` 的转写/翻译结果不落端 |
| 38 MB 内置词典 | 数据有授权，不能直接搬 |
| 音色模型 | HF 公开可下（`csukuangfj/vits-mms-deu` 等），但要自行处理许可与体积 |

## 与前一个样本的对照

| 维度 | 影子跟读Pro（Flutter） | OneBook（原生 Swift） |
| :--- | :--- | :--- |
| 播放内核 | mpv / media_kit | FFmpeg + AVAudioEngine |
| 阅读 | 无 | Foliate（EPUB3 标准） |
| ASR | sherpa_onnx（单一） | whisper.cpp + CoreML + ONNX（三路） |
| 对齐 | 无（靠字幕文件） | 自研 StoryAlignCore |
| AI | 全部在服务端 | 端上 Gemma + 云端可选 |
| 会员判定 | 端上有逻辑 | 纯服务端 |
| 复刻价值 | 视频跟读的播放器手感 | 阅读+音频对齐的完整方法论 |

**结论**：想学"怎么把书和音频缝在一起"，看 OneBook；想学"视频逐句跟读的播放器"，看影子跟读Pro。两者拼起来才是完整的精读产品。

## 未验证项

- 所有接口的**实际请求/响应格式**（静态只能拿到路径，拿不到 JSON 结构）；
- 后台音频 + 对齐同时运行时的真实耗电与内存；
- `StoryAlignCore` 的对齐算法（DTW 还是 CTC，需动态或反编译确认）；
- 39 个 TTS 音色的实际可用性（部分模型可能只在特定区域下载成功）。
