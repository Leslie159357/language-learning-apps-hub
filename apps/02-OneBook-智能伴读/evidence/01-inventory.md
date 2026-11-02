# 01 · 包体与二进制清单

样本：`OneBook- LearnMultilingual Hub` v3.7.24（`com.linyi.OneBook`，MinOS 15.0，2026-10-30 上架版本）
口径：解密后 `baseline/Payload/OneBook.app` 实读，解密结果 `cryptid=0` 全部核对。

## 1. 包体规模

| 项 | 值 |
| :--- | :--- |
| 商店包（enc.ipa） | 165.6 MB |
| 解密包（dec.ipa） | 113.9 MB |
| zip 条目 | 2,366 |
| `.app` 内文件数 | 2,144 |
| `.app` 实际占用 | 214.7 MB |
| Mach-O 二进制 | 13 个 / 116.3 MB / 23,184 页 |
| 主二进制 | `OneBook` 57.24 MB，符号表仅 5,395 条（已 strip） |

主二进制被 strip 到只剩动态符号，所以**类名不能从符号表取**，必须读 `__objc_classname` 与 `__swift5_types` 这两张运行时元数据表。这是本轮与旧报告的方法差异根源。

## 2. 全部二进制（解密后）

| 二进制 | 大小 | 符号数 | 作用 |
| :--- | ---: | ---: | :--- |
| `OneBook` | 57.24 MB | 5,395 | 宿主 + 全部业务代码 |
| `CLiteRTLM` | 42.24 MB | 123,703 | LiteRT(-LM) 端上推理运行时，LLM 执行层 |
| `libavcodec` | 7.74 MB | 831 | FFmpeg 编解码 |
| `libavformat` | 3.33 MB | 984 | 容器解析 |
| `libavfilter` | 2.35 MB | 644 | 滤镜/重采样链路 |
| `OnDeviceWhisperKit` | 1.53 MB | 5,842 | 端上 Whisper 封装（Swift API 层） |
| `ffmpegkit` | 0.54 MB | 3,831 | FFmpeg 命令层封装 |
| `libavutil` | 0.43 MB | 740 | FFmpeg 基础库 |
| `libswscale` | 0.36 MB | 76 | 色彩/尺寸转换 |
| `OnDeviceGemmaKit` | 0.24 MB | 1,047 | Gemma 调用封装（落在 CLiteRTLM 上） |
| `libswresample` | 0.12 MB | 67 | 音频重采样 |
| `OneBookSleepTimerWidgetExtension` | 0.11 MB | 217 | 睡眠定时器小组件 `com.linyi.OneBook.SleepTimerWidget` |
| `libavdevice` | 0.10 MB | 205 | 采集设备（未常规使用） |

对比同类：OneBook 没有 mpv、没有 VLC、没有 AVPlayer 之外的第三方播放内核，音频播放走 AVAudioEngine + FFmpeg 解码，视频播放交给系统/WebView。这与 `影子跟读Pro`（mpv）路线完全不同。

## 3. 包内资源构成

| 资源 | 大小 | 说明 |
| :--- | ---: | :--- |
| `dictionary.sqlite.lzfse` | 38.0 MB | 内置词典库，LZFSE 压缩，首次启动解压 |
| `ExtraPython/site-packages` | 16.0 MB / 1,268 个 `.py` | Python 3.10 站点包：`yt_dlp`、`mdict_utils`、`mutagen`、`requests`、`certifi`、`charset_normalizer`、`urllib3` |
| `SherpaOnnxModels/espeak-ng-data.bundle` | ~12 MB | espeak-ng 音素词典（多语发音，TTS 前置） |
| `phrase_lexicon.sqlite` | 4.4 MB | 短语/固定搭配库 |
| `Python-iOS_PythonSupport.bundle/lib/python310.zip` | 4.0 MB | Python 3.10 标准库 |
| `Assets.car` | 2.5 MB | 图标与图像资源 |
| `foliate-viewer-bundle.js` | — | Foliate 阅读引擎（JS）打进包内，由 WKWebView 加载 |
| `.lproj` 语言包 | 45 个 | 含 `de`、`th`、`zh-Hans`、`zh-Hant` 等 45 种语言 |
| 字体 | 10 个 TTF | Merriweather / Domine / Oswald / Roboto Condensed 等阅读排版字体 |

45 种界面语言里包含德语和泰语，这一点对"给两个人各做一套"有直接参考价值：它的本地化层是完整的，不是只做中英。

## 4. 系统框架依赖（84 个 dylib，节选有信息量的）

`AVFAudio` `AVFoundation` `Accelerate` `CoreML` `CoreImage` `CoreText` `Vision` `VisionKit` `NaturalLanguage` `Speech` `Translation` `StoreKit` `CloudKit` `PDFKit` `PencilKit` `MediaPlayer` `ReplayKit` `WebKit` `AuthenticationServices` `ActivityKit` `LinkPresentation` `Metal` `MetalKit` `libsqlite3` `libcompression` `libbz2` `libswiftAppleArchive`

两个值得注意的信号：

- **`Translation`（Apple 官方翻译框架）与 `NaturalLanguage` 同时存在** → 翻译有本地通道，不是全靠云；
- **`CloudKit` 出现 28 次** → 备份/同步有 iCloud 分支，与 WebDAV 分支并存。
